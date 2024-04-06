
# Project Title: Kubernetes Nginx and Backend Apps Deployment

## Project Overview
This project demonstrates deploying an Nginx service as a reverse proxy alongside two backend Node.js applications within a Minikube Kubernetes environment. The Nginx service routes incoming traffic to either app-1 or app-2, based on the load balancing preferences


## Setup and Deployment
### Step 1: Start Minikube
```bash
minikube start
```
## Step 2: Deploy Nginx
Deploy the Nginx configuration, including the ConfigMap, Deployment, and Service.
```
kubectl apply -f nginx-dep.yaml
kubectl apply -f nginx-configmap.yaml
kubectl apply -f nginx-svc.yaml
```
## Step 3: Deploy Backend Applications
Deploy app-1 and app-2, including their respective Deployments and Services.
```
kubectl apply -f app-1-dep.yaml
kubectl apply -f app-1-svc.yaml
kubectl apply -f app-2-dep.yaml
kubectl apply -f app-2-svc.yaml
```
## Step 4: Configure Ingress
Apply the Ingress configurations to route external traffic to the Nginx service.
```bash      
kubectl apply -f nginx-ingress.yaml
```
## Step 5: Access the Application
Retrieve the Minikube IP and access the Nginx service externally.
You first enter the command
```bash
minikube ip
```
This is necessary because it provides the IP address of the Minikube virtual machine (VM) or environment where your Kubernetes cluster is running. And then after, to test if the loadbalancer is working, you enter this command
```bash
curl http://<your ip>
```
If succesful, you should get outputs from both app-1-dep, and app-2-dep, 
```
@ak033 ➜ /workspaces/ensf400-lab8-kubernetes-2 (main)
 $ curl http://192.168.49.2
Hello World from [app-1-dep-86f67f4f87-jhwgp]!
@ak033 ➜ /workspaces/ensf400-lab8-kubernetes-2 (main) $ curl http://192.168.49.2
Hello World from [app-2-dep-7f686c4d8d-8bzk6]!
@ak033 ➜ /workspaces/ensf400-lab8-kubernetes-2 (main) $ curl http://192.168.49.2
Hello World from [app-2-dep-7f686c4d8d-8bzk6]!
@ak033 ➜ /workspaces/ensf400-lab8-kubernetes-2 (main) $ curl http://192.168.49.2
Hello World from [app-1-dep-86f67f4f87-jhwgp]!
```
## Testing:

You can test it out using the previous command mentioned, to see the output of accesing the application from app-1-dep and app-2-dep

The observed behavior of switching back and forth between app-1-dep and app-2-dep is an expected outcome of the weighted load balancing setup you've implemented. It demonstrates that your configuration is working as intended, effectively distributing traffic between the two applications based on the specified weights.

When the request is made to the application endpoint (in this case, the NGINX service forwarding to the backend applications), the Node.js application processes the request and generates a response, in this case it was Hello world, and the pod Identifier(in this case): "[app-1-dep-86f67f4f87-jhwgp]" - This is the identifier of the pod that handled the request. It's appended to the message to provide additional context about which instance of the application processed the request.

## Troubleshooting
Common troubleshooting steps include :

Verifying pod status with kubectl get pods
Checking logs with kubectl logs <pod-name>
or you can also do 
```bash
 kubectl rollout restart deployment nginx-dep
```
if your  nginx dep pod isnt working
