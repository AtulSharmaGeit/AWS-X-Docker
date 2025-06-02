# AWS-X-Docker
##  Project Overview

---
##  Table Of Content
-  [Deploy an App with Docker](#deploy-an-app-with-docker)
-  [Deploy an App across Accounts](#deploy-an-app-across-accounts)

---
##  Deploy an App with Docker

**Step - 1 : Install Docker**
-  Head over to the official [Docker website](https://www.docker.com/products/docker-desktop/).
-  Select Download Docker Desktop, and then select the version of Docker that matches our local computer's operating system.

![image alt](Docker-1)

-  Downloading **Docker Desktop** usually takes at least a few minutes.

**Docker Desktop** is a program that makes it easy to work with **Docker**, a tool for creating and managing containers. Engineers use Docker Desktop to build, test, and deploy applications right from their computers and use Docker in a user-friendly way.

Containers solve a common problem called the "it works on my machine" problem. Software is not guaranteed to run and behave the way it's supposed to in every computer, because each machine comes with its own operating system, software versions, local resources and more. Containers tackle this problem in two ways:
1.  Containers package up **our application** and everything it needs to run (i.e. dependencies) in one file. Now other developers can run this package instead of the application itself, which gets the application working much faster.
2.  Containers also let us run other develeopers' applications and software much faster, since a container comes with everything we need to get it working ASAP.

Containers are mostly used during the **development** phase of an app. For example, when a team is developing a complex web app, they might use containers to make sure eveyone in the team is working in exactly the same environment, even though they are on different machines. That's why containers are such a popular **DevOps tool** i.e. a tool for making software development and releases more efficient!

-  Head to our computer's **Downloads** folder.
-  Install **Docker Desktop**:
    -  **Mac**: Double-click Docker.dmg to open the installer, then drag the Docker icon to the **Applications** folder.
    -  **Windows**: Double-click Docker Desktop Installer.exe to run the installer. By default, Docker Desktop is installed at **C:\Program Files\Docker\Docker**.
    -  **Linux**: [Install Docker Desktop using your specific Linux distribution](https://docs.docker.com/desktop/setup/install/linux/).
-  Open the folder where we saved Docker Desktop e.g. **Applications** for Mac, or **Program Files** for Windows.
-  Open [Docker Desktop](https://docs.docker.com/desktop/setup/install/linux/).
-  We might run into a confirmation popup that confirms whether we want to open this file. Select **Open/Okay**.

![image alt](Docker-2)

-  Welcome to Docker Desktop!

![image alt](Docker-3)

-  Docker Desktop will ask us to sign in right away - we don't need to do this. Select **Skip**.

![image alt](Docker-4)

If we're just using Docker in our local computer (which is what we're doing), Docker will work just fine and our work will save locally. It's just like other software we download - some software will just work on our computer straight away, without we needing to create an account. If we ever want to save our Docker work in the cloud, that's when we'll definitely need to sign in.

-  We're now looking at our Docker Desktop dashboard.

![image alt](Docker-5)

**Docker Desktop** lets us manage everything about our containers. We can create new containers, adjust their settings, or monitor how they run. 
1.  **Containers** is where we can start, stop, and manage our containers.
2.  **Images** is where we manage our container templates.
3.  **Volumes** is where we can access data saved in our containers, even after we stop/restart them.
4.  **Builds** is where we manage images we custom created or loaded into Docker.
5.  **Docker** Scout is a feature for analysing our Docker images for security issues.
6.  **Extensions** is where we can integrate [other tools](https://hub.docker.com/search?q=&type=extension) into Docker, like container monitoring and security.

-  Let's try to verify that we've downloaded Docker successfully.

**Why would I need to verify I've installed Docker?** Think of **Docker** and **Docker Desktop** as two different things. Docker on its own is a tool for container management, while Docker Desktop is an app that makes using Docker more interactive and efficient. So downloading Docker Desktop doesn't necessarily mean we've installed Docker itself. It's best to check we've installed Docker using the terminal.

-  Open the terminal in our local computer. This should be called **Terminal** for Mac/Linux computers, and **Command Prompt** for Windows.
-  Run the following command:

```bash
docker --version
```

-  We should see our installed Docker version printed.
-  Next, we'll verify that we've installed something called a Docker **daemon**. Look for the Docker icon in our **system tray**, which is the bar at the top (for Mac) or bottom (for Windows) of your computer screen with small icons for running applications, WIFI, battery etc.
-  If we see the Docker icon there, it means the Docker daemon is running on our system.

**Docker daemon** is the engine that actually does the thing when we run Docker commands e.g. create a container. In more technical terms, the Docker daemon is a background process that manages the Docker containers on our computer. It takes commands from the Docker client (i.e. commands we type into the terminal, or clicks we make through the Docker Desktop) and does the heavy lifting of building, running, and distributing our containers.

**Step - 2 : Run a Pre-Built Container Image**

Now that we've installed Docker, let's use it to create a new container from an existing **container image!**

A **container image** is a **blueprint** or **template** for containers. It gives Docker instructions on what to include in a container, such as application code, libraries, dependencies, and necessary files. We can make multiple, identical containers from the same image, and all of those containers will behave the same way regardless of where it's deployed (as long as there is a container platform e.g. Docker to run the container).

This is great for developers working in a team, because it means everyone gets the same experience with an application that they're all running on their individual computers. We're less likely to see situations where an application only works for some members in the team and not for others. It's also a lot faster for new team members to get up to speed with running the applications they need for their project to work.

-  Back in our terminal, run the following command:

```bash
docker run -d -p 80:80 nginx
```

![image alt](Docker-6)

`docker run` starts a new container. We're using a pre-existing container image called `nginx` and we're starting this container in detached mode (`-d`) so it runs in the background. Then, this `-p 80:80` maps port 80 on our host machine to port 80 in the container, which means we'll be able to access the webpage that Nginx is running through our computer's web browser.

**Nginx** (pronounced as "engine-x") is a web server, which means it's a program that serves web pages to people on the internet. Engineers use Nginx because it can handle lots of web traffic smoothly and efficiently. Sometimes, we might hear Nginx referred to as a 'proxy server', which means it can also be used to forward requests from the internet to other servers, helping balance the load or handle more users.

-  We should get this response in our terminal:

```bash
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
... (download logs) ...
... Downloaded newer image for nginx:latest
```

-  Open our web browser and navigate to `http://localhost`. We should see the default Nginx welcome page.

![image alt](Docker-7)

Typing `localhost` in our browser means we want to connect to the server software running on our own local machine. Since we've **mapped** port 80 of our machine to port 80 in the container that's running (this happened when we ran `-p 80:80 nginx` earlier in this step), the server software that localhost is connected to is the Nginx server inside the container. That's why we see the default Nginx webpage!

Web browsers automatically assume we're referring to port 80 when we don't specify a port number. So when we type `localhost` into our browser, it's essentially the same as typing `localhost:80`. Notice how we could run an Nginx server and open a webpage in seconds when we're using containers. Doing this with a virtual machine (like EC2) would take much longer - creating the EC2 instance means we'd need to install a full operating system and other software to create the web server. In general, containers lets us load and deploy applications faster than using virtual machines.

We've just taken our first steps into the world of containers! Get ready to build our own custom image in the next step.

**Step - 3 : Build our custom image**

Now that we understand the basics of Docker, let's build our very own container image.

-  In our local computer's Desktop, create a folder and name it `Compute`. We can do this in our terminal by running the following commands:

```bash
cd ~/Desktop
mkdir Compute
```

`cd` navigates our terminal to our computer's Desktop. `mkdir Compute` creates a new folder named Compute. **mkdir** stands for 'make directory' i.e. create a new folder.

-  Create a new file in our **Compute** folder called `Dockerfile` (note that there is no file extension after the file name). We can do this in our terminal by running the command:

```bash
cd ~/Desktop/Compute
touch Dockerfile
```

`cd ~/Desktop/Compute` navigates our terminal to our Compute folder. `touch Dockerfile` creates an empty file called **Dockerfile** in the current directory i.e. the Compute folder.

A **Dockerfile** is a document with all the instructions for building our Docker image. Docker would read a Dockerfile to understand how to set up our application's environment and which software packages it should install.

-  We can confirm that we've created Dockerfile by clicking into our **Compute** folder on our Desktop.

Now let's fill in Dockerfile.
-  Open the **Dockerfile**. We can use any text editor for this e.g. **TextEdit** on Mac or **Notepad++** on Windows.
-  Add the following lines to our Dockerfile:

```dockerfile
FROM nginx:latest
COPY index.html /usr/share/nginx/html/
EXPOSE 80
```

![image alt](Docker-8)

`FROM nginx:latest` means our image starts as a copy of the latest Nginx image, but we'll make a few modifications/additions to it to customise it for what we need. 

`COPY index.html /usr/share/nginx/html/` replaces the default HTML file provided by Nginx with our own custom **index.html** file, so we're customizing the Nginx server to serve our own web content.

`EXPOSE 80` means we want the container to receive web traffic through port 80, which makes it easier for users and other services to reach our web app.

-  Save our changes to Dockerfile by pressing **Ctrl/Cmd + S** on our keyboard.

Now that we have our Dockerfile ready, what is the web page that our Nginx container should serve? We'll put together a simple HTML file for this project.
-  Run these commands in our terminal to create a new file named `index.html`. This file should be in the same directory as our Dockerfile.

```bash
cd ~/Desktop/Compute
touch index.html
```

-  Open our **index.html** file and add this example HTML content:
    -  Note: if double clicking on the file automatically opens **index.html** in our browser, make sure to right click on index.html instead and open it with your text editor.
    -  Make sure to replace `YOURNAME` with our name.
 
```html
<!doctype html>
<html>
  <head>
    <title>My Web App</title>
  </head>
  <body>
    <h1>Hello from YOURNAME's custom Docker image!</h1>
  </body>
</html>
```

Now, let's build the image!
-  Head back to our local terminal, make sure we're still in the Compute directory, and run:

```bash
docker build -t my-web-app .
```

`-t my-web-app` names our image `my-web-app`, and the `.` tells Docker to find the Dockerfile in the current directory i.e. the Compute folder.

Building an image means we're creating a Docker image using the instructions in a Dockerfile. Docker will then use the built image as the blueprint to create containers that run our application anywhere.

![image alt](Docker-9)

The terminal response shows the steps Docker took to build our image from the Dockerfile:
1.  `Loading build definition`: Docker is loading the instructions in our Dockerfile to understand how to build the image.
2.  `[1/2] FROM docker.io...`: Docker is processing the first line in our Dockerfile.
3.  `[2/2] COPY index.html...`: Docker is processing the second line in our Dockerfile, which tells it to copy our index.html file into the container's file system.
4.  `Exporting to image`: Docker is combining all the instructions applied to the image together, making it ready to run containers.

**Step - 4 : Run a container with our custom image**

Image building done... Next up is to run the container with the image we've built.

-  To run an image, use this command:

```bash
docker run -d -p 80:80 my-web-app
```

Similar to the command we used before, we're asking Docker to run our **my-web-app** image as a container in the background (`-d`), making it accessible through port 80 on your local machine (`-p 80:80`).

-  Did we run into an error?

```bash
Error response from daemon: driver failed to remove network docker_default: Error during ipam/libnetwork driver backend operation: could not remove default bridge network: network docker_default id 6f79e5c38700 has active endpoints
```
![image alt](Docker-10)

This error comes up when there's already a container using port 80, so the new container we're creating can't access it.

**Let's figure out who's taking up port 80 right now.**
-  We can find the container that's running in port 80 by visiting **Docker Desktop** again. The container's icon should be green if it's running.
-  This is the Nginx container we created when we first ran **docker run -d -p 80:80 nginx**. Culprit found!

![image alt](Docker-11)

-  We can then stop the container by selecting the square stop icon. The container icon should turn grey.

![image alt](Docker-12)

-  Another way to find the container is by running `docker ps --filter "publish=80"` in our local terminal. Our terminal should show us the id under **CONTAINER ID**.

![image alt](Docker-13)

-  We can then stop the container by running `docker stop {container_id}`
    -  Replace `{container_id}` with the actual ID of the other container.
 
![image alt](Docker-14)

-  Let's try to run the image again:

```bash
docker run -d -p 80:80 my-web-app
```
-  Sweet! No more errors this time.
-  Open our web browser and navigate to `http://localhost` again. If we've done everything correctly, then we should see the content of our `index.html` file.
-  Hooray!

![image alt](Docker-15)

In this example, the container image is the blueprint that tells Docker the application code, dependencies, libraries etc that should go into a container. The container is the actual software that's created from this image and running the web server displaying your index.html

**Step - 5 : Log in to AWS with our IAM user**

-  [Head to your AWS Account](https://signin.aws.amazon.com/signin?redirect_uri=https%3A%2F%2Fconsole.aws.amazon.com%2Fconsole%2Fhome%3FhashArgs%3D%2523%26isauthcode%3Dtrue%26state%3DhashArgsFromTB_ap-southeast-2_fffdf5be4bb1a27e&client_id=arn%3Aaws%3Asignin%3A%3A%3Aconsole%2Fcanvas&forceMobileApp=0&code_challenge=m-aiqeB2UZeXTGXNyugMP8L64zd_AGUxJl4HLnA-X1o&code_challenge_method=SHA-256) as the root user.
-  Open the **AWS IAM** console.
-  From the left hand navigation panel, choose **Users**.
-  Choose **Create user**.
-  For the User name, use `Yourname-IAM-Admin‍`.
-  Make sure to select the checkbox next to **Provide user access to the AWS Management Console - optional**.‍
-  This does not apply to all accounts, but if we're prompted with a pop up panel that says **Are you providing access to a person?**, choose **I want to create an IAM user**.
-  For the console password, choose **Custom password**.
-  Type in a password that we will be able to remember/access in the future.
-  Deselect the checkbox for **Users must create a new password at next sign-in - Recommended**.
-  Choose **Next**.
-  In the permissions set up page, choose **Attach policies directly**.
-  From the list of **Permissions policies,** select **AdministratorAccess**.
-  Choose **Next**.
-  Choose **Create user**.
-  Voilà - we've just created our new user! **Stay on this page**.
-  Choose **Download .csv file**.
-  Copy the **Console sign-in URL**.
-  Now we're ready to start using our IAM user.
-  Log out of our root user's AWS Account.
-  Paste and go to our copied console sign-in URL.
-  Open our downloaded .csv file containing our user's access instructions.
-  Log in using our IAM user's username and password in the .csv file.

**PLEASE** make sure to log in to our IAM Admin User instead of the root user - it's truly best practice for account security.

**Step - 6 : Deploy our custom image to Elastic Beanstalk**

In this step, we'll deploy our custom Docker image to AWS Elastic Beanstalk, a service that makes it easy for developers to deploy applications in the cloud.

**Creating an Elastic Beanstalk Application**
-  Head to the [AWS Management Console](https://eu-north-1.signin.aws.amazon.com/oauth?response_type=code&client_id=arn%3Aaws%3Asignin%3A%3A%3Aconsole%2Fcanvas&redirect_uri=https%3A%2F%2Fconsole.aws.amazon.com%2Fconsole%2Fhome%3FhashArgs%3D%2523%26isauthcode%3Dtrue%26state%3DhashArgsFromTB_eu-north-1_4ec9e09e48991754&forceMobileLayout=0&forceMobileApp=0&code_challenge=0z9z4k29XAJXb-Lwhmja5AldFSyaFOAtx4pQZlPPJtU&code_challenge_method=SHA-256) and log in as our IAM user.
-  Search for `Elastic Beanstalk` and click on the service.

**AWS Elastic Beanstalk** is a service that makes it easy to deploy cloud applications without worrying about the underlying infrastructure. We simply upload our code and Elastic Beanstalk handles everything needed to get it running, like setting up servers and managing scaling. This lets us focus on our application code without having to spend too much time on managing cloud infrastructure.

Elastic Beanstalk can run applications that are packaged as Docker containers. This means we can develop our application on our local machine and create a Docker container image to package everything needed to run it. Once our container image is ready, we can easily deploy it to Elastic Beanstalk.

-  Click on **Create Application** in the Elastic Beanstalk homepage.

An **Elastic Beanstalk application** is like a folder that contains all the different versions of the same project/software we've deployed and the settings we've used to run them.

-  Welcome to the set up page for Elastic Beanstalk applications!

In the **Configure Environment** page:
-  We'll leave the **Environment tier** as default.

![image alt](Docker-16)

An environment in Elastic Beanstalk is like the settings for the virtual machines that will run our application. When we configure our environment, we're deciding the instance type (the type of EC2 instance that will be running the container), scaling options (how many instances to run), network settings, and more. Elastic Beanstalk uses EC2 instances (i.e. virtual servers) under the hood to run our applications. Even though we're deploying a Docker container, it ultimately runs on an EC2 instance managed by Elastic Beanstalk.

-  Enter `NextWork App` as the application name.
-  We'll leave the **Environment information** section with the default values.
-  Under the **Platform** section, select **Docker** as our Platform.
-  The platform branch and version will be automatically selected for us too.

The platform is the technology that will open and run our application. We picked Docker because it's the best platform for deploying container images built with Docker.
1.  **Platform branch** is the type of Docker we're using e.g. the default **Docker running on 64bit Amazon Linux 2023** means we're using a type of Docker that's optimised to be used on an Amazon Linux 2023 operating system.
2.  **Platform version** is the specific version of Docker we're using within that branch.

-  In the **Application code** section, select **Upload your code**.
-  Before we upload any code, head back to the **Compute** folder in our desktop and open up **index.html** with our text editor.
-  Find the line that says `<h1>Hello from YOURNAME's custom Docker image!</h1>`

![image alt](Docker-17)

-  Update index.html by **adding** a new line underneath the line we've found. For example:

```html
<!doctype html>
<html>
  <head>
    <title>My Web App</title>
  </head>
  <body>
    <h1>Hello from YOURNAME's custom Docker image!</h1>
    <h1>
      If I can see this, it means Elastic Beanstalk has deployed an image with
      my work.
    </h1>
  </body>
</html>
```

![image alt](Docker-18)

-  Elastic Beanstalk needs our source code to be in a ZIP file. Create a ZIP file containing **Dockerfile** and **index.html**.
    -  Our files should be at the root level of the ZIP file, not inside a subfolder.
    -  To create our ZIP file, in our **Compute** folder, multi-select both our **Dockerfile** and **index.html**, then compress them together.
 
- Back to our Elastic Beanstalk setup page, enter `Version One` as the Version label.
- Select **Local file**.
- Under **Upload application**, select **Choose file**.
- Upload the ZIP file we created earlier. This uploads our application code to Elastic Beanstalk.

![image alt](Docker-19)

-  In the **Presets** section, choose **Single instance (Free tier eligible)**.

![image alt](Docker-20)

By selecting a preset, we are selecting a pre-defined way to set up Elastic Beanstalk recommended by AWS. The Single instance preset creates a single EC2 instance, which is great for testing and is free tier eligible.

Other presets suit different kinds of scenarios and needs. For example, high availability comes with tools for handling any changes in traffic volumes quickly, which is great for applications that will be in production and used in the public.

-  Select **Next**.

In the **Configure service access** page:
-  Select **Create and use new service role**. This creates a new IAM role that Elastic Beanstalk can use.
-  Use the default service role name, which should look like **aws-elasticbeanstalk-service-role**.

A service role is an AWS IAM role that lets Elastic Beanstalk permission to call other AWS services on our behalf. One of the benefits of Elastic Beanstalk is that it can handle our application's cloud infrastructure for us, and it can only do that if it has the permission to use our AWS resources automatically e.g. creating a new EC2 instance. It also helps with automating tasks like scaling and health monitoring!

-  Ignore the EC2 key pair dropdown, since we don't need to access the EC2 instance directly for this project.
-  Under EC2 instance profile, choose **ecsInstanceRole**.

![image alt](Docker-21)

An instance profile is another IAM role (similar to service roles), but they're designed to give EC2 instances the permission to access other AWS services that our application might need to work..

The **ecsInstanceRole** grants EC2 instances the permission to manage containers using AWS Elastic Container Service (ECS). While Docker builds and runs containers, think of ECS as a manager that helps Elastic Beanstalk figure out how many containers it should start and stop, based on the amount of traffic going to our application.

-  Select **Next**.

In the **Set up networking, database, and tags** page:
-  Ignore the **Virtual Private Cloud** section - the default VPC looks good for this project!
-  Under **Instance settings**, check **Activated** for the Public IP address option.

![image alt](Docker-22)

This makes our EC2 instances and our application accessible from the internet - exciting!

-  Skip the rest of the page, we don't need to set up a database for our webpage.
-  Select **Next**.

For the **Configure instance traffic and scaling** page:
-  Under the **Instances** section, for our **Root volume type** select **General Purpose 3 (SSD)**.
-  For the **Size** select `10` GB.

![image alt](Docker-23)

The root volume type is our EC2 instance's storage space, just like how our local computer would have it's own storage space for the operating system and files we keep locally. **General Purpose 3 (SSD)** is Free Tier eligible and is fit for most applications.

Other root volume types are fit for special use cases, like **Provisioned IOPS SSD (io1/io2)** for applications with high-performance databases, or **Cold HDD (sci)** for data in long-term storage.

-  Under **Instance metadata service (IMDS)**, make sure IMDSv1 is **Deactivated** i.e. keep the Deactivated checkbox checked.

![image alt](Docker-24)

IMDS is a service that gives applications temporary credentials to our EC2 instances. For example, we can use IMDS to give our application the permission to access S3 to get data.

Since our app won't need access to other AWS services, we won't need IMDS.
-  We don't need to configure anything else. Skip the security groups and Capacity sections.

Security groups control traffic, while Capacity controls the compute capacity of our environment and auto scaling settings to optimize the number of instances used.

-  Select **Next**.

In the **Configure updates, monitoring, and logging page**:
-  Under the **Monitoring** section, select **Basic** for our System.
-  **Make sure to select Basic for our System to keep our project free**.

Monitoring and health reporting tools gives us performance and status updates on our Elastic Beanstalk application, so we can spot any performance issues before they affect our users.

-  Scroll the **Managed platform updates** section.
-  Make sure to **uncheck** the **Activated** checkbox for **Managed updates** option.

![image alt](Docker-25)

Managed updates automatically update the software running our application in Elastic Beanstalk e.g. Docker. This makes sure our application stays secure and stable without needing manual updates.

We don't need managed updates since we aren't keeping this application for long!
-  Now in the **Rolling updates and deployments** section, accept the default **All at once** deployment policy.

**Application deployments** move a new version of our software into production on Elastic Beanstalk. This includes uploading our code, setting configuration options, and finally, replacing the old version with the new one on the live environment that users see. There are two main types of deployments, also called deployment policies:
1.  All at once updates all instances at the same time, resulting in a brief period of downtime.
2.  Rolling updates instances gradually, which minimises downtime.

-  Leave all other options, including the **Platform software** section as default.

Platform software is the software that runs the instances in our Elastic Beanstalk environment. For example, Elastic Beanstalk has set up Nginx as a proxy server for us by default, which means Nginx will handle sending incoming traffic to the EC2 instances running our application and pass responses back to the user.

-  Select **Next**.
-  For the final step, we'll just need to review our Elastic Beanstalk setup. This rounds up our selections across the last five pages.

![image alt](Docker-26)

-  Let's run a quick checklist and scavenger hunt. Is...
    -  **Application name** `NextWork App`?
    -  **Application code** a zip file?
    -  **EC2 instance profile** set to **ecsInstanceRole**?
    -  **Public IP address** set to true?
    -  **Root volume type** set to gp3?
    -  **Environment type** set to Single instance?
    -  **System** set to basic?
    -  **Managed updates** deactivated?
    -  **Deployment policy** AllAtOnce?
-  Click **Submit**. This starts the environment creation and application deployment process. This process can take several minutes...
-  Environment launch success!

![image alt](Docker-27)

-  Shall we see our environment in action? Click on the **Domain** link on the environment dashboard.

![image alt](Docker-28)

-  Wow! The **Domain** link is showing us our updated HTML file, which tells us that Elastic Beanstalk has successfully deployed our ZIP file's contents as a container image.

![image alt](Docker-29)

