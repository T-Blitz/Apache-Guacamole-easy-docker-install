# Apache-Guacamole-easy-docker-install
### Finally an easy way to install Apache guacamole using docker. <br />
"Apache Guacamole is a clientless remote desktop gateway. It supports standard protocols like VNC, RDP, and SSH." <br /> [--Apache Guacamole™](https://guacamole.apache.org) <br />  <br /> 
![Screenshot from 2024-11-26 16-22-40](https://github.com/user-attachments/assets/4bb5413f-8358-4aa5-b462-494e88a4ad5b)

*Thanks to [Sonoran Tech](https://www.youtube.com/@SonoranTech-hf5hf) for the base of my docker compose and comand list.*

## Install Docker
First wee need to install docker on the host system of your choosing. Please follow the guide from the offical side [here](https://docs.docker.com/engine/install/).<br />
Portainer is optional.

## Ready the Docker-Compose and pull every container


Download or copy the Guacamole docker-compose to the file directory of your choosing. <br /> 
You can also just use git to clone the whole repo at once:
```
git clone https://github.com/T-Blitz/Apache-Guacamole-easy-docker-install.git
```
then CD in to the directory:
```
cd Apache-Guacamole-easy-docker-install/
```
*Make sure to change the mysql root password inside the compose to some secure and to remember it*<br /> 
Now pull every container and start them with:
```
sudo docker compose up -d
```
## Prepre the database and  Database-user
No need to fear, this guide makes sure that you only need to copy paste commands, but please make sure to read everything anyways.<br />
After the containers have been pulled, created and are running, use the following command to find the ID of the mysql container (named "guac-sql" in this compose) 

```
sudo docker ps
```
Copy the ID of the mysql container and use it with the next command, to enter the contair via Bash:

```
sudo docker exec -it <guac-sql ID> bash
```
Now we need to enter in to the mysql command field using the password set in the Docker-Compose, using the following command:

```
mysql -u root -p 
```
*Now use the password set in your compose.* <br /> <br /> <br />
After loging in to mysql, we need to create the datatbase and user that gucamole will use for it's data. Use the following commands.<br /> <br />
To create the Database:
```
CREATE DATABASE guacamole_db;
```
To create the user:
```
CREATE USER 'guacamole_user'@'%' IDENTIFIED BY 'new password here';
```
To give the guacamole_user permissions for the database:
```
GRANT SELECT,INSERT,UPDATE,DELETE ON guacamole_db. * TO 'guacamole_user'@'%';
```

To make sure the new permissions are applied:
```
FLUSH PRIVILEGES;
```

Now exit both mysql and the docker container, **using this command twice**:
```
exit
```
## Create the "initdb.sql" file and populate the database inside
Now we need to create the initdb.sql file to populate the database inside the container. Use the following command:
```
sudo docker run --rm guacamole/guacamole /opt/guacamole/bin/initdb.sh --mysql > initdb.sql
```
Now we copy the file directly in to the container, using:
```
sudo docker cp ./initdb.sql guac-sql:/initdb.sql
```
After this, we have to re-enter the container using the same command as before:
```
sudo docker exec -it <guac-sql ID> bash
```
```
mysql -u root -p 
```
*Use the password from the compose again.* <br /> <br />

Now we only need to use two commands to select the database and run the initdb.sql file:
```
use guacamole_db;
```
```
source ./initdb.sql
```

After the file has been executed, we can exit the container.
## Restart the containers and access the WebUI
Best would be to restart all containers using:

```
sudo docker compose restart
```
Finally, you should be able to access the Apache Guacamole login screen. <br />
The login screen can be found by entering your host's local IP, port 8080, and then "/guacamole/#/".

```
http://192.168.xx.yy:8080/guacamole/#/
```
You should be able to use the standart user: *"guacadmin"* and password. *"guacadmin"* to log in.  <br />
Make sure to create a new admin user and password right after. Delete guacadmin after, for security.
