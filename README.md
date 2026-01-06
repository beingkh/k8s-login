# k8s-signup

Lightweight Kubernetes manifests for the k8s-login demo: namespace, storage class, backend, frontend, MySQL and networking resources.

## Quick start

1. Create the namespace and storage class:
```sh
kubectl apply -f Cluster/namespace.yaml
kubectl apply -f Cluster/storage-class.yaml
```

2. Deploy MySQL:
```sh
kubectl apply -f mysql/mysql-secret.yaml
kubectl apply -f mysql/mysql-pvc.yaml
kubectl apply -f mysql/mysql-statefulset.yaml
kubectl apply -f mysql/mysql-service.yaml
```

3. Deploy backend and frontend:
```sh
kubectl apply -f backend/backend-configmap.yaml
kubectl apply -f backend/backend-sa.yaml
kubectl apply -f backend/backend-deployment.yaml
kubectl apply -f backend/backend-service.yaml
kubectl apply -f backend/backend-hpa.yaml

kubectl apply -f frontend/frontend-configmap.yaml
kubectl apply -f frontend/frontend-deployment.yaml
kubectl apply -f frontend/frontend-service.yaml
kubectl apply -f frontend/frontend-hpa.yaml
```

4. Apply networking (Ingress / NetworkPolicies):
```sh
kubectl apply -f Networking/network-policies.yaml
kubectl apply -f Networking/ingress.yaml
```

## File map (what to edit)

- Backend
  - [`k8s-login.backend.backend-configmap`](backend/backend-configmap.yaml) — [backend/backend-configmap.yaml](backend/backend-configmap.yaml) : config for backend app
  - [`k8s-login.backend.backend-deployment`](backend/backend-deployment.yaml) — [backend/backend-deployment.yaml](backend/backend-deployment.yaml) : Deployment manifest
  - [`k8s-login.backend.backend-hpa`](backend/backend-hpa.yaml) — [backend/backend-hpa.yaml](backend/backend-hpa.yaml) : HorizontalPodAutoscaler
  - [`k8s-login.backend.backend-sa`](backend/backend-sa.yaml) — [backend/backend-sa.yaml](backend/backend-sa.yaml) : ServiceAccount
  - [`k8s-login.backend.backend-service`](backend/backend-service.yaml) — [backend/backend-service.yaml](backend/backend-service.yaml) : ClusterIP / Service

- Cluster
  - [`k8s-login.Cluster.namespace`](Cluster/namespace.yaml) — [Cluster/namespace.yaml](Cluster/namespace.yaml) : Namespace and annotations
  - [`k8s-login.Cluster.storage-class`](Cluster/storage-class.yaml) — [Cluster/storage-class.yaml](Cluster/storage-class.yaml) : StorageClass used by MySQL PVC

- Frontend
  - [`k8s-login.frontend.frontend-configmap`](frontend/frontend-configmap.yaml) — [frontend/frontend-configmap.yaml](frontend/frontend-configmap.yaml) : frontend config / env
  - [`k8s-login.frontend.frontend-deployment`](frontend/frontend-deployment.yaml) — [frontend/frontend-deployment.yaml](frontend/frontend-deployment.yaml) : Deployment manifest
  - [`k8s-login.frontend.frontend-hpa`](frontend/frontend-hpa.yaml) — [frontend/frontend-hpa.yaml](frontend/frontend-hpa.yaml) : Autoscaler
  - [`k8s-login.frontend.frontend-service`](frontend/frontend-service.yaml) — [frontend/frontend-service.yaml](frontend/frontend-service.yaml) : Service (ClusterIP / LoadBalancer)

- MySQL
  - [`k8s-login.mysql.mysql-secret`](mysql/mysql-secret.yaml) — [mysql/mysql-secret.yaml](mysql/mysql-secret.yaml) : DB credentials
  - [`k8s-login.mysql.mysql-pvc`](mysql/mysql-pvc.yaml) — [mysql/mysql-pvc.yaml](mysql/mysql-pvc.yaml) : PersistentVolumeClaim
  - [`k8s-login.mysql.mysql-statefulset`](mysql/mysql-statefulset.yaml) — [mysql/mysql-statefulset.yaml](mysql/mysql-statefulset.yaml) : StatefulSet for MySQL
  - [`k8s-login.mysql.mysql-service`](mysql/mysql-service.yaml) — [mysql/mysql-service.yaml](mysql/mysql-service.yaml) : Headless / service

- Networking
  - [`k8s-login.Networking.ingress`](Networking/ingress.yaml) — [Networking/ingress.yaml](Networking/ingress.yaml) : Ingress rules
  - [`k8s-login.Networking.network-policies`](Networking/network-policies.yaml) — [Networking/network-policies.yaml](Networking/network-policies.yaml) : NetworkPolicy definitions

## Recommended workflow to update on GitHub

1. Create a branch:
```sh
git checkout -b docs/update-readme
```
2. Edit files (e.g., change replica count in `backend/backend-deployment.yaml` or update ingress host in `Networking/ingress.yaml`).
3. Commit and push:
```sh
git add <files>
git commit -m "docs: add README and usage"
git push origin docs/update-readme
```
4. Open a Pull Request on GitHub and request review.

## Useful commands

- Check resources in namespace:
```sh
kubectl get all -n <namespace>
```
- Tail logs:
```sh
kubectl logs -f deploy/<deployment-name> -n <namespace>
```
- Port-forward frontend locally:
```sh
kubectl port-forward svc/<frontend-service-name> 8080:80 -n <namespace>
```

## Notes / tips
- Ensure `Cluster/namespace.yaml` namespace matches `metadata.namespace` in manifests.
- Update `mysql/mysql-secret.yaml` with secure credentials before production.
- Tweak `frontend` / `backend` resource requests and HPAs for cluster size.
