# Scenario Beginner‑01 — Service can't reach the Pods (port mismatch)

**Difficulty:** Beginner  
**Focus Areas:** Service `targetPort`, container `ports`, Endpoints, basic curl test

## Story
A simple web app was deployed in namespace `demo`. The Service exists, but users can't reach it. Your task is to make the Service route traffic to the Pods.

## What you get
- A Deployment `web` (nginx) exposing HTTP on **port 80**
- A ClusterIP Service `web-svc` configured to send traffic to **port 8080** (bug)
- Result: the Service shows **0 endpoints** and curl times out

## Your tasks
1) Fix the Service so it targets the actual port exposed by the container.
2) Prove it's working with a quick request from inside the cluster.

## Acceptance criteria
- `kubectl -n demo get endpoints web-svc` shows **at least 1 IP/port**.
- From a debug pod in `demo` namespace:
  ```bash
  kubectl -n demo run tmp --image=busybox:1.36 --restart=Never -it -- wget -qO- http://web-svc
  ```
  returns the nginx welcome HTML (HTTP 200).

## Initialize (broken state)
```bash
kubectl apply -f broken/namespace.yaml
kubectl apply -f broken/deployment.yaml
kubectl apply -f broken/service.yaml
```

## Inspect
```bash
kubectl -n demo get pods -o wide
kubectl -n demo get svc web-svc -o yaml
kubectl -n demo get endpoints web-svc -o yaml
kubectl -n demo describe deploy web
```

## Hint (optional)
- Check the container `containerPort` vs the Service `targetPort`. They must align (name or number).

## Apply the fix
Use files in `solution/` or patch the Service manually.

## Cleanup
```bash
kubectl delete ns demo
```
