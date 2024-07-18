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

```


# Advance Topic for Nginx 

### 0.1 Setup the Ngnix 

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


1. Update Packages
```
Ubuntu - apt-get update
CentOS - yum update
```

2. Download the NGINX source code from nginx.org
```
https://nginx.org/download/nginx-1.19.1.tar.gz
```

3. Unzip the file.
```
tar -zxvf nginx-1.19.1.tar.gz
```

4. Configure source code to the build.
```
./configure
```

4. Install code compiler
```
Ubuntu : apt-get install build-essential
CentOS : yum groupinstall "Development Tools"
```

5. Configure source code to the build.
```
./configure
```

6. GET Support Libraries
```
Ubuntu : apt-get install libpcre3 libpcre3-dev zlib1g zlib1g-dev libssl-dev make
CentOS : yum install pcre pcre-devel zlib zlib-devel openssl openssl-devel make
```

7. Execute configuration again
```
./configure --sbin-path=/usr/bin/nginx --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --with-pcre --
pid-path=/var/run/nginx.pid --with-http_ssl_module
```

8. Compile and install Nginx

```
make
make install
```


### 0.2 Add NGINX Process in Systemd Services to make the WebServer Resilient.

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
