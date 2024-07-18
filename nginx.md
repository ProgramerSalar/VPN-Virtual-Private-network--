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


- download the documentation of nginx 

```
wget https://nginx.org/download/nginx-1.27.0.tar.gz
tar zxvf nginx-1.27.0.tar.gz   // unzip the ducumention 
```


- to build the source code you need to execute this particular configure 
```
cd nginx-1.27.0
./configure   // we will build this file in the configure file 

// get the error: compiler cc is not found 
apt-get install build-essential     // download the essentials file 

// again get error: ./configure: error: the HTTP rewrite module requires the PCRE library.


// libpcre3  -> this is required for the nginx 
// zlibig    -> this is for the zzip 
// libssl-dev  -> this is for the ssh connection 

apt-get install libpcre3 libpcre3-dev zlib1g zlib1g-dev libssl-dev make


// we will do configure again 
./configure


```


