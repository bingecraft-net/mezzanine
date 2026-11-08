# 05 - Make updates and recovery safe

## Goal

When an upstream stable release is available, build and tag the image,
validate it on a secondary route, and promote it through a separate,
explicit operator action that creates a fresh backup of the old world,
restores it into the candidate, then remaps the primary route only after
the candidate is verified.

## Steps

1. When an upstream stable release is available, pin its source, build and
   tag the image, and review the release notes. Treat bleeding-edge builds as
   opt-in, not as stable updates. Once the build is available, decide whether
   to deploy it; a successful build must not automatically make it live.
2. Before deployment, check that Beefy has an open slot for the second
   container, including the resources needed to run it. If no slot is
   available, leave the current server running and defer deployment until a
   slot is available or capacity is deliberately freed.
3. With a slot available, request a consistent backup through the running
   server's RCON (for example, disable saves, issue `save-all`, archive the
   world after writes have flushed, then re-enable saves). Protect RCON
   credentials, verify the command and archive succeeded, and ensure saves
   are re-enabled even when backup creation fails.
4. Store each backup as a uniquely named, versioned artifact. Verify its
   contents and checksum, and keep it outside either replaceable container.
   Restore a specifically selected backup only into the new slot's empty
   world directory; do not implicitly restore whichever archive happens to
   be newest.
5. Add the candidate as a distinct Velocity backend and route a dedicated
   secondary/test hostname to it. Deploying the candidate must not modify
   the primary hostname's route. Validate logs, world loading, and modded
   client access through the secondary route while production remains on the
   old instance. Treat candidate-world changes made during testing as
   disposable; promotion refreshes it from a new backup of the old world.
6. When an update is pending, mark the old route as migration-pending. Use a
   tested Velocity plugin or backend mechanism to notify every player who
   connects to the old container that progress on that connection may not be
   retained and they should reconnect after the update is announced complete.
   Verify this notice works both while the candidate is still starting and
   after it is available but before primary-route promotion. Do not silently
   imply that the old route is the place to continue making durable progress.
7. Promotion is a separate, explicit operator action, invoked only after
   validation via the secondary route. It must not be coupled to or
   automatically triggered by the image build, container deployment, or
   candidate health check.
8. On promotion, enable a migration gate so new connections to the old
   backend receive the progress/reconnect warning and cannot mistake it for
   the durable destination. Prevent new logins to the old world, then take
   and verify a fresh, consistent backup of its world; do not use the
   earlier candidate-test backup. Immediately after the snapshot, notify
   players still connected to the old backend that subsequent progress will
   not be retained, then gracefully disconnect or quiesce them. This makes
   the backup the authoritative cutover point.
9. Restore that fresh backup into the candidate's separate world directory,
   restart/reload the candidate as required, and verify it is healthy and
   serving the restored world. Only then change the primary hostname's
   Velocity route to the candidate. Keep maintenance in place until the
   primary route is verified, then announce completion and have players
   reconnect. Test whether route changes require a controlled Velocity
   reload/restart; do not assume configuration hot-reload or that existing
   sessions move when the route changes.
10. If backup, restore, candidate readiness, or route remapping fails, keep
    the old world and image intact, route the primary hostname to the old
    backend, and reopen it only after confirming the old world is safe to
    resume. A route change alone is not a data rollback.
11. Keep the old image, its separate world data, and the verified backup
    available through the rollback window. If players have already written
    to the new world, do not blindly roll back to the old world; preserve
    both and decide how to reconcile progress.
12. Update the pack's server files from the new release package according to
    Monifactory's server-update guidance (including its config, default
    config, KubeJS, and mods directories). Do not treat the old container's
    entire server directory as persistent state or overwrite the world with
    files from the image.

## Acceptance criteria

- A new release builds and tags a pinned image without automatically going
  live.
- A slot is available (or deployment is deferred) before the candidate is
  started.
- A consistent backup is created through RCON, verified, and stored outside
  replaceable containers.
- The candidate is validated on a secondary hostname without changing the
  primary route.
- The migration-pending warning works for players connecting to the old
  route before and during promotion.
- Promotion is a separate, explicit operator action, not coupled to the
  build or deployment.
- On promotion, a fresh backup of the old world is taken, restored into the
  candidate, and the primary route is remapped only after the candidate is
  verified.
- If promotion fails, the old world and image are preserved and the primary
  route is restored.
- Server files are updated from the new release package per Monifactory's
  server-update guidance.
