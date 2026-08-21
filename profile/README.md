# CobaltCore

Kubernetes-native OpenStack distribution for operating Hosted Control Planes.

CobaltCore (C5C3) runs OpenStack control planes as Kubernetes workloads. FluxCD applies the
declarative infrastructure stack, one operator per OpenStack service projects that service's
Deployments, Jobs, configuration, and Secrets, and a single `ControlPlane` resource ties them
together into a complete control plane.

Most of the work happens in **[cobaltcore](https://github.com/C5C3/cobaltcore)**, a Go workspace monorepo
holding the operators, container images, deployment manifests, and tests. The documentation is
published at **[c5c3.github.io/cobaltcore](https://c5c3.github.io/cobaltcore/)**.

## What is in place

- **Orchestration.** The c5c3-operator reconciles a `ControlPlane` CR into infrastructure CRs,
  service CRs, and K-ORC resources, mints the admin application credential, stewards the service
  catalog, and aggregates readiness.
- **Service operators.** Keystone (identity), Glance (image), Placement, Horizon (dashboard), and
  Barbican (key management). Keystone is the reference implementation and sets the patterns the
  others follow: CRD layout, sub-reconciler chain, webhooks, finalizers, instrumentation.
- **Infrastructure.** FluxCD-managed HelmReleases for cert-manager, OpenBao, the External Secrets
  Operator, MariaDB (Galera), Memcached, and Garage, plus K-ORC for declarative management of
  Keystone domains, projects, users, and catalog entries.
- **Secret flow.** OpenBao is the source of truth for credentials; ESO delivers them into the
  cluster and pushes operator-generated secrets back.
- **Service exposure.** Service CRs opt into external endpoints through the Gateway API.
- **Multi-cluster placement.** Registered target clusters can receive projected service workloads
  while the CRs stay on the management cluster.
- **Testing.** Unit, envtest integration, Chainsaw E2E, Tempest, and Chaos Mesh suites.

Nova, Neutron, and Cinder are not onboarded yet, and the hypervisor and storage clusters of the
original design remain sketches. Planned work is tracked in
[cobaltcore issues](https://github.com/C5C3/cobaltcore/issues).

## Status

The project is under active development. All CRDs are `v1alpha1`, and APIs, architecture, and
deployment layout can still change in breaking ways between releases. The archived
[C5C3](https://github.com/C5C3/C5C3) repository holds the original architecture sketch; where it
and the implementation in cobaltcore differ, the implementation is authoritative.

## Security

Found a vulnerability? Please report it privately through GitHub Private Vulnerability Reporting
rather than opening a public issue. See [SECURITY.md](https://github.com/C5C3/cobaltcore/blob/main/SECURITY.md)
for the reporting process, scope, and response expectations.
