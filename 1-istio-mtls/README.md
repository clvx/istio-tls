By default, Istio tracks the server workloads migrated to Istio proxies, and configures client proxies to send mutual TLS traffic to those workloads automatically, and to send plain text traffic to workloads without sidecars. Thus, all traffic between workloads with proxies uses mutual TLS, without you doing anything.

While Istio automatically upgrades all traffic between the proxies and the workloads to mutual TLS, workloads can still receive plain text traffic. To prevent non-mutual TLS traffic for the whole mesh, set a mesh-wide peer authentication policy with the mutual TLS mode set to STRICT.

To change mutual TLS for all workloads within a particular namespace, use a namespace-wide policy. The specification of the policy is the same as for a mesh-wide policy, but you specify the namespace it applies to under metadata.

A workload-specific peer authentication policy takes precedence over a namespace-wide policy.
