# NGINX Ingress: Empowering Security with ModSecurity

In CG DevX, the NGINX Ingress serves as the default ingress controller and is an integral part of the core setup, meaning it cannot be replaced with another controller within the platform.

### Enhancing Security with the OWASP Rule Set

To bolster web application security, CG DevX integrates the OWASP ModSecurity Core Rule Set (CRS). The CRS consists of a collection of generic attack detection rules that can be used with ModSecurity or compatible web application firewalls. Its primary goal is to safeguard web applications from a wide array of attacks, including but not limited to the OWASP Top Ten, while minimizing false alerts. The CRS offers protection against various common attack categories, such as SQL Injection, Cross-Site Scripting, Local File Inclusion, and more.

### Enabling the OWASP Rule Set

To enable the OWASP rule set, ModSecurity must also be enabled. For detailed information about the OWASP rule set, refer to the provided link.

### Streamlined Automation

CG DevX streamlines the onboarding process for teams. Each team is automatically assigned a Git repository named "team-$teamId-argocd" in Gitea. Furthermore, ArgoCD is automatically configured to access and synchronize the repository. Subsequently, team admins or members with self-service rights only need to populate their repository with the intended state and commit the changes.

### Seamless Integrations

The platform seamlessly integrates the NGINX Ingress Controller into an advanced ingress architecture, facilitating efficient and secure traffic routing for your applications.

### Instructions for Utilizing ModSecurity

By default, ModSecurity is disabled in NGINX. To enable it, navigate to the "values" tab of the app, and under "Mod security," toggle the "enabled" option.

It is essential to exercise caution when enabling ModSecurity, as it operates in blocking mode by default, which can potentially impact your applications negatively. To avoid disruption, disable blocking initially and then make necessary adjustments to your applications. Grafana provides teams with visibility into all ModSecurity warnings, and a preconfigured dashboard makes access to this information even more convenient.

The default ModSecurity snippet added to the NGINX configuration includes the following settings:

```yaml
modsecurity-snippet: |
    SecAuditEngine RelevantOnly
    SecAuditLogParts ABDEFHIJZ
    SecAuditLogFormat JSON
    SecAuditLogType Serial
    SecAuditLog /dev/stdout
    SecRequestBodyLimit 1073741824
    SecRuleRemoveById 920350
```

If you wish to customize the ModSecurity configuration, utilize the "Raw values" option, which allows you to manage all configuration directives efficiently. For further details on available configuration options, refer to the provided documentation.