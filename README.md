## Prerequisites
Make sure you have the following installed: Kubernetes (Minikube, Kind, or AWS EKS), Helm, Docker to build the container image, and kubectl to interact with your Kubernetes cluster.

Step 1: Build and Push Docker Image
First, build the Docker image for the Python app using the command:
docker build -t tusharkasana/simple-python-app:latest .
Then, push the image to Docker Hub using:
docker push tusharkasana/simple-python-app:latest

## Step 2: Set Up Helm Chart
Install Helm on your local system if you haven't already. Then, navigate to the simple-python-app-helm/ directory, which contains the Helm chart for defining the Kubernetes resources (deployments, services, etc.).

## Step 3: Deploy to Kubernetes using Helm
Once connected to your Kubernetes cluster, run the following command to deploy the application with Helm:
helm install simple-app ./simple-python-app-helm
Check the status of the deployment using:
helm status simple-app
Check the Kubernetes services to find the exposed service (either ClusterIP or NodePort) with:
kubectl get svc
If using a NodePort or LoadBalancer, find the external IP and port, and access the application at:
http://<node-ip>:<node-port>

## Step 4: Set Up Monitoring with Prometheus and Grafana
To monitor your application, install Prometheus and Grafana using Helm:
helm install prometheus prometheus-community/prometheus
helm install grafana grafana/grafana
Forward the Grafana port to your local machine with:
kubectl port-forward svc/grafana 3000:80
Then, access Grafana at http://localhost:3000 and log in with the default credentials:
Username: admin
Password: (retrieve it using the following command)
kubectl get secret --namespace default grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
Finally, set up Prometheus as a data source in Grafana (URL: http://prometheus-server) and you can use or import a Kubernetes monitoring dashboard.
