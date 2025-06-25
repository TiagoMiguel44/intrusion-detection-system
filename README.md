# Sistema de Deteção de Intrusões com Snort e ELK Stack

Este projeto implementa um sistema de deteção de intrusões (IDS) com Snort 2, integrando com o ELK Stack para visualização dos alertas em tempo real.

## Tecnologias utilizadas

- Snort 2
- Elasticsearch
- Logstash
- Kibana
- Kali Linux (para simular ataques)
- Ubuntu Server

## Funcionalidades

- Deteção de:
  - Scan de portas (Nmap)
  - Tentativas de brute-force SSH
  - Ping sweep e outros padrões
- Regras personalizadas Snort
- Visualização em tempo real via Kibana
- Simulação de ataques para testes

## 📂 Estrutura do Projeto (PLANO)

snort-elk-ids/
├── config/ # Configurações do Snort
│ └── snort-rules/ # Regras personalizadas de deteção
│ ├── custom.rules # Ficheiro com as tuas regras Snort
│ └── sid-msg.map # Mapeamento de SIDs para mensagens legíveis
│
├── elk/ # Ficheiros de configuração do ELK Stack
│ ├── logstash.conf # Configuração do Logstash para parsing dos alertas do Snort
│ ├── elasticsearch.yml # Configuração do Elasticsearch
│ └── kibana-dashboard.json # Export de dashboards prontos no Kibana
│
├── scripts/ # Scripts auxiliares
│ └── setup-snort.sh # Script para instalar e configurar o Snort
│
├── tests/ # Simulações de ataques
│ └── attack-scenarios.md # Documentação dos ataques realizados para testar o IDS
│
├── docs/ # Documentação e relatórios do projeto
│ └── relatorio.md # Relatório técnico do projeto (explicações, prints, etc.)
│
└── README.md # Este ficheiro com a descrição geral do projeto

## 📂 Estrutura do Projeto (O QUE TENHO POR AGORA)

intrusion-detection-system/
├── README.md
├── docs/
│   └── arquitetura.png         # (opcional) Diagrama de arquitetura
├── config/
│   ├── snort.conf              # Ficheiro principal de configuração do Snort
│   └── rules/
│       └── local.rules         # Regras personalizadas
├── elk/
│   ├── elasticsearch/          # Configurações específicas (se necessário)
│   ├── logstash/
│   │   └── snort.conf          # Pipeline de ingestão do Logstash
│   └── kibana/
│       └── dashboard.ndjson    # Export de dashboard (se aplicável)
├── logs/
│   └── alert                   # Log gerado pelo Snort (pode ser ignorado no Git)
├── scripts/
│   └── test_ping.sh            # Scripts de ataque/simulação
└── .gitignore



## 🧪 Testes Realizados

| Ataque        | Ferramenta   | Resultado Esperado             | Estado  |
|----------------|--------------|----------------------------------|---------|
| Ping           | `ping`       | Alerta ICMP                    | ✅       |
| Port Scan      | `nmap`       | Alerta de scan de portas       | 🔜       |
| SSH BruteForce | `hydra`      | Alerta de brute-force          | 🔜       |

Trabalho dia 24/06

🖥️ 1. Ambiente preparado

    - Máquina virtual criada (Ubuntu Server)

    - Sistema atualizado

    - Conectividade testada (ex: ping entre máquinas)

🕵️‍♂️ 2. Snort instalado e configurado

    - Versão: Snort 2.9.20

    - snort.conf ajustado corretamente

    - Criado e usado o ficheiro de regras personalizadas: local.rules

    - Snort a correr com sucesso (ex: snort -A console -q -c /etc/snort/snort.conf -i <interface>)

    - Teste feito com ping → Alerta detetado com sucesso ✅

    📦 3. Elasticsearch instalado e operacional

    - Corrigido erro inicial relacionado com Java 21. A versao do snort que esta a ser utilizada nao trabalhava com Java 21, entao foi necessario mudar para Java 11

    - Instalado e ativado Java 11

    - Heap ajustado para máquinas com pouca RAM (512m)

    - Elasticsearch iniciado e verificado com:

      - curl -X GET "localhost:9200/"

      - Elasticsearch a funcionar sem erros ✅

 Integração Snort + ELK Stack — Registo do Trabalho (25/06)

Este documento regista o progresso realizado no dia 25/06 na integração entre o Snort e a stack ELK (Elasticsearch, Logstash, Kibana), incluindo a configuração da rede, Snort, Logstash e visualização no Kibana.

---

## 1. Configuração de Rede na Máquina Virtual

Para permitir que o **Snort monitorize tráfego externo** (ex: pings de uma máquina Windows host), foi necessário configurar **dois adaptadores de rede** na máquina virtual:

- **Adaptador 1: NAT**
  - Usado para dar acesso à internet na VM (atualizações, pacotes, etc.)
  - Permite que a VM tenha saída para a internet, mas **não é útil para tráfego local do host** (ex: ping da máquina real)

- **Adaptador 2: Host-only Adapter**
  - Configurado como *Host-only Adapter*
  - Permite que o **host Windows comunique com a VM**
  - Essencial para simular ataques e tráfego real do host para a VM

### 🧪 Teste efetuado:

Depois de configurar os dois adaptadores, foi possível executar:

```bash
ping <IP_da_VM>

A partir do Windows, e o Snort passou a capturar esse tráfego ICMP, gerando alertas no ficheiro snort.alert.fast.

Também foi testada a captura do tráfego ICMP com o comando:

sudo tcpdump -i <interface>

2. Configuração do Snort

    Snort foi configurado para gerar alertas no ficheiro:

/var/log/snort/snort.alert.fast

    Foi necessário verificar a existência do diretório:

sudo mkdir -p /var/log/snort

    Garantido que o Snort tinha permissões para escrever nesse diretório:

sudo chown snort:snort /var/log/snort

3. Configuração do Logstash

    Criado o ficheiro de configuração:

/etc/logstash/conf.d/snort.conf

    Após isso, o Logstash foi reiniciado para aplicar as configurações:

sudo systemctl restart logstash

4. Validação do Logstash e Elasticsearch

    Confirmado que o Logstash está a processar logs e a enviar para o Elasticsearch.

    Usado o seguinte comando para listar os índices existentes:

curl -X GET "localhost:9200/_cat/indices?v"

    Verificou-se que o índice snort-alerts-* existia e estava a receber documentos (docs.count > 0), o que indica que os alertas estavam a ser ingeridos com sucesso.

5. Configuração do Kibana

    Criado um Data View (Index Pattern) com o nome:

snort-alerts-*

    Confirmado que os logs estavam visíveis no Discover do Kibana, com os campos parseados corretamente.

✅ A integração entre Snort e ELK foi validada com sucesso, e os alertas estão agora disponíveis para exploração e visualização no Kibana.


---

Se quiseres, posso adicionar também um sumário geral ou uma secção para "Próximos Passos" 

