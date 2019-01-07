# Linux Web Server Configuration:
- The objective is to turn a brand-new, bare bones, Linux server into the secure and efficient web application host your applications need.
- Amazon Elastic Compute Cloud (Amazon EC2) is a web service that provides secure, resizable compute capacity in the cloud. It is designed to make web-scale cloud computing easier for developers.
- Server Public IP: http://ec2-52-14-146-163.us-east-2.compute.amazonaws.com/

# Configuration Steps
 
### Add a grader user.

- `sudo adduser grader`
- `su - grader`
--------------------------
### Generate a key pair
- `mkdir .ssh && chmod 700 `
- `cd ~/.ssh`
- `touch authorized_keys`
- `chmod 600 authorized_keys`
- Download grader user key pair form aws.
form my machine (client machine) change file permission:
`chmod 400 grader.pem`

- Run `ssh-keygen -y` : 

a prompt message will appear:
Enter file in which the key is (/Users/g.almuhanna/.ssh/id_rsa): grader.pem

a public key will show. 
copy the public key and place it on the server: 
`vi authorized_keys`

- After saving the changes, login as grader user:
 `ssh -p 2200 -i grader.pem grader@ec2-52-14-146-163.us-east-2.compute.amazonaws.com`
 
 ---------------------------
### Firewall
Deny all the incoming requests:
- `sudo ufw default deny incoming`

Allow all the outgoing requests
- `sudo ufw default allow outgoing`

Allow ssh with different default port
- `sudo ufw allow 2200/tcp`
- `sudo ufw allow 2200/udp`

Allow http port
- `sudo ufw allow www`

Allow ntp port

- `sudo ufw allow 123/tcp`
- `sudo ufw allow 123/udp`

Enable firewall
- `sudo ufw enable`

----------------------------------
### Application deployment 

installing gunicorn: (Python WSGI HTTP Server for UNIX.)
- `pip install gunicorn`
- `sudo gunicorn -w 4 -b 0.0.0.0:80 myApp:app`

System packages have been updated to most recent versions:
- `sudo apt-get update`
- `sudo apt-get upgrade`


