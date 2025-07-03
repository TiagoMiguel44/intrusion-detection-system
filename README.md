# Sistema de Deteção de Intrusões com Snort e ELK Stack

Este projeto implementa um sistema de deteção de intrusões (IDS) com Snort 2, integrando com o ELK Stack para visualização dos alertas em tempo real.

## Tecnologias Utilizadas

- Snort 2
- Elasticsearch
- Logstash
- Kibana
- Ubuntu Server
- Kali Linux (para simulação de ataques)
- WSL (para uso do Kali em Windows)

## Funcionalidades

- Deteção de:
  - ✅ Ping (ICMP Echo Requests)
  - ✅ Tentativas de brute-force SSH
  - ✅ Reverse Shells (porta 4444)
  - 🔜 Scan de portas (Nmap)
- Regras personalizadas no Snort (`local.rules`)
- Visualização em tempo real via Kibana
- Simulação de ataques reais para validação do IDS

## 📂 Estrutura do Projeto 

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

# 🔒 Regras Atuais (`local.rules`)

- Deteção de ICMP
alert icmp any any -> any any (msg:"ICMP packet detected"; sid:1000001; rev:1;)

- SSH Brute Force
alert tcp any any -> 192.168.56.102 22 (msg:"Possible SSH brute force attack"; flags:S; threshold:type threshold, track by_src, count 5, seconds 60; sid:1000002; rev:1;)

- Reverse Shell
alert tcp $EXTERNAL_NET any -> $HOME_NET 4444 (msg:"Possible reverse shell attempt"; sid:1000003; rev:1;)


## 🧪 Testes Realizados

| Ataque        | Ferramenta   | Resultado Esperado             | Estado  |
|----------------|--------------|----------------------------------|---------|
| Ping           | `ping`       | Alerta ICMP                    | ✅       |
| Port Scan      | `nmap`       | Alerta de scan de portas       | 🔜       |
| SSH BruteForce | `hydra`      | Alerta de brute-force          | ✅       |
| Reverse Shell  | `nc` / `bash`| Alerta de reverse shell        | ✅       |




