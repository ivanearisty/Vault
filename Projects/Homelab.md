# Homelab Infrastructure

## Current Setup

- **TrueNAS** — main server, runs Docker containers, handles storage
- **ThinkStation Mini** (2010s, i5) — secondary compute, available for additional workloads

## Repos

| Repo | Purpose | Status |
|------|---------|--------|
| `homelab-infra` | Source of truth for what runs on TrueNAS. docker-compose pulling images from ghcr.io | Scaffolded |
| `3d-printer-monitor` | Bambu Lab failure detection via Tapo camera + ONNX model | Running on TrueNAS |
| `building-security` | Hallway camera recording + resident web portal | Not started |

## Architecture

```
Project repos (push to main)
  → GitHub Actions: lint, test, build, push image to ghcr.io

TrueNAS (homelab-infra):
  → Watchtower polls ghcr.io every 5 min
  → Pulls new images, restarts updated containers
```

## Services

| Service | Image | Description |
|---------|-------|-------------|
| watchtower | containrrr/watchtower | Auto-pulls new images from ghcr.io |
| ml_api | ghcr.io/ivanearisty/3d-printer-ml-api | ONNX YOLOv4 failure detection |
| print_monitor | ghcr.io/ivanearisty/3d-printer-monitor | Camera + Bambu printer controller |
| security-recorder | ghcr.io/ivanearisty/building-security-recorder | Planned: ffmpeg RTSP → HLS recording |
| security-nginx | ghcr.io/ivanearisty/building-security-nginx | Planned: serves recordings to web app |
| mosquitto | eclipse-mosquitto:2 | MQTT broker for [[Smart Lighting]] |
| zigbee2mqtt | koenkk/zigbee2mqtt | Zigbee ↔ MQTT bridge for [[Smart Lighting]] |
| homeassistant | ghcr.io/home-assistant/home-assistant:stable | Local smart home control for [[Smart Lighting]] |

## Network

| Device | IP | Protocol |
|--------|----|----------|
| Bambu Lab printer | 192.168.4.23 | MQTT |
| Tapo camera (3D printer) | 192.168.5.77 | RTSP :554 |
| Tapo C110 (hallway) | TBD | RTSP :554 |

## Future Upgrade: k3s Cluster

If a second ThinkStation is added, migrate from docker-compose to **k3s** (lightweight Kubernetes):

### Why
- **Scheduling** — k3s decides which machine runs each workload based on available CPU/RAM
- **Self-healing** — if a node goes down, pods reschedule to the surviving node
- **Rolling updates** — zero-downtime deploys natively, replaces Watchtower
- **Ingress** — ships with Traefik for reverse proxy + TLS

### Setup (two commands)
```bash
# Node 1 (server)
curl -sfL https://get.k3s.io | sh -

# Node 2 (agent)
curl -sfL https://get.k3s.io | K3S_URL=https://node1:6443 K3S_TOKEN=<token> sh -
```

### Migration Path
- CI pipeline stays the same (build images → push to ghcr.io)
- Replace `docker-compose.yml` with Kubernetes manifests (Deployments, Services, Secrets)
- Replace Watchtower with **Flux** or **ArgoCD** for GitOps-style deploys
- The `homelab-infra` repo adds a `manifests/` directory alongside (or replacing) docker-compose

### Hardware Estimate
- k3s server overhead: ~512MB RAM
- Each ThinkStation i5 can comfortably run 3-5 lightweight containers
- GPU not needed for current workloads (ONNX CPU inference, ffmpeg encoding)

### When to Pull the Trigger
Don't migrate until there's a real need (second machine, or need for zero-downtime deploys). The ghcr.io image pipeline works identically whether the consumer is `docker compose pull` or `kubectl apply`.
