# How To Host MERN Website in VPS server 

## 0.0 clone the repo 

Note : Make sure you are in this directory: **root@srv563446:/**

```
git clone <url>
```

## 0.1 setup our Backant 

[A].
```
npm i
```
[b].
```
npm i -g pm2     // install the pm2
pm2 start npm --name "test_server" -- start
pm2 ls
pm2 logs test_server     // start the server
```

[C]. check the server is Run 
```
curl http://localhost:9000
```


## 0.2 setup the Frontant 

[A].
```
npm i
```
[B].
```
npm run build

```
[c].   start your website. 
```
npm run preview -- --host
```


## 0.3 setup the Nginx 

[A].
```
/etc/nginx/sites-enabled# 
rm default 
```

[B].  root@srv563446:/etc/nginx/sites-enabled# cat 213.210.36.105.conf
```
server {
    listen 80;
    root /noise/noise-frontant/dist;
    noise-frontant/dist#

    location / {
        try_files $uri $uri/ =404;
    }
}
```

[C].  create A symbolic Link
```
root@srv563446:/etc/nginx/sites-enabled# ln -s ../sites-available/213.210.36.105.conf
```

[D]. 
-- > dirctory **root@srv563446:/etc/nginx/sites-enabled#**

```
systemctl restart nginx
```

## 0.4 Backent Domain setup 

[A].
```
server {
    listen 80;
    server_name api.gnoise324.online www.api.gnoise324.online;

    add_header 'Access-Control-Allow-Origin' 'http://localhost:5173';
    add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
    add_header 'Access-Control-Allow-Headers' 'Content-Type';
    add_header 'Access-Control-Allow-Credentials' 'true';

    location / {
        root /noise/noise-frontant/dist;
        try_files $uri $uri/ /index.html;
    }

    location /product_details/ {
        proxy_pass http://localhost:9000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }

    location /hot-selling-product-collection {
        try_files $uri $uri/ /index.html;
    }

    location /test{
        return 200 "Hello world";
    }
}
```

[B].
create a sybolic link 




# SSL cirtificate 

[A].

```
openssl genrsa -des3 -out myhostname.key 4096 
```
password: Manish@#$6090


```
openssl req -new -key myhostname.key -out myhostname.csr
```


lock the password of myhostname.key.pw
```
openssl rss -in myhostname.key.pw out myhostname.key
```


- take sertificate 
```
openssl x509 -req -in myhostname.csr -signkey myhostname.key -out myhostname.crt

```

- store the cirtificate in /etc/nginx/ssl

```
sudo mkdir /etc/nginx/ssl
```

- copy thise files 

```

root@srv563446:~# sudo cp myhostname.key /etc/nginx/ssl
sudo cp myhostname.crt /etc/nginx/ssl
```



## How to add ssl certificate 


[A]. you should install the cert like this command.
```
sudo add-apt-repository ppa:certbot/certbot
```

output: 
```
Reading package lists... Done
E: The repository 'https://ppa.launchpadcontent.net/certbot/certbot/ubuntu jammy Release' does not have a Release file.
N: Updating from such a repository can't be done securely, and is therefore disabled by default.
N: See apt-secure(8) manpage for repository creation and user configuration details.
```

[B]. update you configuration 

```
sudo apt-get update
```


[c]. Install the python dependency of certbot 
```
sudo apt-get install python3-certbot-nginx
```
output: 
```
Restarting services...
Service restarts being deferred:
 systemctl restart networkd-dispatcher.service
 systemctl restart unattended-upgrades.service
Service restarts being deferred:
 systemctl restart networkd-dispatcher.service
 systemctl restart unattended-upgrades.service

No containers need to be restarted.

No user sessions are running outdated binaries.

No VM guests are running outdated hypervisor (qemu) binaries on this host.
```

[D].  add you domain in the certbot
```
sudo certbot --nginx -d gnoise.shop -d www.gnoise.shop
```
- you might be enter the email and aggree the term and condition.


output: 
```
Successfully received certificate.
Certificate is saved at: /etc/letsencrypt/live/gnoise.shop/fullchain.pem
Key is saved at:         /etc/letsencrypt/live/gnoise.shop/privkey.pem
This certificate expires on 2024-10-28.
These files will be updated when the certificate renews.
Certbot has set up a scheduled task to automatically renew this certificate in the background.

Deploying certificate
Successfully deployed certificate for gnoise.shop to /etc/nginx/sites-enabled/gnoise.shop.conf
Successfully deployed certificate for www.gnoise.shop to /etc/nginx/sites-enabled/gnoise.shop.conf
Congratulations! You have successfully enabled HTTPS on https://gnoise.shop and https://www.gnoise.shop

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
If you like Certbot, please consider supporting our work by:
 * Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
 * Donating to EFF:                    https://eff.org/donate-le
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
```


[E]. this certificate is valid only for 90 days, so after 90 days you reniew this certificate 
- if you can put this command then after 90 automatically reniew 

```
certbot renew --dry-run
```
output: 
```
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Processing /etc/letsencrypt/renewal/gnoise.shop.conf
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Account registered.
Simulating renewal of an existing certificate for gnoise.shop and www.gnoise.shop

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Congratulations, all simulated renewals succeeded:
  /etc/letsencrypt/live/gnoise.shop/fullchain.pem (success)
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
```