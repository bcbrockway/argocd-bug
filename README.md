wget https://github.com/argoproj/argo-cd/raw/v2.7.1/manifests/install.yaml
Modified install.yaml (added --insecure to server args)


## Set up cluster
k3d cluster create argocd-bug --agents 2

## Set up ArgoCD
```shell
kubectl create ns argocd
kubectl apply -f install.yaml -n argocd
sleep 10
kubectl get secret -n argocd argocd-initial-admin-secret -ojson | jq -r '.data.password' | base64 -d
kubectl port-forward -n argocd $(kubectl get pods -n argocd -l app.kubernetes.io/name=argocd-server -oname) 8080
```

## Set up example app
kubectl create ns bugtest
kubectl apply -f argoproj.io-v1alpha1.Application-argocd-bugtest.yaml -n argocd
