# Haproxy_certbort_-deployment
Haproxy_certbort_ deployment
# Description

HAProxy is a powerful load balancer that distributes incoming traffic across multiple servers, improving availability and reliability. By applying SSL/TLS certificates, we secure client-to-server communication with encryption, ensuring data privacy and trust.

In this setup, we configured HAProxy as the entry point for traffic, terminating SSL/TLS connections and forwarding requests securely to backend servers. Certificates were generated and managed using Let’s Encrypt, an automated, free, and widely trusted Certificate Authority.

**TOOLS**

1. VMS
2. Ngnix
3. Haproxy software
4. letsencrpyt certificate
5. domain name

**Steps 1**

**Create Instances**

- Launch 3 virtual machines (VMs) / cloud instances:
    - 1 HAProxy load balancer
    - 2 backend web servers
- Ensure they are in the same VPC / network.
- Assign a public IP / private IP

![image alt](https://github.com/marahgrace1583-source/Haproxy_certbort_-deployment/blob/a58d049878b1f9504bf722cf5b481be241f2a9da/Screenshot%202025-09-10%20143002.png)

**Steps 2**

**patch the system and install nginx**

- using this command on both server

```bash
sudo apt update && sudo apt upgrade -y
```

- Install Nginx using this command

```bash
       sudo apt install nginx  -y
```

Let’s check the status now. 

```bash
        Sudo systemctl status nginx
```

And also check the web content 

```bash
Cd  /var/www/html 
```

using LL to check the list 

on both server

![image alt](https://github.com/marahgrace1583-source/Haproxy_certbort_-deployment/blob/6a7404a85a99c72411d34db4383afca8ac6521cf/Screenshot%202025-09-10%20162706.png)

![image alt](https://github.com/marahgrace1583-source/Haproxy_certbort_-deployment/blob/fb6a289e40e6f3664a984c48699e4f1f75eef81b/Screenshot%202025-09-10%20163642.png)

![Screenshot 2025-09-10 173011.png](attachment:a0ee7295-eb6e-45df-b508-4b412c7b2899:430f607a-6d2e-47bf-9df7-157b161ec120.png)

**Steps 3**

Uploading files of  the templates from local computer to server using home directory 

- First we will change directory using this command

```bash
                     cd
```

```bash
                   Pwd 
```

- Create a folder using this command

```bash
          mkdir webcontent 
```

- To check what we downloaded

```bash
                    ls 
```

```bash
          Cd webcontent/
```

![Screenshot 2025-09-10 174047.png](attachment:09ee572c-5ccf-4494-8d1e-3a14631dc33a:ccb3a5d9-837c-4711-b1e1-bfeaf8bc15d0.png)

**Steps 4** 

We unzip the file because it was a zip file 

- Install unzip

```bash
            Sudo apt install unzip
```

Then you unzip the file with this command 

                Unzip FreshCart-1.0.0.zip

Use Ls to see the zip and unzip file , and then go back to remove the zip file using this command 

```bash
Sudo rm with the zip file 
```

To make it available for ngnix to serve to the public we need to send to a particular location using this command 

```bash
Cd /var/www/html/
```

LL

To change the ngnix to the template we downloaded we will use this command 

Cd 

```bash
Cd webcontent / 
```

LL

Cd with the template name we downloaded 

Pwd

Ls

To move all the templates content to the location 

```bash
Sudo  cp  -r  ./*   /var/www/html /
```

```bash
Cd  /var/www/html/
```

To remove the old file 

Sudo rm index.ngnix-Debian.html 

To check the full content of the file 

Ll   -la

![Screenshot 2025-09-10 174559.png](attachment:08b4fa50-291d-47b9-ab91-142ca8e9040b:Screenshot_2025-09-10_174559.png)

**Step 5**

Turn off ip address 

Go to instance > action >network>manage Ip address >disable public ip address for both serve 1 and 2 

![Screenshot 2025-09-10 182605.png](attachment:55692ac3-d410-4ae9-850d-5c1191aa70ef:Screenshot_2025-09-10_182605.png)

**Step 6** 

Haproxy 

To install Haproxy we will use this command 

```bash
Sudo apt install Haproxy -y
```

To check the list 

```bash
Ls  -la  /etc/
```

```bash
Cd  /etc/ haproxy 
```

```bash
ls    -lla
```

To modify 

```bash
Sudo nano haproxy.cfg
```

To edit 

frontend http_front
       bind *:80
       backend_webserver webservers

backend webservers

        balance roundrobin

        option httpchk GET 

        server app1 10.0.2.142:80 check

To validate  configuration 

```bash
sudo haproxy   -c  -f  /etc/haproxy/haproxy.cfg
```

To restart 

```bash
sudo systemctl restart haproxy
```

![Screenshot 2025-09-10 182743.png](attachment:6170e1bc-b1d8-4ca6-836e-517a3a356f66:Screenshot_2025-09-10_182743.png)

![Screenshot 2025-09-11 110035.png](attachment:aaad0d60-ba93-484a-b02b-547b1ca2df53:Screenshot_2025-09-11_110035.png)

**step 7**

**Domain name and a record** 

Go to your website name cheap ===Advanced DNS===Arecord  and input your ip address and C record and  add your domain name  

![Screenshot 2025-09-11 114159.png](attachment:3a0730d2-7be8-4ce4-a89e-1f52841f0727:Screenshot_2025-09-11_114159.png)

**step 8**

**install certificate**

we have to install certificate on Haproxy using this command 

```bash
sudo apt install certbot -y
```

To apply for certificate we have to stop the  haproxy service using this command

```bash
sudo systemctl Stop haproxy service
```

To sign the certificate 

marahs.online  & [www.marahs.online](http://www.marahs.online) you have to use this command 

certbot certonly --standalone -d  marahs.online  -d  [www.marahs.online](http://www.marahs.online)  - -email amarah54809@gmail.com

lets check 

```bash
cd /etc /letsencrypt
```

LL

![Screenshot 2025-09-11 121634.png](attachment:ac5044cf-6015-4ae0-a58a-0b2e24c96601:70d6ed96-11d0-4f79-ae03-b557c471da4c.png)

**this file will be generated**

/etc/letsencrypt/live/marahs.online/fullchain.pem
/etc/letsencrypt/live/marahs.online/privkey.pem

# after generating the cert from LetEncrypt
we cat it together in single file

cat /etc/letsencrypt/live/marahs.online/fullchain.pem  /
/etc/letsencrypt/live/marahs.online/privkey.pem > trusted-cloud.pem

# edit the haproxy.cfg
nano /etc/haproxy/haproxy.cfg

# redirecting to https

======================
frontend http_in
bind *:80
redirect scheme https

# applying ssl and linking the certificate location

frontend https_front
       bind *:443 ssl crt /etc/haproxy/trusted-cloud.pem alpn h2,http/1.1
       mode http
       option httplog
       default_backend web_back

backend web_back
        mode http
        balance roundrobin
        server app1 10.0.2.142:80 check

![Screenshot 2025-09-11 153931.png](attachment:92b333bf-eec4-4963-a7c4-ff06b456aa5d:Screenshot_2025-09-11_153931.png)

 **Key Notes / Lessons Learned**

- Always back up HAProxy configuration before changes.
- Use Let’s Encrypt for production, self-signed only for testing.
- Configure health checks on backend servers to avoid serving downed apps.
- Redirect HTTP → HTTPS to enforce secure connections.
- Regularly renew certificates (Certbot auto-renew works best).
- Keep HAProxy updated to avoid security vulnerabilities.

 Best practice:
•	Frontend: bind to public IP (or *) if clients come from outside.
•	Backend: use private IPs for your servers.
