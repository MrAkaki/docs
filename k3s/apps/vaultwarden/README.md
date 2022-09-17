# Vault Warden
VaultWarden is a password manager compatible with all [Bitwarden](https://bitwarden.com) clients

## Instalation

### Create namespace
`kubectl apply -f 00.namespace.yaml`

### Create the Deploy/StatefulSet
`kubectl apply -f 02.statefulset.yaml`

### Create the service
`kubectl apply -f 03.service.yaml`

### Create the ingress
`kubectl apply -f 04.ingress.yaml`
