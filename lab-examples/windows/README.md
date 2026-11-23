# Windows lab

A Windows VM running inside a Linux container with
[dockurr/windows](https://github.com/dockur/windows).

## Requirements

- A lab host with KVM available (`/dev/kvm`).
- At least 4 vCPUs, 8 GB RAM and 40 GB free disk for the VM.
- Internet access on the first boot if you do not provide a local ISO.

## Deploy

```bash
containerlab deploy -t windows.clab.yml
```

The first boot downloads Windows and installs it into `./storage`. This can take
several minutes. Follow the console with:

```bash
docker logs -f clab-windows-win1
```

## Access

- Web console: `http://<lab-host>:8006/`
- RDP: `<lab-host>:3389`
- Username/password: the values in the topology (`clab` / `clab@123`)

Change the password in the topology before using the lab outside an isolated
environment.

## Containerlab app

The app's **Desktop** action detects a graphical console published on container
port `8006`. When the app is served through the HTTPS lab portal, it opens the
path-based console URL:

```
https://<lab-portal>/novnc/windows/win1/
```

The path ingress is a small host-side nginx helper documented in the app
repository under `deploy/windows-console-ingress`.

## Scope

This example is a standalone management VM. It does not attach the Windows guest
NIC to a containerlab data link; the VM uses the container runtime's network.
Linking a Windows guest to a lab switch requires a tap/bridge integration and is
outside this example.
