# FortiGate Address Objects

## SANDBOX

| Field | Value |
|---|---|
| Name | `SANDBOX` |
| Type | Subnet |
| Network | `192.168.100.0/29` |
| Usage | Sandbox network |
| Interface | `port3` |

### Purpose

The `SANDBOX` address object represents the dedicated network used by the malware analysis Sandbox.

The FortiGate does not provide DHCP or internal routing for this network. The object is used to identify the Sandbox traffic within FortiGate firewall policies and logging.

### Traffic Flow

```text
Sandbox
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
FortiGate port1
      |
      v
Internet