# kubevirt-helm

Spins up vms inside K8s using [kubevirt](https://kubevirt.io)

## Pre-req
- virtctl
- metallb - if you want to access the vm from outside

## How to
Look at `values.yaml` file.
The deployment relies on a golden image you have created from a ISO or cow image. [Example here](https://kubevirt.io/2020/KubeVirt-installing_Microsoft_Windows_from_an_iso.html)

At the end of the helm install, commands will be printed that you need to execute.
```bash
Execute the following commands:
=====

export SERVICE_IP=$(kubectl get svc --namespace default mirror-vmspin-lb --template "{{ range (index .status.loadBalancer.ingress 0) }}{{.}}{{ end }}")
export PV=$(kubectl get pvc --namespace default mirror-vmspin-pvc --template "{{ .spec.volumeName }}")
export DESTINATION=$(kubectl get pv --namespace default $PV --template "{{ .spec.hostPath.path }}")
sudo cp ~/kubevirt/disk.img $DESTINATION
virtctl start mirror-vmspin -n default
echo ssh $SERVICE_IP

=====
```

Once the vm has booted, you can edit the vm object and set `running: true`.

```
kubectl edit vm/vm-name
```
