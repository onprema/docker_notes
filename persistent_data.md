## Persistent Data

**Ephemeral** means something that lasts for a short time

**Immutable** means something that does not change

The goal is to have an infrastructure of containers that don't change. *But* if
they do need to change, we replace the old containers with new ones. -- Only
re-deploy containers, never change them.

If things don't change, then what about databases, key/value stores or unique data?

Ideally, the container's should contain unique data mixed in with the
application binaries. (This is known as **separation of concerns**)

Docker has two solutions to the problem of wanting persistent data and ephemeral
containers: **Volumes** and **Bind Mounts**

### Volumes
- Configuration option for a container that designates a specific location
  OUTSIDE of the container's file system to store unique data.
- The container sees this location as a local file path.
- We can connect multiple containers to this data store.
- Can be set with the `VOLUME` command in a `Dockerfile`.
    - ex: `VOLUME /var/lib/mysql`
- Any files we put in that directory will outlive the container until we
  manually delete those files.
- `docker image inspect mysql | grep -i "volumes"` will show you where the
  Volume path is located
- `docker container inspect mysql | grep -i "mounts"` will show the mapping of
  the location IN the container to the location OUTSIDE OF the container.
- `docker volume ls` lists all the volumes created by containers. **These
  volumes will persist, so they can easily add up. You can use** `docker volume
prune` **to clean up unused volumes**.
- `docker run -d mysql -v /var/lib/mysql` **-v** allows us to specify where we
  want our volume to be.
- `docker run -d mysql -v mysql-db:/var/lib/mysql` this is a **named volume**.
  we just named our volume `mysql-db` and it's located at `/var/lib/mysql`. This
is nice because the default name is a big hash.
- `docker volume create` can be useful if you want to use a different driver.

### Bind Mounts
- Sharing, or "mounting" a host directory or file INTO a container.
- Maps a host file or directory to a container file or directory.
- To the container, it will look just like a local file path. It won't know it's
  coming from the host.
- Bind Mounts cannot be specified in a Dockerfile. They have to be used at
  runtime when you do `docker container run...`. This is because they need
specific data that's on the host machine. It wouldn't make sense to pass around
a Dockerfile with Bind Mounts because each host would have to have those
specific files, too.
- `docker container run -v /home/my/stuff:/path/in/container`. This creates a
  Bind Mount from `/home/my/stuff` on host, to `/path/in/container` in
container. Note: `-v` is used for both Volumes and Bind Mounts.
- You can also specifies permissions, like "read only"
