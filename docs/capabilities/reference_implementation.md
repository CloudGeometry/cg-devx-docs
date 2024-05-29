# Reference Implementation

The goal of CG DevX is to provide a reference implementation of a Platform with the ability to switch the implementation of core services to make the reference implementation less opinionated. Currently, we allow the selection of three components for the reference implementation template before rendering/materialization. These components are:

- Hosting (cloud) provider
- Git provider
- DNS registrar

For some of the other components, it is possible to change or turn them on/off using a "feature flags" like mechanism post-rendering. Later, this functionality will be available before rendering via `cgdevxcli`.

![reference_implementation](../assets/diagrams.drawio)
