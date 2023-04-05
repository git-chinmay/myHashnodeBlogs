---
title: "Dockerizing Node.js: From Local Development to the Cloud with AWS ECS"
datePublished: Wed Apr 05 2023 11:06:44 GMT+0000 (Coordinated Universal Time)
cuid: clg3l3nop000j09mo7f3c593e
slug: dockerizing-nodejs-from-local-development-to-the-cloud-with-aws-ecs
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1680535618514/4719b288-3f63-4d72-b570-9077639fbc07.png
tags: docker, nodejs, containerization, clouddeploy, awsecs

---

Welcome to the blog on how to Dockerize a Node.js app and deploy it to AWS ECS! In this post, I'll guide you through the detailed steps with screenshots(that is the only reason the blog looks lengthy) to containerize an app using Docker, push it to a container registry, and then deploy it to Amazon's Elastic Container Service (ECS). We'll cover everything you need to know, from converting a node app to a docker image to configuring your ECS service.

A total of three sections involve

1. Creating the app
    
2. Dockerised the app
    
3. Deploy the image to AWS ECS
    

### Prerequisites

Before proceeding make sure your system has the below items ready

* You should have Node installed
    
* Docker installed and ready for the run
    
* Access to Docker Hub(Registry)
    
* Access to AWS account
    
* An IDE of your choice
    

Once all the above is ready we can start with 1st section

### Section-1: Creating an application

We can create any web application and run it in a container, mind any *web application.*

We can not I mean we should not run any desktop app via a container. (While running a desktop application in a Docker container may be possible, it's not always the best approach, especially for applications that require significant user interaction. In these cases, it may be better to run the application directly on the host machine or consider other virtualization options, such as virtual machines.)

so here for the demo, I will create a super simple web app

* Create your project folder
    
* Do `npm init -y`
    
* Install express by `npm install express –save`
    
* Create a file called *index.js*
    
* Put the code below
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680531528861/d175213f-170e-480f-882c-b1837e2d03e6.png align="center")

Note: you will not see the extra three files (README, .gitignore and .gitattributes) on your system if you are not using git version control and that is fine.

Once our app is ready we will run it locally first to see if it's working

Go to terminal and from the root directory run the command `node index.js`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680531710730/d7a78f4b-d932-4c22-9ed6-c05173fd5e5d.png align="center")

If no error means the code parsed successfully.

Now go to your browser and access the [localhost:3000](http://localhost:3000) and if you are getting the below message then your app is working fine and ready for the next stage.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680531823127/4ba945a1-d38f-43b3-a7c8-2ac21ee19f85.png align="center")

### **Section-2: Dockerising the app**

We saw our app is working fine so we should be good for contenarising it. We know that docker containers are spawned from a docker image so first, we need to convert our app into a docker image.

To do that follow the below steps

Add the start parameter to the *script* section of the node app in *package.json* like below. This will guide the invoking process on how to start our application.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680532068342/63cafed7-f07d-46c2-bc90-96382f23ddf5.png align="center")

Now create a *Dockerfile* in your project root directory and put the following code

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680532142147/c0b1dcca-e2c7-42a1-88c9-86c1ec3f147d.png align="center")

Now build the the docker image by using below command

`docker build -t my-hashnode-demo-app .`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680532233300/103fd752-b22c-4cd3-a3f0-c1631d04a04f.png align="center")

As we can see all the steps declared in the docker file got completed without any error meaning our image got created. We can see our image by running the `Docker image` command.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680532343248/23507c10-2895-4031-84ad-478cdbb3a017.png align="center")

Now test the Docker image locally by running the container with the following command and we should be able to access the same message from `localhost:3000` .

`docker run -p 3000:3000 my-hashnode-demo-app`

We are running the container on port 3000

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680532481104/0fa0e7fc-8260-463b-ba79-9b44a8d71c49.png align="center")

We can confirm it by looking at the active containers by running `docker ps` We can verify the container name and associated image.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680532612263/2aa01220-8077-4456-8bfc-bda240598ee6.png align="center")

Now as we have tested our image and it's working fine, it's time to move to the next stage i.e putting the container in AWS ECS but before that, we have to publish our image in the docker registry(hub) so that AWS can access it .

To do that type below commands in terminal

`docker login` : It will ask for the username and password

Then run `docker tag myimage myusername/myimage:latest`

after that `docker push myusername/myimage:latest`

Note: you just need to replace your username and image name.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680532848996/ff73209f-97a9-4c11-b34d-1ded9df32ec8.png align="center")

Now we can log in to the docker hub web portal and see our uploaded docker image like below.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680532906878/23327d6b-22d4-4c83-a260-721e34c48dff.png align="center")

