# Using Docker and Nginx to build a WordPress environment with load balancing capabilities

## Description: 
Hi, this is my home lab project. I'm trying to document how I build a website server from scratch. Starting from installing Linux server, docker to configuring Docker and Nginx settings, verified firewall rules, and troubleshot multiple deployment issues throughout the setup process.

During implementation, resolved problems related to inconsistent server name configurations and adjusted Nginx header settings to ensure WordPress correctly detected the configured service port instead of the default port 80.

After deployment, encountered persistent login issues caused by session handling across containers. The issue was successfully resolved by implementing sticky sessions, modifying Docker environment variables, and configuring shared volumes between containers.

For optimization, I also adjusted Nginx configuration file to improve transmission performance and to hide the server information for security reason.  

The final project configuration and troubleshooting documentation were uploaded here for knowledge sharing. My goal is to stimulate the enterprise environment as real as possible. Check it out!  


## Features
- Load balancing: using ```ip_hash``` to make sure the consistency of wordpress clusters
- Security hardening: Stopping ```server_tokens``` to hide the version of server
- Performance: Turning on ```sendfile``` and optimizing ```keepalive_timeout``` to improve the efficiency of static file transmission
- Environmental isolation: through ```.env``` to manage sensitive passwords and variables

## Architecture
Using Nginx as a reverse proxy server to guide the traffic to two backend wordpress container, and linking them to a MySQL database.

## Getting Started
*(Skipping the installation steps of Server and docker)*

- Step 1: clone the project  
  ```git clone https://github.com/huangmelon/Using-Docker-to-set-up-Wordpress.git```    
  ```cd Using-Docker-to-set-up-Wordpress```

- Step 2: Setting up environmental variables  
  ```cp .env.example .env```   
  **(please edit .env and fill in your database password)**  

- Step 3: starting the service  
  ```docker compose up -d```

## Directory Structure  
.  
├─── nginx.conf             &emsp; &emsp; &emsp; &emsp;*(Nginx global configuration file)*  
├─── docker-compose.yml     &emsp;*(container definition file)*  
├─── .env.example           &emsp; &emsp; &emsp; &emsp;*(environmental variable template)*  
├─── .gitignore             &emsp; &emsp; &emsp; &emsp;*(exclude sensitive files)*  
└─── README.md              &emsp; &emsp; &emsp; &emsp;*(this document)*  

## Techinical Notes
- In ```nginx.cong```, we have ```server_tokens off;``` because we want to avoid attackers to exploit known vulnerabilities by obtaining the Nginx version number.  

- ```sendfile on;``` is to reduce data copy between core and user, which optimizes the loading speed of wordpress.

- ```ip_hash``` is to guide the same user to the same container, which solves the login state missing issue across multiple containers.  

