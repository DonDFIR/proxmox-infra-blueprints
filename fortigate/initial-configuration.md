# FortiGate Initial Configuration

## Current platform

- FortiOS `7.6.7 build 3704`
- Permanent Evaluation License
- Timezone: `America/Sao_Paulo`

## Initial configuration scope

A configuração inicial contempla:

- License activation
- Host/network identity
- Interfaces
- Administrative access
- Timezone
- NTP
- DNS
- DHCP
- Firewall policies

## Management model

A interface ADMIN é a rede administrativa principal.

```text
ADMIN gateway: 10.0.100.1
```

O acesso administrativo deve ser controlado no próprio FortiGate, separadamente das policies de trânsito.
