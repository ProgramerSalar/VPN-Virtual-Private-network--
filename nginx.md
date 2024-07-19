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
        }
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



## 0.3 NGINX: Lab - Rewrite and Return

➤ Return & Rewrite: Both for one of two purposes- 

➤ First, if a URL has changed. 

➤ To control the request within Nginx. 
For example, a request can be forwarded to an application 
if the content will be generated dynamically. 

➤ Return Directive - Return is the simpler directive to use 
compared to NGINX rewrite. 

➤ Return must be enclosed within a server or location block 
which defines which URLs should be rewritten. 

![alt text](<Screenshot 2024-07-19 143618.png>)




➤ Rewrite Directive - This directive needs to be in a location or server block in 
order to rewrite the URL. 

➤ Rewrite directive can be used to perform more granular tasks as with it you can 
perform more complicated URL distinctions such as: 

➤ Capture elements in the original URL 

➤ Change or add elements in the path 

➤ Syntax : 

**rewrite regex URL [flag];**

➤ Rewrite directive does not send a redirect to the client in all cases. If the 
rewritten URL matches with another following directive, Nginx will rewrite the 
URL again.

➤ A rewrite directive will only return an HTTP 301 or 302 status code. If another 
status code is required, a return directive is needed after the rewrite directive.


```
rewrite ^/guest/\w+ /welcome;

rewrite ^/user/(\w+) /welcome/$1;

location = /welcome/manish{
    return 200 "Hello Manish, How are You";
}

location /welcome{
    return 301 assets/images/about/profile_image.jpg;
}
```

see here i put the url like this: **http://213.210.36.105/welcome/manish** Then i get the content of location which is i set


full code are here: 
```
http {

    include mime.types;

    server {

        listen 80;
        server_name 213.210.36.105;

        root /bloggingtemplate/;

        if ( $arg_name = 'manish' ){
                return 200 "Yes, I am Manish";
        }

        rewrite ^/guest/\w+ /welcome;

        rewrite ^/user/(\w+) /welcome/$1;

        location = /welcome/manish{
                return 200 "Hello Manish, How are You";
        }

        location /welcome{
                return 301 assets/images/about/profile_image.jpg;
        }

        location /about{

                return 200 "Hello, This is About Page";

                }

        }



}
```


## 0.4 NGINX: Lab - try_files in NGINX Conf



➤ try_files: The try_files directive can be used to check 
whether the specified file or directory exists. 


➤ NGINX makes an internal redirect if it does, or returns a 
specified status code if it doesn’t.


```
// try files are find the first resorces that is /testobject if not found then goes to next resources /video that location are found 
try_files /testobject /video;   // in here have two resource path are found 


location /video {
    return 200 "Enjoy the Movie";
}
```
same to like this: 
```

try_files /assets/images/about/profile_image.jpg /video;   


location /video {
    return 200 "Enjoy the Movie";
}
```
and also this: 

```
// try files are find the first resorces that is /testobject if not found then goes to next resources /video that location are found 
try_files /testobject /video;   // in here have two resource path are found 


location /video {
    return 200 "Enjoy the Movie";
}
```
same to like this: 

```

try_files $uri /assets/images/about/profile_image.jpg /video;   //here are three rescources found 


location /video {
    return 200 "Enjoy the Movie";
}
```
and also include the 404 

```

try_files $uri /assets/images/about/profile_image.jpg /video /404;   //here are four rescources found 


location /video {
    return 200 "Enjoy the Movie";
}

location /404{
    return 404 "Sorry, this resources not exists";
}
```


## 0.5 Logging in Nginx

```
/var/log/nginx  // here is the log file that is error.log and access.log 

ll -lsh    // check the size of the file 
tail -f *   // tell the file 

```

- if you create the access.log and error.log in particular file 

```
location /userdata {
    access_log /var/log/nginx/access_user.log;
    return 200 "user data are found here";
}
```
Along with you off the access log
```
location /userdata {
    access_log off;
    return 200 "user data are found here";
}
```


## 0.6 Handle Dynamic Requests
- i will do this leter 


