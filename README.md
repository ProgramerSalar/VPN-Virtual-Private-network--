
-------------------------------------------------------------------------
Note: You should follow this documentation https://thapatechnical.shop/blogs/host-a-mern-stack-app-on-a-vps


# how to setup vps surver in Hostinger: 

- some linux command 
```
$ pwd   // known that which directory you have 
```

## 0.0 Setup the VPS 

1. You have the VPS Information Panal then i will teach you how too do ? 
2. open the your cammand line make sure this: C:\Users\Manjusha Kumari> 
paste the SSH Success Terminal in the command line and then password,


- if you reboat you vps 
```
ssh-keygen -R 213.210.36.105
```


you show this interface:
![you show this interface:](Readme_Photo/access_the_linux_machine_in_your_cammd_line.png)



3. before installing any software, it's good practice to update your VPS packages:

``` 
    # sudo apt update 
    # sudo apt upgrade
```

## 0.1  This is once time Setup 

### 1. first we will install the NVM (Node version Manger) because how to manage the version of package, so we need of this nvm 


```
# curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
```
if you check the version: nvm --version then you get this: Command 'nvm' not found, but there are 14 similar ones.

exit code:
 ```
# exit
```

you should restart the again your command line and enter the ssh Terminal and password 
then you write the 

```
# nvm --version
``` 



### 2. Install the node 

```
# nvm install --lts

```
lts -> long term support 

check the verion

```
# node -v
```

### 3. install the Git 

```
# sudo apt install git
```


if you connect github to your project  
then install the cli of github 

```
# sudo apt install gh
```


Authenticate the your github in nvm 

```
# gh auth login
```



## 0.3 This is connection of Project 

1. you should clone the project 

```
git clone https://github.com/ProgramerSalar/noise-json.git

// if you delete the clone project 
rm -rf /path/to/repository

```

and then go to the project like 

```
cd <your-project-name>
```


you use the ls command then you know the file information


you should install the pnpm because it's need to install the package 

```
# npm i pnpm --global
# pnpm i
```
### 2. Setup environment variables

```
nano .env // Nano is a shell based text editor which comes by default on linux

// We have an environment variable named DB_URL so let's include it.
MONGODB_URI=mongodb://127.0.0.1/vps-test-db
JWT_SECRET=oHSLF&#WsA#WpJ!QCWLz7rCL5^sHgD62Qe8tgb3&SSscRu$sFi*L9MSSgnBM2*!$pyXS&XwHyK8RFFh2cboJv6vy5h#cnpBrymtF*@37!C@ohdHR48@%%4X!Az@wiyCe
PORT=8000

// Then press Ctrl + O and enter to save it.
// The press Ctrl + X to exit out of nano.

// Let's use "cat" command to see if everything is fine.
// The cat command in Linux (short for "concatenate") is primarily used to display the contents of one or more files in the terminal.
cat .env
// This shows content of .env file
```

### 3. To fix this issue, we can use pm2.

pm2 (Process Manager 2) is a popular process manager for Node.js applications. It is used to manage and monitor Node.js applications, ensuring they run reliably in production environments.

pm2 simplifies the process of managing Node.js applications by allowing you to start, stop, restart, and monitor multiple Node.js processes with a single command.



```
npm i -g pm2
pm2 start npm --name "test_server" -- start
pm2 ls
pm2 logs test_server

// if you kill the server | exit the server 
pm2 kill

// if you delete the server using the id 
pm2 delete 0

// check the list 
pm2 list

// -- start (there is a space, Keep in mind)
// To test our backend
curl http://localhost:8000
// Note: In our backend we have a route for homepage so we are using it
```


Here's what each part of the command does:

- pm2 start npm: This tells pm2 to use npm as the interpreter.

- -name "test_server": This assigns the name "test_server" to the application.

- -- start: This specifies that you want to run the "start" script defined in your package.json file.


```
pm2 -v
// 5.3.0
```

# Frontant setup

## 1. start the suver 

- Go to frontend or client folder. Install all the dependencies and build the project:



```
pnpm i
pnpm build
npm run build
bun run build

// setup environment var
nano .env
// We already have some content in our repo so Edit the vite api url to 
http://localhost:8000 
// Note: To make it easier, please setup your project so that it uses API URL environment variable

// as that's our server
// Ctrl + O and Enter to save
// Ctrl + X to exit
```
Run this command to preview our project:


```
pnpm preview --host
npm preview -- --host
```

Now, Here we can’t see our website page, because of firewall. To make our PORT publicly accessible, we need to follow some steps.

```
sudo ufw enable
sudo ufw allow 4173
```

This works but vite preview is not made for production. It’s recommended to use something like nginx to serve the assets.

```
sudo ufw deny 4173 // We are just denying the port that we made public earlier.
```

- if the you want to server is running to continousely 
```
pm2 start npm --name "react-app" -- run preview
```


## 2. we will use the Nginx 

Nginx is a popular web server and reverse proxy server that is commonly used in VPS (Virtual Private Server) and server environments

You can install nginx using this command:

```
sudo apt install nginx
// It is installed by default on hostinger vps
```


```
ls

cd /etc/nginx

cd sites-enabled/
ls
//default.conf

cat default.conf
// we dont need default.conf file 
rm default.conf

cd /etc/nginx/sites-available
nano 62.72.59.218.conf
// It is common convention to name the file with domain name 
// or ip address based on what you are using
```


```
server {
    listen 80;
    root <project-folder>;

    location / {
        try_files $uri $uri/ =404;
    }
}
```



```
<!-- 
server {
    listen 80;
    root /root/noise-frontant/dist;
    noise-frontant/dist#

    location / {
        try_files $uri $uri/ =404;
    }
} 
 -->

```


- project-folder

    - Go to your frontend project where it is, make sure your frontend project is built using npm build. Since, we are using vite, it's inside "dist". It should be inside "build" for create-react-app

    - Our folder: /root/TypeScript-Simple-Mern-Project/client/dist

- We are using that location context to allow all URLS to render index.html because we are using client side react using react-router-dom for routing.

This configuration sets up an Nginx server to listen on port 80 (HTTP) and serve static files from a specific director.

1. listen 80;: This specifies that the server should listen on port 80, which is the default port for HTTP traffic.

2. root /root/TypeScript-Simple-Mern-Project/client/dist;: This sets the document root for this server block. It tells Nginx to serve files from the /root/TypeScript-Simple-Mern-Project/client/dist directory when handling requests to this server block. This is typically used for serving static files like HTML, CSS, JavaScript, and images.

3. location / { ... }: This is a location block that defines how Nginx should handle requests to the root directory ("/"). It contains the following directives:

4. try_files $uri $uri/ =404;: This directive tells Nginx how to handle requests. It tries to find a matching file in the specified directory ($uri) and, if not found, it appends a trailing slash ($uri/) and, if still not found, it returns a 404 error (=404). Essentially, it serves existing files as-is and sends 404 errors for everything else.

Use this command to check if nginx is working properly:

```
sudo nginx -t
// nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
// nginx: configuration file /etc/nginx/nginx.conf test is successful
```


```
// go to /etc/nginx/sites-enabled
ln -s ../sites-available/62.72.59.218.conf .
// It creates symbolic link (also known as a symlink or soft link) of our configuration file in current dir.

systemctl restart nginx
// to restart the Nginx web server on a Linux system.
// By executing systemctl restart nginx, you ensure that any changes you've made to Nginx's configuration or website files take effect, as well as refresh the server's status.
```

Now if you go to 
```
http://<your-ip-address> 
```
then you will see your website. It works right? But let’s see how you can connect it with a proper domain.