# Backup Management

Our platform uses [Velero](https://velero.io/) for backup and recovery operations. Velero provides a simple and reliable way to back up and restore Kubernetes cluster resources and persistent volumes.

## Automated Backups with Velero

By default, Velero is configured to automatically back up specific namespaces every day at 02:00 UTC, with a retention policy of 7 days. The namespaces included in the default backup schedule are:

- `harbor`
- `vault`

The backup schedule is defined in the following file: 

```markdown
gitops-pipelines/delivery/clusters/cc-cluster/core-services/components/velero/backup-schedule.yaml
```


### Customizing Backup Schedules

To add more namespaces to the backup schedule or adjust the backup settings, you can update the existing `backup-schedule.yaml` file or create a new one. Changes made to this file will be detected by ArgoCD and applied automatically, ensuring your backups are always up to date.

## Restoring from Backup

In the event that you need to restore from a backup, follow the steps below.

### Disable ArgoCD Auto-Sync

Before initiating a restore, it's important to disable ArgoCD's auto-sync feature to prevent it from overwriting the restored resources. You can disable auto-sync by:

- Editing the ArgoCD application manifest in the GitOps repository.
- Using the ArgoCD UI.
- Using the ArgoCD CLI.

> **Recommended Approach:** Update the ArgoCD application file in the GitOps repository. This method ensures that changes are version-controlled and easily trackable.

### Install and Configure Velero Locally

1. **Install Velero CLI:**

   Follow the [official Velero installation guide](https://velero.io/docs/main/basic-install/) to install the Velero CLI on your local machine.

2. **Configure Cluster Context:**

   Ensure your kubeconfig is set to the cluster you wish to restore:

   ```bash
   kubectl config use-context <your-cluster-context>

3. **Validate Velero Access:**

   Verify that Velero can communicate with your cluster:

   ```bash
   velero version
    ```


### Perform the Restore

1. **List Available Backups:**

   View all available backups to find the one you need:

   ```bash
   velero backup get
   ```
2. **Initiate the Restore:**

  Use the following command to start the restore process::

   ```bash
   velero restore create --from-backup <backup-name>
   ```
   Replace <backup-name> with the name of the backup you wish to restore. For example:

   ```bash
   velero restore create --from-backup daily-backup-20241120020019
   ```
3. **Monitor the Restore Process:**

  Check the status of the restore:

   ```bash
   velero restore get
   ```
   For detailed information:
   ```bash
   velero restore describe <restore-name>
   ```
   Replace <restore-name> with the name of the restore operation initiated.
   
### Re-enable ArgoCD Auto-Sync

After confirming that the restore was successful and the cluster is functioning as expected, re-enable ArgoCD's auto-sync feature to maintain the desired state of the cluster.

## Additional Considerations

- **Data Consistency:** Ensure that no conflicting operations are occurring during the restore to prevent data inconsistencies.
- **Resource Quotas:** Verify that resource quotas and limits are sufficient for the restored resources.

## References

- [Velero Documentation](https://velero.io/docs/)
- [ArgoCD Documentation](https://argo-cd.readthedocs.io/en/stable/)

---

By following these steps, you can effectively manage backups and restores within your Kubernetes clusters, ensuring data integrity and continuity of service.
