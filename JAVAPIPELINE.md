# PIPELINE AS CODE-DOCUMENTATION

##### SERVER CREATION AND JENKINS SET UP

----

Here we need multiple servers for each individual tool we call it distributed builds

> Create and setup Test, Build, Repository, Deploy servers by following the below github URL's 
>
> https://github.com/DEICONX/PROJECT-SONAR-JAVA.git  for sonarqube setup
>
> https://github.com/DEICONX/PROJECT-JAVA-BUILD-DEPLOY-.git  for maven setup and tomcat setup
>
> https://github.com/DEICONX/PROJECT-NEXUS-JAVA.git   for nexus setup
>
> https://github.com/DEICONX/PROJECT-SAMPLE-JENKINS.git  for Jenkins installation
>
> Now the servers and setup of each tool is ready SSH into terminal 

![](C:\Users\jagan\Downloads\JAVAPI\Screenshot 2025-11-03 125214.png)

--------------------------

> Now lets generate ssh keys by log in as jenkins user 
>
> `sudo su jenkins` and now generate key using 
>
> `ssh-keygen` we will now have a private key and public key stored in .ssh 

![](C:\Users\jagan\Downloads\JAVAPI\Screenshot 2025-11-03 142323.png)

--------------------------------------

> Now iam connected to web browser of jenkins using `<public ip>:8080`

![](C:\Users\jagan\Downloads\JAVAPI\Screenshot 2025-11-03 142332.png)

---------------------------------

> change directory to .ssh `cd .ssh` 
>
> ls and you can see/read the private and public keys
>
> copy the public key

![](C:\Users\jagan\Downloads\PIPELINE DOC\JENKINSCONF\Screenshot 2025-11-03 142401.png)

-----------------------------------

> go to sonar server terminal`ls -a` you will find a .ssh folder `cd .ssh` 
>
> `ls` you find authorized_keys and `sudo nano authorized keys` 

![](C:\Users\jagan\Downloads\PIPELINE DOC\JENKINSCONF\Screenshot 2025-11-03 142459.png)

-----------------------------------------

> Paste the public key here and save it

![](C:\Users\jagan\Downloads\PIPELINE DOC\JENKINSCONF\Screenshot 2025-11-03 142527.png)

------------------------------------------------------

> go to nexus server terminal `ls -a` you will find a .ssh folder `cd .ssh` 
>
> `ls` you find authorized_keys and `sudo nano authorized keys` 

![](C:\Users\jagan\Downloads\PIPELINE DOC\JENKINSCONF\Screenshot 2025-11-03 142601.png)

------------------------------

> Paste the public key here and save it

![](C:\Users\jagan\Downloads\PIPELINE DOC\JENKINSCONF\Screenshot 2025-11-03 142714.png)

-------------------------

> go to nexus server terminal `ls -a` you will find a .ssh folder `cd .ssh` 
>
> `ls` you find authorized_keys and `sudo nano authorized keys` 

![](C:\Users\jagan\Downloads\PIPELINE DOC\JENKINSCONF\Screenshot 2025-11-03 142746.png)

-----------------------------------

> Paste the public key here and save it

![](C:\Users\jagan\Downloads\PIPELINE DOC\JENKINSCONF\Screenshot 2025-11-03 142813.png)

---------------------------

> Now jenkins can itself ssh into other servers by using ssh public key
>
> we can confirm that by log in manually using each server public ip

![](C:\Users\jagan\Downloads\PIPELINE DOC\JENKINSCONF\Screenshot 2025-11-03 142948.png)

-----------------------

> Open jenkins web browser and install necessary plugins for our pipeline 
>
> Github,maven,sonarqube,artifact,ssh,pipeline,tomcat,nexus
>
> We are ready with jenkins and now lets add credentials

![](C:\Users\jagan\Downloads\PIPELINE DOC\JENKINSCONF\Screenshot 2025-11-03 143356.png)

--------------------------------------

### ADDING CREDENTIALS

------------

> Go to settings---->credentials
>
> you will see a dashboard select system 

![](C:\Users\jagan\Downloads\PIPELINE DOC\CREDENTIALS\Screenshot 2025-11-03 143538.png)

