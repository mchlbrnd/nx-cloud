# @werockit/nx-cloud

## Helm umbrella char for nx-cloud

## !! *Super WIP* !!!

## Prerequisite
- [minikube](https://minikube.sigs.k8s.io/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- [helm](https://helm.sh/)
- [Hyper-V](https://learn.microsoft.com/en-us/virtualization/hyper-v-on-windows/about/)
- Docker runtime

## Start cluster
```minikube start --addons volumesnapshots --addons csi-hostpath-driver --container-runtime=docker --driver=hyperv --cpus=4 --delete-on-failure --disk-size=30G --hyperv-virtual-switch="vEthernet (external)" --install-addons=true --memory=8G --nodes=2 --vm=true -p nx-cloud-cluster```

## Add certificate secret for ingriss.tls
```kubectl create secret tls nx-cloud-cert --namespace default --key=./certs/certificate.key --cert=./certs/certificate.crt```

## Tweak and release to on your cluster
```helm install werockit . -f ./examples/values.yaml```

**NOTE:** see _values.yaml_  for ingriss, metallb, mongodb and nx-cloud value configuration, or lookup in respective repositories.