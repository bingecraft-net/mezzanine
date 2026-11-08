# 01 - Prerequisites: Findings

## Server package

Latest build #137 (commit `2e49bd64`, 2026-11-05) provides:

- **server-build** artifact: 220 MB (server.zip)
- **client-build** artifact: 55 MB (client modpack)

## Forge and Minecraft versions

- **Minecraft:** 1.20.1
- **Forge:** 47.4.13 (from README)

## Java version

Minecraft 1.20.1 requires **Java 17** as the baseline. Java 21 LTS is the
recommended production version for 1.20.1. The existing Containerfile uses
`openjdk-25-jre-headless` (Ubuntu Noble), which is a choice, not a
requirement.

## Ports (configurable)

| Service | Default | Exposure |
|---------|---------|----------|
| Velocity proxy | 25565 | Public (DNS → proxy) |
| RCON | 25575 | Private (backup/management) |
| Minecraft backend | 25565 | Private (behind proxy) |

## Memory guidance (from Monifactory FAQ)

- 6 GB+ RAM recommended for the server
- Leave 2 GB+ for system processes
- Total machine should have 8 GB+ RAM

## EULA

Standard Minecraft EULA — `run.sh` generates `eula.txt` on first run.

## Decisions

1. **Java version:** Java 21 LTS
2. **Forge version:** 47.4.13 (verified via server.zip `manifest.json`)
3. **Ports:** Public ingress uses default 25565; other ports are implementation details.
4. **Memory allocation:** 6 GB+ for the server container.
