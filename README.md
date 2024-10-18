# nginx-task
We have  a server which uses 3 nginx image containers to route on the same port but we use different path to access each nginx server 

## First we create 3 html servers 
-server_a
-server_b
-server_c

## Code for server_a

<!-- ./server_a/index.html -->
<html>
  <body>
    <h1>Server A - Path /a</h1>
  </body>
</html>

## Code for server_b

<!-- ./server_a/index.html -->
<html>
  <body>
    <h1>Server B - Path /a</h1>
  </body>
</html>

## Code for server_c

<!-- ./server_c/index.html -->
<html>
  <body>
    <h1>Server C - Path /a</h1>
  </body>
</html>

Now we write a docker-compose file 

## Docker-compose file 

version: '3'
services:
  
  server_a:
    image: nginx:alpine
    container_name: server_a
    volumes:
      - ./server_a:/usr/share/nginx/html
    networks:
      - webnet
  
  server_b:
    image: nginx:alpine
    container_name: server_b
    volumes:
      - ./server_b:/usr/share/nginx/html
    networks:
      - webnet
  
  server_c:
    image: nginx:alpine
    container_name: server_c
    volumes:
      - ./server_c:/usr/share/nginx/html
    networks:
      - webnet
  
  nginx:
    image: nginx:alpine
    container_name: nginx_reverse_proxy
    ports:
      - "80:80"  
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
    depends_on:
      - server_a
      - server_b
      - server_c
    networks:
      - webnet

networks:
  webnet:

We write a nginx config file 

## Nginx file 

events {}

http {
    server {
        listen 80;

        
        location /a {
            proxy_pass http://server_a:80;
            
            rewrite ^/a(.*) $1 break;
        }

        # Proxy to server B for path /b
        location /b {
            proxy_pass http://server_b:80;
            # Remove the /b prefix when proxying
            rewrite ^/b(.*) $1 break;
        }

        # Proxy to server C for path /c
        location /c {
            proxy_pass http://server_c:80;
            # Remove the /c prefix when proxying
            rewrite ^/c(.*) $1 break;
        }
    }
}

