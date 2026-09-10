# FortiGate Networking

## Interface plan

| Interface | Role | Address |
|---|---|---|
| `port1` | WAN | `10.10.160.2/29` via DHCP |
| `port2` | CLUSTER | `172.16.0.1/29` |
| `port3` | ADMIN | `10.0.100.1/29` |

## ADMIN

```text
Network: 10.0.100.0/29
Gateway: 10.0.100.1
DHCP:    10.0.100.2 - 10.0.100.6
```

## CLUSTER

```text
Network: 172.16.0.0/29
Gateway: 172.16.0.1
CORE01:  172.16.0.2
CORE02:  172.16.0.3
CORE03:  172.16.0.4
```

Remaining usable addresses are reserved for the current small cluster design.

## WAN

`port1` obtains upstream connectivity from the Vivo router using DHCP.

The WAN DHCP DNS override is disabled so the FortiGate uses its configured upstream DNS instead of blindly adopting DNS servers supplied by the ISP.
