# CORE02 - Network Final State

## Objetivo

Registrar o estado final da infraestrutura de rede configurada no `CORE02` para suportar a Sandbox isolada.

## Interfaces

| Interface | Address | Função |
|---|---|---|
| `vmbr1` | `172.16.0.3/29` | Management |
| `vmbr2.10` | `10.0.100.2/29` | VLAN 10 |
| `vmbr2.30` | `192.168.100.1/29` | VLAN 30 / Sandbox |

## VLANs

A bridge `vmbr2` está configurada como VLAN-aware:

    bridge-vlan-aware yes
    bridge-vids 10 30

Interfaces VLAN:

    vmbr2.10
    vmbr2.30

## VLAN 10

    Network: 10.0.100.0/29
    CORE02: 10.0.100.2
    Gateway: 10.0.100.1

## VLAN 30

    Network: 192.168.100.0/29
    CORE02: 192.168.100.1
    Sandbox: 192.168.100.3

## Routing

A Sandbox utiliza a `routing table 100`.

    ip rule

    32765: from 192.168.100.0/29 lookup 100

A tabela contém:

    10.0.100.0/29 dev vmbr2.10 scope link src 10.0.100.2
    default via 10.0.100.1 dev vmbr2.10

As rotas e a `ip rule` são reconstruídas automaticamente através das diretivas `post-up` e `post-down` em `/etc/network/interfaces`.

## IP Forwarding

O encaminhamento IPv4 está habilitado no `CORE02`:

    net.ipv4.ip_forward = 1

## Firewall

O isolamento da Sandbox é realizado através do `nftables`.

Arquivo persistente:

    /etc/nftables.conf

Serviço:

    nftables.service

Estado:

    enabled

O ruleset possui chains `forward` e `input`.

## Política de Isolamento

A rede `192.168.100.0/29` possui acesso DNS permitido para `10.0.100.1` utilizando TCP/53 e UDP/53.

O tráfego da Sandbox para as seguintes redes é bloqueado:

    172.16.0.0/29
    10.0.100.0/29
    10.10.160.0/21

O acesso da Sandbox ao próprio endereço `172.16.0.3` do `CORE02` também é bloqueado através da chain `input`.

## Persistência

### Network

    /etc/network/interfaces

Aplicação:

    ifreload -a

### Routing

As rotas da `table 100` e a `ip rule` são reconstruídas pelas configurações `post-up` e `post-down` das interfaces VLAN.

### Firewall

    /etc/nftables.conf

Serviço habilitado:

    systemctl enable nftables

Validação:

    systemctl status nftables --no-pager

## Validação Final

Interfaces:

    ip -br addr

Routing:

    ip rule
    ip route show table 100

Firewall:

    nft list ruleset

Forwarding:

    sysctl net.ipv4.ip_forward

## Resultado

O `CORE02` fornece a infraestrutura de rede necessária para a Sandbox através da VLAN 30, Policy-Based Routing e regras de isolamento no `nftables`.

A configuração é persistente e preparada para reconstrução após reboot do host.