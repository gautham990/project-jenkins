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