## Containers

Docker Engine is the **server**. It's running in the background the whole time.
Docker CLI is the **client**. When a user (me) types `docker version` for example,
there is a request made from the client to the server, and if the connection is
working, the server will return something.

Let's break this down: `docker container run --publish 80:80 nginx`
- `--publish` (or `-p`) exposes a container's port to the host. To expose a port
  is like create a door for the container and host (me) to communicate through.
This allows me to go to `http://localhost` in my browser and see the default
home page for the nginx server.
- `80:80` opens port 80 on the host IP and sends all traffic from it to port 80 of
  the container's IP. When I enter `http://localhost` in my browser, I am
sending a GET request to the nginx server and it's getting to the server through
port 80. It will send responses back to the host and the host will receive them
through port 80.
- I could do `8888:80` instead, but then I'd have to go to
  `http://localhost:8888` to view the nginx page
- When you see `80:80` or `3306:6603`, think `HOST_PORT:CONTAINER_PORT`

`--detach` or `-d` tells Docker to run the container in the background. This
allows us to use our terminal. But we miss out on seeing the logs. Oh, well we
can use `docker container logs nginx` to check out the logs later.

When you run `docker container run nginx` it will grab that nginx image, create
a new container based on it, and **gives it a virtual IP address that's inside a
Docker virtual network inside Docker Engine**. Whoa. It will then **opens up the
ports if we specify with `--publish`**. By default, it does not open any ports.

The container starts by using the `CMD` in the image Dockerfile

`--env` or `-e` is a way to pass environment variables into the containers. For
examples `docker container run --env "MYSQL_ROOT_PASSWORD=password" mysql`

If you run `docker container run -it ubuntu` then install some software and exit
the container, you can get back to where you were by doing `docker container
start -ai ubuntu`. `start -ai` is like continuing where you left off.

`docker container exec -it`: run additional command in existing container, like
if you need to do some admin stuff on a nginx container that is running.
