## Linux-Server-Configuration
By Prasanna Raj Mallipudi

## Get a server

### Steps

#### 1: New Ubuntu Linux server instance on Amazon EC2

- Login to aws.amazon.com and login to default user (ubuntu)
- Choose EC2 and Launch Instance.
- Check for instance `IPv4 public IP - 52.202.47.66`
  we can download a `.pem File` and connect with following command
- `ssh -i camcatalog_30_01_2019.pem ubuntu@52.202.47.66`
- `22` is Port by Default,Later we need to Change `2200`.

#### 2: Update and upgrade installed packages
  - sudo apt-get update
  - sudo apt-get upgrade
  - sudo apt-get install unattended-upgrades

#### 3. Change SSH port from 22 to 2200

 - Edit File: `sudo vi /etc/ssh/sshd_config`
 - Change Port number 22 to 2200
 - Save and exit using esc and confirm with :wq
 - Restart: `sudo service ssh restart`
 - Change inbound rules in Amazon EC2
 - Type : Custom TCP Rule as `2200`
 - To check port 2200: Working or Not
 - `ssh -i camcatalog_30_01_2019.pem -p 2200 ubuntu@52.202.47.66`
 
#### 4: Configure Firewall (UFW)

 - Configure the default firewall allow incoming connections for `SSH (port 2200)` , `HTTP (port 80)` and `NTP (port 123)`
 
   `sudo ufw status                 
   `sudo ufw default deny incoming`   
   `sudo ufw default allow outgoing  
   `sudo ufw allow 2200/tcp          
   `sudo ufw allow www               
   `sudo ufw allow 123/udp           
   `sudo ufw deny 22                