---------------------------------------

> Now select Global Credentials

![](C:\Users\jagan\Downloads\PIPELINE DOC\CREDENTIALS\Screenshot 2025-11-03 143548.png)

-------------------

> Add credentials and now we will add our sonarqube token here, create
>
> kind--->secret text, ID----->your token

![](C:\Users\jagan\Downloads\PIPELINE DOC\CREDENTIALS\Screenshot 2025-11-03 143639.png)

--------------------------------------

> Now we will add our jenkins private key which we have generated using ssh-keygen
>
> kind---->SSH username with private key
>
> private key--->Enter directly(your private key copied from jenkins .ssh) and create

![](C:\Users\jagan\Downloads\PIPELINE DOC\CREDENTIALS\Screenshot 2025-11-03 143713.png)

----------------------------------

> We have created/added our credentials to Jenkins

![](C:\Users\jagan\Downloads\PIPELINE DOC\CREDENTIALS\Screenshot 2025-11-03 143903.png)

-----------------

### SYSTEM AND TOOLS

-------------------

> Settings--->system ,scroll down
>
> you will find sonar qube installations 
>
> Name-->your name, Server URL-->your sonarqube URL, server authentication token-->your Sonar token and save 

![](C:\Users\jagan\Downloads\PIPELINE DOC\TOOLS\Screenshot 2025-11-03 144025.png)

-------------------------------

> Create a folder name jenkins in every server using 
>
> `mkdir jenkins`  pwd we can find the path /home/ubuntu/jenkins

![](C:\Users\jagan\Downloads\PIPELINE DOC\TOOLS\Screenshot 2025-11-03 144124.png)

----------------------

> We need Java,maven paths to configure tools in jenkins browser
>
> Use `readlink -f $(which java) `
>
> for maven use `readlink -f $(which mvn)` 
>
> Note: copy the path upto before /bin

![](C:\Users\jagan\Downloads\PIPELINE DOC\TOOLS\Screenshot 2025-11-03 144332.png)

-------------------

> Settings-->tools-->JDK-->Add JDK
>
> Name-->any name, JAVA_HOME-->paste the copied path
>
> Note: Add all the java versions which you have installed in servers

![](C:\Users\jagan\Downloads\PIPELINE DOC\TOOLS\Screenshot 2025-11-03 144401.png)

-----------------------

> for maven use `readlink -f $(which mvn)` and copy upto before /bin

![](C:\Users\jagan\Downloads\PIPELINE DOC\TOOLS\Screenshot 2025-11-03 144622.png)

-------------------

> Configure maven tool 
>
> name-->any name, MAVEN_HOME-->paste the copied path

![](C:\Users\jagan\Downloads\PIPELINE DOC\TOOLS\Screenshot 2025-11-03 144645.png)

---------------------------

# NODE CREATION

> Settings-->nodes
>
> You will now see a built in node its is created by jenkins (default)
>
> Click on New node to create a node

![](C:\Users\jagan\Downloads\PIPELINE DOC\NODES\Screenshot 2025-11-03 144749.png)

-----------------------

> First node is for our sonar-mvn server 
>
> Node name --> sonarmvn
>
> type-->permanent agent-->create

![](C:\Users\jagan\Downloads\PIPELINE DOC\NODES\Screenshot 2025-11-03 144804.png)

-----------------------------

> Description-->about node
>
> number of executors-->1
>
> here we have to give our path that is the folder which we have created in each server Jenkins
>
> it indicates/tells jenkins where the work should be stored 
>
> labels are very important we should include them in pipeline as code to do job give any label name 

![](C:\Users\jagan\Downloads\PIPELINE DOC\NODES\Screenshot 2025-11-03 144859.png)

------------------------

> Here in our credentials we have given our Jenkins private key i.e ssh
>
> Launch method --> launch agents via ssh 
>
> Host-->Ip address of server
>
> credentials-->any name
>
> verification strategy-->Non verifying Verification strategy
>
> Save

![](C:\Users\jagan\Downloads\PIPELINE DOC\NODES\Screenshot 2025-11-03 144939.png)

