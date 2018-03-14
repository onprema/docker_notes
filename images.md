## Images

A Docker image is **binaries & their dependencies** + **metadata**

`docker history nginx` shows the layers of an image and how it's changed over
time

`docker image inspect nginx` shows the metadata of an image.

`docker image tag nginx eightlimbed/nginx` will 'create' a new image with the
repository name of `eightlimbed/nginx`. Then you can do `docker push
eightlimbed/nginx` to push to Docker Hub.
