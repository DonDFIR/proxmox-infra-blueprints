# CORE02 - Persistence

## Objetivo

Registrar as configurações persistentes implementadas no `CORE02` para garantir que a infraestrutura de rede da Sandbox seja reconstruída automaticamente após reboot.

## Network Interfaces

Arquivo:

    /etc/network/interfaces

As interfaces VLAN são declaradas com `auto` e, portanto, são inicializadas durante o boot.

### VLAN 10

    auto vmbr2.10
    iface vmbr2.10 inet static
            address 10.0.100.2/29
            vlan-raw-device vmbr2

### VLAN 30

    auto vmbr2.30
    iface vmbr2.30 inet static
            address 192.168.100.1/29
            vlan-raw-device vmbr2

## Routing Table 100

As rotas da `table 100` são recriadas automaticamente através de `post-up` da interface `vmbr2.10`.

    post-up ip route replace 10.0.100.0/29 dev vmbr2.10 src 10.0.100.2 table 100
    post-up ip route replace default via 10.0.100.1 dev vmbr2.10 table 100

Durante a remoção da interface, as rotas são removidas:

    post-down ip route del default via 10.0.100.1 dev vmbr2.10 table 100 || true
    post-down ip route del 10.0.100.0/29 dev vmbr2.10 src 10.0.100.2 table 100 || true

## IP Rule

A regra responsável pelo Policy-Based Routing da Sandbox também é persistente.

Configuração associada à `vmbr2.30`:

    post-up ip rule add from 192.168.100.0/29 table 100
    post-down ip rule del from 192.168.100.0/29 table 100 || true

Regra esperada:

    from 192.168.100.0/29 lookup 100

## nftables

O ruleset de isolamento está armazenado em:

    /etc/nftables.conf

O serviço está habilitado para inicialização automática:

    systemctl enable nftables

O carregamento das regras é realizado pelo serviço:

    nftables.service

Validação:

    systemctl status nftables --no-pager

## IPv4 Forwarding

O `CORE02` utiliza IPv4 forwarding para encaminhar o tráfego entre os segmentos.

Estado configurado:

    net.ipv4.ip_forward = 1

## Aplicação das Configurações

Alterações na configuração de rede podem ser aplicadas com:

    ifreload -a

As regras persistentes do `nftables` podem ser recarregadas com:

    systemctl restart nftables

## Validação Pós-Reboot

Após reinicialização do `CORE02`, validar:

    ip -br addr

    ip rule

    ip route show table 100

    sysctl net.ipv4.ip_forward

    nft list ruleset

## Estado Esperado

Após o boot:

- `vmbr2.10` deve estar disponível com `10.0.100.2/29`.
- `vmbr2.30` deve estar disponível com `192.168.100.1/29`.
- A `table 100` deve conter a rota conectada para `10.0.100.0/29`.
- A `table 100` deve possuir default route via `10.0.100.1`.
- A `ip rule` deve direcionar `192.168.100.0/29` para a `table 100`.
- O `nftables` deve carregar o ruleset de isolamento.
- IPv4 forwarding deve permanecer habilitado.