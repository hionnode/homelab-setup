
-----

# 🚀 Homelab Setup: From Bare Metal to a Cloud-Native Platform

This repository documents the journey of building a production-ready homelab platform from scratch. The goal is to demystify the cloud by replicating its core services (like EKS, RDS, and VPC) on bare-metal hardware. This isn't just about running services; it's about understanding the engineering philosophy behind them.

## 🗺️ Infrastructure Diagram

The entire project, from the physical layer to the final production platform, is captured in the diagram below.



-----

## 🧭 Engineering Philosophy

This project is guided by a few core principles:

>   * **🔍 No Black Boxes:** Understand every component, from the bootloader to the application layer.
>   * **🛠️ Build Everything From Scratch:** Avoid managed services to learn the underlying mechanics.
>   * **📚 Document All Failures:** Learning comes from iteration and understanding what went wrong.
>   * **🎯 Production-Ready Standards:** Aim for high availability, robust security, and comprehensive monitoring.
>   * **🎉 Have Fun Learning\!**

-----

## 🎯 Key Success Metrics

Success for this project is measured by the following metrics:

| Metric                 | Target                      | Description                               |
| :--------------------- | :-------------------------- | :---------------------------------------- |
| 🎯 **Uptime** | `99.9%`                     | High availability for all critical services. |
| ⚡ **Provisioning** | `< 5 seconds`               | Time to create a new dynamic workload.    |
| 📊 **Concurrency** | `50+ Workloads`             | Ability to run dozens of services at once. |
| 💰 **Budget** | `$300`                      | Total hardware and operational cost.      |
| ⏰ **Timeline** | `30 Days`                   | From Day 0 to the Day 30 Goal.            |
| 📈 **Learning** | `Demystify the cloud`       | The primary, overarching objective.       |

-----

## 🛠️ Technology Stack

The platform is built using a curated stack of open-source technologies:

| Category          | Technologies                                                    |
| :---------------- | :-------------------------------------------------------------- |
| **Virtualization** | Proxmox VE                                                      |
| **Storage** | ZFS, Longhorn                                                   |
| **Networking** | pfSense, HAProxy, CoreDNS                                       |
| **Containers** | Docker, K3s Kubernetes                                          |
| **Databases** | PostgreSQL (HA Cluster), Redis                                  |
| **Messaging** | RabbitMQ                                                        |
| **CI/CD** | GitLab CI, ArgoCD                                               |
| **Security** | Let's Encrypt, Vault                                            |
| **Observability** | Prometheus, Grafana, Jaeger                                     |
| **IaC** | Terraform                                                       |

-----

## 🗓️ Project Roadmap (The 30-Day Challenge)

The project is broken down into a 4-week timeline.

  * **🟡 Week 1: Infrastructure Foundation**

      * [ ] pfSense Firewall & Router Setup
      * [ ] VLAN Segmentation
      * [ ] Packer/Cloud-Init VM Templates
      * [ ] Terraform Provider for Proxmox (IaC)

  * **🔵 Week 2: Distributed Systems**

      * [ ] High-Availability PostgreSQL Clustering
      * [ ] RabbitMQ Cluster Setup
      * [ ] API Gateway Implementation
      * [ ] Deploying First Stateful & Stateless Microservices

  * **🔵 Week 3: Container Orchestration**

      * [ ] K3s Kubernetes Cluster Deployment
      * [ ] Service Mesh (e.g., Linkerd/Istio) Implementation
      * [ ] Configuring Dynamic Scaling (HPA)
      * [ ] Ingress Controller Setup (HAProxy/Traefik)

  * **🔵 Week 4: Production SRE**

      * [ ] Full Observability Stack (Prometheus, Grafana, Jaeger)
      * [ ] Chaos Engineering Experiments
      * [ ] Automated Backup & Recovery Drills
      * [ ] Performance Tuning & Benchmarking

-----

## 🏛️ Architecture

### Replicating Cloud Services

The core of this challenge is to replace expensive, managed cloud services with self-hosted, open-source alternatives.

| Cloud Service (AWS/GCP) | Homelab Equivalent           |
| :---------------------- | :--------------------------- |
| **VPC** | `pfSense + VLANs`            |
| **EKS / GKE** | `K3s Kubernetes Cluster`     |
| **RDS** | `PostgreSQL HA Cluster`      |
| **SQS / SNS** | `RabbitMQ Cluster`           |
| **Lambda** | `Dynamic Containers (K3s)`   |
| **ALB / NLB** | `HAProxy + Ingress`          |
| **Route53** | `CoreDNS`                    |
| **CloudWatch** | `Prometheus + Grafana`       |

### Network Design

The network is segmented into multiple VLANs for security and traffic isolation.

| Network      | CIDR                | Purpose                                      |
| :----------- | :------------------ | :------------------------------------------- |
| **Management** | `192.168.1.0/24`    | Proxmox hosts, switch, infrastructure access |
| **DMZ** | `192.168.10.0/24`   | Public-facing services (e.g., HAProxy)     |
| **Services** | `192.168.20.0/24`   | Backend applications and APIs              |
| **Storage** | `192.168.30.0/24`   | Dedicated network for ZFS/Longhorn traffic |
| **Container** | `10.42.0.0/16`      | K3s internal pod-to-pod networking         |

-----

## 🤝 Contributing

Contributions, issues, and feature requests are welcome\! Feel free to check the [issues page](https://github.com/hionnode/homelab-setup/issues).

1.  **Fork** the repository.
2.  Create your feature branch (`git checkout -b feature/AmazingFeature`).
3.  Commit your changes (`git commit -m 'Add some AmazingFeature'`).
4.  Push to the branch (`git push origin feature/AmazingFeature`).
5.  Open a **Pull Request**.

## 📜 License

This project is distributed under the MIT License. See `LICENSE` for more information.