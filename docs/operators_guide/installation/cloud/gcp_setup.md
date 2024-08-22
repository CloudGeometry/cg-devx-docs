# Deploying on Google Cloud

When deploying to Google Compute Platform, ensure that you have:

1. An account with billing enabled.
   (Remember, deploying clusters will incur charges. Make sure to destroy
   resources when you're finished with them!)
2. Google Cloud Project you will be deploying to. You could follow
   this [guide]( https://developers.google.com/workspace/guides/create-project) to create one. Project ID will be used
   as cloud-profile during setup.
3. A public DNS zone hosted in Cloud DNS.
   To set this up, you can follow [this guide](https://cloud.google.com/dns/docs/set-up-dns-records-domain-name).
4. A user account with `Owner` permissions.
   You can use [this guide](https://developers.google.com/apps-script/guides/admin/assign-cloud-permissions) to grant
   permissions.
5. The Google Cloud CLI (**gcloud**) and **google-cloud-cli-gke-gcloud-auth-plugin** plugin installed and configured to
   use this user.
   You can use [this](https://cloud.google.com/sdk/docs/install-sdk)
   and [this](https://cloud.google.com/sdk/docs/authorizing) guides to install and configure the CLI.