## 0.7 Worker Process | Load Optimization


➤ NGINX default worker process are 1. 

➤ Worker Processes can be defined via directive 
worker_processes. 

➤ This directive is responsible for letting our virtual server 
know how many workers to spawn once it has become bound 
to the proper IP and port(s).

➤ It is common practice to run 1 worker process per core CPU. 

➤ Anything above 1 Process per CPU won’t hurt your system, 
but it will leave idle processes usually just lying about.

➤ Worker process directive belongs to Global Context.

➤ Worker Connection: This directive tells our worker processes 
how many people can simultaneously be served by Nginx.

➤ This is the child of Events Context. 

➤ The default value is 768; however, considering that every 
browser usually opens up at least 2 connections/server, this 
number can half.

➤ User can check our core’s limitations by issuing a ulimit 
command: 

```
ulimit -n 
```

➤ worker_connections 1000 - This means, NGINX can server 
1000 clients/second.


* check the how much master and worker process are found 
```
ps -aux --forest | grep nginx
````
output: 
```
root        9811  0.0  0.0   6144  2048 pts/0    S+   11:55   0:00  |       \_ tail -f /var/log/nginx/access.log /var/log/nginx/access_user.log /var/log/nginx/error.log
root       10723  0.0  0.0   7076  2176 pts/1    S+   13:03   0:00          \_ grep --color=auto nginx
root        7485  0.0  0.0  11196  4236 ?        Ss   05:12   0:00 nginx: master process /usr/sbin/nginx -g daemon on; master_process on;   // this is one master process 
nobody      9814  0.0  0.0  12708  4664 ?        S    11:56   0:00  \_ nginx: worker process  // this is worker process 
```


```
// process are here 
worker_processes 4;

events {

}

http {

    include mime.types;

    server {

        listen 80;
        server_name 213.210.36.105;

        root /bloggingtemplate/;

        location /userdata {
                access_log /var/log/nginx/access_user.log;
                return 200 "user data are found here";
                }

        }



}
```

- i will check the process 
```
nginx -t 
nginx -s reload 
ps -aux --forest | grep nginx
```
output: 
```
root        9811  0.0  0.0   6144  2048 pts/0    S+   11:55   0:00  |       \_ tail -f /var/log/nginx/access.log /var/log/nginx/access_user.log /var/log/nginx/error.log
root       10742  0.0  0.0   7076  2176 pts/1    S+   13:09   0:00          \_ grep --color=auto nginx
root        7485  0.0  0.0  11196  4236 ?        Ss   05:12   0:00 nginx: master process /usr/sbin/nginx -g daemon on; master_process on;
nobody     10737  0.0  0.0  12716  4152 ?        S    13:09   0:00  \_ nginx: worker process
nobody     10738  0.0  0.0  12716  4280 ?        S    13:09   0:00  \_ nginx: worker process
nobody     10739  0.0  0.0  12716  4152 ?        S    13:09   0:00  \_ nginx: worker process
nobody     10740  0.0  0.0  12716  4280 ?        S    13:09   0:00  \_ nginx: worker process
```

- check your computer configuration 
```
lscpu   // check configuration of cpu
nproc   // same 
```

- you should auto the process, Automatically configure how much cpu found in you machine 

```
worker_processes auto;
```


- check the how much operation can be handled by thie particular machine, default is 1024
```
ulimit -n
```


- i will include this operation 
```
events {
    worker_connections 1024;
}
```

whole nginx.conf file code: 
```
worker_processes auto;
events {
    worker_connections 1024;
}