---------------

> Our sonar-mvn server node is created successfully and its Connected successfully

![](C:\Users\jagan\Downloads\PIPELINE DOC\NODES\Screenshot 2025-11-03 145007.png)

-------------------

> we can view our node in Nodes dashboard of jenkins webpage
>
> Repeat the same process for each server and you can view Nodes image folder for reference in github

![](C:\Users\jagan\Downloads\PIPELINE DOC\NODES\Screenshot 2025-11-03 145017.png)

------------------

> Now we have created node for each server and connected it to jenkins

![](C:\Users\jagan\Downloads\PIPELINE DOC\NODES\Screenshot 2025-11-03 145428.png)

------------------------------

# PIPELINE AS CODE- JAVA JOB CREATION

> New item --> itemname -->item type(pipeline)--> ok

![](C:\Users\jagan\Downloads\JAVAPI\Screenshot 2025-11-05 113814.png)

---------------------

> To setup webhook add your github REPO URL-->save

![](C:\Users\jagan\Downloads\JAVAPI\Screenshot 2025-11-05 121121.png)

--------------------

> In triggers add/select "Github hook trigger for Githubscm polling" for integrating webhook trigger

![](C:\Users\jagan\Downloads\JAVAPI\Screenshot 2025-11-05 121129.png)

----------------------------

