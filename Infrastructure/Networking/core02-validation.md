# CORE02 - Validation

## Objetivo

Registrar os comandos utilizados para validar a configuração de rede, routing, forwarding e firewall implementados no `CORE02`.

## Network Interfaces

Verificar as interfaces VLAN:

    ip -br addr show | grep -E 'vmbr2(\.10|\.30)?'

Estado esperado:

    vmbr2.10   10.0.100.2/29
    vmbr2.30   192.168.100.1/29

## IPv4 Forwarding

Verificar:

    sysctl net.ipv4.ip_forward

Estado esperado:

    net.ipv4.ip_forward = 1

## Routing Table 100

Verificar:

    ip route show table 100

Estado esperado:

    default via 10.0.100.1 dev vmbr2.10
    10.0.100.0/29 dev vmbr2.10 scope link src 10.0.100.2

## Policy Rule

Verificar:

    ip rule

Estado esperado:

    from 192.168.100.0/29 lookup 100

## Gateway

Validar conectividade do `CORE02` com o gateway da VLAN 10:

    ping -c 3 10.0.100.1

Também pode ser validada a resolução de vizinhança:

    ip neigh show dev vmbr2.10

Estado observado:

    10.0.100.1 lladdr bc:24:11:59:1f:f8 REACHABLE

## nftables

Verificar o ruleset carregado:

    nft list ruleset

O ruleset deve apresentar a tabela:

    table inet sandbox_filter

Com as chains:

    forward
    input

## Persistência do nftables

Verificar o serviço:

    systemctl status nftables --no-pager

Estado esperado:

    Loaded: loaded
    enabled
    Active: active (exited)

Verificar o arquivo persistente:

    /etc/nftables.conf

## Estado Final

A validação do `CORE02` deve confirmar:

    vmbr2.10       10.0.100.2/29
    vmbr2.30       192.168.100.1/29
    IPv4 forwarding habilitado
    Policy Rule para 192.168.100.0/29
    Routing Table 100 configurada
    nftables carregado
    Regras de isolamento presentes