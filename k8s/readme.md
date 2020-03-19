# Traefik - Jaeger operator

In this exmaple we will see how to install and configure the Jaeger Operator to scrape Traefik metrics.

First of all we will install a k3s cluster with the following command:
```bash
k3d create --server-arg "--no-deploy=traefik" \
--workers="2" \
--publish="80:80" \
--publish="443:443"
```

Once the k3s is up and running we will install the jaeger opeator:

## Install jaeger-operator

```bash
kubectl create namespace observability
kubectl create -f https://raw.githubusercontent.com/jaegertracing/jaeger-operator/v1.17.0/deploy/crds/jaegertracing.io_jaegers_crd.yaml
kubectl create -n observability -f https://raw.githubusercontent.com/jaegertracing/jaeger-operator/v1.17.0/deploy/service_account.yaml
kubectl create -n observability -f https://raw.githubusercontent.com/jaegertracing/jaeger-operator/v1.17.0/deploy/role.yaml
kubectl create -n observability -f https://raw.githubusercontent.com/jaegertracing/jaeger-operator/v1.17.0/deploy/role_binding.yaml
kubectl create -n observability -f https://raw.githubusercontent.com/jaegertracing/jaeger-operator/v1.17.0/deploy/operator.yaml

kubectl create -f https://raw.githubusercontent.com/jaegertracing/jaeger-operator/v1.17.0/deploy/cluster_role.yaml
kubectl create -f https://raw.githubusercontent.com/jaegertracing/jaeger-operator/v1.17.0/deploy/cluster_role_binding.yaml
```

## Deploy jaeger-all-in-one

```bash
kubectl apply -f - <<EOF
apiVersion: jaegertracing.io/v1
kind: Jaeger
metadata:
  name: simplest
  namespace: observability
EOF
```

## Deploy traefik

```
helm repo add traefik https://containous.github.io/traefik-helm-chart
helm repo update
helm install traefik traefik/traefik --values=values.yaml
```


## Deploy application

```
kubectl apply -f whoami.yaml
```

## Deploy ingresses

```
kubectl apply -f ingresses
```

http://dashboard.docker.localhost
http://jaeger.docker.localhost
http://whoami.docker.localhost
