# 100 Days of DevOps with KodeKloud - Days 41 - 50


Greetings! Welcome back. We're pretty much halfway done with Docker now. Well, 1/3 of the way there at the very least. By the end of the 50th day, we'll be heading into Kubernetes. Lets pick up where we left off. 


## Day 50: Set Resource Limits in Kubernetes Pods

We've finally reached the halfway mark of the challenge! That was fast. Lets knock it out. For this lab, we need to create a pod named `httpd-pod` with a specific container name and image. Here are the resource limits: Requests: Memory: 15Mi, CPU: 100m. Limits: Memory: 20Mi, CPU: 100m. 

Now, once again, I know how to do this in a deployment.yaml file but I never set this in a kubectl run command so let me look this up. I found out how to update a pod to use the resource limits but not how to automatically deploy these parameters when provisioning the pod. Going to rely on AI for this one as well. 

UPDATE: AI told me to use a pod manifest file. It looks like a deployment file but instead for the `kind: ` field, you'll put Pod instead of Deployment. This will make sure the pod is designed specifically how you wanted it to be instead of the name being determined based on deployment factors. 

apiVersion: v1
kind: Pod
metadata:
  name: httpd-pod
spec:
  containers:
    - name: httpd-container
      image: httpd:latest
      resources:
        requests:
          cpu: "100m"
          memory: "128Mi"
        limits:
          cpu: "200m"
          memory: "256Mi"

It looks the same but this got the results I was looking for. Nice. I'll have to keep this in mind. 

## Day 49: Deploy Applications with Kubernetes Deployments

We'll sneak in another one since the first lab was pretty easy. Now, we'll use a deployment file to create a pod. I actually just used the previous deployment.yaml file from the last lab that didn't work. It worked this time though. Here's the code:

apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
spec:
  selector:
    matchLabels:
      app: nginx_app
  minReadySeconds: 5
  template:
    metadata:
      labels:
        app: nginx_app
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80

Easy enough. Lets move onto Day 50. 

## Day 48: Deploy Pods in Kubernetes Cluster

Now for the fun stuff! I've been working with Kubernetes (K8s) for a while now so this will help me cement my skills. The next 20 days are all based on this so I'm going to go back to doing 1 or 2 a day instead of 3. 

We need to create a pod named pod-nginx using the nginx:latest image. We set the app label to nginx_app and we'll name the container nginx-container. We already have kubectl installed on our jumpbox. 

A swift look at the K8s documents says we can use `kubectl create` to complete this task. Alright I was struggling with that so I found a default `nginx` deployment.yml file and just put my information into it. I had to fix the formatting. Once I did that, I used `kubectl apply -f <file_path>` and then `kubectl get pod` and `kubectl describe pod` to make sure the pod was running. I'm going ask AI if there's another way to go about doing this. Since the name didn't match the exact value in the lab, I didn't pass this. So I'm going to find a way to create the pod from the CLI instead of a deployment file. 

UPDATE: I was given the command `kubectl run pod-nginx --image=nginx:latest --labels="app=nginx_app"`. This is basically the same as the `docker run` command. Duly noted. 

## Day 47: Docker Python App

Alright lets knock out the FINAL Docker lab. Ready? Lets go!

There's a Python app that the lab automatically made for us with a requirements.txt file. We need to create an image using this Python script and the requirements. We can use any python base image and we need to expose port 5002. Lastly, we need to automatically run the script when the container starts. Once we create the image, we need to run the container and bind the host port 8099 to container port 5002. We'll do a curl test to make sure we receive the correct message. 

Luckily for me, I've already created a few different Python scripts with a requirements.txt file and the accompanying Dockerfile. They're in my GitHub repos so I copied one of those and changed out the details. After that, we can use the `docker build`, `docker images`, `docker run`, and `docker ps` commands for create the image, verify image creation, run the container, and verify the container is running. Finally, I did the curl test on port 8099 and received the correct output. I'm going to submit the lab now. 

Got the green check! S/o to my GitHub repos. Okay now we move on to Kubernetes!

## Day 46: Deploy an App on Docker Containers

We'll be testing the deployment of an app using a Docker Compose file. We need to create the compose file and have it deploy a web and a DB service. Each service should deploy a container with the following details:

Web service - name: php_apache, image php with any apache tag, port binding 3001:80, and mount binding. 
DB service - name: mysql_apache, image mariadb:latest, port mapping 3306:3306, and mount binding. 

Set MYSQL_DATABASE=database_apache and use a custom user with a password for DB connections. Test the app using a curl test on port 3001. 

services:
  # 1. WEB SERVICE
  web:
    image: php:apache
    container_name: php_apache
    ports:
      - "3001:80"
    volumes:
      - /var/www/html:/var/www/html


  # 2. DATABASE SERVICE
  db:
    image: mariadb:latest
    container_name: mysql_apache
    ports:
      - "3306:3306"
    environment:
      # Mandatory root password for administrative access
      MYSQL_ROOT_PASSWORD: "WhatWouldJesusDo!?!5"
      # Automated creation of your application's database, user, and password
      MYSQL_DATABASE: database_apache
      MYSQL_USER: edward
      MYSQL_PASSWORD: "ILoveGodDoYouLoveGod?!?3"
    volumes:
      - /var/lib/mysql:/var/lib/mysql



