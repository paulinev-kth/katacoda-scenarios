## Creating Redis containers
We will use sampler to monitor Docker containers. We will use simple [Redis](https://hub.docker.com/_/redis/) containers. Redis is an open-source key-value store that functions as a data structure server.

To create and start a Redis container, run the following command. Docker will pull the Redis container image, it can take a few minutes.

`docker run -d --name myFirstRedisContainer redis `{{execute}}

For the purpose of our tutorial, we will create a second container.

`docker run -d --name mySecondRedisContainer redis `{{execute}}

With `docker ps`{{execute}}, you can see all the containers running and check that the two containers have actually been started.

## Adding key-value to our Redis store
To add a key-value pair to our first Redis store, run the following command. It associates the key "pear" with the value "20kr".

`docker exec myFirstRedisContainer redis-cli set pear 20kr`{{execute}}

You can see all the keys stored (only "pear" in our case) in "myFirstRedisContainer" by running:

`docker exec myFirstRedisContainer redis-cli keys \* `{{execute}}
