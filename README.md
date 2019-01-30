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
