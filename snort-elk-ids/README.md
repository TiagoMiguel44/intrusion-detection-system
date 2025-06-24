# Sistema de Deteção de Intrusões com Snort e ELK Stack

Este projeto implementa um sistema de deteção de intrusões (IDS) com Snort 2.9.20, integrado com o ELK Stack para visualização dos alertas em tempo real.

## Tecnologias utilizadas

- Snort 2.9.20
- Elasticsearch
- Logstash
- Kibana
- Kali Linux (para simular ataques)
- Ubuntu Server

## Funcionalidades

- Deteção de scan de portas (Nmap), brute-force SSH, ping sweep, etc.
- Regras personalizadas Snort
- Visualização em tempo real via Kibana
- Simulação de ataques para testes

## Estrutura do projeto

snort-elk-ids/
├── config/
│   └── snort-rules/
│       ├── custom.rules
│       └── sid-msg.map
├── elk/
│   ├── elasticsearch.yml
│   ├── kibana-dashboard.json
│   └── logstash.conf
├── scripts/
│   └── setup-snort.sh
├── tests/
│   └── attack-scenarios.md
├── docs/
│   └── relatorio.md
└── README.md
