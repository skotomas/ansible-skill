# Kubernetes

Pin a supported Kubespray release and use its documented OS, Python, Ansible and Kubernetes requirements together. Follow that release's inventory model; do not regenerate inventory merely because group_vars changed.
Size control plane/etcd for quorum and failure domains. Three control-plane/etcd nodes are a common HA topology; two extra workers are not a universal minimum. Plan etcd backup/restore, endpoint availability and storage lifecycle.
Use kubernetes.core.k8s/helm with explicit kubeconfig/context/namespace and appropriate RBAC. Protect kubeconfig and Kubernetes Secret contents. Pin workload/chart versions and wait for readiness.
A Deployment rollout requires changing its Pod template. For an intentional restart patch spec.template.metadata.annotations, not top-level metadata.annotations. Use a handler or explicit maintenance operation and then verify rollout completion. Example task fragment (supply deployment_name and deployment_namespace):

```yaml
- name: Request deployment rollout
  kubernetes.core.k8s:
    state: patched
    api_version: apps/v1
    kind: Deployment
    name: "{{ deployment_name }}"
    namespace: "{{ deployment_namespace }}"
    definition:
      spec:
        template:
          metadata:
            annotations:
              kubectl.kubernetes.io/restartedAt: "{{ now(utc=true, fmt='%Y-%m-%dT%H:%M:%SZ') }}"
```

Check rollout status separately with bounded waiting. Scaling to zero introduces downtime and is not a general restart strategy.

Reference: [official documentation](https://docs.ansible.com/projects/ansible/latest/collections/kubernetes/core/k8s_module.html). Verify against the selected installed versions.
