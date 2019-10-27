# Linux-Server-Configuration-Project

### About the project
> This page explains how to secure and set up a Linux distribution on a virtual machine, install and configure a web and database server to host a web application.

### Technical Information About the Project

- **Server IP Address:** 3.120.178.18
- **SSH server access port:** 2200
- **SSH login username:** grader
- **Application URL:** http://3.120.178.18/

#### 1. Update all packages
```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get dist-upgrade
sudo apt-get install unattended-upgrades
sudo dpkg-reconfigure --priority=low unattended-upgrades
```

#### 2. Change timezone to UTC and Fix language issues 
```
sudo timedatectl set-timezone UTC
sudo update-locale LANG=en_US.utf8 LANGUAGE=en_US.utf8 LC_ALL=en_US.utf8
```

#### 3. Create a new user grader and Give him `sudo` access
```
sudo adduser grader
sudo nano /etc/sudoers.d/grader 
```
Then add the following text `grader ALL=(ALL) ALL`

#### 4. Setup SSH keys for grader
* On local machine 
```
ssh-keygen
```
```
sudo su - grader
mkdir .ssh
touch .ssh/authorized_keys 
sudo chmod 700 .ssh
sudo chmod 600 .ssh/authorized_keys 
nano .ssh/authorized_keys 
```

#### 5. Change the SSH port from 22 to 2200 | Enforce key-based authentication | Disable login for root user
```
sudo nano /etc/ssh/sshd_config
```

#### 6. Configure the Uncomplicated Firewall (UFW)
```
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 2200/tcp
sudo ufw allow www
sudo ufw allow ntp
sudo ufw allow 8000/tcp  `serve another app on the server`
sudo ufw enable
```

#### 7. Install Apache2 and mod-wsgi for python3 and Git
```
sudo apt-get install apache2 libapache2-mod-wsgi-py3 git
```

#### 8. Install and configure PostgreSQL
```
sudo apt-get install libpq-dev python3-dev
sudo apt-get install postgresql postgresql-contrib
sudo su - postgres
psql

CREATE USER catalog WITH PASSWORD 'password';
CREATE DATABASE catalog WITH OWNER catalog;
\c catalog
REVOKE ALL ON SCHEMA public FROM public;
GRANT ALL ON SCHEMA public TO catalog;
\q
exit
```

#### 9. Clone the Catalog app from GitHub and Configure it

#### 10. Configure apache server

#### 11. Reload & Restart Apache Server
