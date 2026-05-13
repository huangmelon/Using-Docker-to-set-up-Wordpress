# Using Docker to set up Wordpress clusters and enhancing Nginx cybersecurity 


## Description: 
Hi, this is my home lab project. I'm trying to document how I build a website server from scratch. Starting from installing Linux server, docker to setting up nginx and wordpress, I'm using Docker Compose to build a wordpress environment with load balancing and also implementing transmission performance optimization and hiding server information for security through Nginx configuration file. My goal is to stimulate the enterprise environment as real as possible. Check it out!

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

