# Capstone Project Group 2 Batch 15
This project was cloned on 08-11-2025 for our Capstone Project

## How to run this webpage in your Debian server:

### First use this command to copy the repository to the desired directory:  
`git clone https://github.com/acampos96/CapstoneProjectG2.git`
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
`sudo docker build -t capstone-nginx-1 .`  
### Run the container
`sudo docker run --name capstone-nginx-1 -d -p 80:80`  



## Europe-Travel-Website-html-css-js
Create A Responsive Tour &amp; Travel Agency Website Design Using HTML / CSS / JS
A Responsive Adventure & Tour Website Design Using HTML CSS  & JavaScript Step By Step

  
### HTML
### CSS
### JS
