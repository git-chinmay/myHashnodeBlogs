# Hosting a WebApp on AWS

A web application (or web app) is application software that runs in a web browser, unlike software programs that run locally and natively on the operating system (OS) of the device. Web applications are delivered on the World Wide Web to users with an active network connection.

With the ever-increasing popularity and advancement of Web App developments, web services are constantly evolving to satisfy client requests irrespective of geographical location. This essentially gives businesses the opportunity to achieve a global presence by expanding their audience reach with minimal asset cost (e.g. the purchasing and managing of physical computing infrastructures, data centres, servers, security measures etc).

We can deploy and manage the web applications in the Cloud without worrying about the infrastructure that runs those applications. This reduces management complexity without restricting choice or control.

Here we will discuss on how to host such an app on Amazon Web Service(AWS) platform.
AWS provides various options to host/build-deploy any web application to its platform. Some of them are

- AWS S3 Static Webhosting
- Amazon Amplifier
- App Runner
- Fargate
- EC2 Instance
- AWS Elastic Beanstalk
- CFT
- Lambdas  etc

We will see how to deploy a simple vanilla JavaScript based webapp(a simple & cool web based game) to AWS S3 Static webhosting and AWS Amplify.

### About the app

This is simple vanilla JS app. It does not use any database or any fancy modules to run. Its a pure html, CSS and JS based configuration. I have developed and tested it locally and its ready to deploy to cloud.We have total three files to deploy


- index.html = The webpage file
- style.css = For the styling purposes
- script.js = The JavaScript file for all the actions

### Hosting on S3 Static WebHost

Hosting a static web app is simple and straightforward. Steps as below

- Login to AWS and Create a S3 bucket

- Upload the files (html, css and js)

- Open you bucket & go to Properties

- Go to Static website hosting option and click on edit

- mention your index file(index.html) and save changes

- It will generate an URL like below but if you access it now you will receive the access denies error.

If we access the URL now it may throw error 403 

`http://simplegame.s3-website-us-east-1.amazonaws.com/`

**Error message**

```
403 Forbidden
Code: AccessDenied
Message: Access Denied
RequestId: 8HAGG3JP2JWZ7ZSB
HostId: xneYq64NCVhcKWIoH/aU4nMvLMI/3gqcb5krQy7WdGA1q2kDmbGLg3GgzOAsHw3BZVyAHcGBhgQ=
```

**To resolve Permission issue**

The most common reason for the access denies error is two reasons 


1.  Check if the `block public access` is enabled for buckets on both bucket and/or account level and disable them if not already.

	-  Bucket level
	-  Account level if its enabled

2. Create a Bucket Policy if not available


```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": [
                "arn:aws:s3:::simplegame/*"
            ]
        }
    ]
}

``` 

Note: Replace `simplegame`  at Resource field with your bucket name.

Once done to access your application just open the `index.hml` object from your bucket and open the index.html.



![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1665484691083/I7Gke47yE.png align="left")



Open the Object URL :  `https://simplegame.s3.amazonaws.com/practice/index.html`

It will be your webapp link:



![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1665484756310/Gr_CCsPD5.png align="left")



### Hosting on AWS Amplify

We can use AWS Amplify to develop and deploy cloud-powered mobile and web apps. The Amplify Framework is a comprehensive set of SDKs, libraries, tools, and documentation for client app development. Amplify provides a continuous delivery and hosting service for web applications.

For more read on it please refer documentation [here](https://docs.aws.amazon.com/amplify/index.html).

For complete user guide refer [here](https://docs.aws.amazon.com/amplify/latest/userguide/welcome.html).

When you login to your AWS Amplify console. You will see two options

- Amplify Studio
- Amplify Hosting.

We need to choose the `Amplify hosting` as we already have developed the app and we just need to host it for the consumption.



![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1665485143326/iwx-SqROa.png align="left")



**Amplify Hosting**

Amplify Hosting is a fully managed hosting service for web apps. Connect your repository to build, deploy, and host your web app.



![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1665485175621/n75-9WARK.png align="left")



Either we can link the git repository and AWS will fetch the code from there or we can manually upload the code files as we did with S3 static web hosting.Here I am using the Manual option as we have just three demo files but in real world organisational application use the repository.


**Manual Deploy**

Before uploading convert your files into a zip file.


*When you create the zip folder, make sure you zip the contents of your build output and not the top level folder. For example, if your build output generates a folder named “build” or “public”, first navigate into that folder, select all of the contents, and zip it from there. If you do not do this, you will see an “Access Denied” error because the site's root directory will not be initialised properly.*


Manually upload objects to deploy your app. You can choose to drag and drop the artifacts directly, pull a zip from an existing S3 bucket or any other URL.



![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1665485369349/_Zog550vW.png align="left")



Once deploy completed the app will be created successfully



![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1665485404487/hThwg3dn5.png align="left")



Open the Domain URL to access your webapp. 
`https://dev4504.d1gwn3sagl9n99.amplifyapp.com/`



![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1665485435228/vEPcfcV0r.png align="left")



**Using the custom domain**

Use your own custom domain with free HTTPS to provide a secure, friendly URL for your app. Register your domain on Amazon Route53 for a one-click setup, or connect any domain registered on a 3rd party provider.


### Cost of Hosting this webapps

A static website set up on AWS typically costs $1.50 to $2 a month, but the price will depend on how much bandwidth you use when your site is accessed.you can host a static website within the AWS S3 services. And it will cost you pennies a month to host it and if you are in free tire then its free.

AWS Amplify Console is priced for two features ‒ build & deploy, and hosting. For the build & deploy feature the price per build minute is $0.01. For the hosting feature the price per GB served is $0.15 and price per GB stored is $0.023. With the AWS Free Usage Tier, you can get started for free.

I don't see a much price differences for hosting small webapps with moderate traffics. But I will prefer Amplify over S3 hosting. 


### Conclusion

In this blog we saw what are various services offered by AWS to host a webapps and we deployed our demo static app to AWS S3 static webhosting and AWS Amplify. We discussed about their pricing. I hope you liked the article. Happy learning!!!

[GitHub Link](https://github.com/git-chinmay/guess_my_number_game)


















