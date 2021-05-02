### rabbit-mq

### Project structure
This project is use to set a rabbit-mq cluster with 3 nodes, node1 is the main node.
We use nginx as reverse proxy and load balancer for the rabbitmq cluster TCP protocol is used.

To simplify our life, the RabbitMQ cluster acts in such a way that all logical objects (users, queues, ...),
wherever they are, are known and addressable from all members but not replicated.

For has a main queue and mirrors that can take over in the event of a node failure.

Configure policies in definitions.json globally or for each queue with ha-mode option.

This s an exemple for global config
`  "policies":[
     {"vhost":"antiseche","name":"ha","pattern":"", "definition":{"ha-mode":"all","ha-sync-mode":"automatic","ha-sync-batch-size":5}}
   ],`
   
In production we set two volumes

- rabbitmq_vol_master:
- reverse_proxy_vol:
 
### Expose port

To manage your cluster node we expose port:

- Main node listen on port 15671
- Replica node 2 listen on port 15670
- Repliica node 3 listen on port 15669

Nginx expose port:
- You can access cluster with port 5100

### Log
As the cluster run in container you can access log directly when the cluster run:
- In root/log in developpment
- In reverse_proxy_vol volume iin production/staging/recette

Access log will have a strict pattern

[14/Apr/2021:20:26:06 +0000] remote-address => 172.29.0.1 - to: upstream address => 172.29.0.4:5672

### Environnement variable

You have to define in the .env file RABBIT_ERLANG_COOKIE. This variable is a secret key, that allows two nodes of a cluster to interact with each other.
In developpment environnement put your file in docker-compose folder to bind with production architecture.

You have to define one value in developpment you can simply create a .env and paste it:

RABBITMQ_ERLANG_COOKIE=HEJDKKZHHE12

### Lunch project

#Dev

Too lunch a dev environnement:

`$docker network create custom_network`
(if it return Error response from daemon: network with name custom_network already exists then that mean you already good)

`$ docker-compose -f docker-compose/docker-compose.yml up -d --build`

#Prod

You have two variable set diirectly in gitlab CI/CD,

RABBITMQ_ERLANG_COOKIE => Communicate between node

FILE_PATH => Use for build nginx-rabbitmq-cluster image
