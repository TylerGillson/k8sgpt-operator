<img src="./images/banner-white.png" width="600px;" />

This Operator is designed to enable K8sGPT within a Kubernetes cluster.
It will allow you to create a custom resource that defines the behaviour and scope of a managed K8sGPT workload. Analysis and outputs will also be configurable to enable integration into existing workflows.


## Installation

```
helm repo add k8sgpt https://charts.k8sgpt.ai/
helm install release k8sgpt/k8sgpt-operator
```

## Run the example

<img src="images/demo1.gif" width="600px;"/>

1. Install the operator from the [Installation](#installation) section.

2. Create secret:
```sh 
kubectl create secret generic k8sgpt-sample-secret --from-literal=openai-api-key=$OPENAI_TOKEN -n default
```

3. Apply the K8sGPT configuration object:
```sh
kubectl apply -f - << EOF
apiVersion: core.k8sgpt.ai/v1alpha1
kind: K8sGPT
metadata:
  name: k8sgpt-sample
spec:
  namespace: default
  model: gpt-3.5-turbo
  backend: openai
  noCache: false
  version: v0.2.7
  enableAI: true
  secret:
    name: k8sgpt-sample-secret
    key: openai-api-key
EOF
```

4. Once the custom resource has been applied the K8sGPT-deployment will be installed and
you will be able to see the Results objects of the analysis after some minutes ( if there are any issues):

```bash
❯ kubectl get results -o json | jq .
{
  "apiVersion": "v1",
  "items": [
    {
      "apiVersion": "core.k8sgpt.ai/v1alpha1",
      "kind": "Result",
      "metadata": {
        "creationTimestamp": "2023-04-26T09:45:02Z",
        "generation": 1,
        "name": "placementoperatorsystemplacementoperatorcontrollermanagermetricsservice",
        "namespace": "default",
        "resourceVersion": "108371",
        "uid": "f0edd4de-92b6-4de2-ac86-5bb2b2da9736"
      },
      "spec": {
        "details": "The error message means that the service in Kubernetes doesn't have any associated endpoints, which should have been labeled with \"control-plane=controller-manager\". \n\nTo solve this issue, you need to add the \"control-plane=controller-manager\" label to the endpoint that matches the service. Once the endpoint is labeled correctly, Kubernetes can associate it with the service, and the error should be resolved.",
```

## Architecture

<img src="images/1.png" width="600px;" />

## Contributing

Please see our community [contributing guidelines](https://github.com/k8sgpt-ai/community) for more information on how to get involved.

