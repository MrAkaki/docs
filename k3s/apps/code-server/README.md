# Code Server in K8S
This is a basic deployment + ingress of the *code server*(vscode in the browser).

You should tune the files `01.pvc.yaml` and the `04.ingress.yaml` according to your implementation.

## Instalation

### Create the namespace
`kubectl apply -f 00.namespace.yaml`

### Create the secrets
```bash
kubectl -n code-server create secret generic \
    passwords \
    --from-literal=password='YOUR PASSWORD' \
    --from-literal=sudo_password='YOUR SUDO PASSWORD'
```

### Create the PVC
`kubectl apply -f 01.pvc.yaml`

### Create the Deployment
`kubectl apply -f 02.deployment.yaml`

### Create the Service
`kubectl apply -f 03.service.yaml`

### Create the Ingress
`kubectl apply -f 04.ingress.yaml`
