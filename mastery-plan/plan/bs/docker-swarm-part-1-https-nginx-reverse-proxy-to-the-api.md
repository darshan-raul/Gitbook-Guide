# Docker Swarm Part 1: HTTPS Nginx reverse proxy to the API

So in Part 1 , I covered the basics of Docker Swarm and created a working swarm cluster with 1 master 2 slave nodes. But with just one service, My API container.

So in this part I will create another service which will be a Nginx reverse proxy for the API service. Here we go !

![Setting up three servers in the swarm, 2 slaves 1 master](../../../.gitbook/assets/image%20%2849%29.png)

### 

### 

### NGINX config file setup for reverse proxy

This is the nginx config file to reverse proxy to the api container.

```text
worker_processes 1;
events { worker_connections 1024; }

http {

    

    upstream docker-awsapi {
        server awsapi:8080;
    }


    server {
        listen 80;
        listen 443 ssl;

        ssl_certificate     /etc/nginx/awsapi.com.crt;
        ssl_certificate_key /etc/nginx/awsapi.com.key;
        server_name awsapi.com;
        location / {
            proxy_pass         http://docker-awsapi;
            proxy_redirect     off;
            proxy_set_header   Host $host;
            proxy_set_header   X-Real-IP $remote_addr;
            proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header   X-Forwarded-Host $server_name;
        }
    }


}
```

Going through some important blocks in the configuration file:

#### Upstream awsapi:

```text
upstream docker-awsapi {
        server awsapi:8080;
    }
   
```

This parameter is mostly used for load balancing in Nginx. For example:

> ```text
> http {
>     upstream myapp1 {
>         server srv1.example.com;
>         server srv2.example.com;
>         server srv3.example.com;
>     }
>
>     server {
>         listen 80;
>
>         location / {
>             proxy_pass http://myapp1;
>         }
>     }
> }
> ```

In the example above, there are 3 instances of the same application running on srv1-srv3. When the load balancing method is not specifically configured, it defaults to round-robin. All requests are [proxied](http://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_pass) to the server group myapp1, and nginx applies HTTP load balancing to distribute the requests.

#### Ports to listen and https certificate location

```text
        listen 80;
        listen 443 ssl;

        ssl_certificate     /etc/nginx/awsapi.com.crt;
        ssl_certificate_key /etc/nginx/awsapi.com.key;
        server_name awsapi.com;
        
```

Here,  in the first two lines we are listening on port 80 and port 443 respectively. The later will be used for https purposes. Next we have the ssl certificate location and the ssl key location \(More on this later\). Lastly we have a server name: awsapi.com in this case.

#### Mentioning reverse proxy to api server

```text
location / {
            proxy_pass         http://docker-awsapi;
            proxy_redirect     off;
            proxy_set_header   Host $host;
            proxy_set_header   X-Real-IP $remote_addr;
            proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header   X-Forwarded-Host $server_name;
        }
```

In these lines we mention the proxy\_pass to be the upstream server we mentioned above.

Now that the configuration file for nginx cotainer as reverse proxy is made. Lets create certificated cert and private key files to make the nginx container https.

Here's how I have kept the structure for the code:

![](../../../.gitbook/assets/image%20%2841%29.png)

Go to the nginx folder

`cd nginx`

Create cert file and key 

`openssl req -newkey rsa:2048 -nodes -keyout nginx/awsapi.com.key -x509 -days 365 -out nginx/awsapi.com.crt`

You will have to fill in the following questions;

1. Country Name \(2 letter code\)
2. State or Province Name \(full name\)
3. Locality Name \(eg, city\)
4. Organization Name \(eg, company\)
5. Organizational Unit Name \(eg, section\)
6. Common Name \(eg, fully qualified host name\)
7. Email Address

Once done your folder structure should look like this:

![](../../../.gitbook/assets/image%20%2856%29.png)

### Adding service in docker compose file:

This was the previous docker-compose file:

```text
version: '3.0'




services:

  awsapi:

    image: darshanraul/awsapi:latest

    container_name: awsapi

    deploy:

      replicas: 4

    ports:

      - "80:8080"

    networks:

      - sample_network_name

networks:

  sample_network_name:
  
```

There was just one service with four replicas. This is the new docker-compose.yml

```text

version: '3'
  
services:
    reverseproxy:
        image: nginx:alpine
        #container_name: reverseproxy
        ports:
            - 80:80
            - 443:443
        deploy:
          replicas: 4
        volumes:
          - ./nginx/nginx.conf:/etc/nginx/nginx.conf
          - ./nginx/awsapi.com.crt:/etc/nginx/awsapi.com.crt 
          - ./nginx/awsapi.com.key:/etc/nginx/awsapi.com.key
        networks:
          - aws_network

        restart: always

    awsapi:
        depends_on:
            - reverseproxy
        #container_name: awsapi
        image: darshanraul/awsapi
        deploy:
          replicas: 4
        restart: always
        networks:
          - aws_network

networks:
  aws_network:
```

![](../../../.gitbook/assets/image%20%2813%29.png)

![](../../../.gitbook/assets/image%20%2836%29.png)

![](../../../.gitbook/assets/image%20%28173%29.png)

![](../../../.gitbook/assets/image%20%2839%29.png)

