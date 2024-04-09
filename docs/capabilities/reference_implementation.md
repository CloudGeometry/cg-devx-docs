# Reference Implementation

The goal of CG DevX is to provide a reference implementation of a Platform with an ability to switch implementation of
core services to make reference implementation less opinionated.
Currently, we allow selection of three components for reference implementation template before rendering /
materialisation. Those components are:

- Hosting (cloud) provider
- Git provider
- DNS registrar

For some of other components its possible to change or turn them on/off using "feature flags" like mechanism post
rendering. Later this functionality will be available before rendering via `cgdevxcli`.

![reference_implementation](../assets/diagrams.drawio)
