## Compose

Compose is a combination of a command line tool and a config file. It makes
things easier when there is more than one container involved in your
application.

**YAML** is a configuration language that describes how we want to compose
containers, networks and volumes.

**docker-compose** is the CLI tool used for local development based on the YAML
file.

`docker-compose --help` for help

`docker-compase.yml` is the default name for configs, but you can name it
anything as long as its a `.yml` file and you specify with `docker-compose -f`.

All the things you do in the `docker run` command, you can save them in the
`docker-compose.yml` file. Some people make shell scripts to automate the `run`
commands, but this can be a better alternative.

Here's an example of a compose based on the jekyll site from previous lesson:

```
version: '2'

services:
    jekyll:
        image: bretfisher/jekyll-serve
        volume:
            - .:/site
        ports:
            - '80:4000'
```

^^^ same as `docker run -p 80:4000 -v $(pwd):/site bretfisher/jekyll-serve`

### CLI

`docker-compose up` sets up volumes/networks and start containers
`docker-compose down` stops containers and cleans up
`docker-compuse down -v` will remove the volumes, too if you don't need them
