# Henry Domínguez Grullón

**Cloud Infrastructure Engineer**
GCP · OCI | Kubernetes · Terraform · CI/CD | DevOps & DevSecOps | Platform Engineering · SRE

Barcelona, Spain · Open to remote · [LinkedIn](https://www.linkedin.com/in/henry-cloud)

## Profile

Ten years in systems and infrastructure, the last six in cloud and four designing and operating highly available multicloud platforms on GCP and Oracle Cloud Infrastructure. I am accountable for the reliability of a fintech/telecom platform serving millions of users across EMEA: multi-region Kubernetes, infrastructure as code, unified CI/CD and end-to-end observability.

**How I work:** everything that exists is described in code and versioned, deployments go through a pipeline and not through hands, security is applied in the pipeline rather than after it, and every incident ends in documentation that prevents the next one.

## Selected work

| Area | What I did | Why it matters |
|---|---|---|
| **Multicloud architecture** | Split the platform across two providers: compute and outbound traffic on OCI, data and KPIs on GCP (BigQuery, Looker, Cloud Run). Peering where traffic is constant, VPN where only control signalling crosses. | Lower cost without giving up capability |
| **Unified CI/CD pipeline** (ongoing) | Central CI logic, a minimal dispatcher per repository, and one generic Helm chart replacing ~30 bespoke charts. GitLab CI builds and publishes; ArgoCD deploys. | One release procedure instead of thirty; the pipeline never stores production credentials |
| **Modular Ansible + Terraform** (ongoing) | Server configuration as reusable pieces instead of one script per environment. Terraform describes networking, clusters, reserved IPs and dual region. | Changes stay contained; any environment can be rebuilt from the repository |
| **Layered observability** | Prometheus for Kubernetes state and alerting; ELK for application logs and call detail records, with Logstash filtering and normalising before BigQuery. | Each kind of data in the right tool, at lower warehouse cost |
| **Capacity and diagnostics** | Sized the voice platform (FreeSWITCH/Kamailio) against projected growth; HAProxy load balancing; Redis tuning (memory ceiling, eviction, persistence). | The system's limit is known before the peak arrives |

## Projects

Each repository documents the problem, the design decisions and the trade-offs.

| Repository | Description | Status |
|---|---|---|
| [ansible-lego](https://github.com/henrydominguez-cloud/ansible-lego) | Ansible roles built by composition: small, reusable pieces instead of one long playbook per environment. MIT. | Public |
| homelab-iac | Proxmox homelab defined as code | In progress |
| k3s-gitops | K3s cluster managed with GitOps | In progress |
| llm-log-triage | LLM-assisted log triage | In progress |
| ask-my-runbooks | Question answering over runbooks | In progress |
| devops-orchestrator | Agent that coordinates operations tasks | In progress |

## Technical stack

| Area | Tools |
|---|---|
| **Cloud** | GCP (GKE, Cloud Run, Pub/Sub, BigQuery, Looker) · OCI (OKE, VCN, registry) · AWS (EC2, VPC, S3, IAM, ECS/EKS, Lambda) |
| **Containers** | Kubernetes · Docker · Helm · ArgoCD · RBAC · HPA · Ingress · StatefulSets |
| **IaC & CI/CD** | Terraform · Ansible · GitLab CI/CD · GitHub Actions · Git · Jenkins |
| **Observability** | Prometheus · PromQL · Grafana · ELK (Elasticsearch, Logstash, Kibana) |
| **Networking** | VPC/VCN · routing & DNS · load balancing · HAProxy · TLS · VoIP/SIP |
| **Security** | Least-privilege RBAC across clusters and clouds · CI holds no cluster credentials · secrets outside the repository · private endpoints, firewall rules, TLS |
| **Data** | Redis · MySQL · PostgreSQL · BigQuery |
| **Code & systems** | Python · Bash · Linux · VMware · Proxmox |
| **Applied AI** | LLMs (Gemini, Claude, ChatGPT) for technical documentation, merge-request review and workflow automation |

## Experience

| Period | Role | Company |
|---|---|---|
| Sep 2022 – Present | Cloud Infrastructure Engineer (EMEA) | fonYou, Barcelona |
| Sep 2020 – Sep 2022 | Operations Technician | fonYou, Barcelona |
| Dec 2019 – Sep 2020 | Technical Support Engineer, Google Cloud | Webhelp (now Concentrix), Barcelona |
| 2015 – 2019 | IT Support Technician | Masoliver · Ajuntament d'Olot |

**Languages:** Spanish (native) · Catalan (professional) · English (professional)

## Contact

[linkedin.com/in/henry-cloud](https://www.linkedin.com/in/henry-cloud)