Now click on the image and it will be redirected to another page

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680532939820/a2b5e5e5-9b90-4090-bc7b-791966b3e9af.png align="center")

Click on *Public View* and you will see the below page

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680533027827/7b1fb5a8-4df9-417e-82a6-b6aae2f1d4cc.png align="center")

Copy the *Docker pull command* and put it in a notepad

`docker pull cicadadock/my-hashnode-demo-app`

we need this image *URI* in AWS ECS service, hence remove the docker pull part and keep the rest. `cicadadock/my-hashnode-demo-app`

Now we are ready for the next stage

### **Section-3: Deploy the app to AWS ECS**

Login to your AWS account and open ECS service and click either on *clusters* or *Get started.*

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680533331558/d1fc3fa4-93b4-4ced-a806-e39a287d2f73.png align="center")

Let's choose *Clusters* and create a cluster

Give a unique cluster name. We can leave rest configurations as default. By default, the cluster will be created with FarGate which ok for us.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680533424473/c22e619a-e3e2-412e-95b1-e72250d130d6.png align="center")

By clicking on *create* a cluster will start building and we can see the events in cloud formation.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680533487512/b6e7d31a-dd75-434e-90a7-e8935925bfac.png align="center")

Once the cluster is created, click on the *Task definition* at the left side bar and then create a new task definition

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680533554468/6c945399-72ef-46a2-9ce1-c981a3dc779c.png align="center")

Fill in the value below. **Hndemocon** will be the container name when our image runs on the ECS cluster.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680533635872/773af72d-96bf-4647-a880-d5358296d91a.png align="center")

We can leave all other fields as default and click on next

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680533690742/87eb30f1-16ad-4797-8d69-64bab4b7b6f6.png align="center")

Here also nothing to change. All the default values are good to go. So click *Next* for review and again *Next* to create the task definition

Once it is created successfully, we can see it as active in the console as below

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680533795832/dc32f886-80a2-4663-aca9-7742e081467b.png align="center")

Now go back to the cluster we created and go to the *Service* and create a service.

We can leave the compute configuration as a default setting and configure the deployment part below

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680533958199/99277027-1a23-4825-bf51-6c2f06fb961a.png align="center")

Use the task definition created in the previous step as *Family.*

Give a unique name to the service and create it. It will take a few seconds to create the server.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680534104561/60ed71f7-dcc4-4225-bb96-bfd5c2949381.png align="center")

Once the service is created successfully we can see the service status as active and Task deployment completed with green.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680534347985/e238a6a8-6f08-446d-a18d-2ece057ca9e5.png align="center")

Now go to the Task section and click on the open address and it will launch our web app

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680534210941/38d4d761-1917-435b-9552-c8f1da22b583.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680534237398/b2216f9b-e8b7-4d7f-b06e-be2962967fcf.png align="center")

Congratulation, your app is now running on the cloud as a docker container.

Note: If someone could not access the URL after opening the public ip, please make sure to add a custom TCP port range 3000 with CIDR 0.0.0.0/0 as an inbound rule. Remember for production allowing everything is not acceptable but for the demo it's fine.

You can do that as below.

Go to `service >> Networking>>Security group`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680534460625/f7bc8260-cc47-4ca3-a1d8-7357c547c5f3.png align="center")

Add the inbound rule.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680534511388/c0e2ebf8-0bbd-4435-985f-e6e8dd3629f3.png align="center")

### Cleanup

Once you have completed it, make sure to clean up all these resources so that it will not incur any cost.

If we try to delete the cluster it would fail because its associated service and Task are in progress. But it would have been great if AWS provided that feature to auto-delete all the resources by deleting the cluster, until then we have to go sequentially.

First, stop the task

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680534619729/2e435b87-8cc8-4758-b186-c39f1e9ebf63.png align="center")

Delete the service

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680534645325/2427ab31-e5a6-4525-8a15-a2e3c91ba862.png align="center")

Select *Force delete* service otherwise it will always create a new task due to replica setup.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680534681129/64e169de-ae90-4270-86be-83a2efa1d031.png align="center")

Delete the cluster

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680534756181/9cd135cf-3888-43ac-9ec1-f4ea4dd5a305.png align="center")

### Conclusion

Dockerizing your Node.js app and deploying it to AWS ECS can be a daunting task, but it doesn't have to be boring. Remember to always keep your sense of humour handy, because sometimes things can get a little crazy in the world of DevOps. As the saying goes, "With great power comes great responsibility, but with Docker, comes great portability." So go forth and Dockerize your apps, but don't forget to enjoy the journey and have a little fun along the way!

You can access the code [here](https://github.com/git-chinmay/hashnode-demo-app-docker-ecs). Hope you have enjoyed the blog.

Happy learning!!