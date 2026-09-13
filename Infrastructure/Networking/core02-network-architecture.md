# CORE02 - Network Architecture

## Role

O `CORE02` atua como ponto L3 para os segmentos de rede utilizados pela infraestrutura da Sandbox.

A arquitetura utiliza:

- `vmbr2` como bridge VLAN-aware
- `VLAN 10` para a rede `10.0.100.0/29`
- `VLAN 30` para a rede `192.168.100.0/29`
- `Policy-Based Routing` através da `table 100`
- `nftables` para isolamento da Sandbox

## Network Flow

    Sandbox
    192.168.100.3
          |
          | VLAN 30
          |
      vmbr2.30
    192.168.100.1
          |
       CORE02
          |
      vmbr2.10
    10.0.100.2
          |
    10.0.100.1

## VLAN 10

    Network: 10.0.100.0/29
    CORE02:  10.0.100.2
    Gateway: 10.0.100.1

## VLAN 30

    Network: 192.168.100.0/29
    CORE02:  192.168.100.1
    Sandbox: 192.168.100.3

## Routing

O tráfego originado pela rede `192.168.100.0/29` utiliza a `table 100`.

    from 192.168.100.0/29 lookup 100

A tabela utiliza `10.0.100.1` como default gateway:

    default via 10.0.100.1 dev vmbr2.10

## Forwarding

O `CORE02` possui IPv4 forwarding habilitado:

    net.ipv4.ip_forward = 1

Isso permite o encaminhamento de tráfego entre as interfaces L3 utilizadas pela arquitetura.

## Isolation

O isolamento da Sandbox é realizado no próprio `CORE02` através do `nftables`.

A rede `192.168.100.0/29` não deve possuir comunicação lateral com:

    172.16.0.0/29
    10.0.100.0/29
    10.10.160.0/21

O acesso ao próprio endereço de gerenciamento `172.16.0.3` também é bloqueado.

DNS para `10.0.100.1` permanece explicitamente permitido em TCP/53 e UDP/53.

## Persistence

As configurações de rede e routing estão armazenadas em:

    /etc/network/interfaces

As regras de firewall estão armazenadas em:

    /etc/nftables.conf

A inicialização automática do firewall é realizada pelo serviço:

    nftables.service

## Related Documentation

    Infrastructure/Networking/vlan-30-sandbox.md
    Infrastructure/Networking/routing-table-100.md
    Security/Rules/sandbox-isolation.md
    Infrastructure/Networking/core02-persistence.md
    Infrastructure/Networking/core02-validation.md