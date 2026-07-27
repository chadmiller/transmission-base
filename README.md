# transmission-base

Ubuntu LTS with Transmission built from source. Shared base for
[haugene/docker-transmission-wireguard](https://github.com/haugene/docker-transmission-wireguard)
and
[haugene/docker-transmission-openvpn](https://github.com/haugene/docker-transmission-openvpn),
so both stay on the same Ubuntu and Transmission versions.

Published on Docker Hub as [`haugene/transmission-base`](https://hub.docker.com/r/haugene/transmission-base).

## Consume in a downstream Dockerfile

```dockerfile
FROM haugene/transmission-base:4.1.3-ubuntu26.04
```

Pinning options:

| Pin | Example | When to use |
| --- | --- | --- |
| Floating tag | `4.1.3-ubuntu26.04` | Normal dependency; picks up rebuilds for the same Transmission + Ubuntu major (security / base patch refreshes) |
| Immutable tag | `4.1.3-ubuntu26.04-a1b2c3d` | Reproducible builds; never moves once published |
| Digest | `haugene/transmission-base@sha256:…` | Strongest reproducibility |

## Tag format

Tags encode **Transmission version** and **Ubuntu version**:

```text
{transmission}-ubuntu{ubuntu}           # floating (updated by rebuilds on main)
{transmission}-ubuntu{ubuntu}-{sha}     # immutable (git short SHA)
```

Examples:

- `4.1.3-ubuntu26.04` — current build of Transmission 4.1.3 on Ubuntu 26.04
- `4.1.3-ubuntu26.04-a1b2c3d` — that same combo from commit `a1b2c3d`

### Dev channel

Pushes to the `dev` branch publish a separate channel so unfinished work does
not look like a release:

- `dev`
- `4.1.3-ubuntu26.04-dev`
- `4.1.3-ubuntu26.04-dev-{sha}`

## Build arguments

| Arg | Default | Purpose |
| --- | --- | --- |
| `UBUNTU_VERSION` | `26.04` | Ubuntu LTS tag used for both build and runtime stages |
| `TRANSMISSION_VERSION` | `4.1.3` | Transmission release built from the official source tarball |
| `TRANSMISSION_SHA256` | `ce7d2d8b…` | Expected SHA-256 of that tarball (bump together with the version) |

## Local build

```bash
docker build -t haugene/transmission-base:local .
```

Override versions explicitly (checksum from [transmissionbt.com/download](https://transmissionbt.com/download)):

```bash
docker build \
  --build-arg UBUNTU_VERSION=26.04 \
  --build-arg TRANSMISSION_VERSION=4.1.3 \
  --build-arg TRANSMISSION_SHA256=ce7d2d8b101f7eb54bc3cf0bc55f52f7ebd4a25fa48e00bdca9a7e0fc02617da \
  -t haugene/transmission-base:4.1.3-ubuntu26.04 \
  .
```

Multi-arch (requires Buildx + QEMU):

```bash
docker buildx build \
  --platform linux/amd64,linux/arm64,linux/arm/v7 \
  --build-arg UBUNTU_VERSION=26.04 \
  --build-arg TRANSMISSION_VERSION=4.1.3 \
  --build-arg TRANSMISSION_SHA256=ce7d2d8b101f7eb54bc3cf0bc55f52f7ebd4a25fa48e00bdca9a7e0fc02617da \
  -t haugene/transmission-base:4.1.3-ubuntu26.04 \
  .
```
## License

MIT — see [LICENSE](LICENSE).
