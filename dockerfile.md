## Dockerfile

You can use `-f` to build images from a specific file. The default is the
`Dockerfile` in the CWD. But it can be elsewhere. Ex: `docker build -f
/tmp/d-file`

Let's look at some of the stanzas that make up a Dockerfile...

### `FROM`
Required. Usually a linux distro like alpine or ubuntu. One of the main benefits 
for using linux distros in containers is to use their package distribution systems
like `apt` or `yum` to install whatever software you need. If you truly want to
begin with an empty container you can say `FROM scratch`

### `ENV`
Environment variables. Required in some containers such as MySQL with the
`MYSQL_ROOT_PASSWORD=whatever` thing. This is the main way we set keys and
values for building and running containers

* side note: each stanza is a layer, and it matters what order they are in.
**Put the things you change the least often at the TOP and the things you change
the most often at the BOTTOM**

### `RUN`
Used when you need to install software from a package repository, do some
unzipping, file edits, etc inside the container itself. `RUN` can also run shell
scripts you may have copied in the container. I image you can do some `git
clone`s here, too.

You should chain together `RUN` commands with `&&` instead of seperate `RUN`
commands for each one. This is because each stanza is a layer and it's more
effecient to clump everything into one layer.

### `EXPOSE`
Exposes ports to the Docker virtual network. You still need to add `-P` or
`--publish` to open these ports to the host.

### `CMD`
Required. This is the final command that will be run every time you launch a new
container from the image, or every time you restart a stopped container.

### `WORKDIR`
Basically the same as `RUN cd /path/to/somewhere`. Its best practice to use
`WORKDIR` when you are changing directories.

### `COPY`
Copy your source code into the container images. This is **not** the same as the
`cp` Unix command. It will actualy create a directory if it doesn't exist in the
`FROM`'s filesystem! Very nice.

---

## Building an Image

`docker image build -t myname .` builds an image based on the Dockerfile in the
CWD.

## Extending an Image

Here is how you can extend a Docker image. For example, adding an html file into
the image (assuming you have a file called `index.html` in your dir):

```
FROM nginx:latest
WORKDIR /usr/share/nginx/html
COPY index.html index.html
```

The default directory for nginx HTML files is `/usr/share/nginx/html`.

Note: I don't have to have `CMD` or `EXPOSE` commands because they are baked
into the `FROM nginx:latest` stanza :).
