# Kubernetes Interview Prep — Quick Reference

> Covers everything from real challenges. Read this before your interview.

---

## 1. Resources & Limits

### Golden Rules
- `limits` must always be **≥ requests** (memory especially)
- CPU is **throttled** when over limit — pod keeps running but gets slow
- Memory is **OOMKilled** when over limit — pod is killed immediately (exit code 137)

### Quick Check
```yaml
resources:
  requests:
    memory: "256Mi"   # what the pod needs guaranteed
    cpu: "250m"
  limits:
    memory: "512Mi"   # max allowed — must be >= request
    cpu: "500m"
```

### Java + Kubernetes Trap
```yaml
# ❌ WRONG — JVM heap exceeds container limit → OOMKilled
limits:
  memory: "1Gi"
env:
  - name: JAVA_OPTS
    value: "-Xmx2g"    # 2GB heap > 1GB limit 💥

# ✅ RIGHT — let JVM auto-detect container limits (Java 11+)
env:
  - name: JAVA_OPTS
    value: "-XX:+UseContainerSupport -XX:MaxRAMPercentage=75.0"
```

### ResourceQuota vs LimitRange
| | ResourceQuota | LimitRange |
|---|---|---|
| Scope | Whole namespace | Per container |
| Controls | Total CPU/memory/pods | Min/max per container |
| Effect | Rejects pod if namespace quota exceeded | Injects defaults if no resources set |

---

## 2. Deployments

### Common Errors
```yaml
# ❌ replicas hardcoded with HPA — fights each other
replicas: 3     # remove this when HPA is present

# ❌ maxUnavailable too high for critical service
rollingUpdate:
  maxUnavailable: 2   # 66% capacity gone during deploy 💥
  
# ✅ zero-downtime rolling update
rollingUpdate:
  maxSurge: 1
  maxUnavailable: 0   # new pod up before old pod down
```

### Selector Mismatch — Silent Traffic Loss
```yaml
# ❌ Pod has label: app: webapp
# Service selector: app: webapp + tier: frontend  → no traffic ever reaches pod

# ✅ Pod labels must contain ALL selector labels
template:
  metadata:
    labels:
      app: webapp
      tier: frontend   # must match service selector exactly
```

---

## 3. Services & Ingress

### Service Types
| Type | Access | Use case |
|---|---|---|
| ClusterIP | Inside cluster only | Default, microservices |
| NodePort | Node IP + port | Dev/testing |
| LoadBalancer | External IP via cloud | Production external access |

### Port Mapping — Easy to Confuse
```
User → port (Service) → targetPort (container) → containerPort
         80                  8080                    8080
```

### Ingress Path Ordering — Most Specific First
```yaml
# ❌ WRONG — /payments catches /payments/webhook first
- path: /payments
- path: /payments/webhook

# ✅ RIGHT — specific before general
- path: /payments/webhook   # specific first
- path: /payments           # general second
```

### Ingress Checklist
- [ ] IngressClass exists (`kubectl get ingressclass`)
- [ ] TLS secret exists in same namespace
- [ ] Backend services exist and selector matches pods
- [ ] `rewrite-target` uses capture groups if stripping paths

---

## 4. Storage

### PV + PVC Binding Rules — ALL must match
| Field | Must match |
|---|---|
| accessModes | Exact match |
| storage | PVC ≤ PV capacity |
| storageClassName | Exact match |

### accessModes Quick Reference
| Mode | Shorthand | Meaning |
|---|---|---|
| ReadWriteOnce | RWO | One node at a time (EBS, most block storage) |
| ReadWriteMany | RWX | Many nodes (EFS, NFS) |
| ReadOnlyMany | ROX | Many nodes, read only |

### EBS Zone Trap
```yaml
# ❌ Immediate binding — EBS created in wrong zone before pod schedules
volumeBindingMode: Immediate

# ✅ Wait for pod to schedule first, then provision in correct zone
volumeBindingMode: WaitForFirstConsumer
```

### StatefulSet Rules
- `serviceName` must match the headless Service name exactly
- Each pod gets its own PVC — `data-postgres-0`, `data-postgres-1`
- Use `RWO` — each pod owns its own volume

---

## 5. RBAC

