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



## Install Nginx from Source Code 

- Q. what is the need of install the nginx in source code

Installing NGINX from source code offers several benefits, especially if you need more control over your web server setup. Here are some key reasons why you might choose to install NGINX from source:

### Benefits of Installing NGINX from Source

1. **Customization**:
   - **Add or Remove Modules**: You can include specific modules you need and exclude those you don't, optimizing performance and reducing the attack surface¹.
   - **Custom Configuration**: Tailor the configuration options and compile-time parameters to suit your specific requirements¹.

2. **Latest Features and Updates**:
   - **Newest Version**: Access the latest stable or mainline versions of NGINX, which might not be available in prebuilt packages².
   - **Security Patches**: Apply the latest security patches and updates immediately².

3. **Performance Optimization**:
   - **Optimized Compilation**: Compile NGINX with optimizations specific to your hardware and operating system, potentially improving performance³.

4. **Flexibility**:
   - **Custom Installation Paths**: Define custom installation paths and directories, which can be useful for specific deployment environments¹.
   - **Third-Party Modules**: Easily integrate third-party modules that might not be included in prebuilt packages².

### Example Use Cases
- **High-Traffic Websites**: Customizing NGINX for performance and security can be crucial for high-traffic websites.
- **Specialized Applications**: Applications requiring specific modules or configurations not available in standard packages.
- **Development and Testing**: Developers might need the latest features and patches for testing and development purposes.

### Summary
While installing NGINX from prebuilt packages is quicker and easier, compiling from source provides greater control, customization, and access to the latest features and updates. This approach is particularly beneficial for advanced users and specific use cases where performance, security, and flexibility are paramount¹²³.

> see the pdf
[text](<nginx-course-files/nginx-course-files/2.1 Text Direction  Building NGINX from Source Code .pdf>)

- make sure you are in root directory 
```
root@srv563446:~#
```

not for this directory 
```
root@srv563446:/#
```

1. Update Packages
```
Ubuntu - apt-get update
CentOS - yum update
```

2. Download the NGINX source code from nginx.org
```
wget https://nginx.org/download/nginx-1.19.1.tar.gz
```

3. Unzip the file.
```
tar -zxvf nginx-1.19.1.tar.gz
```

4. Configure source code to the build.
```
cd nginx-1.19.1
./configure
```
Note: if you get the error: ./configure: error: C compiler cc is not found

5. Install code compiler
```
Ubuntu : apt-get install build-essential
CentOS : yum groupinstall "Development Tools"
```

6. Configure source code to the build.
```
./configure
```
Note: if get error Again: the HTTP rewrite module requires the PCRE library.

7. GET Support Libraries
```
Ubuntu : apt-get install libpcre3 libpcre3-dev zlib1g zlib1g-dev libssl-dev make
CentOS : yum install pcre pcre-devel zlib zlib-devel openssl openssl-devel make
```

8. Configure source code to the build.
```
./configure
```
output:
```
Configuration summary
  + using system PCRE library
  + OpenSSL library is not used
  + using system zlib library

  nginx path prefix: "/usr/local/nginx"
  nginx binary file: "/usr/local/nginx/sbin/nginx"
  nginx modules path: "/usr/local/nginx/modules"
  nginx configuration prefix: "/usr/local/nginx/conf"
  nginx configuration file: "/usr/local/nginx/conf/nginx.conf"
  nginx pid file: "/usr/local/nginx/logs/nginx.pid"
  nginx error log file: "/usr/local/nginx/logs/error.log"
  nginx http access log file: "/usr/local/nginx/logs/access.log"
  nginx http client request body temporary files: "client_body_temp"
  nginx http proxy temporary files: "proxy_temp"
  nginx http fastcgi temporary files: "fastcgi_temp"
  nginx http uwsgi temporary files: "uwsgi_temp"
  nginx http scgi temporary files: "scgi_temp"
```

8. Execute configuration again
```
./configure --sbin-path=/usr/bin/nginx --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --with-pcre --
pid-path=/var/run/nginx.pid --with-http_ssl_module
```

output:
```
Configuration summary
  + using system PCRE library
  + using system OpenSSL library
  + using system zlib library

  nginx path prefix: "/usr/local/nginx"
  nginx binary file: "/usr/bin/nginx"
  nginx modules path: "/usr/local/nginx/modules"
  nginx configuration prefix: "/etc/nginx"
  nginx configuration file: "/etc/nginx/nginx.conf"
  nginx pid file: "/var/run/nginx.pid"
  nginx error log file: "/var/log/nginx/error.log"
  nginx http access log file: "/var/log/nginx/access.log"
  nginx http client request body temporary files: "client_body_temp"
  nginx http proxy temporary files: "proxy_temp"
  nginx http fastcgi temporary files: "fastcgi_temp"
  nginx http uwsgi temporary files: "uwsgi_temp"
  nginx http scgi temporary files: "scgi_temp"
```

9. Compile and install Nginx

[A].
```
make
```
output:
```
objs/ngx_modules.o \
-lcrypt -lpcre -lssl -lcrypto -lz \
-Wl,-E
sed -e "s|%%PREFIX%%|/usr/local/nginx|" \
        -e "s|%%PID_PATH%%|/var/run/nginx.pid|" \
        -e "s|%%CONF_PATH%%|/etc/nginx/nginx.conf|" \
        -e "s|%%ERROR_LOG_PATH%%|/var/log/nginx/error.log|" \
        < man/nginx.8 > objs/nginx.8
make[1]: Leaving directory '/root/nginx-1.27.0'
```

