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

Probably going to knock out another 3 of these today. 

## Day 42: Create a Docker Network

Keep the streak alive!

## Day 41: Write a Docker File

Alright for this lab, we'll be creating our own Dockerfile to make a custom imag. The base image is ubuntu:24.04 and we need to install apache2 and configure it to work for port 5000. Now we can start off the Dockerfile using the FROM command and use RUN to install apache. I'm a little iffy on the port 5000 thing because I had to change two different files to get it to listen on that port. Maybe they just want us to use the EXPOSE 5000 line. I'll just try those lines and see if it works. If not, we'll have to use something like sed to edit those two files. Seems like overkill for this lab but we'll see what they want. 

UPDATE: Yikes they did want use to edit the files. I mean, well 1.) I wonder if `sed` is native to Linux or if we need to install that tool and 2.) I haven't used `sed` in a long time. I would've asked AI to generate the two commands for that. Hate to say it but I just let AI do this one for me just to test out the solution. It was correct but like I previously stated, I would've done everything but the `sed` lines because that's a bit much. 

I do know how to create Dockerfile files so this wasn't much for me to learn anyway. Lets go to the next lab. 