### Hierarchy
```
ServiceAccount → RoleBinding → Role          (namespace scoped)
ServiceAccount → ClusterRoleBinding → ClusterRole  (cluster scoped)
```

### Common Errors
```yaml
# ❌ RoleBinding subject namespace wrong
subjects:
  - kind: ServiceAccount
    name: my-sa
    namespace: default      # SA actually lives in production 💥

# ✅ Must match where SA is created
    namespace: production
```

### Privilege Escalation Red Flag
```yaml
# ❌ DANGEROUS — looks like read-only but grants full RBAC control
- apiGroups: ["rbac.authorization.k8s.io"]
  resources: ["roles", "rolebindings", "clusterroles", "clusterrolebindings"]
  verbs: ["get", "list", "watch", "create", "update", "patch"]  # 💥 can create cluster-admin binding

# ✅ Read only
  verbs: ["get", "list", "watch"]
```

### Quick Audit Command
```bash
# Find dangerous RBAC — anyone who can modify RBAC resources
kubectl get clusterroles -o yaml | grep -A5 "rbac.authorization.k8s.io"
```

---

## 6. Networking & DNS

### CoreDNS Checklist
```yaml
# ❌ cache too low → DNS storm with many pods
cache 5      # 5 seconds

# ✅ standard value
cache 30

# ❌ hardcoded upstream — risk of forwarding loop
forward . 8.8.8.8

# ✅ use node's DNS resolver
forward . /etc/resolv.conf

# ⚠️ loop plugin — detects loops but can crash CoreDNS if loop exists
loop   # ensure upstream DNS doesn't route back to CoreDNS
```

### Intermittent NXDOMAIN — Checklist
1. CoreDNS pods OOMKilling → gaps in DNS resolution
2. `cache` TTL too low → too many queries → memory pressure
3. Namespace label for NetworkPolicy wrong → CoreDNS blocked
4. Scale up CoreDNS replicas for large clusters

```bash
# Scale CoreDNS
kubectl scale deployment coredns --replicas=4 -n kube-system
```

### NetworkPolicy Namespace Selector Trap
```yaml
# ❌ Kubernetes does NOT auto-add name label
namespaceSelector:
  matchLabels:
    name: ingress-nginx    # doesn't exist unless manually added 💥

# ✅ Auto-label added by Kubernetes since v1.21
namespaceSelector:
  matchLabels:
    kubernetes.io/metadata.name: ingress-nginx
```

---

## 7. Scheduling, Taints & Affinity

### Mental Model
```
Taint      = Bouncer blocking entry to node
Toleration = Your name on the guest list
nodeAffinity = Which section of the venue you want to sit in
```

### Toleration + nodeAffinity — Both Required
```bash
# Node must have BOTH for pod to land there
kubectl taint nodes <node> dedicated=payment:NoSchedule
kubectl label nodes <node> node-type=payment-node   # ← don't forget this
```

### podAffinity vs podAntiAffinity
```
podAffinity     = "sit next to teammate" → pods attract → DANGEROUS with required
podAntiAffinity = "own desk"             → pods repel  → use for HA
```

```yaml
# ❌ podAffinity required → deadlock, all pods Pending
podAffinity:
  requiredDuringSchedulingIgnoredDuringExecution: ...

# ✅ podAntiAffinity preferred → spread pods, won't block
podAntiAffinity:
  preferredDuringSchedulingIgnoredDuringExecution:
    - weight: 100
      podAffinityTerm:
        labelSelector:
          matchLabels:
            app: my-app
        topologyKey: kubernetes.io/hostname
```

### Use `preferred` unless you truly need `required`
> `required` = hard rule → pod stays Pending if not satisfied
> `preferred` = best effort → pod schedules even if rule can't be met

---

## 8. Probes

### Three Probe Types
| Probe | Purpose | Failure action |
|---|---|---|
| startupProbe | App finished booting | Kill & restart |
| livenessProbe | App is alive | Kill & restart |
| readinessProbe | App ready for traffic | Remove from endpoints |

