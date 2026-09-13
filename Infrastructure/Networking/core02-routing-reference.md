# CORE02 - Routing Reference

## Networks

| Network | Interface | CORE02 Address | Purpose |
|---|---|---|---|
| `172.16.0.0/29` | `vmbr1` | `172.16.0.3` | Management |
| `10.0.100.0/29` | `vmbr2.10` | `10.0.100.2` | VLAN 10 |
| `192.168.100.0/29` | `vmbr2.30` | `192.168.100.1` | VLAN 30 / Sandbox |
| `10.10.160.0/21` | `vmbr0` | `10.10.160.5` | Infrastructure network |

## Main Routing Table

The main routing table contains the directly connected networks and the default route:

    default via 172.16.0.1 dev vmbr1 proto kernel onlink
    10.0.100.0/29 dev vmbr2.10 proto kernel scope link src 10.0.100.2
    10.10.160.0/21 dev vmbr0 proto kernel scope link src 10.10.160.5
    172.16.0.0/29 dev vmbr1 proto kernel scope link src 172.16.0.3
    192.168.100.0/29 dev vmbr2.30 proto kernel scope link src 192.168.100.1

## Policy-Based Routing

Traffic originating from the Sandbox network is selected by:

    from 192.168.100.0/29 lookup 100

The policy is implemented through `ip rule`.

## Routing Table 100

The dedicated routing table contains:

    10.0.100.0/29 dev vmbr2.10 scope link src 10.0.100.2
    default via 10.0.100.1 dev vmbr2.10

Therefore, traffic originating from `192.168.100.0/29` uses `10.0.100.1` as its default gateway.

## IPv4 Forwarding

IPv4 forwarding is enabled on `CORE02`:

    net.ipv4.ip_forward = 1

## Verification

Display the complete policy routing configuration:

    ip rule

Display the dedicated routing table:

    ip route show table 100

Display the main routing table:

    ip route

Verify the route selected for a Sandbox source address:

    ip route get 8.8.8.8 from 192.168.100.3

## Persistence

The routing configuration is persisted in:

    /etc/network/interfaces

The `table 100` routes are created through `post-up` directives on `vmbr2.10`.

The Sandbox policy rule is created through the `post-up` directive on `vmbr2.30`.