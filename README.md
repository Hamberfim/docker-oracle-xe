# Docker Oracle XE for use with Oracle SQL Developer
Setup docker image of latest Oracle XE to use with Oracle SQL Developer on Windows OS

## USE AT YOUR OWN RISK - THIS IS FOR STUDY - NOT PRODUCTION - SHUT IT DOWN WHEN NOT IN USE

FYI, The '--net=host' flag sets it so the host and the guest can communicate locally without all the specific port forwarding.
<br />You can get more specific and with security in mind it would be preferred but not everyone is up on configuring port forwarding. 

1. From the terminal run this replacing 'YourIntendedPassword' / 'yourUserProfileName'  to get the latest official image from Oracle (all one line):

``` 
docker run --name oracle-xe-db --net=host -e ORACLE_PWD=<YourIntendedPassword> -v C:\Users\<yourUserProfileName>\docker\desktop\oracle_data:/opt/oracle/oradata container-registry.oracle.com/database/express:latest
```

This can take a while depending on your internet speed and system specs. I have a 1GB fiber line and it took about 20 minutes to download, extract and install. 
Eventually in the console it will say the database is ready along with some other information.
You must then restart the container.

2. Create a connection logging in as sys leaving the connection type/details as default (attached pict 01):
```
username: sys (Role drop down set to SYSDBA)
password: <YourIntendedPassword>
```
![Alt](https://github.com/Hamberfim/docker-oracle-xe/blob/main/01_sysLogin.png "SYS Login Screen Shot")

3.Then run this SQL to create a user replacing 'username'/'password' (I'm attempting to utilizes the least amount of privileges) (attached pict 02):
```
CREATE USER c##<username> IDENTIFIED BY <password>;
GRANT CREATE SESSION TO c##<username>;
GRANT CREATE TABLE, CREATE SEQUENCE TO c##<username>;
GRANT CREATE VIEW TO c##<username>;
ALTER USER c##<username> QUOTA UNLIMITED ON USERS;
```
![Alt](https://github.com/Hamberfim/docker-oracle-xe/blob/main/02_userScript.png "new user script Screen Shot")

4.Disconnect and create a connection logging in under the new user with all the default settings (attached pict 03):
```
username: c##<username>
password: <password>
```
![Alt](https://github.com/Hamberfim/docker-oracle-xe/blob/main/03_userLogin.png "new user connection Screen Shot")

From this new user account, you can run the scripts from the previous book or the ones for this class and not have to navigate all the installed oracle tables (attached pict 04). If it gets messed up too bad to drop and re-run scripts you can delete the container and start over.
![Alt](https://github.com/Hamberfim/docker-oracle-xe/blob/main/04_userRunningScript.png "query under new user Screen Shot")

### When not in use, shut the docker container down.
