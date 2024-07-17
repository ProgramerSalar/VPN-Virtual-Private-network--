# UFW 

how to install ufw 
```
sudo apt install ufw
```

- some common command in ufw

```
sudo ufw status   // known the status active or not 
sudo ufw enable   // enable the ufw service
sudo ufw disable  // disable the ufw  service
sudo ufw allow 4243  // allow the particular port 


vim /etc/default/ufw   // known the ufw configuration file 
nano /etc/default/ufw

:q                      // exit of the vim editor 

sudo netstat -tulpn     // know that which port are where 



// before running this command you should disable the ufw 
sudo ufw reset   // reset all the ufw rules 



```

- setting up the default policies and this is recommended for every configuration the default polices are incoming connection is that we deny them and the default policy of outgoing connection is allow them 


```
sudo ufw default deny incoming 
sudo ufw default allow outgoing 

sudo systemctl status ufw   // know the status of systemctl 
sudo systemctl stop ufw    // stop the systemctl before the setting the rules 


```

### Make you own Rules: 

- when adding our rules when confuguration of your various rules for your fallwall it's very important that you understand what rules or what survices you want to keep active 
in regards to incoming and outgoing connection and what services and protocols you want to actually disable or prevent from being accessed 

- in our case we know that the two essential protocols that we want to keep active are going to be http both on a port 80 and port 443 for https and port22 for ssh 


```
sudo ufw allow ssh
sudo ufw allow http
sudo ufw allow https 
sudo ufw allow ftp 
sudo ufw allow from 232.342.342.32   -> this you ip address 



// if you want to allow a particular connection to particular port 
// for example i deny all ssh connections but i only allow an ssh connection from a particular ip address to the ssh port 

sudo ufw allow from 20.223.0.3 to any port 22



```

- i can also specify an entire subnet and it's very important when spacify ip address to ensure that the ip address that you will be using or that you are setting with in you rules these ips have to be static not dynamic 

because if you setup your firewall to allow connections from your ip address changes then you will not get access because rule has been set for that ip address alone 

so you can also set subnet if you want 

```
sudo ufw allow from 20.223.0.3/24 to any port 22
```



- start the ufw 

```
sudo systemctl start ufw
sudo ufw enable 
sudo ufw status
```


### Delete the Rules in ufw: 

```
sudo ufw delete 5  -> 5 number of the rules 
```

- 80/tcp (v6)                ALLOW       Anywhere (v6)  
this is you computer ip address so you delete this 

```
sudo ufw status numbered
sudo ufw delete 8
```


- also we will deny the ftp connection 

The File Transfer Protocol (FTP) is a standard network protocol used to transfer computer files between a client and server on a computer network

```
sudo ufw deny ftp

// and also deny the http 
sudo ufw deny http
```

- Some usefull connection which You may want to allow
```
To Allow SSH Connection: ufw allow ssh or ufw allow 22/tcp
To Secure Web Server: ufw allow 80/tcp
To Allow FTP Connection: ufw allow ftp or ufw allow 21/tcp and 20/ftp
To Allow Web Server Profile: ufw allow www
```