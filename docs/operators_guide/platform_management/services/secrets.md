# Secrets Management

## Vault

CG DevX uses [Vault](https://www.vaultproject.io/), an open source secrets manager and identity provider.

### Authentication

You can log in into Vault using platform user credentials provided by CG DevX CLI.
You can also log in using the root token, which has full administrative permission--but we strongly advise against it.

![vault_login_token.png](../../../assets/vault_login_token.png)
![vault_login_userpass.png](../../../assets/vault_login_userpass.png)

Vault is also used as an OIDC provider, so all other services are using it to provide an SSO experience.

Initially, there will be only a single user created, representing the CG DevX machine user account.
You can create additional accounts by updating IaC and creating a PR, and you can find more
details [here](../iac/users_management.md).
You will be able to get additional user account passwords from Vault once the PR is merged.

> Note, it's only possible to access user account passwords when using the root token to log in.

Additional authentication methods can be configured for Vault as
described [here](https://developer.hashicorp.com/vault/docs/auth).

### Kubernetes

The `external-secrets-operator` service is preconfigured with a service account that can pull secrets from Vault.
This makes secrets stored in Vault available as native Kubernetes Secrets for your workload to consume.

### Secrets setup

Secrets for platform services are stored by the `secrets` engine -- by default, the vault kv v2 backend.

![vault_platform_secrets.png](../../../assets/vault_platform_secrets.png)

When working with K8s manifests, you can reference a secret in Vault using the following definition.
Good practice is to use `external-secrets.yaml` files.

```yaml
apiVersion: external-secrets.io/v1beta1
kind: ExternalSecret
metadata:
  name: awesome-secret
spec:
  target:
    name: awesome-secret
  secretStoreRef:
    kind: ClusterSecretStore
    name: vault-kv-secret
  refreshInterval: 10s
  dataFrom:
    - extract:
        key: secret/awesome-service/awesome-secret
```

This resource will be deployed with the service, connecting to `vault-kv-secret`, and pulling secrets from Vault to K8s.
You can pull all the key value pairs, or specify exactly which specific pairs should be pulled.
