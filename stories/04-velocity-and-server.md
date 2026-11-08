# 04 - Build and deploy Velocity and the game server

## Goal

Build and deploy a containerized Velocity proxy and the initial Monifactory
server slot, verifying that players can connect through the proxy.

## Steps

1. Build Velocity from pinned version/build inputs, following the prior-art
   container pattern, and keep its configuration and forwarding secret out
   of the image. Store secrets outside Git and inject them securely at
   runtime.
2. Configure Velocity's public listener, online-mode/authentication,
   forwarding mode, protected forwarding secret, named backend entries,
   hostname routes, and fallback behavior. Verify the backend modpack
   supports the selected Velocity forwarding mode; Monifactory's Forge
   server may require a compatible forwarding mod.
3. Run Velocity and both backend slots on a private shared container
   network. Publish the Minecraft port only from Velocity; do not publish
   backend ports or expose RCON publicly. Use container DNS/network aliases
   or another stable internal addressing scheme, not the Podman-specific
   `host.containers.internal` address from the prior-art configuration.
4. Configure the image/build source, resource limits, and environment for
   the game server in Coolify or the chosen declarative deployment
   definition. Mount persistent storage for each slot's world and any
   required server configuration or mod data.
5. Accept the Minecraft EULA only after reviewing its terms.
6. Point a test DNS name at Velocity and route it to the initial backend.
   Verify startup logs, player identity/IP forwarding, modded client
   connectivity, hostname routing, and that direct backend access is
   blocked.

## Acceptance criteria

- Velocity is built from pinned version/build inputs.
- Velocity's forwarding mode is compatible with Monifactory's Forge server.
- Only Velocity's Minecraft port is publicly reachable; backend ports are
  private.
- The initial backend is reachable through Velocity on a test hostname.
- Player identity and IP forwarding work correctly.
- Direct backend access is blocked.
