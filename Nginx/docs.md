# NGINX

Keyword in nginx are called directives

Docs : https://nginx.org/en/docs/

- It is web server

![Alt text](images/image.png)

Install on Ubuntu: `sudo apt-get install nginx`

Check Status: `sudo systemctl status nginx`

Entry Point of Nginx is nginx.conf

Nginx Have tree Structure

There are lot of blocks and the first block is main block

(first block or called context)

These below are the configuration of nginx.conf file

## main:

- number of worker process
- username
- PID
  We can configure these in main block

## main-->events:

- number of connection per worker process can handle

## main-->stream:

- TCP / UDP setting

## main--> HTTP:

- log locations for (log purpose)

It contains two blocks.
HTTP block can have multiple server blocks

## main-->HTTP-->server:

-virtual server

we can do setting of different websites here.
one server block can have multiple locations block

## main-->HTTP-->server-->location:

- used for routing (URI) eg: /api/product

## main-->HTTP-->upstream:

- used as reverse proxy

  It can be used to request to backend server and used as for load balancing

![Alt text](<images/Screenshot from 2023-11-15 12-32-08.png>)

# Commands

validate nginx configuration: `sudo nginx -t`
View All nginx configuration: `sudo nginx -T`

reload nginx : `sudo systemctl reload nginx`

## For site host nginx on conf.d folder

```
#this is called virtual server and in apache it is called virtual host
#this keyword are called directives
#we can create multiple server blocks

server {
        # default_server can only be one for one machine
        listen 80 default_server;

        # this root directive contains the path of the project
        root /var/www/blog;

        # this server name contain the name of server
        server_name _;

        # here it contains the entry file with some combinations like .html or .htm or index
        index index.html index.htm;

        # this / is path it can be any route list /api or /login we can use regex by ~ or use = to exact match, and go to docs for this
        location / {
                #uri is the route after added to route directive
                # if uri not present return 404
                try_files $uri $uri/ = 404;

        }
}
```

## For Accessing website with username and password

## For site host nginx on conf.d folder

```
#this is called virtual server and in apache it is called virtual host
#this keyword are called directives
#we can create multiple server blocks

server {
        listen 80;

        root /var/www/blog;


        # url of the website
        # eg arjunsaud.com.np or www.arjunsaud.com.np or blog.arjunsaud.com.np
        server_name _;


        # This is build in for most of browser
        auth_basic "Any Message";
        auth_basic_user_file /etc/nginx/.htpasswd;

        # This .htpasswd can be generated by below command
        # cmd: sudo sh -c "echo -n 'username:' >> /etc/nginx/.htpasswd"
        # generate password cmd: sudo sh -c "openssl passwd -apr1 >> /etc/nginx/.htpasswd"

        index index.html index.htm;

        location / {
                # this turn off the auth at this route ("/")
                # This override the above auth
                auth_basic off;
                try_files $uri $uri/ = 404;
        }

        location /admin {
                try_files $uri $uri/=404;
        }
}
```

# Reverse Proxy

```

upstream backend{
        server localhost:3000;
}

server {
        listen 80;

        # use domain instead of localhost
        server_name localhost;

        location / {
                proxy_pass http://backend
        }
}

```

# load balancing: nginx use round robin algorithm for load balancing

```

upstream backend{
        # This weight is used how many number of request it have to handle and after 100 of request it request to next instance of the server

        server localhost:3000 weight=100;

        # this 8001 server handle one request and pass next request to port 3000 server
        server localhost:3001;
        # for this you should run project on both port
}

server {
        listen 80;

        # use domain instead of localhost
        server_name localhost;

        location / {
                #this is for not caching
                #add_header Cache-Control no-store;
                proxy_pass http://backend
        }
}

```

# To make backup server

```
upstream backend{

        server localhost:3000;

        # this backup is used as backup server, it gets triggered when main server goes down
        server localhost:3001 backup;

        # this makes server down although it is running in background
        server localhost:3002 down;

}

server {
        listen 80;
        # use domain instead of localhost
        server_name localhost;

        location / {
                #this is for not caching
                #add_header Cache-Control no-store;
                proxy_pass http://backend
        }
}

```

## For Host React app

```
# build react app and copy react build to react_app folder

server {
        listen 80;

        root /var/www/react_app;



        server_name _;

        index index.html index.htm;

        location / {
                try_files $uri $uri/ /index.html;

                # here /index.html is added so that when refreshing it doesn't throw error
                # if it is not given client side navigation works but when we refresh it does't work
        }
}
```

# How to use ssl
