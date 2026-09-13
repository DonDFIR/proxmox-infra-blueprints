# Sandbox Network - Summary

## Network

| Item | Valor |
|---|---|
| VLAN | `30` |
| Network | `192.168.100.0/29` |
| Gateway | `192.168.100.1` |
| Sandbox | `192.168.100.3` |
| CORE02 VLAN interface | `vmbr2.30` |

## Transport

A VLAN 30 é transportada pela bridge VLAN-aware `vmbr2`.

    bridge-vlan-aware yes
    bridge-vids 10 30

## Routing

O tráfego originado pela Sandbox utiliza Policy-Based Routing:

    from 192.168.100.0/29 lookup 100

A `table 100` utiliza:

    default via 10.0.100.1 dev vmbr2.10

O IPv4 forwarding está habilitado:

    net.ipv4.ip_forward = 1

## Isolation

O isolamento da Sandbox é aplicado pelo `nftables` no `CORE02`.

Permitido:

    192.168.100.0/29 → 10.0.100.1:53 TCP
    192.168.100.0/29 → 10.0.100.1:53 UDP

Bloqueado:

    192.168.100.0/29 → 172.16.0.0/29
    192.168.100.0/29 → 10.0.100.0/29
    192.168.100.0/29 → 10.10.160.0/21

Também é bloqueado o acesso da Sandbox ao endereço `172.16.0.3` do próprio `CORE02`.

## Persistence

Network:

    /etc/network/interfaces

Routing:

    post-up / post-down em `/etc/network/interfaces`

Firewall:

    /etc/nftables.conf

Service:

    nftables.service

## Validation

    ip -br addr
    ip rule
    ip route
    ip route show table 100
    sysctl net.ipv4.ip_forward
    nft list ruleset

## Result

A infraestrutura de rede da Sandbox foi implementada no `CORE02` utilizando VLAN 30, Policy-Based Routing e `nftables`.

A configuração foi aplicada de forma persistente para sobreviver à reinicialização do host.