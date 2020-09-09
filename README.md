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
