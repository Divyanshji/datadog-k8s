# datadog-k8s
Deploy a Datadog agent with OTLP configuration enabled in a Kubernetes cluster.

To deploy a Datadog agent with OTLP configuration enabled in a Kubernetes cluster, you can follow the steps outlined below. I'll provide a high-level overview of the process, and you can create a more detailed document based on this information.

**Preferred Approach:**

I recommend using a Kubernetes DaemonSet to deploy the Datadog agent. DaemonSets ensure that the Datadog agent runs on every node in the cluster, making it a suitable choice for monitoring purposes. Additionally, Datadog has official documentation and Kubernetes manifests for deploying their agent, which simplifies the process.

**Steps to Deploy Datadog Agent with OTLP Configuration:**

1. **Prerequisites:**

   - Access to the Kubernetes cluster (kubectl configured).
   - A Datadog account and API key.
   - Familiarity with Kubernetes manifests and configuration.

2. **Create a Kubernetes Secret for Datadog API Key:**

   Create a secret to securely store your Datadog API key:

   ```yaml
   kubectl create secret generic datadog-secret --from-literal api-key=<your-datadog-api-key>
   ```

3. **Create a Datadog Agent DaemonSet Manifest:**

   Create a Kubernetes manifest for the Datadog Agent DaemonSet. Here's a basic example:

   ```yaml
   apiVersion: apps/v1
   kind: DaemonSet
   metadata:
     name: datadog-agent
     labels:
       app: datadog-agent
   spec:
     selector:
       matchLabels:
         name: datadog-agent
     template:
       metadata:
         labels:
           name: datadog-agent
       spec:
         containers:
         - name: datadog-agent
           image: datadog/agent:latest
           env:
           - name: DD_API_KEY
             valueFrom:
               secretKeyRef:
                 name: datadog-secret
                 key: api-key
           - name: DD_OTLP_ENABLED
             value: "true"  # Enable OTLP
           # Add any additional Datadog configuration/environment variables as needed
           # ...
         # Add volume mounts, securityContext, and other configurations as needed
   ```

   Adjust the image tag, environment variables, and additional configuration according to your requirements.

4. **Apply the Manifest:**

   Apply the DaemonSet manifest to deploy the Datadog agent:

   ```bash
   kubectl apply -f datadog-agent.yaml
   ```

5. **Verify Deployment:**

   Check if the Datadog agent pods are running:

   ```bash
   kubectl get pods -l app=datadog-agent
   ```

6. **Access Datadog Dashboard:**

   Log in to your Datadog account to access the dashboard and verify that data from your Kubernetes cluster is being collected.

**Documentation:**

Create a comprehensive document that includes the following:

1. Detailed explanation of the chosen approach (DaemonSet).
2. Reasons behind the approach (scalability and ease of deployment).
3. Step-by-step guide for each of the above steps.
4. Any additional configurations or environment variables you used.
5. Sample manifest file (`datadog-agent.yaml`) with comments explaining each section.
6. Instructions for verifying the deployment and accessing Datadog's monitoring dashboard.

By following these steps and documenting the process, you should be able to successfully deploy a Datadog agent with OTLP configuration enabled in your Kubernetes cluster.
