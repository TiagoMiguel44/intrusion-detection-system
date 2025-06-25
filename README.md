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

Trabalho dia 25/06

## 1. Configuração de Rede na Máquina Virtual

Para permitir que o Snort monitorize tráfego externo (ex: pings de uma máquina Windows host), foi necessário configurar dois adaptadores de rede na máquina virtual:

    Adaptador 1: NAT

      - Usado para dar acesso à internet na VM (atualizações, pacotes, etc.)

      - Permite que a VM tenha saída para a internet, mas não é útil para tráfego local do host (ex: ping da máquina real)

    Adaptador 2: Host-only Adapter

      - Configurado Host-only Adapter

      - Permite que o host Windows comunique com a VM

      - Essencial para simular ataques e tráfego real do host para a VM

🧪 Teste feito:

Depois de configurar os dois adaptadores, foi possível executar:

ping <IP_da_VM>

...a partir do Windows, e o Snort passou a capturar esse tráfego ICMP, gerando alertas no ficheiro snort.alert.fast. Foi tambem testado a captura do trafego ICMP pelo tcpdump (sudo tcpdump -i ).

## 2. Configuração do Snort

    Snort configurado para gerar alertas no ficheiro /var/log/snort/snort.alert.fast. Foi necessario fazer a verificacao de que existia de facto o diretorio /var/log/snort/ (caso nao existisse teriamos que criar pelo comando sudo mkdir -p /var/log/snort) e tambem verificar se snort tinha permissoes para escrever no diretorio (sudo chown snort:snort /var/log/snort).

## 3. Configuração do Logstash

    Criado ficheiro /etc/logstash/conf.d/snort.conf
    Foi reiniciado o Logstash para aplicar configuração.

## 4. Validação do Logstash e Elasticsearch

    Foi verificado que o Logstash está a processar logs e a enviar para Elasticsearch. Foi usado o comando curl -X GET "localhost:9200/_cat/indices?v". Este comando permitiu listar todos os ativos no Elasticsearch e confirmou que o indice snort-alerts-* existia e que estava a receber documentos (docs.count > 0).

## 5. Configuração do Logstash

    Foi criado Data View (Index Pattern) snort-alerts-* no Kibana.

    Confirmada a visualização dos logs no Discover do Kibana.



