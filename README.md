# Deploying rootstalk-static to Grinnell's static Server 

This project is used to deploy "rootstalk-static" to Grinnell's "static" server using 'docker-compose'.

## Deploying this Site

As of 30-Apr-2020, this site is intended to be deployed using my [docker-traefik2-host](https://github.com/McFateM/docker-traefik2-host) approach.  A `docker container run` command is no longer used to launch [the site](https://static.grinnell.edu/) on Grinnell College's `static.Grinnell.edu` server, so no more...

```
docker container run -d --name rootstalk-static \
    --label traefik.backend=rootstalk-static \
    --label traefik.docker.network=web \
    --label "traefik.frontend.rule=Host:rootstalk-static.grinnell.edu" \
    --label traefik.port=80 \
    --label com.centurylinklabs.watchtower.enable=true \
    --network web \
    --restart always \
  mcfatem/rootstalk-static
```

### Now Using Docker-Compose

With the introduction of [Traefik v2.x](https://traefik.io) this site now relies solely on files, most importantly `docker-compose.yml`, and a single `docker-compose up -d` command, to execute a one-time application launch. On Grinnell's `static.grinnell.edu` server, after the host has been initailized (see [README.md](https://github.com/McFateM/docker-traefik2-host)), the whole command sequence, executed as _root_, looked like this:

```
sudo su
cd /opt/containers
git clone https://github.com/DigitalGrinnell/rootstalk-static
cd rootstalk-static
docker-compose up -d
```

Since the above commands have already been run once, there should be no need to do it again. However, the pertinent portions of the process can now be specified like so:

```
sudo su
cd /opt/containers/rootstalk-static
git pull  # assumes the git remote is origin -> https://github.com/DigitalGrinnell/rootstalk-static
docker-compose up -d
```

## Local Development

It is recommended that you clone (or fork and clone) the FULL 'rootstalk-static' repository (https://github.com/McFateM/rootstalk-static) to an OS X workstation where [Hugo](https://gohugo.io) is installed and running an up-to-date version.

My typical workflow for local development is:

```
cd ~/GitHub/
git clone https://github.com/McFateM/rootstalk-static
cd rootstalk-static
git checkout -b <new-branch-name>
atom .
hugo server
```

The `atom .` command opens the project in my [Atom](https://atom.io) editor, and `hugo server` launches a local instance of the site and provides a link to that site if there are no errors.  This local site will respond immediately to any changes made in Atom.

# Updating the Production Server

You can use a `./push-update.sh` command to push your changes into production.  Study the `./push-update.sh` script and corresponding `push-update-Dockerfile` configuration to see all that it does.

# An Even Easier Update

Not long ago I added the _Atom Shell Commands_ package to my _Atom_ config, added a command named **Push a Static Update**, and pointed that command at the _push_update.sh_ script that is now part of this project.
