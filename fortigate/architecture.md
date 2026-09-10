# FortiGate Architecture

## Role

O FortiGate atua como edge firewall e ponto central de controle de rede.

## Logical flow

```text
                         Internet
                            |
                         Vivo ISP
                            |
                         port1 WAN
                            |
                     +--------------+
                     |   FortiGate  |
                     +--------------+
                       |          |
                port3 ADMIN    port2 CLUSTER
                10.0.100.1     172.16.0.1
                       |          |
                    Admin PC    Proxmox
                                Nodes / VMs
```

## Security layers

```text
Endpoint
   |
FortiGate
   |-- Firewall / NAT
   |-- Security Profiles
   |-- DNS Forwarding
   |-- NTP
   |
NextDNS
   |
Internet
```

## Design principles

1. FortiGate é o ponto central de controle de tráfego.
2. Clientes utilizam o FortiGate como DNS resolver.
3. NextDNS fornece DNS Security e filtering.
4. ADMIN possui maior liberdade operacional.
5. CLUSTER possui uma política de saída mais restritiva.
6. Segmentação de workloads será realizada no Proxmox Firewall.
7. Security Profiles são ativados e validados individualmente.
