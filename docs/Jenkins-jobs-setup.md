
<img src="../images/c4logo.png">

# In this tutorials we are going to learn
  1. **Setting Up Jenkins Maven and Freestyle job**
  2. **Jenkins parametrized jobs setup (choice params,boolean params)**
  3. **Email notification jobs**
  4. **Parallel jobs configuration**
  5. **Master slave configuration**
  6. **Create Maven build on Jenkins Slave**
  7. **Create Maven build on Jenkins with Custom Docker Slave**


  ## Setting Up Jenkins Maven and Freestyle job

  #### Freestyle job
  For Freestyle job Login to jenkins then click on New Item then provide a name to youre job then select FreeStyle then click on OK.

  <img src="../images/Jenkins-New-Item.PNG">

  <img src="../images/Jenkins-Dev.PNG">

  Now got to the Build and click on **Add Build Step** then select **Execute Shell**. In the Command box provide the command to execute, then click on **save** button then click on BuildNow to trigger a build.

  <img src="../images/Jenkins-FreeStyle-Job.PNG">

  <img src="../images/Jenkins-FreeStyle-Job-BuildNow.PNG">

  To view the Console output

  <img src="../images/Jenkins-FreeStyle-Job-Log.PNG">

  <img src="../images/Jenkins-FreeStyle-Job-ConsoleOutput.PNG"> 
  
  #### Maven job
  Login to Jenkins then click on **Manage Jenkins** then click on **Global Tool Configuration**. Go to Maven and click on the checkbox to Install automatically then save the changes.
  
  To Install Git 
  <img src="../images/Jenkins-Maven-Git-Installation.PNG">
  
  <img src="../images/Jenkins-Maven-Installation.PNG">

  To Create a Maven job Click on New Item on you Jenkins dashboard then provide name to you job then select Maven Project

  <img src="../images/Jenkins-Maven-Job.PNG">

  GitHub Project URL https://github.com/c4clouds/maven-helloworld.git
 
  Go to Source Code Management and select Git then provide the repository url as above else you can specify you own github url.
  
  <img src="../Jenkins-Maven-Git.PNG">
  
  On Build specify the Goals and Options as **clean package**

  <img src="../images/Jenkins-Maven-Build.PNG">
 
  Save the changes and clik on Build now, to see the Console Output click on hte build Number

  <img src="../images/Jenkins-Maven-Build-Console-Outpu.PNG">

  ## Jenkins parametrized jobs setup
  #### Choice Parameter
 Go to Any Job and click on **This project is parameterized** checkbox. Then click on Add parameter and select **Choice Parameter**
 
 <img src="../images/Jenkins-Choice-Parameter.PNG">
 Go to Build and select **Execute Shell** and Provide the below command 
 ```shell
 echo "You have selected ${Fruit}"
 ```
 <img src="../images/Jenkins-Choice-Parameter-Build.PNG">

 Now click on **Build with Parameters** Select your choice and click on Build

 <img src="../images/Jenkins-Choice-Parameter-Build-Job.PNG">

 <img src="../images/Jenkins-Choice-Parameter-Build-Job-Output.PNG">

### Boolean Parameter
Go to any project click on **This project is parameterized** then select **String Parameter** under Name field specify FIRST_NAME, add one more **string parameter** under name field specify LAST_NAME then select **boolean Parameter** and under Name field specify SHOW.

<img src="../images/Jenkins-boolean-Parameter.PNG">

Now go to Build section and select **Execute Shell** and provide the below code
```shell
#!/bin/bash
 

if [ "$SHOW" = "true" ]; then
echo "Hello, $FIRST_NAME $LAST_NAME"
else
echo "You have not selected the show box"
fi
```
<img src="../images/Jenkins-boolean-Parameter-Build.PNG">

