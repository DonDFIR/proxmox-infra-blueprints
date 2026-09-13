# Sandbox Isolation Rules

## Objetivo

Controlar o tráfego originado pela rede da Sandbox (`192.168.100.0/29`) no `CORE02`, impedindo comunicação com as redes internas definidas no escopo do ambiente.

A Sandbox deve possuir conectividade de saída, mas não deve realizar comunicação lateral com as redes internas protegidas.

## Firewall

O controle é realizado através do `nftables`.

Arquivo persistente:

    /etc/nftables.conf

Tabela utilizada:

    sandbox_filter

## FORWARD

A chain `forward` controla o tráfego que atravessa o `CORE02` em direção a outros destinos.

### DNS permitido

O DNS utilizado pela Sandbox é `10.0.100.1`.

Somente DNS TCP/53 e UDP/53 é permitido:

    ip saddr 192.168.100.0/29 ip daddr 10.0.100.1 tcp dport 53 accept
    ip saddr 192.168.100.0/29 ip daddr 10.0.100.1 udp dport 53 accept

### Redes bloqueadas

O tráfego da Sandbox para as seguintes redes é bloqueado:

    192.168.100.0/29 → 172.16.0.0/29
    192.168.100.0/29 → 10.0.100.0/29
    192.168.100.0/29 → 10.10.160.0/21

Regras:

    ip saddr 192.168.100.0/29 ip daddr 172.16.0.0/29 drop
    ip saddr 192.168.100.0/29 ip daddr 10.0.100.0/29 drop
    ip saddr 192.168.100.0/29 ip daddr 10.10.160.0/21 drop

A chain `forward` mantém `policy accept`, permitindo tráfego que não corresponda às regras de bloqueio.

## INPUT

A chain `input` controla o tráfego destinado ao próprio `CORE02`.

O acesso da Sandbox ao endereço de gerenciamento do `CORE02` é bloqueado:

    ip saddr 192.168.100.0/29 ip daddr 172.16.0.3 drop

Essa regra é necessária porque o tráfego destinado ao próprio `CORE02` utiliza o caminho `INPUT`, e não `FORWARD`.

## Configuração Final

    table inet sandbox_filter {
            chain forward {
                    type filter hook forward priority filter; policy accept;
                    ip saddr 192.168.100.0/29 ip daddr 10.0.100.1 tcp dport 53 accept
                    ip saddr 192.168.100.0/29 ip daddr 10.0.100.1 udp dport 53 accept
                    ip saddr 192.168.100.0/29 ip daddr 172.16.0.0/29 drop
                    ip saddr 192.168.100.0/29 ip daddr 10.0.100.0/29 drop
                    ip saddr 192.168.100.0/29 ip daddr 10.10.160.0/21 drop
            }

            chain input {
                    type filter hook input priority filter; policy accept;
                    ip saddr 192.168.100.0/29 ip daddr 172.16.0.3 drop
            }
    }

## Persistência

O ruleset foi salvo em:

    /etc/nftables.conf

O serviço `nftables` está habilitado para inicialização automática:

    systemctl enable nftables

Validação do serviço:

    systemctl status nftables --no-pager

O ruleset pode ser validado com:

    nft list ruleset

## Validação Funcional

A política foi validada a partir da Sandbox.

Resultados esperados:

| Destino | Resultado |
|---|---|
| `10.0.100.1:53` TCP/UDP | Permitido |
| Internet | Permitido |
| `10.0.100.3` | Bloqueado |
| `10.10.160.2` | Bloqueado |
| `172.16.0.3` | Bloqueado |
| `172.16.0.2` | Bloqueado |
| `172.16.0.4` | Bloqueado |

## Princípio de Isolamento

A VLAN 30 fornece conectividade para a Sandbox.

O routing determina o caminho de saída.

O `nftables` determina quais destinos internos podem ser alcançados.

Dessa forma, a Sandbox permanece funcional para acesso externo sem possuir conectividade lateral com as redes internas definidas neste escopo.