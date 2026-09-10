# NextDNS Integration

## Role

NextDNS provides the DNS Security layer upstream of the FortiGate.

## Traffic flow

```text
ADMIN / CLUSTER
       |
       v
FortiGate DNS
       |
       v
NextDNS
       |
       v
Internet
```

## Client identification

The NextDNS profile uses the public egress IP to associate queries with the environment.

The public IP observed from the administrative workstation was associated with the NextDNS profile.

## Validation

The NextDNS dashboard confirmed:

- Queries received
- Queries associated with the public source IP
- Blocked queries
- Domain analytics

A real DNS block was observed, confirming that NextDNS is actively enforcing policy.

## Current transport

```text
FortiGate → NextDNS
UDP/TCP 53
cleartext
```

Encrypted upstream DNS is not part of the current implementation because the current FortiGate configuration exposes only `cleartext` for this function.

## Operational rule

Endpoints should use the FortiGate interface address as DNS, never the NextDNS addresses directly.
