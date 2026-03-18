# python-app — Kubernetes Image Test Project

A minimal Kubernetes setup to test a cleanstart Python Docker image (`cleanstart/python:latest-dev`).

---

## Project Structure

```
python-app/
├── namespace.yaml       # Namespace: python-app
├── job.yaml             # Job to run and test the image
└── README.md
```

---

## Prerequisites

- `kubectl` configured and pointing to your cluster
- Docker image `cleanstart/python:latest-dev` accessible by your cluster

---

## Run

```bash
kubectl apply -f namespace.yaml
kubectl apply -f job.yaml -n python-app
```

---

## Test Image Functionality

### 1. Watch the Job complete

```bash
kubectl get jobs -n python-app --watch
```

Expected output when successful:
```
NAME             COMPLETIONS   DURATION   AGE
python-app-job   1/1           3s         5s
```

`1/1` means the container ran and exited with code 0.

---

### 2. Get the pod name

```bash
kubectl get pods -n python-app
```

The pod status will be `Completed` on success or `Error` on failure.

---

### 3. Check logs

This is the primary way to verify what your image actually did:

```bash
kubectl logs <POD-NAME> -n python-app
```

Expected output:
```
=== Python Image Verification ===
Python version : 3.14.3 (main, Feb  5 2026, 09:31:03) [GCC 13.2.1 20240309]
Platform       : Linux 6.6.87.2-microsoft-standard-WSL2
Architecture   : x86_64
Executable     : /usr/bin/python
=== Image is working correctly ===
```

If the pod has already finished, logs are still available until the namespace is deleted.

---

### 4. Inspect exit code

```bash
kubectl describe pod <POD-NAME> -n python-app
```

Look for `Last State: Terminated` → `Exit Code` in the output:

- **Exit Code 0** — image ran and completed successfully
- **Exit Code non-0** — image encountered an error; check logs above

---

## Re-run the Job

Kubernetes does not allow re-applying the same Job name. Delete and re-create it to run again:

```bash
kubectl delete job python-app-job -n python-app
kubectl apply -f job.yaml -n python-app
```

---

## Teardown

```bash
kubectl delete namespace python-app
```

This removes the namespace along with all resources inside it.