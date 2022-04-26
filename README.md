# Checkly Agent Kubernetes Manifests

This repo contains a few example Kubernetes manifests that can be used to run the Checkly agent in order to enable private locations in your environment.

## [checklyNamespace.yaml](https://github.com/checkly/checkly-k8s/blob/main/checklyNamespace.yaml)

Optional - Create a namespace for the Checkly agent resources.

## [agentSecret.yaml](https://github.com/checkly/checkly-k8s/blob/main/agentSecret.yaml)

Create a secret containing the API key your agents use to connect to the private location. Useful if you want to obfuscate the key so others can't see it with the `kubectl describe pod` command. The pod and deployment manifests are configured to use this secret.

## [agentPod.yaml](https://github.com/checkly/checkly-k8s/blob/main/agentPod.yaml)

Create a single pod running the Checkly agent. Connects to the private location using the API key specified in [agentSecret.yaml](https://github.com/checkly/checkly-k8s/blob/main/agentSecret.yaml). Uses the [latest image](https://github.com/checkly/checkly-lambda-runners/pkgs/container/agent).

## [agentDeployment.yaml](https://github.com/checkly/checkly-k8s/blob/main/agentDeployment.yaml)

Create a deployment of Checkly agent pods (default: 2). Connects to the private location using the API key specified in [agentSecret.yaml](https://github.com/checkly/checkly-k8s/blob/main/agentSecret.yaml). Uses the [latest image](https://github.com/checkly/checkly-lambda-runners/pkgs/container/agent). Rolling updates are enabled.