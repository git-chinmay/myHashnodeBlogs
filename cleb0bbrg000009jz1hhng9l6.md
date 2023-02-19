# "From Code to Cloud: Automating Node.js Deployment with AWS Elastic Beanstalk and CodePipeline"

When it comes to deploying a web app, there are various cloud services available in the market. AWS Elastic Beanstalk is one of the most popular services for deploying web applications as it reduces management complexity, allowing you to focus on your app's development. AWS CodePipeline Service provides an easy way to automate the application deployment process. In this article, we will go through the high-level steps required to deploy a Node.js web app to AWS Elastic Beanstalk and set up a CI-CD pipeline using AWS CodePipeline Service.

### **Prerequisites**

* A node app
    
* GitHub Repository
    
* AWS account
    

Before we begin, let us assume that we have a simple weather app developed using Node.js. Here is the [GitHub link](https://github.com/git-chinmay/weather-app.) to the app.

Let's start with the high-level steps required to deploy the app to AWS Elastic Beanstalk with CodePipeline

1. Build the app.
    
2. Make configuration changes to the code entry file and package.json to make your app deployable.
    
3. zip all your code files except the node-module folder.
    
4. Create an application and environment in the Beanstalk console using manual file upload.
    
5. Create an AWS code deploy pipeline using a GitHub connection (Integrating GitHub with AWS CodePipeline).
    
6. Make sure your source and deploy stage succeeded.
    
7. Commit the code changes and auto-deploy your app.
    

### **Configurational Changes**

When developing an app locally, we often run it on [localhost](http://localhost) with a port number. However, when deploying to AWS, we need to use the port provided by AWS. Therefore, we need to capture that port from the environment.

```javascript
const port = process.env.PORT || 3000
```

we also have to make changes to the *package.json* file as this going to tell AWS what is out app’s configuration requirements. For this, I have to make these three changes. It varies based on your coding style

1. ElasticBeanstalk for now supports only node16 hence make sure to change the node version in package.json same.
    
2. By default the main module is index.json - change it based on your project setup. Most people use app.js as the main entry file.
    
3. Define how to start your app under the scripts section
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676743344263/e7b04574-ff4d-4968-ae85-dd414efa0021.png align="center")

### **Creating an APP environment in Beanstalk**

1. Log in to the AWS Elastic Beanstalk console and click on the *Create New Environment* button.
    
2. Select the environment tier as a web server environment.
    
3. Give a unique application name: **hashnodeblog-weatherapp** and observe that It will auto-populate the environment.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676743575965/b1073eae-159b-4048-93a8-90252fe349ae.png align="center")
    
4. Select the platform - this is critical. Make sure you have declared the same node version in your package.json file; otherwise, the app deployment will fail.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676782729659/0995c5a9-6d6e-4f43-9870-62666515b9fd.png align="center")
    
5. Choose the *upload your code* option and upload the code zip file.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676783077550/d7274af5-e1ba-4d1e-97d7-1280437c135b.png align="center")
    
6. Click on *Create Environment*.
    
7. You will have to wait for about 10 minutes for it to spin up a bunch of services like an S3 file to hold the zip file, spin up an EC2, do all dependency installation, create security groups, add load balancers, add auto-scaling facilities, and create CloudWatch alarms.
    
8. Once the environment has been created successfully, you will see this page. Health should be green.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676783333770/4175c0b4-ce88-4f44-9305-bdf6ba51392f.png align="center")
    
9. Click on the link given at top of the page to see your app up and running fine.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676783431960/b6969462-6872-4c95-9ee6-f710bfca717d.png align="center")
    

Our weather app is up and running on AWS now.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676783534139/fc408360-340f-4ef6-ba20-71fd2ac681b5.png align="center")

### **Automating the Deployment process**

Now, let's set up the CI-CD pipeline using AWS CodePipeline Service.

1. Open the AWS CodePipeline service console.
    
2. Click on the *Create Pipeline* button and fill in the details.
    
3. Choose the *New Service Role* option.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676783827545/d4bce98c-d43e-42df-ad95-6bac1e5a8201.png align="center")
    
4. The next option asks for Source, choose *GitHub Version 2* as we are going to use GitHub as our version controller.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676783977537/2a16ceb6-5c03-41a3-b9ee-e571c7c6834b.png align="center")
    
5. The next step is setting up a connection between AWS CodePipeline and GitHub.
    
    For a new connection click **Connect to GitHub.**
    