http {

    include mime.types;

    server {

        listen 80;
        server_name 213.210.36.105;

        root /bloggingtemplate/;

        location /userdata {
                access_log /var/log/nginx/access_user.log;
                return 200 "user data are found here";
                }

        }



}
```

## 0.8 NGINX: Performance Optimization

➤ Buffer : Buffer Size is another aspect to manage the NGINX 
Performance

![alt text](<Screenshot 2024-07-19 190157.png>)

➤ If the buffer sizes are too low, then Nginx will have to write to 
a temporary file causing the disk to read and write constantly.

➤ client_body_buffer_size: This handles the client buffer size, 
meaning any POST actions sent to Nginx. POST actions are 
typically form submissions. 

➤ client_header_buffer_size: Similar to the previous directive, 
only instead it handles the client header size.

➤ client_max_body_size: The maximum allowed size for a 
client request. If the maximum size is exceeded, then Nginx 
will spit out a 413 error or Request Entity Too Large. 

➤ large_client_header_buffers: The maximum number and 
size of buffers for large client

➤ Timeouts - 

➤ client_body_timeout and client_header_timeout directives 
are responsible for the time a server will wait for a client body 
or client header to be sent after request. If neither a body or 
header is sent, the server will issue a 408 error or Request 
time out. 

➤ keepalive_timeout assigns the timeout for keep-alive 
connections with the client. Nginx will close connections with 
the client after this period of time.

➤ send_timeout is established not on the entire transfer of answer , buy only between two operations of reading; if after this time client will tke nothing, then Nginx is shutting down the connection.

➤ Gzip Compression - 

➤ Gzip can help reduce the amount of network transfer Nginx 
deals with. However, be careful increasing the 
gzip_comp_level too high as the server will begin wasting cpu 
cycles.

![alt text](<Screenshot 2024-07-19 195500.png>)

➤ Static File Caching - 

➤ It’s possible to set expire headers for files that don’t change 
and are served regularly. This directive can be added to the 
actual Nginx server block.

![alt text](<Screenshot 2024-07-19 195547.png>)


```
http {

    include mime.types;

    # Buffer size for POST submissions 
    client_body_buffer_size 10k;    // it means it will accept up to the all the post request which have the vody size within the 10 kilobytes they will served as a buffer, the post request whcih have the size more than 10 kilobytes will not be served as a buffer like: 100; -> bite 100k; -> kilobyte 100M; -> MegaByte
    client_max_body_size 8m;   // if client body will exceed 8 megabyte then you will get the response 413 request is too large

    # Buffer size for Headers 
    client_header_buffer_size 1k;

    # Max time to recieve client headers/body
    client_body_timeout 12;    // 12 milisecond
    client_header_timeout 12;

    # Max time to keep a connection open for 
    keepalive_timeout 15;

    # Max time for the client accept/receive a response 
    send_timeout 10;

    # skip buffering for static files
    sendfile on;

    location ~(jpg|jpeg|png|gif|ico|css|js)$ {
        expires 365d;
    }



}
```



## 0.8 Add Dynamic Modules 

- i will do leter

## 0.9 Reverce Proxy

➤ Reverse Proxy: A reverse proxy is a server that sits in front 
of web servers and forwards client (e.g. web browser) 
requests to those web servers.

➤ Reverse proxies are typically implemented to help 
increase security, performance, and reliability. 

![alt text](<Screenshot 2024-07-19 210126.png>)

➤ Benefits of Reverse Proxy - 

➤ Load Balancing - A popular website that gets millions of users every 
day may not be able to handle all of its incoming site traffic with a 
single origin server. Instead, the site can be distributed among a pool 
of different servers, all handling requests for the same site.

➤ A reverse proxy can provide a load balancing solution which will 
distribute the incoming traffic evenly among the different servers to 
prevent any single server from becoming overloaded.

➤ Protection from attacks - With a reverse proxy in place, a web site 
or service never needs to reveal the IP address of their origin 
server(s). This makes it much harder for attackers to leverage a 
targeted attack against them such as DOS attack, CDN attack and 
more

➤ Benefits of Reverse Proxy - 

➤ Caching - A reverse proxy can also cache content, resulting in 
faster performance. 

➤ SSL encryption - Encrypting and decrypting SSL 
communications for each client can be computationally 
expensive for an origin server. A reverse proxy can be 
configured to decrypt all incoming requests and encrypt all 
outgoing responses, freeing up valuable resources on the 
origin server.


![alt text](<Screenshot 2024-07-19 212046.png>)



