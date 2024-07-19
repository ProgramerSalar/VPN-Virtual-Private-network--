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


- i will include the types in nginx.conf but how many types you put in here so also nginx are give the folder mime.types in which you define the types of foler 


so i will remove the types in nginx.conf file 
```
events {

}

http {

    include mime.types;

    server {

        listen 80;
        server_name 213.210.36.105;

        root /bloggingtemplate/;

        }



}
```


And add the types in mime.types file

```
/etc/nginx# nano mime.types

```


## 0.1 NGINX: Lab - Location Context

[Location Context Pdf](<nginx-course-files/nginx-course-files/4.1 Location-Block-Context-in-NGINX.pdf>)

```
events {

}

http {

    include mime.types;

    server {

        listen 80;
        server_name 213.210.36.105;

        root /bloggingtemplate/;

        location /about{
            return 200 "Hello This is About Page";
        }

    }



}
```



➤ location_match: Defines what Nginx should check the request URI against.

➤ The existence or nonexistence of the modifier, affects the way that the Nginx attempts to match the location block.

➤ The modifiers below will cause the associated location block to be interpreted as follows: 

➤ (none): If no modifiers are present, the location is interpreted as a prefix match. This means that the location given will be matched against the beginning of the request 
URI to determine a match. 

➤ =: If an equal sign is used, this block will be considered a match if the request URI 
exactly matches the location given. 

```
 location = /about{
            return 200 "Hello This is About Page in the Exact Match";
        }
```

➤ ~: If a tilde modifier is present, this location will be interpreted as a case-sensitive 
regular expression match. 

```
 location ~ /about{
            return 200 "Hello This is About Page ";
        }
```

➤ ~*: If a tilde and asterisk modifier is used, the location block will be interpreted as a 
case-insensitive regular expression match. 

```
 location ~* /about{
            return 200 "Hello This is About Page ";
        }
```

➤ ^~: If a carat and tilde modifier is present, and if this block is selected as the best 
non-regular expression match, regular expression matching will not take place
```
 location ^~ /about{
            return 200 "Hello This is About Page ";
        }
```


➤ Priority of Modifiers: Nginx chooses the location that will be 
used to serve a request in a similar fashion to how it selects a 
server block. It runs through a process that determines the 
best location block for any given request. 

➤ Understanding this process is a crucial requirement in being 
able to configure Nginx reliably and accurately. 

1. Exact Match = URI 
2. Preferential Prefix Match ^~ URI 
3. REGEX Match. ~* URI


for example you can define location like this: 
```
 location ^~ /Home{
            return 200 "Hello This is About Page ";
        }
 location ^~ /Eduction{
            return 200 "Hello This is About Page ";
        }
 location ^~ /Skills{
            return 200 "Hello This is About Page ";
        }
 location ^~ /Experience{
            return 200 "Hello This is About Page ";
        }
```

[configuration techinology](<nginx-course-files/nginx-course-files/4.2 NGINX-Configuration-Terminology.pdf>)

[performance optimization](<nginx-course-files/nginx-course-files/4.3 NGINX-Performance-Optimization.pdf>)

[performance optimization 2](<nginx-course-files/nginx-course-files/4.4 NGINX-Performance-Optimization-2.pdf>)

[Rewrite and Return directories](<nginx-course-files/nginx-course-files/4.5 Rewrite-and-Return-Directives.pdf>)

[Try-files-in-nginx-conf](<nginx-course-files/nginx-course-files/4.6 try-files-in-NGINX-Conf.pdf>)

[variable in nginx.conf](nginx-course-files/nginx-course-files/4.7Variables-in-NGINX-Conf.pdf)

## 0.2 NGINX: Lab - Variable NGINX

➤ Variables: NGINX configuration can hold the variable and 
NGINX variable can hold the String Value. 

```
nano /etc/nginx/nginx.conf

 location  /find{
            return 200 "%hostname \n $args \n $connection_requests \n $nginx_version";  // in the stgring all are variable 
        }


// variable %hostname -> this is hostname of your VPN 
// $args   -> this is argument in here i don't pass it 
// $connection_requests   -> this is connection requestions means how many times your website are open in  ip address
// $nginx_version    -> this is nginx version which is you used 
// \n  -> this is new line 

```

➤ Set directive is used to assign variable in NGINX. 
set $a "hello world";  

➤ User can put conditional statements on basis of variables. 

```
if( $arg_name = 'manish' ){
                return 200 "Yes, I am Manish";
        }turn 200 "Yes, I am Manish"
}
```
for example: 
```
events {

}

http {

    include mime.types;

    server {

        listen 80;
        server_name 213.210.36.105;

        root /bloggingtemplate/;

        if( $arg_name = 'manish' ){
                return 200 "Yes, I am Manish";
        }

        location /about{

                return 200 "Hello, This is About Page";

                }

        }



}
```

see here:
> my url is like this: **http://213.210.36.105/?name=manish** if i put this **http://213.210.36.105/?name=tom** then condtion are not worked




➤ Few Important inbuilt variables: 
- $args - List of arguments on the request line. 
- $body_bytes_sent - Number of bytes sent to the client. 
- $bytes_received - Number of bytes received from a client.
- $connection_requests - Current number of requests made 
via connection. 
-$date_local - Current time in the local time zone.
-$hostname - Host name . 
-$nginx_version - Shows the nginx version.