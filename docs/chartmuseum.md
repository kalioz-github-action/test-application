```bash
kubectl create ns chartmuseum
helm repo add chartmuseum https://chartmuseum.github.io/charts
helm upgrade --install -n chartmuseum --set env.open.DISABLE_API=false chartmuseum chartmuseum/chartmuseum
```