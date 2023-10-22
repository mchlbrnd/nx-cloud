# @werockit/nx-cloud

## Helm umbrella char for nx-cloud

## Prerequisites
### Required
- [helm](https://helm.sh/)
### Flexible
This chart is developed and tested against below tools. You can use your own platform tools and change commands in release steps accordingly.
- [minikube](https://minikube.sigs.k8s.io/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- [Hyper-V](https://learn.microsoft.com/en-us/virtualization/hyper-v-on-windows/about/)
- Docker Container runtime

## Release steps
1. Add external network switch in Hyper-V Manager so cluster nodes get adresses from your network (optional, see --hyperv-virtual-switch flag)
2. Create fresh cluster:
```minikube start --addons volumesnapshots --addons csi-hostpath-driver --container-runtime=docker --driver=hyperv --cpus=4 --delete-on-failure --disk-size=50G --hyperv-virtual-switch="vEthernet (external)" --install-addons=true --memory=6G --nodes=3 --vm=true -p nx-cloud-cluster```
3. Add certificate secret (optional, see values.ingress.tls)
```kubectl create secret tls nx.example.com-cert --namespace default --key=./certs/certificate.key --cert=./certs/certificate.crt```
4. Modify values.yaml to your liking (optional, see _values.yaml_  for ingriss, metallb, mongodb and nx-cloud value configuration, or lookup in respective repositories)
5. Release umbrella chart cluster
```helm install my-release . -f ./examples/values.yaml```
6. Restore external nrwl-api database to new cluster (optional)
   1. ```kubectl port-forward service/werockit-mongodb-headless 27018:27017```
   2. ```mongodump mongodb+srv://***:***@cluster0.24vsjsx.mongodb.net  --db=nrwl-api --authenticationDatabase=admin --archive | mongorestore mongodb://root:Password123@localhost:27018 --preserveUUID --drop --keepIndexVersion --noIndexRestore --maintainInsertionOrder --nsInclude=nrwl-api.* --authenticationDatabase=admin --archive```