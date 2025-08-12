# Capstone Project Group 2 Batch 15
This project was cloned on 08-11-2025 for our Capstone Project

## How to run this webpage in your Debian server:
### Requirements

To run this commands you should have the following dependencies installed 
- gcloud CLI 
- GIT
- Docker
- GCP Account

### First use this command to copy the repository to the desired directory:  

`cd /path_to_desired_directory`  
`git clone https://github.com/acampos96/CapstoneProjectG2.git`  
`cd CapstoneProjectG2/Europe\ Travel`  

### Create a dockerfile inside the working directory  

`nano dockerfile`  

Inside the dockerfile you fill the following commands:  
```dockerfile,
FROM nginx  
COPY ./index.html /usr/share/nginx/html  
COPY ./js /usr/share/nginx/html/js  
COPY ./css /usr/share/nginx/html/css  
COPY ./images /usr/share/nginx/html/images  
```
Then, you press Ctrl + X to writeout the file.  

### Build the Container Image  
The next step is to build the docker image, in order to do that we'll use the following command:  

`sudo docker build -t your-docker-image .`  

### Run the container  

`sudo docker run --name your-docker-image -d -p 80:80`    

## Push Docker image to Artifact Registry  

### Enable the Artifact Registry API  
Go to Navigation Menu -> View all products -> CI/CD -> Artifact Registry -> Enable API  
### Create a Repository  
In Artifact Registry click on  Create a repository selecting Docker as format, you can leave the other settings with the default options, name your repository  
### Push image  
Run the following commands to tag your image and then push it to Artifact Registry  

`gcloud auth configure-docker your-region-docker.pkg.dev`  
`docker tag your-docker-image your-region-docker.pkg.dev/gcp-project/name-of-your-repo/your-docker-image`  
`docker push your-region-docker.pkg.dev/gcp-project/name-of-your-repo/your-docker-image`  

**Make sure to replace "your region", "gcp-project", "name-of-your-repo" and "your-docker-image" with your information.**  

## Deploy on Cloud Run  
### Enable the Cloud Run API  
Go to Navigation Menu -> View all products -> Serverless -> Cloud Run-> Enable API  
### Deploy Container  
In Cloud Run click on Deploy Container:  

1. Select "Deploy one revision from an existing container image"  
2. Select your image from Artifact Registry.  
3. Name your service.  
4. In the Containers, Volumes, Networking, Security section, change the container port to 80.  
5. You can leave all other settings with default options.  
6. Click create.  
7. You can see your app with the Endpoint URL shown in the service details.  

## Deploy on App Engine  
### Enable the App Engine API  
Go to Navigation Menu -> View all products -> Serverless -> Cloud Run -> Enable API  
### Deploy Service
Run the following command with gcloud CLI:

`gcloud app create`  

Then, you're gonna need an app.yaml to let App Engine know some details about your aplication, this would be a basic app.yaml file to run our application:  

```yaml,
runtime: python312  `  
handlers:`  
- url: /  `  
  static_files: index.html`    
  upload: index.html`    
- url: /(.*)`  
  static_files: \1`  
  upload: (.*) `  
```
Run the following command to deploy your service  

`gcloud app deploy`  

You can get your service's URL with the following command:  

`gcloud app browse`  

## Deploy on Google's Kubernetes Engine
### Enable GKE's API
Go to Navigation Menu -> Kubernetes Engine -> Enable API  
Run the following command with gcloud CLI to create your GKE Cluster:  

`gcloud container clusters create my-gke-cluster --zone us-central1-a --num-nodes=2`   

Then we need to GKE how to deploy our app with a deplyment.yaml file including the following configuration:  

```yaml,
apiVersion: apps/v1  
kind: Deployment  
metadata:  
  name: my-app-deployment 
spec:
  replicas: 3  
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-app-container
       
        image: us-central1-docker.pkg.dev/your-gcp-project-id/your-repo-name/your-image:tag
        ports:
        - containerPort: 8080 # The port your application listens on inside the container
```
Then we need to apply the deployment with the following command:  

`kubectl apply -f deployment.yaml`  

We also need a service.yaml to let GKE know how we want to access our application:  

```yaml,
apiVersion: v1
kind: Service
metadata:
  name: my-app-service # The name of your service
spec:
  type: LoadBalancer
  selector:
    app: my-app # This MUST match the 'app' label in your deployment.yaml
  ports:
  - protocol: TCP
    port: 80       # The port the public Load Balancer will listen on
    targetPort: 8080 # The port to forward traffic to on your container
```

Then we need to apply this configuration with the following command:

`kubectl apply -f services.yaml`  

In order to get the IP address from our GKE Cluster Load Balancer we can run the following command:

`kubectl get service my-app-service --watch`  

**That's How you deploy on the different GCP's Services**
