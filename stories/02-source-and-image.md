# 02 - Prepare the source and image

## Goal

Build a reproducible server image from a pinned release, keeping mutable
data out of the image and keeping the build separate from the decision to
deploy it.

## Steps

1. Follow the dedicated-server instructions for the exact Monifactory
   release: download its server package, install the matching Forge version,
   and apply the release's required setup steps.
2. Build from a pinned release/tag or commit, and tag the resulting image
   with that version (and preferably its source commit); do not build a
   moving branch tip for a production update.
3. Create a reproducible container build using the Java runtime supported by
   that release. Keep mutable server data out of the image; document image
   inputs and version/tag conventions.
4. Build and smoke-test the image before configuring deployment.

## Acceptance criteria

- The image is built from a pinned release, not a moving branch tip.
- The image is tagged with the release version and source commit.
- Mutable server data (world, config) is not baked into the image.
- The image starts and runs a smoke test without errors.
