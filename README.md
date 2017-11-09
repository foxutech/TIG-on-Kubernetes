### Setup Telegraf, InfluxDB and Grafana on Kubernetes

Please find the steps to setup TIG on Kubernetes

### InfluxDB

Start by deploying InfluxDB

```bash
# kubectl create -f influxdb-service.yml
# kubectl create -f influxdb-deployment.yml
```

Wait for the InfluxDB deployment to finish, then create a database for our metrics.

```bash
# POD_NAME=$(kubectl get pod -l app=influxdb -o "jsonpath={.items[0].metadata.name}")
# kubectl exec $POD_NAME -it -- influx
# create database production
# exit
```

### Grafana

```bash
# kubectl create -f grafana-service.yml
# kubectl create -f grafana-deployment.yml
```

### Example Flask app

Create a [ConfigMap](http://kubernetes.io/docs/user-guide/configmap/) for Telegraf to use.

```
# kubectl create configmap telegraf-config --from-file=config/telegraf.conf
# kubectl create -f daemonsets/telegraf.yaml
```

To get the status of the POD, use,
```
# kubectl get pods
```

Find the external IP addresses using `kubectl get svc`.

## Clean up

When you're done, you can destroy everything with the following.

```bash
# kubectl delete deployments/hello-flask
# kubectl delete deployments/grafana
# kubectl delete deployments/influxdb
# kubectl delete svc/hello-flask
# kubectl delete svc/grafana
# kubectl delete svc/influxdb
# kubectl delete configmap/telegraf-config
```
