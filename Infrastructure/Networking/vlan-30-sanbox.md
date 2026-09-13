# VLAN 30 - Sandbox

## Objetivo

Criar o segmento de rede dedicado à Sandbox no `CORE02`.

## Configuração

| Item | Valor |
|---|---|
| VLAN | `30` |
| Network | `192.168.100.0/29` |
| Interface | `vmbr2.30` |
| Gateway | `192.168.100.1` |
| Sandbox | `192.168.100.3` |

## Interface

    auto vmbr2.30
    iface vmbr2.30 inet static
            address 192.168.100.1/29
            vlan-raw-device vmbr2

## Endereçamento

| Endereço | Função |
|---|---|
| `192.168.100.0` | Network |
| `192.168.100.1` | CORE02 |
| `192.168.100.2` | Disponível |
| `192.168.100.3` | Sandbox |
| `192.168.100.4` | Disponível |
| `192.168.100.5` | Disponível |
| `192.168.100.6` | Disponível |
| `192.168.100.7` | Broadcast |

## Persistência

A configuração está armazenada em:

    /etc/network/interfaces

As alterações foram aplicadas utilizando:

    ifreload -a

Validação:

    ip -br addr show | grep -E 'vmbr2(\.10|\.30)?'