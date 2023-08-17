# HashiCorp Vault: Securing Secrets

HashiCorp Vault is a powerful and secure application designed to manage secrets, providing robust encryption and access control. In CG DevX, Vault has been thoughtfully integrated and made tenant-aware, allowing for seamless and secure secret storage for each team in the platform.

### Vault Integration in CG DevX

When enabled, HashiCorp Vault automatically creates a dedicated space for each team in CG DevX. This ensures that team secrets are isolated and only accessible to authorized team members. Moreover, Vault integrates seamlessly with CG DevX's Keycloak OIDC settings, streamlining user login through CG DevX's Single Sign-On (SSO) authentication.

### Native Kubernetes Deployment

Like all components in CG DevX, Vault operates natively on Kubernetes. This native deployment ensures optimal performance and scalability while taking advantage of Kubernetes' built-in features.

### Ensuring Data Persistence

Data persistence is a critical aspect of Vault's operation. To safeguard against data loss during rolling cluster upgrades, CG DevX provides the option to configure external (blob) storage. By configuring data persistence, teams can rest assured that their secrets remain intact and accessible throughout any infrastructure updates.

### Full Access to Vault

While CG DevX offers convenient limited access to Vault for team members, users may require full access for specific purposes. To sign in with full access:

**Step 1:** Obtain the token by running the following command:

```bash
kubectl get secret -n vault vault-unseal-keys -o jsonpath='{.data.vault-root}' | base64 -d | pbcopy
```

**Step 2:** Open Vault and sign in using the token as the authentication method.

By following these steps, users can access Vault with full privileges, granting them comprehensive control over secrets as needed.

HashiCorp Vault stands as a robust and essential component of CG DevX, enabling teams to securely store and manage their secrets. Its integration within the platform ensures a smooth and secure experience, empowering teams to safeguard their applications and data effectively. Explore the potential of HashiCorp Vault in CG DevX, and elevate your secret management to a new level of security and control.