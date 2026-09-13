# Routing Table 100 - Sandbox

## Objetivo

Implementar Policy-Based Routing (PBR) no `CORE02` para que o tráfego originado pela rede da Sandbox utilize a `routing table 100`.

## Routing Table

A `table 100` possui as seguintes rotas:

| Destination | Gateway | Interface | Source |
|---|---|---|---|
| `10.0.100.0/29` | - | `vmbr2.10` | `10.0.100.2` |
| `default` | `10.0.100.1` | `vmbr2.10` | - |

Estado:

    ip route show table 100

Resultado:

    default via 10.0.100.1 dev vmbr2.10
    10.0.100.0/29 dev vmbr2.10 scope link src 10.0.100.2

## Policy Rule

O tráfego originado pela rede `192.168.100.0/29` utiliza a `table 100`.

    ip rule add from 192.168.100.0/29 table 100

Estado final:

    ip rule

    0:      from all lookup local
    32765:  from 192.168.100.0/29 lookup 100
    32766:  from all lookup main
    32767:  from all lookup default

## Persistência

A `table 100` é reconstruída através das diretivas `post-up` da interface `vmbr2.10` em:

    /etc/network/interfaces

Configuração:

    post-up ip route replace 10.0.100.0/29 dev vmbr2.10 src 10.0.100.2 table 100
    post-up ip route replace default via 10.0.100.1 dev vmbr2.10 table 100

As rotas são removidas durante a desativação da interface:

    post-down ip route del default via 10.0.100.1 dev vmbr2.10 table 100 || true
    post-down ip route del 10.0.100.0/29 dev vmbr2.10 src 10.0.100.2 table 100 || true

A `ip rule` é reconstruída através das diretivas `post-up` da interface `vmbr2.30`:

    post-up ip rule add from 192.168.100.0/29 table 100

E removida durante a desativação:

    post-down ip rule del from 192.168.100.0/29 table 100 || true

## Validação

Consultar a policy:

    ip rule

Consultar a tabela:

    ip route show table 100

Testar a rota para um destino externo utilizando a origem da Sandbox:

    ip route get 8.8.8.8 from 192.168.100.3

A configuração deve direcionar o tráfego da rede `192.168.100.0/29` para o gateway `10.0.100.1` através da interface `vmbr2.10`.