Okay I ended up with the above code by copying someone else's work. I had to change the DB environment variables to fit MySQL instead of Postgres though. I'm asking AI to check my answer. It told me to put my passwords in quotation marks. I went and created my compose file and used `docker compose -f <file_path> config` to validate my file. Lastly, we'll run the same command but with the `up -d` options. It pulled the correct images and it says my containers were created. Use the usual validation checks `docker images` and `docker ps` to make sure the containers are running. I'm going to go ahead and submit my lab now. Oh wait I still need to run my curl test. I ran `curl -i http://localhost:3001/` and received a Welcome to xFusionCorp Industries! message with the HTTP 200 OK result. 

Green check! Lets move to our final Docker lab. 


## Day 45: Resolve Dockerfile Issues

For this lab, the Dockerfile build is displaying an error so we need to fix that. So I searched for the Dockerfile and right off the back, I noticed the first line says IMAGE httpd:2.4.43. I know this should say FROM instead. So lets change that. I ran a `docker build` and ran into the error with one of the `sed` commands. Looks like I may need single quotation marks here instead of double quotation marks. I'm going to change that and try it again. Okay that actually wasnt the issue. There was a glaring issue right in my face that I missed :-). All the sed commands had ADD in front of them instead of RUN. I changed it and the docker image was able to be built. 

I received the green check mark. Nice. 

## Day 44: Write a Docker Compose File

Okay I haven't worked with Docker Compose or Docker Swarm in about 2 years so we'll be doing a refresher here. With Docker Compose, you can basically automate your Docker commands configurations. It's like an Ansible playbook. You create a YAML file with the specifications that you want for your container and you deploy the YAML file. You can reuse this file over and over so it makes deployment easier. Seems like Docker, Ansible, and Kubernetes are all found of YAML so lets get the format down. 

I asked AI for a Docker Compose refresher and they gave me a general format for a simple Compose file. So in our lab, we need to grab the `httpd:latest` image, name the container httpd, map port 3001 on the host to port 80 on the container, and then map one of the container's directories to the host's directories. It'll look something like this:

services:
  service-name:
    image: httpd:latest
    container_name: httpd
    ports:
      - "3001:80"
    volumes:
      - /opt/sysops:/usr/local/apache2/htdocs

Once you pull the image, verify it's on your system using `docker images`. Then, create your file. You can validate your YAML file using `docker compose -f <file_path> config`. I didn't see an error message so we'll use `docker compose -f <file_path> up -d` to start the container. The -f flag specifies which YAML file to use. The -d flag is for detached. The up argument says create and start the services defined in the file. You should get a confirmation saying your container was created and you can check using `docker ps` or `docker compose -f <file_path> ps`. 

Lastly since this is Apache, you can do a curl on the localhost to the mapped port to make sure you received some HTML back. That completes this lab!

## Day 43: Docker Ports Mapping

Port mapping! I believe this is when you map a port on your host machine to a port on the container machine. In this label, we'll pull down the image `nginx:stable`, create a container named `blog` using that image, and then map host port 6000 to the container port 80. We can use the `docker pull nginx:stable` command, then `docker run -d -p 6000:80 --name blog nginx:stable` to finish off the lab. You can use the `docker images` and `docker ps` commands to make sure the correct image was pulled and the container is running, respectively. 

Press the check mark and received a succcess message! That was pretty straightforward. I double checked the dockerdocs for this but I was able to do this solo. 

## Day 42: Create a Docker Network

Okay today is all about Docker networking. The funny thing about this is I have a background in networking and I dont know much of anything about Docker networking. For this lab, we'll be creating docker networks for different environments. We need to create a docker network named `official`, configure it to use `macvlan` drivers, and then set the subnet and IP ranges as `172.168.0.0/24`. Why are those two different things, I do not know. I'm going to try to google this before using AI. 

Looking at the dockerdocs (didnt know this was a thing), I'm seeing the `docker network create` command with the all the flags listed here. I see the --ip-range flag, --subnet flag, and id flag for drivers. I'm going to go ahead and try `docker network create -d macvlan --subnet 172.168.0.0/24 --ip-range 172.168.0.0/24 official`. Wow it actually accepted that. Lets find out how to verify that it has been created. I googled and found `docker network ls`. You can also do `docker network inspect <network_name>` to see the detailed version. I did that and I can see the subnet and IPRange lines. It also confirms the Driver and name. I think we're good here. 

Got the green check! Okay this was easy although I've never done it before. I say this to say, you don't always need to use AI for everything. Reading the white pages can get you far. 

## Day 41: Write a Docker File

Alright for this lab, we'll be creating our own Dockerfile to make a custom imag. The base image is ubuntu:24.04 and we need to install apache2 and configure it to work for port 5000. Now we can start off the Dockerfile using the FROM command and use RUN to install apache. I'm a little iffy on the port 5000 thing because I had to change two different files to get it to listen on that port. Maybe they just want us to use the EXPOSE 5000 line. I'll just try those lines and see if it works. If not, we'll have to use something like sed to edit those two files. Seems like overkill for this lab but we'll see what they want. 

UPDATE: Yikes they did want use to edit the files. I mean, well 1.) I wonder if `sed` is native to Linux or if we need to install that tool and 2.) I haven't used `sed` in a long time. I would've asked AI to generate the two commands for that. Hate to say it but I just let AI do this one for me just to test out the solution. It was correct but like I previously stated, I would've done everything but the `sed` lines because that's a bit much. 

I do know how to create Dockerfile files so this wasn't much for me to learn anyway. Lets go to the next lab. 