> Write your JSON file pipeline script it should include all the commands to execute
>
> My pipeline script:
>
> pipeline {
>     agent { label 'sonar' }
>
>     tools {
>         jdk 'java17'
>         maven 'maven'
>     }
>     
>     environment {
>         SONARQUBE_SERVER = 'SONAR-MVN'
>         MVN_SETTINGS = '/etc/maven/settings.xml'
>         NEXUS_URL = 'http://18.232.136.245:8081'
>         NEXUS_REPO = 'JAVA'
>         NEXUS_GROUP = 'com/web/cal'
>         NEXUS_ARTIFACT = 'webapp-add'
>         TOMCAT_URL = 'http://18.207.93.131:8080/manager/text'
>     }
>     
>     stages {
>         stage('Checkout Code') {
>             steps {
>                 echo '📦 Cloning source from GitHub...'
>                 checkout([$class: 'GitSCM',
>                     branches: [[name: '*/main']],
>                     userRemoteConfigs: [[
>                         url: 'https://github.com/DEICONX/Java-Web-Calculator-JEN.git'
>                     ]]
>                 ])
>             }
>         }
>     
>         stage('SonarQube Analysis') {
>             steps {
>                 echo '🔍 Running SonarQube static analysis...'
>                 withSonarQubeEnv("${SONARQUBE_SERVER}") {
>                     sh 'mvn clean verify sonar:sonar -DskipTests --settings ${MVN_SETTINGS}'
>                 }
>             }
>         }
>     
>         stage('Build Artifact') {
>             steps {
>                 echo '⚙️ Building WAR...'
>                 sh 'mvn clean package -DskipTests --settings ${MVN_SETTINGS}'
>                 sh 'echo ✅ Build Completed!'
>                 sh 'ls -lh target/*.war || true'
>             }
>         }
>     
>         stage('Upload Artifact to Nexus') {
>             steps {
>                 withCredentials([usernamePassword(credentialsId: 'nexus', usernameVariable: 'NEXUS_USR', passwordVariable: 'NEXUS_PSW')]) {
>                     sh '''#!/bin/bash
>                         set -e
>                         WAR_FILE=$(ls target/*.war | head -1)
>                         FILE_NAME=$(basename "$WAR_FILE")
>                         VERSION="0.0.${BUILD_NUMBER}"
>     
>                         echo "📤 Uploading $FILE_NAME to Nexus as version $VERSION..."
>     
>                         curl -v -u ${NEXUS_USR}:${NEXUS_PSW} --upload-file "$WAR_FILE" \
>                           "${NEXUS_URL}/repository/${NEXUS_REPO}/${NEXUS_GROUP}/${NEXUS_ARTIFACT}/${VERSION}/${NEXUS_ARTIFACT}-${VERSION}.war"
>     
>                         echo "✅ Artifact uploaded successfully to Nexus!"
>                     '''
>                 }
>             }
>         }
>     
>         stage('Deploy to Tomcat') {
>             agent { label 'tomcat' }
>             steps {
>                 withCredentials([
>                     usernamePassword(credentialsId: 'nexus', usernameVariable: 'NEXUS_USR', passwordVariable: 'NEXUS_PSW'),
>                     usernamePassword(credentialsId: 'tomcat', usernameVariable: 'TOMCAT_USR', passwordVariable: 'TOMCAT_PSW')
>                 ]) {
>                     sh '''#!/bin/bash
>                         set -e
>                         cd /tmp; rm -f *.war
>     
>                         echo "🔍 Fetching latest WAR from Nexus..."
>                         DOWNLOAD_URL=$(curl -s -u ${NEXUS_USR}:${NEXUS_PSW} \
>                             "${NEXUS_URL}/service/rest/v1/search?repository=${NEXUS_REPO}&group=com.web.cal&name=webapp-add" \
>                             | grep -oP '"downloadUrl"\\s*:\\s*"\\K[^"]+\\.war' | grep -vE '\\.md5|\\.sha1' | tail -1)
>     
>                         echo "📎 Download URL: $DOWNLOAD_URL"
>     
>                         if [[ -z "$DOWNLOAD_URL" ]]; then
>                             echo "❌ No WAR found in Nexus!"
>                             exit 1
>                         fi
>     
>                         echo "⬇️ Downloading WAR: $DOWNLOAD_URL"
>                         curl -u ${NEXUS_USR}:${NEXUS_PSW} -O "$DOWNLOAD_URL"
>                         WAR_FILE=$(basename "$DOWNLOAD_URL")
>                         echo "📦 Downloaded WAR file: $WAR_FILE"
>     
>                         APP_NAME=$(echo "$WAR_FILE" | sed 's/-[0-9].*//')
>                         echo "📛 Application name: $APP_NAME"
>     
>                         echo "🧹 Removing old deployment..."
>                         curl -v -u ${TOMCAT_USR}:${TOMCAT_PSW} "${TOMCAT_URL}/undeploy?path=/${APP_NAME}" || true
>     
>                         echo "🚀 Deploying new WAR to Tomcat..."
>                         curl -v -u ${TOMCAT_USR}:${TOMCAT_PSW} --upload-file "$WAR_FILE" \
>                             "${TOMCAT_URL}/deploy?path=/${APP_NAME}&update=true"
>     
>                         echo "✅ Deployment attempted. Check Tomcat Manager UI."
>                     '''
>                 }
>             }
>         }
>     }
>     
>     post {
>         success { echo '🎉 Pipeline completed successfully — Application live on Tomcat!' }
>         failure { echo '❌ Pipeline failed — Check Jenkins logs.' }
>     }
> }

![](C:\Users\jagan\Downloads\JAVAPI\Screenshot 2025-11-05 121855.png)

------------------------

> Save and Select Build now and go to pipeline over view
>
> Here our build is Success and you can view outputs in each individual server

![](C:\Users\jagan\Downloads\JAVAPI\Screenshot 2025-11-05 121523.png)

--------------------------

> Output of our sonarqube web page

![](C:\Users\jagan\Downloads\JAVAPI\Screenshot 2025-11-05 122016.png)

----------------------

> Output of our nexus webpage

![](C:\Users\jagan\Downloads\JAVAPI\Screenshot 2025-11-05 121957.png)

----------------------

> output of our Tomcat web page

![](C:\Users\jagan\Downloads\JAVAPI\Screenshot 2025-11-05 124845.png)

---------------------

> Output of our application

![](C:\Users\jagan\Downloads\JAVAPI\Screenshot 2025-11-05 124854.png)

---------------

> New commit in github and pipeline is auto builded

![](C:\Users\jagan\Downloads\JAVAPI\Screenshot 2025-11-05 125349.png)

-------------------

> Output of our application

![](C:\Users\jagan\Downloads\JAVAPI\Screenshot 2025-11-05 125416.png)

