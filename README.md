# Kubernetes Practical Assignment (50 Marks)

This repository contains the necessary configuration files and step-by-step instructions to execute the 10 required Kubernetes tasks. 

**Note for Submission:** For each step below, you should run the command in your terminal and take a screenshot of the command and its output to include in your final submission document.

## Files Provided in this Repository
- `pod.yaml`: Declarative configuration for an Nginx Pod.
- `deployment.yaml`: Declarative configuration for an Nginx Deployment.
- `service.yaml`: Declarative configuration to expose the Deployment as a NodePort Service.

---

## Step-by-Step Instructions

### 1. Start Minikube / cluster (5 marks)
Since you are using Docker Desktop, you can enable its built-in Kubernetes cluster:
1. Open Docker Desktop.
2. Go to **Settings** (the gear icon) > **Kubernetes**.
3. Check the box for **"Enable Kubernetes"** and click **Apply & restart**.
*Output to capture:* Take a screenshot of the Docker Desktop UI showing the green "Kubernetes is running" icon at the bottom left corner.

### 2. Check cluster status (5 marks)
To verify that your Docker Desktop Kubernetes cluster is up and running correctly, open your terminal (Command Prompt or PowerShell) and run:
```bash
kubectl cluster-info
```
*Output to capture:* Information showing that the Kubernetes control plane is running. You can also run `kubectl get nodes` to show that the `docker-desktop` node is "Ready".

### 3. Create a Pod (nginx) (5 marks)
We will use the provided `pod.yaml` file to create the Nginx pod declaratively.
```bash
kubectl apply -f pod.yaml
```
*(Alternative imperative command if you prefer not using the file: `kubectl run nginx-pod --image=nginx`)*
*Output to capture:* `pod/nginx-pod created`

### 4. List Pods (5 marks)
To see the list of all running pods in the default namespace:
```bash
kubectl get pods
```
*Output to capture:* A table showing `nginx-pod` with its STATUS as `Running`. (You may need to wait a few seconds and run it again if it says ContainerCreating).

### 5. Describe Pod (5 marks)
To view detailed configuration and status events for the pod you just created:
```bash
kubectl describe pod nginx-pod
```
*Output to capture:* The detailed output including labels, IP, containers, and the "Events" table at the bottom showing how the image was pulled and started.

### 6. View Pod logs (5 marks)
To check the internal logs of the Nginx container running inside the pod:
```bash
kubectl logs nginx-pod
```
*Output to capture:* The startup logs of the Nginx server (usually showing worker processes starting).

### 7. Create Deployment (5 marks)
Now we will create a Deployment, which manages replicas of Pods. We will use the provided `deployment.yaml` file:
```bash
kubectl apply -f deployment.yaml
```
*(Alternative imperative command: `kubectl create deployment nginx-deployment --image=nginx`)*
*Output to capture:* `deployment.apps/nginx-deployment created`

You can verify it by running `kubectl get deployments`.

### 8. Scale Deployment (5 marks)
To scale the deployment up to 3 replicas (3 running instances of Nginx):
```bash
kubectl scale deployment nginx-deployment --replicas=3
```
*Output to capture:* `deployment.apps/nginx-deployment scaled`. 
*Bonus output to capture:* Run `kubectl get pods` to show that there are now 3 pods running for the deployment.

### 9. Expose Deployment as Service (5 marks)
To expose the deployment so it can be accessed over the network, we will create a NodePort service using the `service.yaml` file:
```bash
kubectl apply -f service.yaml
```
*(Alternative imperative command: `kubectl expose deployment nginx-deployment --type=NodePort --port=80`)*
*Output to capture:* `service/nginx-service created`.
*Bonus output to capture:* Run `kubectl get svc` to show the service and its mapped NodePort.

### 10. Delete resources (5 marks)
To clean up and delete all the resources (Pods, Deployments, Services) we just created:
```bash
kubectl delete -f pod.yaml
kubectl delete -f deployment.yaml
kubectl delete -f service.yaml
```
*(Alternative to delete everything in the default namespace at once: `kubectl delete all --all`)*
*Output to capture:* The confirmation messages that the pod, deployment, and service have been deleted.
