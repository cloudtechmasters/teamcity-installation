# teamcity-installation
## Step 1: Update the server
	  sudo yum update -y
## Step 2: Install java 8
	  sudo yum install java-1.8.0-openjdk-devel -y
## Setup PostgreSQL 10 Database For SonarQube
    amazon-linux-extras install postgresql10 vim epel -y
    yum install -y postgresql-server postgresql-devel
    /usr/bin/postgresql-setup --initdb
Need to change config file as shown in below
    
    vi /var/lib/pgsql/data/pg_hba.conf
Replace Method name "peer" to "md5"

![image](https://user-images.githubusercontent.com/68885738/90953619-aef2f800-e48a-11ea-9b50-489183e9b0c1.png)

Enable  postgresql:
    
    systemctl enable postgresql
Start postgresql:

    systemctl start postgresql

Login into Database
	  
    su - postgres
You can get into Postgres console by typing
	  
    psql
Create a teamcitydb database
	  
    create database teamcitydb;
Create the teamcity user with a strongly encrypted password
	  
    create user teamcity with encrypted password 'Cloud#436';
Next, grant all privileges to teamcity user on teamcitydb
	  
    grant all privileges on database teamcitydb to teamcity;
Exit the psql prompt using the following command
	  
    \q
Switch to your sudo user using the exit command
	  
    exit
Download TeamCity
Firstly we will download the TeamCity tar archive from the official website.

wget https://download.jetbrains.com/teamcity/TeamCity-2020.1.3.tar.gz

tar xvf TeamCity-2020.1.3.tar.gz

Install TeamCity
Now we will install TeamCity.

mkdir /opt/teamcity
mv TeamCity /opt/teamcity

Download PostgreSQL JDBC driver
We need to download PostgreSQL JDBC driver in order to use PostgreSQL database for TeamCity. You can download it from this link also or use the following command to download via yum.

mkdir -p /opt/teamcity/.BuildServer/lib/jdbc/
https://repo1.maven.org/maven2/org/postgresql/postgresql/42.2.16/postgresql-42.2.16.jar

wget https://jdbc.postgresql.org/download/postgresql-42.2.16.jar -P /opt/teamcity/.BuildServer/lib/jdbc/



We will create a startup script to start teamcity. Create a new file in /etc/systemd/system/teamcity.service.

vim /etc/systemd/system/teamcity.service

[Unit]
Description=TeamCity Server
After=network.target

[Service]
Type=forking
PIDFile=/opt/teamcity/logs/teamcity-server.pid
; Make sure the CATALINA_PID env variable is setup in $TEAMCITY_HOME/bin/catalina.sh
ExecStart=/opt/teamcity/bin/teamcity-server.sh start
ExecStop=/opt/teamcity/bin/teamcity-server.sh stop

[Install]
WantedBy=multi-user.target
Enable and Start TeamCity Services
Now we will enable and start TeamCity using systemctl. Before we need to reload the daemon because we have created new service file.

systemctl enable teamcity

systemctl start teamcity

TeamCity web interface
Now we will go to the web interface to continue our TeamCity installation. Go to ip-address:8111

Step 1: Change the Data Directory

Here we will change the data directory to /opt/teamcity/.BuildServer