## Email notification jobs
Login to Jenkins, go to **Manage Jenkins** then click on **Configure System** under **Email Notification** provide SMTP server
you can get list of SMTP Server details [here](https://www.arclab.com/en/kb/email/list-of-smtp-and-pop3-servers-mailserver-list.html)

<img src="../images/Jenkins-Email-Notification.png">

Now go to the Jenkins job where you want to setup email notification and then click on Configure.

Under Post-build Actions click on E-mail Notification.

<img src="../images/Jenkins-Email-PostBuild.png">


## Parallel jobs configuration
By default, only a single build of a project is executed at a time — any other requests to start building that project will remain in the build queue until the first build is complete.

To enable parallel job configuration for a job, login to **Jenkins dashboard** then click on **Configure**  and enable the checkbox **Execute concurrent builds if necessary**

<img src="../images/Jenkins-Parallel-Job.PNG">

## Master Slave configuration
Jenkins is an awesome Continuous Integration tool which allows you to add multiple slaves as per your project requirement to a central Master server.

It will also monitor the slave state (offline or online) and getting back the build result responses from slaves and the display build results on the console output. The workload of building jobs is delegated to multiple slaves.

> Note: java must be install on the slave node
```code
sudo yum install java-11-openjdk-devel
sudo yum install git -y
sudo yum install maven -y
```

Login you Jenkins and click on **Manage Jenkins** in the left corner on the Jenkins dashboard.

<img src="../images/Manage-Jenkins.png">

Then click on the option Manage Nodes.

<img src="../images/Jenkins-Manage-Nodes.PNG">

Select New Node and enter the name of the node in the Node Name field.

Select Permanent Agent and click the OK button. Initially, you will get only one option, "Permanent Agent." Once you have one or more slaves you will get the "Copy Existing Node" option.

<img src="../images/Jenkins-Slave-Permanent-Agent.PNG">

<img src="../images/Jenkins-Slave-Configuration.PNG">

## Create Maven build on Jenkins Slave
In jenkins go the the Maven job, Under general section click on the checkbox **Restrict where this project can be run** and in **Label Expression** provide the node label name i.e **demo-slave** now click on apply and save.

<img src="../images/Jenkins-Slave-Maven-Build.PNG">

## Create Maven build on Jenkins with Custom Docker Slave
Inorde to setup Docker slave on jenkins, first we have to [Install Docker](https://github.com/submah/docker-tutorials/blob/master/install_docker_centos7.md) and crate our custom Dockerfile. Use the below code and commands

On CentOS7

**File: /usr/lib/systemd/system/docker.service**

```
#Replace the Execstart with below code

ExecStart=/usr/bin/dockerd -H tcp://0.0.0.0:2376 -H unix:///var/run/docker.sock

#Reload the daemon

systemctl daemon-reload

#Restart the Docker service

systemctl restart docker

```

  * File: Dockerfile

```Dockerfile    
  FROM jenkins/ssh-slave    
  MAINTAINER submah@c4clouds.com    
  RUN apt-get -y update    
  RUN apt-get install maven -y    
  RUN mkdir /root/.m2    
```    

  * Create Image from the Dockerfile

```
docker build -t docker-slave:v1 .

```

  * Check the Image is creating container and giving maven version output 

```
docker run --rm -it docker-slave:v1 mvn -version

```

<img src="../images/docker-mvn-version.PNG">

  * Install required Plugins on Jenkins 
    1. Docker plugin
    2. git plugin

  * Once plugins has been install go to 
  Jenkins Dashboard >> Manage Jenkins >> Manage Nodes and Clouds >> Configure Clouds

<img src="../images/Manage-Jenkins.png">  

<img src="../images/manage-nodes-and-clouds.png">  

<img src="../images/configure-clouds.png"> 

  * Now Click on Add and New Cloud >> Docker

<img src="../images/container-slave.PNG">  

  * Click on Docker Cloud Details and provide Docker Host URI
    type **tcp://127.0.0.1:2376** >> Click on Test Connection

  **Note: If you are getting docker version then its working. proceed to next step**

  *  Now click on Docker Agent templates
  provide a name i.e. **docker-slave-demo** on **Labels** >> click on the **Enable** checkbox 
  provide a name i.e. **docker-slave-demo** on **Name**, Provide Docker Image i.e. **docker-slave:v1**
  provide **Remote File System Root** as **/home/jenkins**, Usage as **Use this node as much as possible** 
  **Connection Method** as **Attach Docker Container**, **User** as **root**
  now click on **apply** and **save**

<img src="../images/docker-cloud-details.png">
<img src="../images/docker-agent-templates-1.png">
<img src="../images/docker-agent-templates-2.png">
<img src="../images/docker-agent-templates-3.png">


  * Create a Jenkins job to test 
    Go to Jenkins Dashboard >> New Items 

    provide a name i.e. **jenkins-maven-docker-slave-job**, select job as **Maven Project** then click on **OK**

<img src="../images/jenkins-maven-docker-slave-job.png">  

  * Now click on **Configure** >> On **Restrict where this project can be run** mentioned the **Label Expression** as **docker-slave-demo**

<img src="../images/docker-slave-label-expression.png">

  * On Source Code Management mention **Repository URL** as **https://github.com/submah/maven-helloworld.git**

<img src="../images/docker-slave-git.png">

  * On Post Build Action, 
    click Add Post Build Action >> Archive the Artifacts
    
    Mentioned Files to archive as **target/*.war** then click on **apply** and **save**

<img src="../images/docker-slave-post-build-action-archice-artifacts.png"> 
    

<img src="../images/docker-slave-post-build-action.png">  

  * Now click on **Build Now**

<img src="../images/docker-slave-job-output.png">













