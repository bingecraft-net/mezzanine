# Mezzanine

## Goal

Run a persistent Monifactory 0.14 Minecraft server on **Beefy**, with **Stepper**
providing the Coolify control plane and deployment automation. Updates should be
repeatable, preserve the world, and have a documented rollback path.

## References

- [Monifactory](https://github.com/omicron-industries/monifactory)
- [Coolify](https://github.com/coollabsio/coolify)
- [Coolify Docs](https://github.com/coollabsio/coolify-docs)
- [Velocity](https://github.com/PaperMC/Velocity)
- [Monifactory Containerization](https://github.com/bingecraft-net/superfluous)
- [Velocity Containerization](https://github.com/bingecraft-net/velocity)

### Prior art to verify against current releases

- The [Superfluous Containerfile](../superfluous/Containerfile) builds the
  dedicated server from a pinned Monifactory refspec, and its
  [build command](../superfluous/commands/build) tags the image. Its
  [backup command](../superfluous/bin/backup) uses RCON save controls before
  archiving world data, while its [startup command](../superfluous/bin/start)
  restores a backup when no world exists. Reuse these patterns only after
  reviewing their assumptions and improving failure handling and backup
  selection for this deployment. This is prior art, not a guarantee that its
  base image, Java version, or scripts are appropriate for 0.14.
- Monifactory's [dedicated server instructions](https://github.com/Omicron-Industries/Monifactory#dedicated-server-installation)
  and [server update FAQ](https://github.com/Omicron-Industries/Monifactory/blob/main/FAQ.md#servers)
  describe the release package and files to replace. Follow the exact target
  release's instructions.
- Coolify's [rolling update guide](https://github.com/coollabsio/coolify-docs/blob/main/content/docs/applications/deployments/rolling-updates.mdx)
  documents when container overlap is unavailable; its
  [rollback guide](https://github.com/coollabsio/coolify-docs/blob/main/content/docs/applications/deployments/rollbacks.mdx)
  makes clear that rolling an image back does not restore persistent world
  data.
- The [Velocity Containerfile](../velocity/Containerfile) builds from pinned
  version/build inputs and bundles the Ambassador plugin. Its
  [velocity.toml](../velocity/overrides/velocity.toml) maps hostnames in
  `forced-hosts` to named server entries and uses modern player-info
  forwarding with a mounted secret. Its [run command](../velocity/commands/run)
  publishes the proxy listener. Adapt these patterns, but replace its
  Podman-specific host addressing and verify plugin purpose, versions,
  configuration reload behavior, and Forge forwarding compatibility for this
  deployment.
