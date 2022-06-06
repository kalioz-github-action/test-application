# Create a local environment suitable for github-actions

## step 1 : create a kubernetes cluster

```bash
minikube start
```

## step 2 : install the github-action CRD

To install the CRD, you need to create a github app or a PAT on your github repo / account / organization / enterprise.
The steps are documented on the ARC (Actions-Runner-Controller) repo.

```bash
# create the namespace
kubectl create ns actions-runner-system

# Create the authentification secret
kubectl create secret generic controller-manager \
    -n actions-runner-system \
    --from-literal=github_app_id=${APP_ID} \
    --from-literal=github_app_installation_id=${INSTALLATION_ID} \
    --from-file=github_app_private_key=${PRIVATE_KEY_FILE_PATH}

# Install cert-manager, required by ARC
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.8.0/cert-manager.yaml

# Install actions-runner-controller
helm repo add actions-runner-controller https://actions-runner-controller.github.io/actions-runner-controller
helm upgrade --install --namespace actions-runner-system --create-namespace \
             --wait actions-runner-controller actions-runner-controller/actions-runner-controller
```

## step 3 : create a runner

define a runner, and start it :
```bash
kubectl create ns github-action-runners
kubectl apply -f runners/
```

You should see it appear in your organization : https://github.com/organizations/kalioz-github-action/settings/actions/runners
