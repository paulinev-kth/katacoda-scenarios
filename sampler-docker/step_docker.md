We will use sampler to monitor dockers containers. We will use simple [Redis](https://hub.docker.com/_/redis/) containers. Redis is an open source key-value store that functions as a data structure server.

It is aldready installed in the system.

To create and start a redis container, execute:

`docker run -d --name myFirstRedisContainer redis `{{execute}} 

For the purpose of our tutorial we will create a second container.
`docker run -d --name mySecondRedisContainer redis `{{execute}}

With `docker ps`{{execute}} you can see all your containers that are running and you can find the id of your container. 

To add a value to our Redis store : 

`docker exec myFirstRedisContainer redis-cli set pear 20kr`{{execute}}

You can see all the values store by

`docker exec myFirstRedisContainer redis-cli keys \* `{{execute}}


