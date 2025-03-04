# OCI-KubeFlow

- OCI-KubeFlow is a fully automated CI/CD pipeline designed to deploy a WordPress application on Oracle Cloud Infrastructure (OCI) using Kubernetes (OKE), Jenkins, Terraform, and GitHub.
- The project follows an Infrastructure as Code (IaC) approach, leveraging Terraform to provision OCI resources such as Kubernetes clusters, networking, and storage.
- Jenkins automates the deployment process by integrating with GitHub, ensuring that any code changes trigger an end-to-end deployment workflow.
- The application consists of three primary Kubernetes deployments:
  - Nginx (as a reverse proxy), MariaDB (for database management), and WordPress (as the main application), along with supporting resources such as Secrets, ConfigMaps, Persistent Volume Claims (PVCs), and Services.
- The deployment process ensures scalability, reliability, and automation while maintaining seamless integration with OCI’s managed services.
