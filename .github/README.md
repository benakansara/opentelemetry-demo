<!-- markdownlint-disable-next-line -->
# <img src="https://opentelemetry.io/img/logos/opentelemetry-logo-nav.png" alt="OTel logo" width="32"> :heavy_plus_sign: <img src="https://images.contentstack.io/v3/assets/bltefdd0b53724fa2ce/blt601c406b0b5af740/620577381692951393fdf8d6/elastic-logo-cluster.svg" alt="OTel logo" width="32"> OpenTelemetry Demo with Elastic Observability

The following guide describes how to setup the OpenTelemetry demo with Elastic Observability using [Docker compose](#docker-compose) or [Kubernetes](#kubernetes). This fork introduces several changes to the agents used in the demo:

- The Java agent within the [Ad](../src/adservice/Dockerfile.elastic), the [Fraud Detection](../src/frauddetectionservice/Dockerfile.elastic) and the [Kafka](../src/kafka/Dockerfile.elastic) services have been replaced with the Elastic distribution of the OpenTelemetry Java Agent. You can find more information about the Elastic distribution in [this blog post](https://www.elastic.co/observability-labs/blog/elastic-distribution-opentelemetry-java-agent).
- The .NET agent within the [Cart service](../src/cartservice/src/Directory.Build.props) has been replaced with the Elastic distribution of the OpenTelemetry .NET Agent. You can find more information about the Elastic distribution in [this blog post](https://www.elastic.co/observability-labs/blog/elastic-opentelemetry-distribution-dotnet-applications).
- The Elastic distribution of the OpenTelemetry Node.js Agent has replaced the OpenTelemetry Node.js agent in the [Payment service](../src/paymentservice/package.json). Additional details about the Elastic distribution are available in [this blog post](https://www.elastic.co/observability-labs/blog/elastic-opentelemetry-distribution-node-js).
- The Elastic distribution for OpenTelemetry Python has replaced the OpenTelemetry Python agent in the [Recommendation service](..src/recommendationservice/requirements.txt). Additional details about the Elastic distribution are available in [this blog post](https://www.elastic.co/observability-labs/blog/elastic-opentelemetry-distribution-python).

Additionally, the OpenTelemetry Contrib collector has also been changed to the [Elastic OpenTelemetry Collector distribution](https://github.com/elastic/elastic-agent/blob/main/internal/pkg/otel/README.md). This ensures a more integrated and optimized experience with Elastic Observability.


## Testathon APM (Kubernetes)

### Prerequisites:
- Create a Kubernetes cluster. There are no specific requirements, so you can create a local one, or use a managed Kubernetes cluster, such as [GKE](https://cloud.google.com/kubernetes-engine), [EKS](https://aws.amazon.com/eks/), or [AKS](https://azure.microsoft.com/en-us/products/kubernetes-service).
- Set up [kubectl](https://kubernetes.io/docs/reference/kubectl/).
- Set up [Helm](https://helm.sh/).

### Start the Demo
1. Execute the following commands to deploy the OpenTelemetry demo to your Kubernetes cluster:
   ```
   # clone this repository
   git clone https://github.com/elastic/opentelemetry-demo

   # switch to the kubernetes/elastic-helm directory
   cd kubernetes/elastic-helm

   # !(when running it for the first time) add the open-telemetry Helm repostiroy
   helm repo add open-telemetry https://open-telemetry.github.io/opentelemetry-helm-charts

   # !(when an older helm open-telemetry repo exists) update the open-telemetry helm repo
   helm repo update open-telemetry

   # deploy the demo through helm install
   helm install -f deployment.yaml my-otel-demo open-telemetry/opentelemetry-demo
   ```
2. Generate some traffic:
   ```
   # Locate a Pod named "my-otel-demo-frontendproxy-<RANDOM_SUFFIX>"
   kubectl get pods

   # Forward the fronend proxy port
   k port-forward my-otel-demo-frontendproxy-<RANDOM_SUFFIX_CHANGE_ME> 8080:8080

   # Visit localhost:8080 in your browser
   ```
