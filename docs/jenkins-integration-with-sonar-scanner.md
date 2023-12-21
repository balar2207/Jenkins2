<img src="../images/c4logo.png">

# In this tutorials we are going to learn
1. How to install SonarScanner plugin 
2. How to configure SonarScanner
3. How to scan you project

## How to install SonarScanner plugin
1. Login to Jenkins --> Manage Jenkins --> Plugins. Now in the Search box type "SonarQube Scanner for Jenkins" and click on install button to install th plugin.

## How to configure SonarScanner
1. To configure the SonarScanner go to  Jenkins --> Manage Jenkins --> Tools --> SonarQube Scanner installations
2. Provide a name to SonarQube Scanner --> apply -->save

<img src="../images/sonarscanner-configure.PNG">

3. Now we need to provide Sonar Server details like Sonar Host URL and Sonar Token for Authentication. To do so go to Jenkins --> Manage Jenkins --> Credentials --> Add Credentials --> on Kind select Secret text. Provide your sonar token in Secret box, provide id and Description.

<img src="../images/sonarscanner-cred.PNG">

4. To provide Sonar host URL go to Jenkins --> Manage Jenkins --> System --> SonarQube Server
Provide a name, provide server URl and select "server authentication token" that you have already configured in the previous step. now click on Apply --> Save

<img src="../images/sonarserver-details.PNG">