### Common Errors
```yaml
# ❌ startupProbe window too short — app takes 60s but probe gives 15s
startupProbe:
  failureThreshold: 3
  periodSeconds: 5       # 3 × 5 = 15 seconds only 💥

# ✅ give enough time for slowest startup
startupProbe:
  failureThreshold: 30   # 30 × 5 = 150 seconds
  periodSeconds: 5

# ❌ readinessProbe initialDelay too short — app not warmed up yet
readinessProbe:
  initialDelaySeconds: 5   # pod marked Ready at 5s, traffic sent, app still loading 💥

# ✅ match actual warmup time
readinessProbe:
  initialDelaySeconds: 30

# ❌ successThreshold > 1 on livenessProbe — invalid, rejected
livenessProbe:
  successThreshold: 2    # only 1 is valid for liveness 💥
```

### Best Practice — App Controls Its Own Readiness
```
GET /ready → 503  (cache loading...)
GET /ready → 503  (cache loading...)
GET /ready → 200  (ready!) ✅ now Kubernetes sends traffic
```

---

## 9. Pod Disruption Budgets (PDB)

### Golden Rule
```
minAvailable must always be < replicas
maxUnavailable must always be > 0
```

```yaml
# ❌ minAvailable = replicas → kubectl drain hangs forever
replicas: 3
minAvailable: 3    # no pod can ever be evicted 💥

# ✅
minAvailable: 2    # allows 1 disruption
# or
minAvailable: "60%"   # scales with HPA automatically
```

### Before Draining a Node — Always Check
```bash
kubectl describe pdb -n <namespace>
# If Allowed disruptions: 0 → drain will hang
```

### Fix a Stuck Drain
```bash
# Option 1 — scale up first
kubectl scale deployment my-app --replicas=4

# Option 2 — temporarily patch PDB
kubectl patch pdb my-pdb -p '{"spec":{"minAvailable": 1}}'

# Option 3 — bypass PDB (last resort)
kubectl drain <node> --disable-eviction
```

---

## 10. HPA

### Common Errors
```yaml
# ❌ min = max → HPA exists but can never scale
minReplicas: 2
maxReplicas: 2    # pointless 💥

# ❌ Only CPU metric — blind to memory issues
# ✅ Always add memory metric for memory-intensive apps
metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 80
  - type: Resource
    resource:
      name: memory
      target:
        type: Utilization
        averageUtilization: 75

# ❌ replicas hardcoded in Deployment + HPA → fight each other
# ✅ Remove replicas field from Deployment when using HPA
```

---

## 11. StatefulSet vs Deployment

| | Deployment | StatefulSet |
|---|---|---|
| Pod names | Random | Stable (pod-0, pod-1) |
| Storage | Shared or none | Each pod gets own PVC |
| Scaling | Simultaneous | Ordered (0→1→2) |
| Use case | Stateless apps | Databases, queues |

```yaml
# ❌ serviceName must match headless Service name exactly
serviceName: postgres        # headless service named postgres-headless 💥

# ✅
serviceName: postgres-headless
```

---

## 12. Jobs & CronJobs

### restartPolicy Rules
```
Deployment/StatefulSet → Always
Job/CronJob            → OnFailure or Never (Never use Always)
```

### CronJob Checklist
```yaml
concurrencyPolicy: Forbid        # ✅ skip if previous still running (use for backups)
successfulJobsHistoryLimit: 3    # ✅ keep history for debugging
failedJobsHistoryLimit: 5        # ✅ keep more failures than successes

# ❌ both 0 → no history, flying blind in production
successfulJobsHistoryLimit: 0
failedJobsHistoryLimit: 0
```

### Job Parallelism Warning
```yaml
# ❌ parallelism > 1 for DB migration → race conditions, data corruption
completions: 1
parallelism: 3    # 3 pods running same migration simultaneously 💥

# ✅ migrations always serial
parallelism: 1
```

---

## 13. Security — PodSecurityAdmission

### Three Levels
| Level | What it allows |
|---|---|
| privileged | Everything — no restrictions |
| baseline | Blocks most dangerous settings |
| restricted | Strictest — production standard |

### restricted Policy — Mandatory Fields
```yaml
securityContext:
  allowPrivilegeEscalation: false   # required
  readOnlyRootFilesystem: true      # required
  runAsNonRoot: true                # required
  capabilities:
    drop: ["ALL"]                   # required
    add: []                         # only safe caps if absolutely needed
```

