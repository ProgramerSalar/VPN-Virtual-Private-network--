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

