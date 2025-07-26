
# 🔍 Kubernetes Health Check Probes

Kubernetes provides three key types of **health-check probes** to monitor and manage the state of containerized applications:

---

## 🔄 1. Liveness Probe

- **Purpose:** Checks if a container is *still running*.
- **Mechanism:** If this probe fails, Kubernetes will **restart** the container.
- **Use Case:** Useful for detecting **deadlocks** or **hung processes** where the container is still running but unresponsive.

### ✅ Example (HTTP):
```yaml
livenessProbe:
  httpGet:
    path: /healthz
    port: 8080
  initialDelaySeconds: 10
  periodSeconds: 5
```

---

## 🚦 2. Readiness Probe

- **Purpose:** Checks if a container is *ready to serve requests*.
- **Mechanism:** If this probe fails, the pod is **removed from service endpoints** but **not restarted**.
- **Use Case:** Prevents traffic from reaching the pod during **initialization** or **temporary unavailability**.

### ✅ Example (HTTP):
```yaml
readinessProbe:
  httpGet:
    path: /ready
    port: 8080
  initialDelaySeconds: 5
  periodSeconds: 10
```

---

## 🛫 3. Startup Probe

- **Purpose:** Helps with **slow-starting** applications.
- **Mechanism:** Overrides other probes until the application has **fully started**. If this probe fails, the container is **killed**.
- **Use Case:** Ensures Kubernetes doesn't mark the pod as unhealthy **too early**.

### ✅ Example (HTTP):
```yaml
startupProbe:
  httpGet:
    path: /startup
    port: 8080
  failureThreshold: 30
  periodSeconds: 10
```

---

## 📌 Summary

| Probe Type     | Restart Container | Remove from Endpoints | Use Case                        |
|----------------|-------------------|------------------------|---------------------------------|
| Liveness Probe | ✅ Yes            | ❌ No                  | Recover from deadlocks          |
| Readiness Probe| ❌ No             | ✅ Yes                 | Wait for app to be ready        |
| Startup Probe  | ✅ Yes (on fail)  | ❌ No                  | Handle slow-starting containers |

---

## 📂 Integration Tips

- Probes can use `httpGet`, `tcpSocket`, or `exec` commands.
- Define probes in each container spec inside the pod YAML.
- Use `kubectl describe pod <pod-name>` to troubleshoot probe failures.

---

## 🧪 Testing Commands

```bash
kubectl apply -f my-app-with-probes.yaml
kubectl describe pod <pod-name>
kubectl logs <pod-name>
```

---

## 📚 References

- [Kubernetes Docs - Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/)
