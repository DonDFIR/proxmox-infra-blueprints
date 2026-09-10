# FortiGate Security Profiles

## Current strategy

Security Profiles are being enabled incrementally and validated against real workloads.

The initial baseline was tested with all profiles disabled to prove that basic routing, NAT and DNS were functional.

## Validation method

```text
Disable all profiles
        |
        v
Validate workload
        |
        v
Enable one profile
        |
        v
Repeat test
        |
        v
Inspect Security Events
```

## Known finding

Debian/Proxmox APT traffic was blocked while Web Filter was enabled.

Example:

```text
Source:      172.16.0.2
Destination: download.proxmox.com
Port:        80
Agent:       Debian APT-HTTP/1.3
Event:       ftgd_err
Subtype:     webfilter
```

The event reported:

```text
all FortiGuard servers failed to respond
A rating error occurs
```

## Interpretation

The firewall policy already allowed HTTP. The block occurred in the Web Filter security layer because the Trial environment does not provide the required FortiGuard service response.

## Operating principle

Do not disable all security controls just to restore connectivity.

Instead:

1. Identify the profile responsible.
2. Determine why it blocked.
3. Determine whether the profile is usable without FortiGuard.
4. Adjust the profile or exception only when justified.
5. Re-test the workload.

## Current status

Security Profiles are not yet considered final baseline configuration.
