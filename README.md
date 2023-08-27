
https://docs.k3s.io/datastore/ha-embedded

To get started, first launch a server node with the cluster-init flag to enable clustering and a token that will be used as a shared secret to join additional servers to the cluster.

```
curl -sfL https://get.k3s.io | K3S_TOKEN=SECRET sh -s - server \
    --cluster-init \
    --tls-san 10.20.5.100 \
    --tls-san kube.skytest.local \
    --disable servicelb \
    --disable traefik
```

On the other nodes, join the cluster using the same token and the IP address of the server node.

```
curl -sfL https://get.k3s.io | K3S_TOKEN=SECRET sh -s - server \
    --server https://10.20.5.101:6443 \
    --tls-san 10.20.5.100 \
    --tls-san kube.skytest.local \
    --disable servicelb \
    --disable traefik
```


## 2. Install argocd
https://argo-cd.readthedocs.io/en/stable/

```
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```
forward the argocd-server service to your local machine
```
kubectl port-forward svc/argocd-server -n argocd 8080:443
```
get the admin password
```
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```
login https://localhost:8080/ with admin and the password

## 3. argocd cli
https://argo-cd.readthedocs.io/en/stable/cli_installation/
locally
```
brew install argocd
```
another way to get the admin password
```
argocd admin initial-password -n argocd
```
change the password
```
argocd account update-password
```




