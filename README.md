# Overview
This repository build a simple Hello container based on this [repository](https://github.com/GoogleCloudPlatform/cloud-run-hello) but without the HTML template.

The purpose is to return a simple string with all the important information. It's easier to handle with `curl`

The `hello.yaml` file allow to deploy the container on any Knative compliant cluster.

# Container registry

The container is publicly accessible: [gcr.io/cloudrun-hello-go/hello](gcr.io/cloudrun-hello-go/hello)

# Build

Build the container with the tool that you prefer

```
# Docker
docker build . -t <tag>

# Cloud Build
gcloud builds submit --tag <tag>
```

# Deployment

On K8S cluster, perform this command
```
kubectl apply -f hello.yaml
```

# Test

Perform the test of the Knative service on your cluster with this command
```
curl -H "Host: $(kubectl get route.serving.knative.dev hello-yaml \
   -o jsonpath='{.status.url}' | sed 's/http:\/\///g')" \
   $(kubectl get svc istio-ingressgateway --namespace istio-system \
   -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
```

*Be careful, ingress definition can change from cluster to another one. Try to change `ip` by `hostname`. This is the case for AWS EKS cluster*

# License

This library is licensed under Apache 2.0. Full license text is available in
[LICENSE](https://github.com/guillaumeblaquiere/cloudrun-hello-go/tree/master/LICENSE).