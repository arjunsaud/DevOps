# NGINX

- It is web server

![Alt text](images/image.png)

Install on Ubuntu: `sudo apt-get install nginx`

Check Status: `sudo systemctl status nginx`

Entry Point of Nginx is nginx.conf

Nginx Have tree Structure

There are lot of blocks and the first block is main block

(first block or called context)

## main:

- number of worker process
- username
- PID
- log locations
  We can configure these in main block

## main-->events:

- number of connection per worker process can handle

## main-->stream:

- TCP / UDP setting

## main--> HTTP:

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
