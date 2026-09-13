# CORE02 - Post-Change Checklist

## Objetivo

Registrar a validação final realizada após a configuração da VLAN 30, Policy-Based Routing, IPv4 forwarding e isolamento da Sandbox.

## Network

- [x] `vmbr2.10` configurada
- [x] `vmbr2.30` configurada
- [x] VLAN 30 utilizando `192.168.100.0/29`
- [x] CORE02 utilizando `192.168.100.1/29`
- [x] Sandbox utilizando `192.168.100.3`

## Routing

- [x] IPv4 forwarding habilitado
- [x] `table 100` criada
- [x] Connected route para `10.0.100.0/29`
- [x] Default route via `10.0.100.1`
- [x] `ip rule` para `192.168.100.0/29`
- [x] Configuração persistente em `/etc/network/interfaces`

## Firewall

- [x] `nftables` configurado
- [x] DNS para `10.0.100.1` permitido em TCP/53
- [x] DNS para `10.0.100.1` permitido em UDP/53
- [x] Acesso à rede `172.16.0.0/29` bloqueado
- [x] Acesso à rede `10.0.100.0/29` bloqueado
- [x] Acesso à rede `10.10.160.0/21` bloqueado
- [x] Acesso da Sandbox ao `172.16.0.3` bloqueado
- [x] Ruleset persistente em `/etc/nftables.conf`
- [x] `nftables.service` habilitado

## Aplicação

Configuração de rede:

    ifreload -a

Firewall:

    systemctl restart nftables

## Validação

Interfaces:

    ip -br addr

Policy routing:

    ip rule

Routing table:

    ip route show table 100

IPv4 forwarding:

    sysctl net.ipv4.ip_forward

Firewall:

    nft list ruleset

## Resultado

A infraestrutura necessária para a rede da Sandbox foi configurada e validada no `CORE02`.

A configuração de rede, Policy-Based Routing e regras de isolamento possuem persistência após reinicialização do host.