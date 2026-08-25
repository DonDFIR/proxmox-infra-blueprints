# UniFi Network Application

## Objetivo

Documentar a implantação e operação do UniFi Network Application utilizado para gerenciamento dos equipamentos UniFi do laboratório.

O UniFi Network Application atua como plano de gerenciamento da infraestrutura UniFi, sendo responsável pela administração do switch UniFi Flex Mini e pelas configurações de rede compatíveis com os equipamentos adotados.

## Ambiente

| Item                | Valor                           |
| ------------------- | ------------------------------- |
| Sistema operacional | Ubuntu 26.04 LTS                |
| Arquitetura         | x86_64                          |
| UniFi OS Server     | 5.1.37                          |
| UniFi Network       | Gerenciado pelo UniFi OS Server |
| IP inicial          | 192.168.15.4                    |
| Porta HTTPS         | 11443                           |
| Serviço             | `uosserver`                     |

> O endereço IP `192.168.15.4` é temporário e será substituído quando a segmentação definitiva do laboratório for implementada.

## Arquitetura

O UniFi OS Server é executado inicialmente em uma estação Ubuntu para implantação e validação.

Posteriormente, o serviço será migrado para uma VM dentro da infraestrutura Proxmox, mantendo o gerenciamento independente da estação de trabalho.

```text
UniFi OS Server
       │
       ▼
UniFi Network
       │
       ▼
UniFi Flex Mini
```

## Responsabilidades

O UniFi será utilizado para:

* Gerenciamento do Flex Mini
* Configuração de redes
* Configuração de VLANs
* Segmentação da rede LAN
* Gerenciamento da infraestrutura UniFi
* Administração futura de equipamentos UniFi adicionais

O routing, NAT, firewall e políticas de segurança serão responsabilidade do FortiGate.

## Estado atual

* UniFi OS Server instalado
* Serviço `uosserver` ativo
* Inicialização automática habilitada
* Interface Web de configuração disponível
* Flex Mini ainda será integrado ao controller
* Segmentação definitiva ainda não implementada

## Próximos passos

1. Finalizar o setup inicial do UniFi OS Server.
2. Associar o servidor à conta Ubiquiti.
3. Configurar o UniFi Network.
4. Adotar o Flex Mini.
5. Validar gerenciamento do switch.
6. Definir redes e VLANs.
7. Migrar posteriormente o UniFi OS Server para o Proxmox.
