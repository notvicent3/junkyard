# GPT Researcher Deployment on Kubernetes

🚀 Welcome to the guide on how to deploy GPT Researcher using Kubernetes for scalability and efficient resource management.

## Prerequisites

Before you begin, make sure you have the following:

- A Kubernetes cluster up and running.
- Docker Compose YAML file for GPT Researcher in your repository.
- Your OpenAI API Key.

## Step 1: Create Kubernetes Deployment YAML

Create a Kubernetes Deployment YAML file, e.g., `gpt-researcher-deployment.yaml`, to define how your application should be deployed:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: gpt-researcher-deployment
spec:
  replicas: 3 # Adjust the number of replicas as needed
  selector:
    matchLabels:
      app: gpt-researcher
  template:
    metadata:
      labels:
        app: gpt-researcher
    spec:
      containers:
      - name: gpt-researcher
        image: assafelovic/gpt-researcher
        ports:
        - containerPort: 8000
        env:
        - name: OPENAI_API_KEY
          value: ${OPENAI_API_KEY}
```

This YAML file deploys your GPT Researcher application as a Kubernetes Deployment with three replicas. It sets up the container, exposes port 8000, and injects the `OPENAI_API_KEY` environment variable.

## Step 2: Create Kubernetes Service YAML

Create a Kubernetes Service YAML file, e.g., `gpt-researcher-service.yaml`, to expose your application within the cluster:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: gpt-researcher-service
spec:
  selector:
    app: gpt-researcher
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8000
  type: LoadBalancer # Use 'ClusterIP' if you don't need external access
```

This YAML file creates a Kubernetes Service named `gpt-researcher-service` that exposes your application's port 8000. It is set to `LoadBalancer` type to enable external access. You can use `ClusterIP` if external access is not required.

## Step 3: Apply Kubernetes Manifests

Apply the Kubernetes Deployment and Service manifests using `kubectl`:

```bash
kubectl apply -f gpt-researcher-deployment.yaml
kubectl apply -f gpt-researcher-service.yaml
```

This will create the deployment and service resources in your Kubernetes cluster.

## Step 4: Scaling

You can scale your deployment up or down by adjusting the `replicas` field in the Deployment YAML. For example, to scale to five replicas:

```yaml
spec:
  replicas: 5
```

## Step 5: Access Your Application

If you used a LoadBalancer type service, it will allocate an external IP address (in a cloud-based Kubernetes cluster) that you can use to access your application. If you used ClusterIP, you can access the application within the cluster.

🎉 Congratulations! Your GPT Researcher application is now running on Kubernetes, ready to empower your research team with AI-driven insights.

Ensure that you monitor your application, set resource limits, and configure any additional Kubernetes features or policies as needed for your specific use case.

Happy researching! 📚🔍
