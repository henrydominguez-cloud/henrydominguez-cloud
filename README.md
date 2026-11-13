# Henry Domínguez Grullón

**Cloud Infrastructure Engineer** · GCP · OCI | Kubernetes · Terraform · CI/CD | DevOps & DevSecOps | Platform Engineering · SRE
Barcelona, Spain · Open to remote · [LinkedIn](https://www.linkedin.com/in/henry-cloud)

---

## In one minute

- **What I do:** I keep a fintech/telecom platform with tens of millions of users across EMEA reliable: multi-cluster, multi-region Kubernetes on GCP and OCI, infrastructure as code, GitOps-based CI/CD, observability end to end.
- **How I think:** if it isn't in Git, it doesn't exist. If a human has to do it twice, it becomes code. If an alert has no clear action, it shouldn't page anyone.
- **Where I'm going:** learning to build AI agents that improve operations workflows: triaging logs, answering questions from runbooks and automating routine tasks.

---

## How I work, and why

### Everything is code and versioned
**Why:** Any environment can be rebuilt from the repository. Changes are reviewed, not remembered.  
**In practice:** Terraform defines networking, clusters, reserved IPs and dual region; Ansible roles configure the servers. Rebuilding an environment means applying the repository, not following notes.  
**Cost:** Slower first change. Faster every change after that.

### Deploys go through a pipeline, never through hands
**Why:** Same steps every time. No "it worked on my machine".  
**In practice:** GitLab CI builds and publishes the image; ArgoCD syncs it into Kubernetes. Nobody runs `kubectl apply` against production.  
**Cost:** The pipeline becomes critical. It needs the same care as production.

### Security lives in the pipeline, not after it
**Why:** The CI never holds cluster credentials, so a leaked CI token can't touch production.  
**In practice:** ArgoCD pulls from Git inside the cluster, so GitLab CI never stores a kubeconfig; secrets stay outside the repository and RBAC follows least privilege.  
**Cost:** One more moving part (GitOps) between build and deploy.

### Every incident ends in documentation
**Why:** Tacit knowledge leaves with people. Written procedure stays.  
**In practice:** The fix becomes a runbook in Confluence, and any alert that fired without a clear action is changed or removed.  
**Cost:** Time spent writing when the fire is already out.

---

## Decisions I've made, and what I'd tell you about them

### Why two clouds instead of one?
**Context:** compute and outbound traffic are the biggest cost on a voice platform.
**Decision:** compute and egress run on **OCI**, where machines and data transfer cost materially less. Data and KPIs stay on **GCP**, because BigQuery and Looker already solve that as managed services. Small services go to Cloud Run instead of getting a cluster of their own.
**How they connect:** direct peering where traffic is constant, VPN where only control signalling crosses. Pay for bandwidth only where it is used.
**Trade-off:** two providers to operate, two IAM models, cross-cloud networking to maintain. Worth it because the saving is recurring and the complexity is one-off.

### Why a shared release workflow?
**Problem:** each service had its own way to release, so every process change had to be repeated service by service.  
**Decision:** shared CI logic and a common **Helm** template, with layered configuration (chart → infrastructure → application) so each service tunes its own values.  
**For the software teams:** they only touch their code, Dockerfile and `app.yaml`, and launch deployments from a deploy console, without needing to know how the pipeline works inside.  
**Checks before anything ships:** Gitleaks catches leaked passwords and tokens; syntax and lint checks catch broken configuration.  
**Trade-off:** a shared standard means coupling, so it's the default path, not a rule forced on every service.  
**Result:** the same checks on every release, one place to improve the process, and developers focused on their code.

### Why GitLab CI builds but ArgoCD deploys?
**Decision:** GitLab CI builds and publishes the image. **ArgoCD** pulls the change into the cluster.
**Why:** the pipeline never stores production credentials. The cluster pulls; nobody pushes into it.
**Trade-off:** two tools, and debugging a release sometimes means looking in both.

### Why build infrastructure you can rebuild?
**Decision:** Terraform describes networking, clusters, reserved IPs and dual region; Ansible roles configure the servers.  
**Why:** any environment can be rebuilt from the repository, not from someone's memory.  
**Detail that prevents outages:** booting machines through metadata (`startup-script`) stops Terraform from destroying and recreating a VM every time that script changes.

### Why Prometheus *and* ELK?
**Decision:** each kind of data in the tool built for it.
- **Prometheus** watches Kubernetes state and fires alerts: how much, and when.
- **ELK** handles raw data: application logs and call detail records (CDRs). **Logstash** filters and normalises before anything reaches BigQuery.

**Why:** dumping raw data into the warehouse costs more and leaves it untidy.
**Trade-off:** filters must be maintained as log formats change.

### Why fewer alerts?
**Decision:** an alert exists only if it has a defined response.
**Why:** alert fatigue makes people ignore the alert that matters.
**Trade-off:** each new alert needs design work up front.

---

## Tools: how I use them, and why that one

| Tool | How I use it | Why this one |
|---|---|---|
| **Kubernetes** (GKE, OKE) | Multi-region workloads with HPA, Ingress, StatefulSets, RBAC | Same operating model on both clouds |
| **Terraform** | Networking, clusters, reserved IPs, dual region | Declarative: describes what should exist, so drift is visible |
| **Ansible** | Server configuration through composable roles | Agentless; fits machines Kubernetes doesn't run |
| **GitLab CI + ArgoCD** | Build in CI, deploy through GitOps | Keeps production credentials out of the pipeline |
| **Helm** | One shared chart template for all services; each service sets only its own values | Templates + layered values = standard without rigidity |
| **Prometheus · Grafana** | Cluster state, alerting, dashboards | Pull model and PromQL suit Kubernetes metrics |
| **ELK + BigQuery** | Logs and CDRs filtered by Logstash, analysed in BigQuery | Right tool per data type; lower warehouse cost |
| **HAProxy · Redis** | Load balancing; Redis tuned for memory ceiling, eviction, persistence | Capacity found before the peak, not during it |
| **LLMs** (Claude, Gemini, ChatGPT) | Automate repetitive tasks and improve team workflows: documentation, merge-request review, task automation | Less manual work, faster feedback |

---

## Projects: why each repository exists

### [ansible-lego](https://github.com/henrydominguez-cloud/ansible-lego) · Public · MIT
**Why:** components like Java, Lua, FreeSWITCH or Kamailio get installed across many projects, sometimes clean, sometimes with project-specific config. One repo per project duplicates code and lets configs drift apart.  
**How:** three layers: bricks (one technology each, written once), services that compose them, and an inventory per project and environment. Examples ship with nginx and Postgres.  
**Result:** one source per component, full control over what each service gets.  
**Cost:** more variables to track, so documentation is part of the design.

### [unified-deploy-pipeline](https://github.com/henrydominguez-cloud/unified-deploy-pipeline) · Public · v0.1.0
**Why:** the shared-pipeline idea from my day job, rebuilt as a generic, tested reference to fork.  
**How:** every component calls one pipeline pinned by tag through a ~20-line dispatcher, with four modes (`full`, `config_only`, `deploy_image`, `rollback`) so a config change never costs a rebuild.  
**Result:** 32 script tests and end-to-end checks on kind. The GitHub Actions adapter is self-tested; the GitLab CI adapter is written and linted.

### In progress

| Repository | The question it answers |
|---|---|
| homelab-iac | Can I rebuild my whole lab from a repository? |
| k3s-gitops | What does GitOps look like end to end, outside a corporate setup? |
| llm-log-triage | Can an LLM sort noisy logs so a human only reads what matters? |
| ask-my-runbooks | Can runbooks answer questions instead of waiting to be read? |
| devops-orchestrator | Can an agent coordinate routine ops tasks safely? |

**Why public, and generic:** these are not copies of my day job. They use standard tools any team recognises, so you can read them, run them, and judge how I think.

---

## What I'm learning, and why

| Topic | Why now | Where it shows |
|---|---|---|
| **AI agents for operations** | Most ops toil is reading, sorting and repeating. That is what agents do well. | `llm-log-triage`, `ask-my-runbooks`, `devops-orchestrator` |
| **AWS** | Most of the market runs on it. My cloud fundamentals transfer; the service names change. | Labs in progress |

---

## Experience

| Period | Role | Company |
|---|---|---|
| Sep 2022 — Present | Cloud Infrastructure Engineer (EMEA) | fonYou, Barcelona |
| Sep 2020 — Sep 2022 | Operations Technician: observability with Grafana and ELK, incident prevention, KPIs for product owners | fonYou, Barcelona |
| Dec 2019 — Sep 2020 | Technical Support Engineer, Google Cloud: diagnosis, customer workarounds, product feedback | Webhelp (now Concentrix) |
| 2015 — 2019 | IT Support Technician: systems, VMware, networking | Masoliver · Ajuntament d'Olot |

**Languages:** Spanish (native) · Catalan (professional) · English (professional)

**Let's talk:** [linkedin.com/in/henry-cloud](https://www.linkedin.com/in/henry-cloud). Ask me about any trade-off above.
