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
 
   `sudo ufw status`                
   `sudo ufw default deny incoming`   
   `sudo ufw default allow outgoing`  
   `sudo ufw allow 2200/tcp`          
   `sudo ufw allow www`               
   `sudo ufw allow 123/udp`           
   `sudo ufw deny 22`
  
  - `sudo ufw enable`
  - After Proceed Option Y/N - Y
  - `sudo ufw status`
```  
Status: active

To                         Action      From
--                         ------      ----
2200/tcp                   ALLOW       Anywhere                  
80/tcp                     ALLOW       Anywhere                  
123/udp                    ALLOW       Anywhere                  
22                         DENY        Anywhere                  
2200/tcp (v6)              ALLOW       Anywhere (v6)             
80/tcp (v6)                ALLOW       Anywhere (v6)             
123/udp (v6)               ALLOW       Anywhere (v6)             
22 (v6)                    DENY        Anywhere (v6)
```

#### 5: Create a ``New User`` account named `grader`

 - While logged in as ubuntu, add user: sudo adduser grader
 - Enter a password
 - Edits the sudoers file: `sudo visudo`
   ``root    ALL=(ALL:ALL) ALL``
 
 - add a new line to give sudo privileges to grader user
   ```
   root    ALL=(ALL:ALL) ALL
   grader  ALL=(ALL:ALL) ALL
   ```
 - Save and exit using ctrl+x and confirm with Y.
 
 - Run su - grader, enter the password
 
#### 6: Create an SSH key pair for grader

- create .ssh folder by mkdir /home/grader/.ssh
- su grader
- RUN this command `sudo cp /home/ubuntu/.ssh/authorized_keys /home/grader/.ssh/authorized_keys`
- change Ownership ``chown grader.grader /home/grader/.ssh``
- `sudo su` and goto root directorie after next command
- sudo Group add `usermod -aG sudo grader`
- change permissions for .ssh folder ``chmod 0700 /home/grader/.ssh/``, for authorized_keys ``chmod 644 authorized_keys``
- Check in ``vi /etc/ssh/sshd_config`` file if `PermitRootLogin is set to No`
- Restart SSH: `sudo service ssh restart`
- grader account Working or Not by RUNNING this command : `ssh -i camcatalog_30_01_2019.pem -p 2200 ubuntu@52.202.47.66`
- Configure the local timezone to UTC Logged On grader Account
- TIME ZONE: ``sudo dpkg-reconfigure tzdata``. Choose time zone UTC

####  Deploying the project Steps



 
 
