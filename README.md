# CG DevX Documentation

Welcome to the documentation for **CG DevX**, a delivery framework that applies the best practices of Platform Engineering to improve the way teams build, deploy, and operate cloud-native applications.

## Purpose

CG DevX helps organizations unlock the potential of cloud-native software development and deployment by introducing structure and delivery discipline to the software lifecycle. Built on Kubernetes, containers, and GitOps principles, it enables robust, resilient, and repeatable workflows that scale across teams and environments.

Unlike generic platform toolkits, CG DevX applies opinionated Platform Engineering patterns to:

- Reduce delivery variation
- Accelerate developer onboarding
- Enforce organizational standards by design

## Key Concepts

- **Platform Engineering**: A structured approach to delivering internal platforms and workflows that codify best practices.
- **Golden Paths**: Curated workflows that provide production-ready defaults for building and deploying applications.
- **Scaffold-as-Code**: Automated project bootstrapping with built-in CI/CD pipelines, test harnesses, and observability defaults.
- **GitOps-Driven Deployment**: Declarative infrastructure and application delivery with version control, policy enforcement, and rollback support.

## Audience

This documentation is intended for:

- Platform Engineering and DevOps teams
- SREs building internal delivery workflows
- Application developers onboarding to CG DevX
- Cloud architects designing scalable, compliant delivery systems

## Structure

- [`/platform_engineering/`](./platform_engineering): Overview of the delivery discipline and structured approach behind CG DevX
- [`/cgdevx/`](./cgdevx): Technical details of CG DevX components and how they are applied in practice
- [`/`](./): Project overview and introduction to the documentation

## Goals

By adopting CG DevX, organizations can:

- Deliver software more reliably and securely
- Shift from ad hoc DevOps workflows to governed delivery pipelines
- Enable application teams to move faster, with confidence in production readiness


CG DevX brings engineering rigor to cloud-native software delivery — turning internal tools and infrastructure into products that scale with your teams.

## Contributing

This project uses [MkDocs](https://www.mkdocs.org/).

To install MkDocs and required plugins run 

```shell
pip install mkdocs mkdocs-mermaid2-plugin mkdocs-drawio-file
```

```shell

.
├── README.md
├── docs
│   ├── ...
│   └── ...
└── mkdocs.yml

```
The configuration file is `mkdocs.yml`, and 
documentation source files are located in the `docs` folder. 
The documentation entry point is `index.md`.

To run in a local dev-server mode, run the `mkdocs serve` command:

```shell
mkdocs serve
```

The documentation website will be available at http://127.0.0.1:8000/.

To build a static website run the build documentation command: 

```shell
mkdocs build
```
This output will be generated to the `site` directory.

To host using [GitHub Pages](https://pages.github.com/) and deploy to a branch `gh-pages`, run the following command: 

```shell
mkdocs gh-deploy
```
