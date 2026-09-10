# FortiGate Validation

## IPv4 connectivity

Validated:

- DHCP
- Default gateway
- IPv4 routing
- NAT
- HTTPS connectivity

Example:

```bash
curl -4 -I https://example.com
```

Expected result: successful HTTP response.

## DNS

ADMIN:

```bash
resolvectl status
```

Expected:

```text
DNS: 10.0.100.1
```

CLUSTER:

```bash
dig www.google.com
```

Expected:

```text
SERVER: 172.16.0.1#53
```

## NextDNS

Validated through the NextDNS dashboard:

- Queries received
- Public source IP recognized
- Filtering operational
- Block events recorded

## NTP

FortiGate validation confirmed synchronization with NTP.br.

## Security Profiles

Security Profiles must be validated independently. A successful firewall policy test does not prove that a Security Profile is compatible with the workload.
