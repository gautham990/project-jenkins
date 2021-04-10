# project-jenkins

// Installing and configuring Jenkins master //
* Install Java in an EC2 instance with SG 8080 open.
* Install Jenkins.
* Download and extract maven inside /opt and specify mvn path globaly.
* Instal git
* Add java,git and maven path in global tool configuration with respective versions in their names. 
* Enable timestamper in Jenkins configuration.

//Adding slaves to Jenkins master//
* Install Java in an EC2 instance.
* Create an user and generate SSH keypair.
* Copy id_rsa.pub of the user to authorized_keys.
* Go to .ssh directory of jenkins user in the master and run the command
  ssh-keyscan -H //IP of the slave// >>known_hosts
* Make sure that permissions of all SSH files are as required.
* Go to new node addition page of jenkins, select launch method as "agents via ssh"
* Add credentials of newly created slave user with private key.
* Select the key verification stratergy as "Known host file verification stratergy".

// Postgresql server installation for Sonarqube //
* Install Postgresql in an EC2 instance.
* From postgres user, run following commands using psql
  CREATE USER sonar WITH PASSWORD 'sonar' ;
  ALTER USER sonar WITH SUPERUSER
  CREATE DATABASE sonar;
  GRANT ALL PRIVILEGES ON DATABASE sonar TO sonar; 
* Modify pg_hba.conf file to change "peer" to "trust" to allow connection from localhost and restart the service.

// Sonarqube installation //
* In the same Postgresql instance, install openjdk-11.
* Extract Sonarqube package in /opt, change the ownership to a new dedicated user.
* Edit /opt/sonarqube/conf/sonar.properties to add below properties.
  sonar.jdbc.username=sonar
  sonar.jdbc.password=sonar
  sonar.jdbc.url=jdbc:postgresql://localhost/sonar
  sonar.path.data=/var/sonarqube/data
  sonar.path.temp=/var/sonarqube/temp
* Make changes in the limits and sysctl files as required in the documentation.

// Sonarqube integration with Jenkins //
* Install Sonarqube scanner plugin.
* Generate a token in Sonarqube and add it to Jenkins credentials. 
* In Jenkins "Configure system", add Sonarqube server details.
* In Sonarqube server, generate a webhook for Jenkins so that SQ informs Jenkins when analysis is finished.

// Artifactory integration //
* Install OpenJDK11 in an EC2 instance with SG 8081 and 8082 open.
* Download Artifactoy package inside /opt, extract and run it.

// Jenkinfile //
* Secrets can be injected into Jenkinfile by first defining it in "credentials" then using "withCredentials"
* Environment variables can be defined using "environment" in Jenkinfile.
* "post" specified action to be taken in case of an event. Eg events can be always, unstable, success, failure etc.
