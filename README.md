# OCI-KubeNexus
<!-- git submodule update --recursive --remote -->
- OCI-KubeNexus is a fully automated CI/CD pipeline designed to deploy a [**Kubernetes**](https://kubernetes.io/) application on [**Oracle Cloud Infrastructure (OCI)**](https://www.oracle.com/cloud/) using [**Oracle Kubernetes Engine (OKE)**](https://www.oracle.com/cloud/cloud-native/kubernetes-engine/), [**Jenkins**](https://www.jenkins.io/), [**Terraform**](https://www.terraform.io/), and [**GitHub**]().
- The project follows an Infrastructure as Code (IaC) approach, leveraging Terraform to provision OCI resources such as Kubernetes clusters, networking, and storage.
- Jenkins automates the deployment process by integrating with GitHub, ensuring that any code changes trigger an end-to-end deployment workflow.

## Architecture Overview

```
                                OCI-KubeNexus Architecture
┌───────────────────────────────────────────────────────────────────────────────────────────────┐
│  ┌─────────────┐                            ┌────────────────────────────────────────┐        │
│  │             │◄───────────────────────────│                                        │        │
│  │  GitHub     │         1-Clone            │                 User                   │───┐    │
│  │  Repository │                            │                                        │   │    │
│  │             │                            └────────────────────────────────────────┘   │    │
│  └──────┬──────┘                                                                         │    │
│         │                                                                                │    │
│         │ 2-Execute                                                                      │    │
│         │                                                                                │    │
│  ┌──────▼──────┐                            ┌────────────────────────────────────────┐   │    │
│  │             │ 3-Provision The OCI Infra  │      Oracle Cloud Infrastructure       │   │    │
│  │   OCI-IAC   ├────────────────────────────► ┌───────────────────────────────────┐  │   │    │
│  │ (Terraform) │                            │ │        OKE-compartment            │  │   │    │
│  │             │                            │ │        ┌─────────────┐            │  │   │    │
│  └──────┬──────┘                            │ │        │   Jenkins   │    8080    │  │   │    │
│         │    ┌────────────────────────────────────────►│   Instance  │◄──────────────────┤    │
│         │    │        4-Deploy Jenkins      │ │        └──────┬──────┘            │  │   │    │
│         │    │                              │ │               │                   │  │   │    │
│  ┌──────▼────┴─┐                            │ │               │                   │  │   │    │
│  │             │                            │ │               │                   │  │   │    │
│  │   Jenkins   │                            │ └───────────────│───────────────────┘  │   │    │
│  │   Module    │                            │                 │                      │   │    │
│  │             │                ┌─────────────────────────────┘                      │   │    │
│  └─────────────┘                │           │ ┌───────────────────────────────────┐  │   │    │
│                                 │           │ │        OKE-compartment            │  │   │    │
│                                 │           │ │                                   │  │   │    │
│                                 │           │ │        ┌─────────────┐            │  │   │    │
│  5-Apply K8s Manifests to OKE   │           │ │        │     OKE     │   80/443   │  │   │    │
│  ┌─────────────┐                │           │ │        │   Cluster   │◄──────────────────┘    │
│  │             │◄───────────────┘           │ │        └────▲────────┘            │  │        │
│  │    k8s-     ├────────────────────────────────────────────┘                     │  │        │
│  │  inception  │                            │ └───────────────────────────────────┘  │        │
│  │             │                            └────────────────────────────────────────┘        │
│  └─────────────┘                                                                              │
└───────────────────────────────────────────────────────────────────────────────────────────────┘
```

## Workflow

1. **Infrastructure Provisioning** - OCI-IAC module uses Terraform to provision:
   - OKE Kubernetes cluster
   - Network resources (VCN, subnets, security lists)
   - Compute instance for Jenkins

2. **CI/CD Setup** - Jenkins module:
   - Automatically configures Jenkins with required plugins
   - Sets up OCI and Kubernetes credentials
   - Creates deployment pipeline

3. **Application Deployment** - k8s-inception module:
   - Deploys WordPress application with MariaDB database
   - Configures Nginx as reverse proxy
   - Sets up persistent storage and networking

## Modules

### 1. [OCI-IAC](OCI-IAC)

This module provides a comprehensive setup for deploying and managing infrastructure on [**Oracle Cloud Infrastructure (OCI)**](https://www.oracle.com/cloud/) using [**Terraform**](https://www.terraform.io/). It includes configurations for setting up an [**Oracle Kubernetes Engine (OKE)**](https://www.oracle.com/cloud/cloud-native/kubernetes-engine/) cluster and a [**Jenkins**](https://www.jenkins.io/) server for CI/CD pipelines.

#### Key Features:
- Terraform configurations for OKE cluster and Jenkins server.
- Shell scripts for various automation tasks.
- Docker setup for OCI dashboard.

### 2. [Jenkins](jenkins)

This module contains the configuration and setup files for a [**Jenkins**](https://www.jenkins.io/) CI/CD environment. The setup includes Docker, Docker Compose, and various Groovy scripts to automate the initialization and configuration of Jenkins.

#### Key Features:
- Dockerfile to build the Jenkins image with necessary tools.
- Groovy scripts to automate Jenkins setup.
- Pipeline job to deploy Kubernetes manifests to the OKE cluster.

### 3. [k8s-inception](k8s-inception)

This module sets up a [**Kubernetes**](https://kubernetes.io/) environment to deploy a [**WordPress**](https://wordpress.org/) site with a [**MariaDB**](https://mariadb.org/) database and an [**Nginx**](https://nginx.org/) reverse proxy. The setup includes the necessary deployments, services, secrets, and persistent volume claims.

#### Key Features:
- Kubernetes manifests for WordPress, MariaDB, and Nginx.
- Secrets and ConfigMaps for environment configurations.
- Persistent Volume Claims for data persistence.

## Full Project Launch

The full project can be launched with a single command from the [**OCI-IAC**](OCI-IAC) module. This command builds the infrastructure and automatically runs Jenkins on an OCI instance that deploys the Kubernetes manifests to the cluster.

### Prerequisites

- [**Docker**](https://www.docker.com/)
- [**Docker Compose**](https://docs.docker.com/compose/)
- [**Git**](https://git-scm.com/)

### Steps to Launch:

1. **Clone the Repository:**
   ```bash
   git clone --recurse-submodules https://github.com/AhmedFatir/OCI-KubeNexus.git
   ```

2. [**Go to the OCI-IAC Module:**]()
   ```bash
   cd OCI-KubeNexu/OCI-IAC/
   ```
3. [**Copy the Example Environment File and Set the Required Variables:**]()
   ```bash
   cp .env.example .env
   # Edit the .env file and set the values accordingly
   ```

4. [**Build and Start the Docker Containers:**]()
   ```bash
   make
   ```

This will provision the necessary OCI resources, set up Jenkins, and deploy the WordPress application on the Kubernetes cluster.

[**Submodules Update**]()
- To update the submodules to latest commit
   ```bash
   git submodule update --recursive --remote
   ```
- If you encounter an error durirng the submodules update, try the following steps:
   ```bash
   git submodule deinit -f --all; \
   git submodule update --init --recursive; \
   git submodule update --recursive --remote
   ```
## Directory Structure

```
/OCI-KubeNexus
├── OCI-IAC/                         # Infrastructure as Code module
│   ├── resources/
│   ├── scripts/
│   ├── entrypoint.sh
│   ├── Dockerfile
│   ├── Makefile
│   └── README.md
├── jenkins/                         # Jenkins CI/CD environment module
│   ├── conf/
│   ├── init.groovy.d/
│   ├── Dockerfile
│   ├── docker-compose.yml
│   ├── Makefile
│   └── README.md
├── k8s-inception/                   # Kubernetes deployment module
│   ├── k8s/
│   ├── Makefile
│   └── README.md
└── README.md                        # Root README file
```