# How to get Helm Working on Docker EE 2.0

## 1)

Download the helm binary https://kubernetes-helm.storage.googleapis.com/helm-v2.7.2-linux-amd64.tar.gz

Extract this, chmod +x it, and move it into your PATH.

## 2)

Download a UCP client bundle or set the $KUBECONFIG= variable to your UCP 3 cluster.

You will need to make an addition here. The default KubeConfig for UCP3 is pointing to the UCP controller (:443), this will need to be changd to the KUBE API Server (:6443). 

## 3)

Create a new Service Account as UCP does not give any rights to the default service accounts today (This may change).

```
cat > /tmp/serviceaccount.yaml <<EOF
apiVersion: v1
kind: ServiceAccount
metadata:
  name: tiller
EOF
kubectl create -f /tmp/serviceaccount.yaml --namespace kube-system
```

## 4) 

Install the Tiller Service

```
helm init --debug --service-account tiller
```

## 5)

Verifiy installation with:

```
$ kubectl get pods -n kube-system | grep tiller
tiller-deploy-546cf9696c-68tjs             1/1       Running   0          47m

$ helm version
Client: &version.Version{SemVer:"v2.7.2", GitCommit:"8478fb4fc723885b155c924d1c8c410b7a9444e6", GitTreeState:"clean"}
Server: &version.Version{SemVer:"v2.7.2", GitCommit:"8478fb4fc723885b155c924d1c8c410b7a9444e6", GitTreeState:"clean"}
```