[B].
```
make install
``` 
Last LIne output:
```
cp conf/nginx.conf '/etc/nginx/nginx.conf.default'
test -d '/var/run' \
        || mkdir -p '/var/run'
test -d '/var/log/nginx' \
        || mkdir -p '/var/log/nginx'
test -d '/usr/local/nginx/html' \
        || cp -R html '/usr/local/nginx'
test -d '/var/log/nginx' \
        || mkdir -p '/var/log/nginx'
make[1]: Leaving directory '/root/nginx-1.27.0'
```

10. start the nginx 
```
nginx -v   // check the version 
nginx     // start the nginx 
ps -ef | grep nginx   // check the status 
``` 

## Add Nginx as Service 

- Add Nginx in Systemd services.

- Nginx to run at all times, be restarted in case of failure (unexpected exit), and even survive server restarts.

```
reboot  // reboot 
```
Note: if you reboot the machine then connection is lost and connection is fail 
so we can do this process 

- if you can do this process and if you **reboot** the machine then not connection is lost. **OK**

Let's Go.

1. create the file in this directory.
```
touch /lib/systemd/system/nginx.service   
```

2. and open it
```
nano /lib/systemd/system/nginx.service
```

3. paste this 

```
[Unit] 
Description=The NGINX HTTP and reverse proxy server
After=syslog.target network-online.target remote-fs.target nss-lookup.target
Wants=network-online.target

[Service] 
Type=forking 
PIDFile=var/run/nginx.pid 
ExecStartPre=/usr/bin/nginx -t 
ExecStart=/usr/bin/nginx 
ExecReload=/usr/sbin/nginx -s reload 
ExecStop=/bin/kill -s QUIT $MAINPID 
PrivateTmp=true 

[Install] 
WantedBy=multi-user.target
```
4. save and exit the file 
```
ctrl + 0 -> save command
enter 
ctrl + x   -> exit command 
```

5. check nginx is running or not 
```
ps -ef | grep nginx
```



6. Start Again nginx 
```
nginx    // start the survice 
nginx -s stop 
systemctl start nginx      // start the nginx by master process 
systemctl status nginx     // check the status of nginx 
```

7. if you reboot the machine then also you survice is not work so you can enable the nginx 
```
systemctl enable nginx
```
output:
```
Created symlink /etc/systemd/system/multi-user.target.wants/nginx.service → /usr/lib/systemd/system/nginx.service.
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

- i will do later 

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



```
nginx -V    // check out where is you module and file 
```

- install the Module 

If the module is available as a package, install it using your package manager. For example:

```
sudo apt-get install ngx_http_realip_module 
nginx -V    // Again check module is add or not 
```

---------------------------------------

To configure the `nginx.conf` file to use the `--with-http_realip_module`, follow these steps:

### Step-by-Step Configuration

1. **Ensure the Module is Compiled**: First, make sure NGINX is compiled with the `--with-http_realip_module` option. You can check this by running:
   ```bash
   nginx -V
   ```
   Look for `--with-http_realip_module` in the output.

2. **Edit the `nginx.conf` File**: Open the `nginx.conf` file, usually located in `/etc/nginx/` or `/usr/local/nginx/conf/`.
   ```bash
   sudo nano /etc/nginx/nginx.conf
   ```

3. **Add Configuration Directives**: Add the following directives to the `http` block or the specific `server` block where you want to apply the real IP configuration.

   ```nginx
   http {
       # Define trusted IP addresses or CIDR blocks
       set_real_ip_from 192.168.1.0/24;
       set_real_ip_from 192.168.2.1;
       set_real_ip_from 2001:0db8::/32;

       # Specify the header to use for the real IP
       real_ip_header X-Forwarded-For;

       # Enable recursive search for the real IP
       real_ip_recursive on;

       # Other configurations...
   }
   ```

   - **`set_real_ip_from`**: Defines trusted addresses that are known to send correct replacement addresses.
   - **`real_ip_header`**: Specifies the header field to use for the real IP.
   - **`real_ip_recursive`**: Enables recursive search for the real IP.

4. **Save and Exit**: Save the changes and exit the editor.

5. **Test the Configuration**: Test the NGINX configuration to ensure there are no syntax errors.
   ```bash
   sudo nginx -t
   ```

6. **Restart NGINX**: Restart NGINX to apply the changes.
   ```bash
   sudo systemctl restart nginx
   ```

### Example Configuration
Here’s an example configuration snippet for the `nginx.conf` file:
```nginx
http {
    set_real_ip_from 192.168.1.0/24;
    set_real_ip_from 192.168.2.1;
    set_real_ip_from 2001:0db8::/32;
    real_ip_header X-Forwarded-For;
    real_ip_recursive on;

    server {
        listen 80;
        server_name example.com;

        location / {
            proxy_pass http://backend;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }
    }
}
```

This configuration ensures that NGINX uses the real IP address from the `X-Forwarded-For` header, which is useful when NGINX is behind a reverse proxy or load balancer¹².

If you have any more questions or need further assistance, feel free to ask! 😊

¹: [NGINX Documentation on ngx_http_realip_module](https://nginx.org/en/docs/http/ngx_http_realip_module.html)
²: [X4B Knowledgebase on Real IP in NGINX](https://www.x4b.net/kb/Technology/RealIP-Nginx)

Source: Conversation with Copilot, 20/7/2024
(1) Module ngx_http_realip_module - nginx. https://nginx.org/en/docs/http/ngx_http_realip_module.html.
(2) How to obtain the connecting clients Real IP on Nginx :: X4B. https://www.x4b.net/kb/Technology/RealIP-Nginx.
(3) Module ngx_http_realip_module - Get docs. https://getdocs.org/Nginx/docs/latest/http/ngx_http_realip_module.
(4) undefined. http://nginx.org/download/nginx-1.7.4.tar.gz.