6. Give a connection name and click on *Connect to GitHub*.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676784195135/f03a5a1f-cc40-4b58-82c2-d9b93419ad7f.png align="center")
    
7. On the next page, AWS Connector for GitHub requests permission to verify your GitHub identity and control access to your resources. To grant permission, click on *Authorize AWS Connector for GitHub*.
    
8. Upon authorization, you’ll be redirected back to the Create connection page.
    
9. To have GitHub Apps generate a link to your GitHub to be used by CodePipeline, click on *Install a new app*.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676784342762/7a799236-9da0-4cc9-9b44-ebfff37cbc37.png align="center")
    
10. This time, you’ll be redirected to a page to select the GitHub account or organization to which you want to connect. Select the appropriate option.
    
11. Next, you’ll be prompted to decide whether you want to give AWS access to all the repositories in your account or only specific ones. Here, you can select the option you prefer. I’ll choose All repositories.
    
12. Click on *Install*.
    
13. Upon installation, you’ll be redirected to the Create connection page.
    
14. Click on *Connect* to complete the process.
    
    *Note: I have not put any screenshots for a few steps above because the prompts will self-direct you so there is nothing to confuse.*
    
15. Once the connection is established successfully, we can fill in the rest fields like repository name and branch name.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676784705531/d888e59d-7d2c-461b-96d2-42597d7b928c.png align="center")
    
16. Choose the output artifact format as *CodePipeline Default*.
    
17. Click Next and we can *skip build stage*.
    
18. We have to fill the deployment stage. Choose *Beanstalk* as the deployer.
    
19. Our formerly created application name and environment name will pop up by clicking on the search option in the application name and environment field.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676785003256/7285c4c2-25e4-45b5-894c-ee37ba8d87b9.png align="center")
    
20. Click *Next*
    
21. It will create a Review page, go through all the selections for verification and click on *Create Pipeline*.
    
22. And wait for the two-stage completion. Source and Deploy.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676785171834/c2b1c3f8-de92-4a50-8d1e-bd885bec786d.png align="center")
    
23. Once done, let's access the app again and see everything running as it is.
    

### **Testing the Auto Deployment**

1. Once make sure the app is fine, let's do some changes to our app and commit the change and let’s see how auto-deploy gets triggered.
    
2. Let's make some changes to the *help* page.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676785428910/beeb6a1e-8c01-42fe-9bad-89c72a77c695.png align="center")
    
3. Here I have added a new paragraph element and removed one.
    
4. Let's commit and push the changes. And we can see the moment we push the code the auto deployer has triggered in the CodePipeline console.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676785626761/90431019-3fdf-4f33-a074-093adced27e2.png align="center")
    
5. We can confirm that by clicking on the commit link. It will direct you to the commit on which this deployment was triggered.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676785707140/a74abab2-88b1-4db6-8705-4e8a51c02bce.png align="center")
    
6. Now let’s refresh the app and see the updates on the help page.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676786027302/033f5caa-c407-4f16-9513-245b9b24185d.png align="center")

And with this, we have completed the setup for our auto-deployment feature.

### Cleanup

As this is for educational purposes to save money you may want to clean up all the services from AWS.

1. Delete the code pipeline from the Code Pipeline dashboard. It should be quick.
    
2. Then go to the elastic beanstalk console, choose your environment and click on *Terminate environment* from the *Action* dropdown.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676786259513/eb847a9c-6c99-46a4-a2c4-ff2b3faf2313.png align="center")
    
3. It will take a few minutes to complete as a lot of services in the background have to be cleaned up. This is the best thing about using this service.
    
4. After a few minutes, If you click on refresh, it will automatically show you the application list. Choose your application to terminate and click on *Delete application* from the *Action* menu.
    
5. Confirm the delete
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676786394489/a9f65fc0-3cc6-45e6-814a-bdfa1206f368.png align="center")
    
6. If you refresh your app URL you should get the *page cannot be found* error page. You are done.
    

### Conclusion

To conclude, this blog post walked us through the process of deploying a node-based web app to the AWS cloud via the Elastic Beanstalk service and configuring a CI/CD pipeline with GitHub and AWS CodePipeline. We also made some changes to the weather app and observed the auto-deploy feature in action. While AWS offers various options for code deployment, such as Amplify, CloudFormation, AppRunner, and EC2, I am leaning towards Beanstalk because I found it is the most user-friendly and efficient deployment option. However, when making decisions at the enterprise level, various other factors like *cost* should be considered.

Hope you have liked the blog. Happy learning!!