# Deploying on Azure

When deploying to Azure, ensure that you have:

1. An Azure account with billing enabled.
   (Remember, deploying clusters will incur charges. Make sure to destroy
   resources when you're finished with them!)
3. A public DNS zone hosted in Azure DNS.
   To set this up,
   you can follow [this guide](https://learn.microsoft.com/en-us/azure/dns/dns-delegate-domain-azure-dns).
4. A user account with `Owner` access.
   You can
   use [this guide](https://learn.microsoft.com/en-us/azure/role-based-access-control/role-assignments-portal-subscription-admin)
   to set it up,
   or [this guide](https://learn.microsoft.com/en-us/azure/role-based-access-control/quickstart-assign-role-user-portal)
   to grant permissions to an existing user.
5. The Azure CLI (**az**) and **[kubelogin](https://aka.ms/aks/kubelogin)** installed and configured to use this user.
   You can
   use [this](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli)
   and [this](https://azure.github.io/kubelogin/install.html) guides
   to install the CLI.