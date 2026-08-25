# Instalação do UniFi OS Server

## Objetivo

Instalar o UniFi OS Server em Ubuntu 26.04 LTS para disponibilizar o UniFi Network Application e iniciar a administração do UniFi Flex Mini.

## Pré-requisitos

* Ubuntu 26.04 LTS
* Arquitetura x86_64
* Conectividade com a Internet
* Conta Ubiquiti
* Binário oficial do UniFi OS Server

## Validação do sistema

Arquitetura identificada:

```text
x86_64
```

Sistema operacional:

```text
Ubuntu 26.04 LTS
```

## Download

Foi obtido o seguinte build oficial:

```text
UniFi OS Server 5.1.37
```

Arquivo:

```text
9aee-linux-x64-5.1.37-a88d909c-2ac0-43f8-bb22-2bff3b673cbb.37-x64
```

SHA-256 calculado:

```text
4a5b1f7f29f25733cfc5f7497a63e3dbd4f4a616b352cbbd817a89eb1fa66b61
```

## Validação do arquivo

O arquivo foi identificado como:

```text
ELF 64-bit LSB pie executable
x86-64
```

Inicialmente o arquivo não possuía permissão de execução.

Foi aplicada:

```bash
chmod +x 9aee-linux-x64-5.1.37-a88d909c-2ac0-43f8-bb22-2bff3b673cbb.37-x64
```

## Instalação

O instalador foi executado com privilégios administrativos:

```bash
sudo ./9aee-linux-x64-5.1.37-a88d909c-2ac0-43f8-bb22-2bff3b673cbb.37-x64
```

O instalador confirmou:

```text
You are about to install UniFi OS Server version 5.1.37.
```

A instalação foi confirmada.

## Setup Web

Após a instalação, o UniFi OS Server disponibilizou a interface inicial em:

```text
https://192.168.15.4:11443
```

O endereço é temporário e será alterado durante a implementação da segmentação definitiva.

## Serviço

O serviço instalado pelo UniFi OS Server é:

```text
uosserver.service
```

Validação:

```bash
systemctl status uosserver
```

Resultado esperado:

```text
Loaded: loaded ... enabled
Active: active (running)
```

Foi confirmado que o serviço está:

* ativo;
* executando corretamente;
* configurado para iniciar automaticamente com o sistema.

## Problema encontrado

### HTTP 403 durante tentativa inicial

A primeira tentativa utilizou uma URL de instalação que retornou:

```text
curl: (22) The requested URL returned error: 403
```

A abordagem foi abandonada e o instalador foi obtido posteriormente através do mecanismo oficial de download da Ubiquiti.

## Estado final

O UniFi OS Server 5.1.37 está instalado e operacional no Ubuntu 26.04 LTS.

O próximo estágio é concluir o setup inicial, autenticar a conta Ubiquiti, configurar o UniFi Network e adotar o UniFi Flex Mini.

## Observação de arquitetura

A instalação atual é provisória.

O UniFi OS Server será posteriormente migrado para uma VM dentro do Proxmox Enterprise Lab, permitindo que o serviço permaneça disponível independentemente da estação de trabalho utilizada para administração.
