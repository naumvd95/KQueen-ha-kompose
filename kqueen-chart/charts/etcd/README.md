# Etcd

## Deploy chart

```console
$ git pull https://github.com/naumvd95/KQueen-ha-kompose.git
$ kubectl label nodes vnaumov-test app=my-etcd
$ helm install kqueen-chart/etcd --name my-etcd
```
