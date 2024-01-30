# Checkly Agent Kubernetes resources

This repo contains a Helm chart and a few example Kubernetes manifests that can be used to run the Checkly Agent for your
[Checkly Private Location](https://www.checklyhq.com/docs/private-locations) on Kubernetes.

Clone the repo to get started.

```bash
git clone https://github.com/checkly/checkly-k8s.git
cd checkly-k8
```

## Helm chart

Find the Helm chart in the `/helm-cart` directory. The Helm chart does two basic things:
- Creates a secret for the API key.
- Spins up two pods running the Checkly Agent


Assuming you have Helm set up to point at your K8S cluster, run it with the following command, making sure you 
**replace the `apikey="pl_..."` with your Checkly Private Location API key**.

```bash
helm install checkly-agent --set apiKeySecret.apiKey="pl_..."  ./helm-chart
```

### Alternative ways to set Agent API Key
Instead of setting `apiKeySecret.apiKey` you can also choose an existing secret with the following options 

```
apiKeySecret
    create: false
    name: <NAME_OF_EXISTING_SECRET>
```

or create the secret with `extraManifests`

```
apiKeySecret
    create: false
    name: <NAME_OF_SECRET_CREATED_BY_EXTRAMANIFEST>

extraManifests:
    - apiVersion: external-secrets.io/v1beta1
      kind: ExternalSecret
      metadata:
      name: checkly-agent-secret
      namespace: monitoring
      spec:
      target:
        name: my-checkly-secret-in-aws
```

## Kubernetes manifests

If you are not using Helm, you can also use these K8S manifest files to create your preferred cluster setup for the Checkly
Agent.

## [agentSecret.yaml](https://github.com/checkly/checkly-k8s/blob/main/agentSecret.yaml)

Creates a secret containing the API key your agents use to connect to the private location. Useful if you want to obfuscate 
the key so others can't see it with the `kubectl describe pod` command. The pod and deployment manifests are configured 
to use this secret.

## [agentPod.yaml](https://github.com/checkly/checkly-k8s/blob/main/agentPod.yaml)

Creates a single pod running the Checkly agent. Connects to the Private Location using the API key specified in 
[agentSecret.yaml](https://github.com/checkly/checkly-k8s/blob/main/agentSecret.yaml). Uses the 
[latest image](https://github.com/checkly/checkly-lambda-runners/pkgs/container/agent).

## [agentDeployment.yaml](https://github.com/checkly/checkly-k8s/blob/main/agentDeployment.yaml)

Create a deployment of Checkly agent pods (default: 2). Connects to the private location using the API key specified in 
[agentSecret.yaml](https://github.com/checkly/checkly-k8s/blob/main/agentSecret.yaml). Uses the 
[latest image](https://github.com/checkly/checkly-lambda-runners/pkgs/container/agent). Rolling updates are enabled.

## [checklyNamespace.yaml](https://github.com/checkly/checkly-k8s/blob/main/checklyNamespace.yaml)

Optional - Creates a namespace for the Checkly agent resources.


