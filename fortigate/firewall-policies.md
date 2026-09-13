# FortiGate Firewall Policies

## Policy model

The current model explicitly separates ADMIN and CLUSTER Internet access.

### ADMIN → CLUSTER

```text
Incoming:   port3
Outgoing:   port2
Source:     port3 address
Destination: all
Service:    ALL
Action:      ACCEPT
NAT:        OFF
```

Purpose: administrative access to cluster resources without source NAT.

### ADMIN → WAN

```text
Incoming:   port3
Outgoing:   port1
Source:     port3 address
Destination: all
Service:    ALL
Action:      ACCEPT
NAT:        ON
```

Purpose: unrestricted operational Internet access for the administration workstation.

### CLUSTER → WAN

```text
Incoming:   port2
Outgoing:   port1
Source:     port2 address
Destination: all
Service:    DNS + HTTP + HTTPS
Action:      ACCEPT
NAT:        ON
Inspection: Flow-based
Logging:    All
```

Purpose: Internet access for cluster nodes/workloads with a smaller initial service surface.

## Important distinction

Traffic destined to the FortiGate itself is not normal transit traffic. Access to the FortiGate management plane is controlled through interface administrative access and Local-In mechanisms, not by an ordinary transit policy.

## NAT principle

- Internal ADMIN → CLUSTER: NAT OFF.
- ADMIN → Internet: NAT ON.
- CLUSTER → Internet: NAT ON.


# FortiGate Firewall Policies

## Sandbox Internet Egress

| Field | Value |
|---|---|
| Source Interface | `port3` |
| Source Address | `SANDBOX` (`192.168.100.0/29`) |
| Destination Interface | `port1` |
| Destination | Internet |
| NAT | Enabled |
| Purpose | Sandbox Internet egress |

### Purpose

This policy permits traffic originating from the Sandbox network to reach the Internet.

The FortiGate performs source NAT for this traffic before forwarding it through `port1`.

The FortiGate does not provide DHCP or act as the internal gateway for the Sandbox network.

### Traffic Flow

```text
192.168.100.3
      |
      v
Proxmox / CORE02
      |
      v
10.0.100.2
      |
      v
FortiGate port3
      |
      v
Firewall Policy
      |
      v
NAT
      |
      v
FortiGate port1
      |
      v
Internet

### Logs

O tráfego da Sandbox pode ser identificado nos logs de tráfego do FortiGate pelo endereço de origem:

192.168.100.3

A identificação do hostname/dispositivo pode não estar disponível. Portanto, o endereço IP de origem é o principal identificador do tráfego da Sandbox.