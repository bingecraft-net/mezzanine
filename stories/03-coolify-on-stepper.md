# 03 - Install and configure Coolify on Stepper

## Goal

Install Coolify on Stepper, secure access, and register Beefy as a managed
server so that Coolify can deploy workloads there.

## Steps

1. Install Coolify using its current official instructions.
2. Secure access to the control plane and configure its required persistent
   storage and backups.
3. Add Beefy as a remote server and verify Coolify can deploy a harmless test
   workload there.

## Acceptance criteria

- Coolify is installed and accessible on Stepper.
- Beefy is registered as a managed server in the Coolify instance.
- A test workload deploys to Beefy successfully.
