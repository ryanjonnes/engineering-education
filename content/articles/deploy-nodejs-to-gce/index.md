
### What is NodeJs?
NodeJs is a javascript based backend engine used to write server-side code in most web applications. Node operates on the Javascript engine provided by chrome. According to Wikipedia, Nodejs was established in May 2009, and since then, backend development of applications has been seamless for Javascript Developers.

As a NodeJs enthusiast, I like developing most applications with Node.js because of the wide range of frameworks and tech stacks that stand with Node.js. In addition, Node.js can easily be linked to various frameworks like React, Angular and Vue, forming a very independent and reliable tech stack.

Besides the frontend frameworks, Nodejs is easily integrable with various types of databases. However, the most efficient is MongoDB, which presents data in simple, readable JSON format and gives a developer an option to host his database and access it without technical difficulties remotely. 

### Infrustructure as a servrice
Infrusture as a service is cloud-based computing where developers get access to cloud-hosted hardware resources instead of acquiring them on their own. Most computing hardware is expensive to acquire, so the need to provide them over the internet.

There are several Infrastructure as a service(IaaS) providers, including Digital Ocean, Amazone Web Services, Azure, and Google Compute Engine, but in this article, we will explore Google's. 

The benefit of using Iaas is resource pooling, where everyone gets access to a large pool of shared resources to which they would have otherwise not had access. In addition, with strategy, deployed systems are fault-tolerant and more users get supported.

### The Google Compute Engine(GCE)
From its Wikipedia page, Google Compute Engine (GCE) is Google's Infrastructure as a Service (IaaS) developed in June 2012. Currently, GCE  runs most of Google's applications, including Gmail, YouTube, and its search engine.

The Iaas allows one to create a virtual machine, set it up according to an application's requirement and host it over the internet, thereby reducing the risk of downtime that one would have exposed his hardware to.


### Article overview
This article focuses on building a Node.js application and deploying it to one of the most reliable IaaS infrastructures; the Google Compute Engine. 

We will set up a virtual machine in the google cloud console and deploy the Node.js application to the setup machine. Besides, we will specify the number of instances that run our application to minimize downtime.

In the end, the reader should have mastered the steps of creating and deploying a Node js application to a remote virtual machine.

### Prerequisites
- Basic skills in working with Node.js
- Google Cloud Platform account

### Creating a project on Google Cloud shell
Most services provided by google are accessed using an API key that uniquely identifies one user from other users. For the same reason, we need to create a [new project](https://console.cloud.google.com/iam-admin/projects) and have our unique API key to access the services we need from Google.

The next step is to install the Google Cloud Software Development Kit. Then, since we intend to use the gcloud command-line tool in creating and deploying the application, we need to open up the console. 

After a complete installation of the Gcloud SDK, you can follow this [link](https://www.section.io/engineering-education/nodejs-app-express-generator/) to find out how to create the application using the `express-generator package. Thus, it is easier to understand and use, thereby saving time. 

If you are familiar with Node.js, you can do different styling of your application in the `views` folder. However, for the precision and shortness of the article, I will not style my application. So instead, check out [this guide](https://www.tutorialspoint.com/styling-html-pages-in-node-js) for instructions on styling a Node.js application.


### Creating a firewall rule
Navigate to the networking group ⇾ VPC network ⇾ select firewall. This page lists all the firewall rules set for the current project. 

In this step, we will configure a new rule that will allow traffic through port 8080. First, click the `CREATE FIREWALL BUTTON` then fill in the details as below.

- Name : allow-http-8080
- Targets: Specified target tags
- Target tags: http-server-8080
- Source-ip: 0.0.0.0/0
- TCP: 8080


### Creating a virtual machine on the project
The virtual machine is like a computer but hosted remotely. Like we usually run our application locally during development, the virtual machine will run the application over the internet.

To create a virtual machine, go to the Navigation menu ⇾ Compute Engine ⇾ VM instance ⇾ Create Instance.

![Creating a virtual machine](create-machine.png)

You can fill the fields below and ensure you expand the networking tab to fill in the name of the tag specified while creating the firewall rule on the `Networking tags` field.

![Vm instance configuration](configuration.png)

These configurations can vary from one project to another. Therefore, one can only choose to set up his VM according to the project they want to deploy.

### Setting up the VM for the deployment
I prefer login in from the Google Cloud shell because that will allow us to copy the application we created from the shell to the virtual machine with ease.

- Head over to the cloud shell the execute the command:

    ```bash
    gcloud compute ssh node-app --zone us-central1-a
    ```

    You can change the zone according to the place nearest to you as specified while creating the virtual machine.

- The next step is to update the Debian package list.

    ```bash
    sudo apt update
    ```
- Upgrade the upgradable dependencies using the command:

    ```bash
    sudo apt upgrade
    ```

- Install Node.Js using the command(s):

    ```bash
    curl -sL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
    sudo apt-get install -y nodejs
    ```
- Log out of the virtual machine to get to the Google cloud shell using CRTL+D

### Copying files to the virtual machine
In this step, we are copying all the project files to the virtual machine for deployment, but first, we need to remove unnecessary files from the project.

```bash
rm -r node_modules
```

Next, `CD` in the parent directory then copy all the application's files to the virtual machine.

```bash
gcloud compute scp --recurse node-app lab1: --zone us-central1-a
```
### Deployment of the application
To deploy the application, we have to be logged into the virtual machine. 

- Head over to the Google Cloud Shell, then `ssh` into the VM using the command:

    ```bash
    gcloud compute ssh node-app --zone us-central1-a
    ```
- Change the directory to get into the applications folder.
    ``bash'
    cd node-app
    ```
- Install the required Node.Js dependencies.
    ```bash
    npm install
    ```
- Run the application using the command below:
    ```bash
    PORT=8080 DEBUG=node-app:* npm start
    ```
### Conclusion
Most developers are using Google Compute Engine to host their applications betting on reliability and efficiency. Using this article, knowing to deploy a Node.js application to this platform is an advantage any programmer can afford.

The article discussed a stepwise process of deploying a NOde.js application to GCE. First, we developed an application using Node.js and deployed it to compute engine. This process should give the reader an insight into Infrastructure as a service and change the thinking dynamics of hosting web applications.

### Further reading
This concept does not stop here. The reader should learn how to develop and deploy using other tech stacks revolving around mobile and web development. 

Concerning the same, I recommend these articles:
- [Node.js establishement](https://en.wikipedia.org/wiki/Node.js)
- [Running Docker on GCE](https://www.section.io/engineering-education/docker-containers-on-compute-engine/)
- [Deploy Flutter to GCE](https://www.section.io/engineering-education/deploy-flutter-to-google-computer-engine/)
- [Google Cloud and Docker](https://www.section.io/engineering-education/docker-images-on-google-cloud/)
