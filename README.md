# simple-supply-chain

### Cluster Setup

```bash
## Create Kind cluster 
kind create cluster --name sc-test  

## Install cert manager. Refer https://cert-manager.io/docs/installation/
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.11.0/cert-manager.yaml

## Install Cartographer from release https://github.com/vmware-tanzu/cartographer/releases
kubectl apply -f https://github.com/vmware-tanzu/cartographer/releases/download/v0.7.2/cartographer.yaml

## Add tekton from https://tekton.dev/docs/installation/pipelines/
kubectl apply --filename https://storage.googleapis.com/tekton-releases/pipeline/latest/release.yaml

## Install kapck by taking the latest release yaml file from https://github.com/pivotal/kpack/releases
 kubectl apply -f https://github.com/pivotal/kpack/releases/download/v0.9.4/release-0.9.4.yaml

 ## Install flux as per https://fluxcd.io/flux/installation/
 kubectl apply -f https://github.com/fluxcd/flux2/releases/latest/download/install.yaml
```

## Apply rbac, templates and supply chain created
```
cd siple-supply-chain
kubectl apply -f rbac
kubectl apply -f template  
kubectl apply -f supplychain
kubectl apply -f kpack
```
## Create a secret with push credentials for the docker registry that you plan on publishing OCI images to with kpack.
```bash
kubectl create secret docker-registry docker-credentials \
    --docker-username=user \
    --docker-password=password \
    --docker-server=string \
    --namespace default
```
follow the details mentioned here https://github.com/pivotal/kpack/blob/main/docs/tutorial.md 

## Create workload using simple-supply-chain
```bash
tanzu apps workload apply petclinic --image springcommunity/spring-framework-petclinic --label workload-type="pre-built" --tail -y
```

## Create workload using fancy-supply-chain
```bash
tanzu apps workload apply tanzu-java-web-app \
--git-repo https://github.com/sample-accelerators/tanzu-java-web-app \
--git-branch main \
--label workload-type="from-git" \
--tail \
--yes
```
## Delete the workload 
```bash
tanzu apps workload delete tanzu-java-web-app --yes
```

### Supply chain

