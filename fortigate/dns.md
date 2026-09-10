# FortiGate DNS

## Architecture

Clients do not point directly to NextDNS.

```text
Client
  |
  v
FortiGate interface IP
  |
  v
FortiGate DNS forwarding
  |
  v
NextDNS
```

## Upstream DNS

```text
Primary:   45.90.28.250
Secondary: 45.90.30.250
Protocol:  cleartext
```

The current FortiOS/license combination exposes `cleartext` for the FortiGate upstream DNS configuration.

## DNS Server mode

```text
port2 → forward-only
port3 → forward-only
```

## DHCP DNS

### ADMIN

```text
DNS: 10.0.100.1
```

### CLUSTER

```text
DNS: 172.16.0.1
```

## Validation

ADMIN:

```bash
resolvectl status
```

Expected DNS:

```text
10.0.100.1
```

CLUSTER:

```bash
dig www.google.com
```

Expected server:

```text
SERVER: 172.16.0.1#53
```

## Design rationale

Centralizing DNS at the FortiGate prevents endpoints from bypassing the intended DNS security path and provides one control point for future logging and policy enforcement.
