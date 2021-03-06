# Cilium install
```
$ helm repo add cilium https://helm.cilium.io/
$ helm upgrade cilium cilium/cilium --version 1.11.2 --namespace kube-system --set monitor.enabled=true --set hubble.metrics.enabled="{dns,drop,tcp,flow,icmp,http}" --set operator.prometheus.enabled=true --set hubble.metrics.serviceMonitor.enabled=true --set operator.prometheus.serviceMonitor.enabled=true --set prometheus.enabled=true --set prometheus.serviceMonitor.enabled=true
```

# Flux install
- export GitHub access token and username
```
export GITHUB_TOKEN=<your-token>
export GITHUB_USER=<your-username>
```

- install Flux CLI
`brew install fluxcd/tap/flux`

- flux zsh completions
`. <(flux completion zsh)`

- preflight check
`flux check --pre`

- bootstrap
```
flux bootstrap github --owner=$GITHUB_USER --repository=mycluster --branch=main --path=./mycluster --personal
```

- Clone repository
`git clone  https://github.com/$GITHUB_USER/mycluster`

## Cert-manager Install
- flux create source helm jetstack --url https://charts.jetstack.io --export ./mycluster/flux-system/helm-repositories/jetstack.yaml

- flux create HelmRelease cert-manager --chart cert-manager --source HelmRepository/jetstack --target-namespace cert-manager --export ./mycluster/cert-manager/helm-release.yaml

- kubectl create ns cert-manager --dry-run=client -o yaml > ./mycluster/cert-manager/namespace.yaml

- create kustomization.yaml
