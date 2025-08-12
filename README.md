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

`FROM nginx`  
`COPY ./index.html /usr/share/nginx/html`  
`COPY ./js /usr/share/nginx/html/js`  
`COPY ./css /usr/share/nginx/html/css`  
`COPY ./images /usr/share/nginx/html/images`  

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

## Deploy in Cloud Run  
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

## Deploy in App Engine  
### Enable the App Engine API  
Go to Navigation Menu -> View all products -> Serverless -> Cloud Run -> Enable API  
### Deploy Service
