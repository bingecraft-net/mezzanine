# 01 - Confirm prerequisites and constraints

## Goal

Verify that the target environment can support the planned deployment before
investing in builds or configuration.

## Steps

1. Verify Monifactory 0.14's server startup instructions, required Java
   version, ports, memory guidance, and license/EULA requirements.
2. Check Stepper and Beefy's operating systems, available CPU/RAM/disk,
   network reachability, DNS, and firewall rules.
3. Decide where the public DNS names resolve, where Velocity listens, and
   whether the server is private or publicly reachable. Allow public inbound
   Minecraft traffic only to Velocity; keep backend and RCON ports private.
4. Confirm how the selected Coolify deployment resource will attach the
   proxy and game containers to Beefy's private container network.

## Acceptance criteria

- Monifactory 0.14 server package and matching Forge version are identified.
- Stepper and Beefy have sufficient resources for the planned workload.
- DNS names, firewall rules, and port exposure are decided.
- The Coolify deployment resource supports the required network and TCP-port
  configuration.
