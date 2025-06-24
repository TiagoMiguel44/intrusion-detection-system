# Sistema de Deteção de Intrusões com Snort e ELK Stack

Este projeto implementa um sistema de deteção de intrusões (IDS) com Snort 3, integrando com o ELK Stack para visualização dos alertas em tempo real.

## Tecnologias utilizadas

- Snort 3
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

## 📂 Estrutura do Projeto

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

## 🧪 Testes Realizados

| Ataque        | Ferramenta   | Resultado Esperado             | Estado  |
|----------------|--------------|----------------------------------|---------|
| Ping           | `ping`       | Alerta ICMP                    | ✅       |
| Port Scan      | `nmap`       | Alerta de scan de portas       | 🔜       |
| SSH BruteForce | `hydra`      | Alerta de brute-force          | 🔜       |

Trabalho dia 24/06

🖥️ 1. Ambiente preparado

    Máquina virtual criada (Ubuntu Server)

    Sistema atualizado

    Conectividade testada (ex: ping entre máquinas)

🕵️‍♂️ 2. Snort instalado e configurado

    Versão: Snort 2.9.20

    snort.conf ajustado corretamente

    Criado e usado o ficheiro de regras personalizadas: local.rules

    Snort a correr com sucesso (ex: snort -A console -q -c /etc/snort/snort.conf -i eth0)

    Teste feito com ping → Alerta detetado com sucesso ✅

    📦 3. Elasticsearch instalado e operacional

    Corrigido erro inicial relacionado com Java 21

    Instalado e ativado Java 11

    Heap ajustado para máquinas com pouca RAM (512m)

    Elasticsearch iniciado e verificado com:

curl -X GET "localhost:9200/"

Elasticsearch a funcionar sem erros ✅




