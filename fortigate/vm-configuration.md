# FortiGate VM Configuration

## Purpose

Registrar os requisitos da VM do FortiGate no ambiente Proxmox.

## Current license constraints

A Permanent Evaluation License limita o appliance a:

- 1 CPU
- 2 GiB RAM
- 3 interfaces
- 3 firewall policies
- 3 routes
- 2 VDOMs

## VM network mapping

| FortiGate | Role | Network |
|---|---|---|
| `port1` | WAN | Vivo / upstream |
| `port2` | CLUSTER | `172.16.0.0/29` |
| `port3` | ADMIN | `10.0.100.0/29` |

## Design decision

O desenho utiliza três interfaces e não depende de VLANs no FortiGate devido às limitações da licença de avaliação.

A microsegmentação dos workloads permanece responsabilidade do Proxmox Firewall.
