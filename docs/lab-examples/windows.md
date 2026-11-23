|                               |                                                                         |
| ----------------------------- | ----------------------------------------------------------------------- |
| **Description**               | A Windows 10 VM running inside a Linux container                         |
| **Components**                | [dockurr/windows][dockurr]                                              |
| **Resource requirements**[^1] | :fontawesome-solid-microchip: 4 <br/>:fontawesome-solid-memory: 8192 MB |
| **Topology file**             | [windows.clab.yml][topofile]                                            |
| **Version information**[^2]   | `containerlab:0.83.0`, `dockurr/windows:6.06`                           |

## Description

This lab runs a Windows 10 VM with [dockurr/windows][dockurr]. The Windows guest
is managed by QEMU inside a Linux container, so it can run next to the rest of a
containerlab topology without a separate hypervisor.

The container publishes two services:

- `8006/tcp` - the noVNC web console
- `3389/tcp` and `3389/udp` - RDP

## Requirements

- A lab host with KVM (`/dev/kvm`).
- 4 vCPUs, 8 GB RAM and 40 GB free disk for the VM.
- Internet access on first boot unless a local ISO is supplied.

## Deployment

```bash
containerlab deploy -t windows.clab.yml
```

The first boot downloads Windows and installs it into the `storage` directory.
This takes several minutes. Follow the installation log with:

```bash
docker logs -f clab-windows-win1
```

## Access

Open `http://<lab-host>:8006/` for the web console, or connect an RDP client to
`<lab-host>:3389`. The credentials are the `USERNAME` and `PASSWORD` values in
the topology file.

## Containerlab app

The containerlab app's **Desktop** action detects a graphical console on
container port `8006`. When the app is served through an HTTPS lab portal, it
opens:

```
https://<lab-portal>/novnc/windows/win1/
```

The reference path ingress is documented in the app repository under
`deploy/windows-console-ingress`.

## Scope

This example is a standalone management VM. The Windows guest uses the container
runtime's network; it is not attached to a containerlab data link. Connecting
the guest to a lab switch requires a tap/bridge integration beyond this
example.

[dockurr]: https://github.com/dockur/windows
[topofile]: https://github.com/srl-labs/containerlab/tree/main/lab-examples/windows/windows.clab.yml

[^1]: Resource requirements are provisional. Consult the dockurr/windows documentation for details.
[^2]: The lab has been validated with the listed versions. Other versions may work.
