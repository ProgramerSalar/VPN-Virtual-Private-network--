# Ngnix 


- some basic command 

```
sudo apt install nginx
sudo systemctl status nginx 
sudo apt-get autoremove --purge    // unintall the nginx 


// nginx are found in etc folder 
cd /etc/nginx 

// knwn the nginx file 
ls -l /etc/nginx/ 

tail -f *   // see the file content 

ls   // know the description 


ps aux | grep nginx    // known which things are running in the nginx 

ps -ef | grep nginx   // known which things are running in the nginx 

```

---> if you uninstall the nginx then follow this documentation: https://gcore.com/learning/how-to-uninstall-nginx-ubuntu/


- nginx.conf  -> we have this file in the nginx this is configuration file 
```
vim nginx.conf    // open the nginx configuration file 

```

- configuration of nginx:

![configuration of nginx](<WhatsApp Image 2024-07-18 at 11.23.09_84232e8f.jpg>)



```
sudo nginx -t   \\ if you do any chanages in file 
sudo systemctl reload nginx 
```


- let's find out ip of my machine 
```
ifconfig 
```



### 0.0 Add NGINX Process in Systemd Services to make the WebServer Resilient.

1. NGINX Init Services File.
https://www.nginx.com/resources/wiki/start/topics/examples/systemd/

2. Start Service
```
systemctl start nginx
```
3. Stop Service
```
systemctl stop nginx
```
4. Restart-Service
```
systemctl restart nginx
```
5. Check Service Status
```
systemctl status nginx
```
6. Enable service, auto-start on boot
```
systemctl enable nginx
```


- suppose that you reboot the nginx 

```
reboot   // if you reboot the machine then you should wait some minute 
```

### 0.1  uploading the normal index.html file 

- make sure. upload the folder in this directory [ root@srv563446:/# ] not for this [root@srv563446:~# ] if you are in this directory and upload the index.html folder then not worked. 


- we will work in /etc/nginx/conf.d file  so make a backup file 
```
cd /etc/nginx
mv nginx.conf nginx_backup.conf   // move  the file 
cp nginx_backup.conf nginx.conf   // copy the file 
```


add the content of nginx.conf file: 
```
events {

}

http {

    server {

        listen 80;
        server_name 213.210.36.105;      // this is my ip address 

        root /media/bloggingtemplate/;    // this is file which is my website 

        }



}
```

```

nginx -t            // check the nginx 
systemctl reload nginx   // realod the nginx 


```

```
nginx -s reload     // reload command of nginx 
```
-> get this 2024/07/18 15:12:59 [notice] 3511#3511: signal process started


* start the website 
```
cat /var/log/nginx/*
tail -f /var/log/nginx/*    // tail where is the problem if have the problem in you web 
```
Note: nginx are recognized the css and js file but not get these file so only row html are found.

- known the partucular file are 
```
 curl -I http://213.210.36.105/assets/css/bootsnav.css
```
get this 
```
HTTP/1.1 200 OK    // status is ok 
Server: nginx/1.24.0 (Ubuntu)
Date: Fri, 19 Jul 2024 05:33:47 GMT
Content-Type: text/plain       // my css file are not text plain 
Content-Length: 35191
Last-Modified: Fri, 19 Jul 2024 05:09:50 GMT
Connection: keep-alive
ETag: "6699f51e-8977"
Accept-Ranges: bytes
```


so i will changes in nginx.conf file 
```
events {

}

http {

    

    server {

        listen 80;
        server_name 213.210.36.105;

        root /bloggingtemplate/;

        }



}

```
To 
```
events {

}

http {

    types{
            text/html html;     // add the types of html 
            text/css  css;      // add the types of css 
        }

    server {

        listen 80;
        server_name 213.210.36.105;

        root /bloggingtemplate/;

        }



}
```

and reload the page
```
nginx -s reload
```