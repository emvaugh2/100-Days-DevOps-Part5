# 100 Days of DevOps with KodeKloud - Days 41 - 50


Greetings! Welcome back. We're pretty much halfway done with Docker now. Well, 1/3 of the way there at the very least. By the end of the 50th day, we'll be heading into Kubernetes. Lets pick up where we left off. 


## Day 50: Set Resource Limits in Kubernetes Pods
## Day 49: Deploy Applications with Kubernetes Deployments
## Day 48: Deploy Pods in Kubernetes Cluster
## Day 47: Docker Python App
## Day 46: Deploy an App on Docker Containers
## Day 45: Resolve Dockerfile Issues
## Day 44: Write a Docker Compose File



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
