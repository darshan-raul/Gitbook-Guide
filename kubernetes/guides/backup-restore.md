# Backup Restore

####

### Backing Up and Restoring Your Cluster State

#### Backup etcd

1. **Identify etcd endpoints**: Typically located at `https://<etcd-endpoint>:2379`.
2.  **Use etcdctl to back up**:

    ```bash
    ETCDCTL_API=3 etcdctl snapshot save /path/to/backup/snapshot.db \
      --endpoints=https://<etcd-endpoint>:2379 \
      --cacert=/etc/kubernetes/pki/etcd/ca.crt \
      --cert=/etc/kubernetes/pki/etcd/server.crt \
      --key=/etc/kubernetes/pki/etcd/server.key
    ```
3. **Store backup**: Securely store the snapshot (e.g., in a cloud storage service, S3 bucket) with versioning enabled.

#### Automating Backups

* **CronJob for automated backups**: Set up a Kubernetes CronJob or an external script to automate `etcdctl snapshot` commands at regular intervals.

#### Persistent Volume Backups

* **Cloud provider snapshots**: If you're using cloud providers like AWS, GCP, or Azure, you can configure automatic snapshots for persistent volumes (e.g., AWS EBS, GCP PD) using either the cloud console or tools like Velero.
* **Velero for PV backups**:
  *   Install and configure Velero:

      ```bash
      velero install --provider <cloud-provider> --bucket <bucket-name> --secret-file <credentials-file>
      ```
  *   Backup a namespace:

      ```bash
      velero backup create my-backup --include-namespaces <namespace>
      ```

### Restoring the Cluster from Backup

#### Restoring etcd Snapshot

1.  **Stop kube-apiserver**: Stop the Kubernetes API server so that it does not interfere with the restore process.

    ```bash
    systemctl stop kube-apiserver
    ```
2.  **Restore etcd**:

    ```bash
    ETCDCTL_API=3 etcdctl snapshot restore /path/to/backup/snapshot.db \
      --data-dir /var/lib/etcd-from-backup
    ```
3. **Update etcd data directory**: Ensure the `etcd` service is configured to use the restored directory (`/var/lib/etcd-from-backup`).
   * Edit `/etc/kubernetes/manifests/etcd.yaml` and update the data directory to point to the restored location.
4.  **Restart the kube-apiserver**:

    ```bash
    systemctl start kube-apiserver
    ```

#### Restoring Persistent Volumes (PV)

1. **Velero restore**:
   *   List available backups:

       ```bash
       velero backup get
       ```
   *   Restore from backup:

       ```bash
       velero restore create --from-backup <backup-name>
       ```

#### Key Considerations for Cluster Recovery

* **Disaster recovery**: Document all backup and recovery processes and periodically test them to ensure they work when needed.
* **Regular audits**: Ensure that the backup files and snapshots are regularly tested and validated.
* **Security of backups**: Ensure backups (etcd snapshots, persistent volume snapshots) are encrypted and access-controlled to avoid tampering or unauthorized access.
