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
