# KubeBreach

* Emulating breaches in Kubernetes
* Adversarial Emulation for k8s workloads

# High Level Architecture

```mermaid
graph LR
    subgraph Operator["👤 Red Team Operator"]
        Browser["🖥️ Browser"]
    end

    subgraph Caldera["⚔️ Caldera Server\nhttps://&lt;caldera-server&gt;:8443"]
        CalderaCore["Attack Orchestrator\n(MITRE Caldera)\n──────────────\nCampaigns & Abilities\nMITRE ATT&CK TTPs\nPlugins"]
    end

    subgraph K8sCluster["☸️ Kubernetes Cluster"]
        direction TB
        subgraph AgentPod["Agent Workload"]
            K8sSandcat["k8s-sandcat\n──────────────\nCaldera Sandcat Agent\n+ kubectl\n+ etcdctl  + crictl\n+ helm"]
        end
        subgraph TargetPods["Target Workloads"]
            WL1["Pod"]
            WL2["Pod"]
            WLN["..."]
        end
    end

    Browser -- "Configure &\nLaunch Attacks" --> CalderaCore
    CalderaCore -- "C2 Channel\nTask Assignment\n& Results (HTTPS)" --> K8sSandcat
    K8sSandcat -- "Execute Adversarial\nActions (TTPs)" --> TargetPods
```

| Component | Role |
|-----------|------|
| [Caldera-Server](https://github.com/mitre/caldera) | Central attack orchestrator; manages adversarial campaigns, abilities, and results |
| [K8s-Sandcat](https://github.com/accuknox/k8s-sandcat) | Caldera Sandcat agent deployed inside the cluster; bundles Kubernetes tools ([kubectl](https://kubernetes.io/docs/reference/kubectl/), [etcdctl](https://etcd.io/docs/v3.5/dev-guide/interacting_v3/), [crictl](https://github.com/kubernetes-sigs/cri-tools), [helm](https://helm.sh/)) alongside [MITRE Caldera Sandcat](https://github.com/mitre/sandcat) to drive k8s adversarial TTPs |

# Emulating Adversarial behaviour in k8s workloads

## Hashicorp Vault


