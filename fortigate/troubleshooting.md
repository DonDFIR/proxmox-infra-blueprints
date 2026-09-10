# FortiGate Troubleshooting

## Method

Troubleshooting follows a layered model:

```text
Physical / Interface
        ↓
IP / Routing
        ↓
Firewall Policy
        ↓
NAT
        ↓
Security Profile
        ↓
DNS / External dependency
        ↓
Application
```

Do not change multiple layers simultaneously.

## Debian/Proxmox update blocked

First inspect:

`Log & Report → Security Events`

Record:

- Source
- Destination
- Port
- Policy
- Security Profile
- Event
- Error/message

### Known case

```text
download.proxmox.com:80
Web Filter
ftgd_err
FortiGuard rating error
```

The correct response is not to blindly change the firewall policy. The firewall policy already permitted HTTP; the Security Profile was the blocking layer.

## DNS troubleshooting

Verify the client DNS first.

ADMIN:

```bash
resolvectl status
```

CLUSTER:

```bash
cat /etc/resolv.conf
dig www.google.com
```

Direct FortiGate test:

```bash
dig @172.16.0.1 example.com
```

## NextDNS troubleshooting

Verify:

1. Client uses FortiGate as DNS.
2. FortiGate has NextDNS upstream.
3. FortiGate DNS Server is enabled on the client interface.
4. NextDNS receives queries.
5. Public egress IP is linked to the NextDNS profile.

## Rule

Fix the layer that is actually failing. Do not broaden firewall permissions as a substitute for identifying the blocking mechanism.