### Namespace Labels
```yaml
# ✅ correct usage — warn at stricter level than enforce
pod-security.kubernetes.io/enforce: baseline
pod-security.kubernetes.io/warn: restricted    # advance warning before tightening

# ❌ warn = enforce → redundant, confusing
pod-security.kubernetes.io/enforce: restricted
pod-security.kubernetes.io/warn: restricted
```

---

## 14. Incident Response Playbook

### OOMKilled — 2am Checklist
```bash
# 1. Check what's OOMKilling
kubectl describe pod <pod> -n <ns> | grep -A5 "Last State"

# 2. Check node memory pressure
kubectl describe node <node> | grep -A5 "Conditions"
kubectl top nodes

# 3. Quick fix — bump memory limit
kubectl set resources deployment <name> --limits=memory=1Gi -n <ns>

# 4. Roll back if recent deploy caused it
kubectl rollout undo deployment/<name> -n <ns>

# 5. Cordon bad node immediately
kubectl cordon <node>
```

### Node NotReady — Common Causes
1. kubelet crashed (OOM on node itself)
2. Disk pressure (logs/images filled disk)
3. Network partition (can't reach API server)
4. Container runtime crashed (containerd/docker)

```bash
# Diagnose
kubectl describe node <node>       # check Conditions
kubectl get events --field-selector involvedObject.name=<node>
ssh <node> && journalctl -u kubelet -n 50
```

### DNS NXDOMAIN Intermittent — Checklist
```bash
# 1. Check CoreDNS health
kubectl get pods -n kube-system | grep coredns
kubectl top pods -n kube-system | grep coredns   # memory usage

# 2. Scale up CoreDNS
kubectl scale deployment coredns --replicas=4 -n kube-system

# 3. Test DNS resolution
kubectl run debug --image=busybox --rm -it -- nslookup <service>.<namespace>.svc.cluster.local

# 4. Check cache TTL in ConfigMap
kubectl edit configmap coredns -n kube-system   # set cache 30
```

---

## 15. Quick Commands Cheat Sheet

```bash
# Check why pod is not scheduling
kubectl describe pod <pod> -n <ns>           # look at Events section

# Check service endpoints (is selector matching?)
kubectl get endpoints <svc> -n <ns>          # <none> = selector mismatch

# Check resource quota usage
kubectl describe resourcequota -n <ns>

# Check which pods are consuming most memory
kubectl top pods -n <ns> --sort-by=memory

# Force delete stuck pod
kubectl delete pod <pod> -n <ns> --grace-period=0 --force

# Check RBAC — what can a serviceaccount do
kubectl auth can-i list secrets \
  --as=system:serviceaccount:<ns>:<sa> -n <ns>

# Drain node safely
kubectl cordon <node>
kubectl drain <node> --ignore-daemonsets --delete-emptydir-data

# Rollout commands
kubectl rollout status deployment/<name> -n <ns>
kubectl rollout history deployment/<name> -n <ns>
kubectl rollout undo deployment/<name> -n <ns>
```

---

## 16. Interview Golden Rules

| Topic | Rule |
|---|---|
| Limits | limits ≥ requests always |
| Memory | OOMKilled = limit too low or leak |
| CPU | Throttled = limit too low, pod survives |
| HPA | Scale on memory too, not just CPU |
| HPA + Deployment | Remove `replicas` from Deployment |
| PDB | minAvailable always < replicas |
| Drain | Check `Allowed disruptions` before drain |
| Storage | WaitForFirstConsumer for zonal storage |
| StatefulSet | serviceName must match headless Service |
| Ingress | Specific paths before general paths |
| DNS | Use `kubernetes.io/metadata.name` for NS selector |
| RBAC | Never grant create/patch on RBAC resources |
| Probes | App controls /ready endpoint, not fixed delays |
| Jobs | Never `parallelism > 1` for migrations |
| CronJob | Never `historyLimit: 0` in production |
| Security | restricted policy needs 4 mandatory fields |
| Java | Always use `UseContainerSupport` or set Xmx < limit |

---

*Good luck! You've earned this. 🎯*
