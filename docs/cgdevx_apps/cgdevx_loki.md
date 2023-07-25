# Loki: Centralized Log Aggregation

CG DevX leverages Loki, a powerful log aggregation system, to consolidate all container logs from the platform and store them in a storage endpoint of your preference (defaults to PVC). This robust feature ensures streamlined log management and easy access to critical information.

### Multi-Tenancy Mode and Enhanced Log Isolation

In scenarios where CG DevX is installed in multi-tenancy mode, it efficiently segregates logs from team namespaces, making them exclusively accessible to team members. This isolation not only enhances security but also streamlines log monitoring for individual teams.

### Simplified Log Access with CG DevX Shortcuts

To facilitate streamlined log analysis and troubleshooting, CG DevX offers convenient shortcuts to log selections based on your interests. These shortcuts empower teams to swiftly access logs that are relevant to their specific projects and tasks.

### Team-Oriented Grafana Instances

CG DevX takes log monitoring to the next level by creating dedicated Grafana instances for each team. These personalized instances provide teams with granular control over log visualization, allowing them to focus on the metrics and insights that matter most to their operations.

### Secure Access Control with Grafana Authentication

In order to uphold data security and ensure that sensitive logs are only accessible to authorized personnel, CG DevX meticulously configures authentication for Grafana. This approach guarantees that only team members with appropriate privileges can access the logs, minimizing the risk of unauthorized access and data breaches.

Embrace the power of Loki in CG DevX to centralize and effectively manage your container logs, empowering your teams with the insights they need for efficient troubleshooting and operational excellence. With multi-tenancy support, dedicated Grafana instances, and secure access control, CG DevX delivers a seamless and secure log aggregation solution that optimizes your Kubernetes-based workflows.