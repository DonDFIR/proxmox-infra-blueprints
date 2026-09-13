# FortiGate Static Routes

## SANDBOX

| Field | Value |
|---|---|
| Destination | `192.168.100.0/29` |
| Gateway | `10.0.100.2` |
| Outgoing Interface | `port3` |
| Purpose | Route Sandbox traffic to the Proxmox environment |

### Purpose

This static route allows the FortiGate to reach the dedicated Sandbox network through the Proxmox environment.

The FortiGate does not act as the gateway for the Sandbox network. The Sandbox gateway is provided by the Proxmox environment.

### Traffic Flow

```text
192.168.100.0/29
       |
       v
10.0.100.2
       |
       v
FortiGate port3