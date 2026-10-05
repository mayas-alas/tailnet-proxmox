# Tailnet Proxmox image

Minimal maintained copy of [dockur/proxmox](https://github.com/dockur/proxmox),
updated to upstream commit `5fccb4a7b902f6f150fb440582d7b946bce91042`.

The Dockerfile has one functional change: it **does not download or install
pve-fake-subscription**. Native Proxmox subscription behavior is retained.
`src/entrypoint.sh` and `src/network.sh` are unchanged upstream files. The previous
custom Tailscale, bridges, branding and devcontainer rules have been removed.
Network policy, CA, credentials, volumes and workloads belong to the consuming
product, not to this image repository.

## Build and publication

One workflow builds Linux/amd64, verifies package absence, boots a disposable
Proxmox container, checks pveproxy/pvedaemon/pvestatd, HTTPS and KVM, and then uses
**Skopeo** to publish the exact tested OCI to:

`ghcr.io/mayas-alas/tailnet-proxmox:<version>`

The remote manifest digest is verified. Existing release tags are not replaced.
Run **Build and publish** with a new version, or push a new `v*` tag. The workflow
uses its repository-scoped `GITHUB_TOKEN` with `packages: write`; no new user token
or credential bridge is required. Only the disposable smoke container uses a
self-signed certificate; consuming products must retain real CA validation.

Quetzalcoatl consumes `ghcr.io/mayas-alas/tailnet-proxmox@sha256:…` through its
existing Podman/Quadlet flow. It does not carry this Dockerfile or image builder.
Image boot tests are not acceptance of an upgrade with existing VMs/LXC: that
requires explicit validation on the consuming product. No host deployment is
performed by this repository's workflow.

## License

Upstream source remains MIT: [UPSTREAM_LICENSE.md](UPSTREAM_LICENSE.md).
Repository modifications are AGPL-3.0-only: [license.md](license.md).
Debian, Proxmox and other components retain their original licenses.
