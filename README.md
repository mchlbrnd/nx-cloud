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
```minikube start --addons volumesnapshots --addons csi-hostpath-driver --container-runtime=docker --driver=hyperv --cpus=4 --delete-on-failure --disk-size=75G --hyperv-virtual-switch="vEthernet (external)" --install-addons=true --memory=10G --nodes=2 --vm=true -p nx-cloud-cluster```

## Add certificate secret for if you require ingress.tls
```kubectl create secret tls nx-cloud-cert --namespace default --key=./certs/certificate.key --cert=./certs/certificate.crt```

## Tweak and release to on your cluster
```helm install my-release . -f ./examples/values.yaml```

**NOTE:** see _values.yaml_  for ingriss, metallb, mongodb and nx-cloud value configuration, or lookup in respective repositories.

## Restore
mongorestore --authenticationMechanism="SCRAM-SHA-256" mongodb://root:Password123@localhost:27018/ --archive=mongodb/archives/nrwl-api.atlas.vcs.archive  --preserveUUID --drop --keepIndexVersion --noIndexRestore --maintainInsertionOrder --db=nrwl-api --authenticationDatabase=admin