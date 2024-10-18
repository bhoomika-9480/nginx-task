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


