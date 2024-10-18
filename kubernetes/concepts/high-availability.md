# High Availability

To ensure your Kubernetes (K8s) cluster is production-grade and remains up and running reliably, follow these key steps:

#### 1. **High Availability (HA) Setup**

* **Multi-master architecture**: Deploy multiple control plane (master) nodes to avoid a single point of failure. Use etcd in a clustered setup across these nodes.
* **Load balancing**: Use a load balancer in front of the API servers to distribute traffic across multiple control plane nodes.
* **Multi-zone/multi-region deployments**: Deploy your worker nodes across multiple availability zones or regions for resilience against regional failures.
* **Use managed services (if possible)**: Managed Kubernetes services (like AWS EKS, GCP GKE, or Azure AKS) offer built-in HA and automatic recovery options.

#### 2. **Node Health and Scaling**

* **Cluster autoscaling**: Implement autoscaling for both the control plane and worker nodes to automatically add or remove resources based on demand.
* **Pod autoscaling**: Configure Horizontal Pod Autoscaler (HPA) and Vertical Pod Autoscaler (VPA) to ensure your applications can scale dynamically based on traffic and resource usage.
* **Use taints and tolerations**: Isolate critical workloads to specific nodes by using taints and tolerations to ensure critical pods don’t end up on unstable nodes.

#### 3. **Monitoring and Alerting**

* **Prometheus and Grafana**: Use Prometheus to collect cluster and application metrics, and visualize them with Grafana. Set up alerts for key metrics (e.g., node health, API server performance, etc.).
* **Logging with ELK/EFK**: Set up logging solutions like Elasticsearch, Fluentd, and Kibana (EFK) or Loki to capture logs from your cluster for analysis.
* **Liveness and readiness probes**: Ensure all your critical pods have proper health checks via liveness and readiness probes to restart unhealthy containers automatically.

#### 4. **Security**

* **RBAC and pod security policies**: Enforce Role-Based Access Control (RBAC) and apply security policies to control who can perform what actions.
* **TLS everywhere**: Encrypt all communications within the cluster, especially API traffic, etcd communications, and traffic between pods.
* **Network policies**: Use network policies to control traffic flow between services in the cluster and prevent unauthorized access.
* **Secret management**: Use Kubernetes secrets (or external tools like HashiCorp Vault) to store sensitive information securely.

#### 5. **Regular Cluster and Application Backups**

* **etcd backups**: Since etcd stores the entire state of the cluster, backing it up regularly is critical. For a managed service, ensure the backup options are configured correctly. For a self-managed cluster, create cron jobs to back up etcd regularly.
* **Persistent volume backups**: Use snapshot features provided by your cloud provider or storage solutions to back up persistent volumes for stateful applications.

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

By following these steps, you can maintain high availability, secure backup, and quick restoration of your Kubernetes cluster.
