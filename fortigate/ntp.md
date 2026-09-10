# FortiGate NTP

## Upstream NTP

The FortiGate synchronizes with NTP.br:

```text
a.ntp.br
b.ntp.br
c.ntp.br
```

## Internal NTP server

The FortiGate operates as an NTP server on:

- `port2`
- `port3`

## Timezone

```text
America/Sao_Paulo
```

## Validation

The FortiGate was validated with:

```text
synchronized: yes
ntpsync: enabled
server-mode: enabled
```

This provides a consistent time source for the internal lab.
