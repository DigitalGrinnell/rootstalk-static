---
Host: static.grinnell.edu
Service: rootstalk
URL: https://rootstalk.grinnell.edu
---

This document should be used to launch the `rootstalk-static`, service name `rootstalk`, on host `static.grinnell.edu` as part of a `docker-traefik2-acme-host` stack.

## Preparation

Before entering the prescribed "Command Sequence", below, the user should take steps to...

  - Comment out one of the `CASERVER=` specifications.  Comment out the second one (includes 'staging') for **production**, or the first one for **testing/staging**.
  - Make sure that the `-d`, `--key-file`, and `--cert-file` flags all specify `rootstalk.grinnell.edu`.

## Command Sequence

```
# CASERVER=https://acme-v02.api.letsencrypt.org/directory
CASERVER=https://acme-staging-v02.api.letsencrypt.org/directory
cd ~/host/rootstalk
docker-compose up -d; docker-compose logs
docker exec -it acme --issue --dns dns_azure --server ${CASERVER} -d rootstalk.grinnell.edu --domain-alias _acme-challenge.leverify.info --key-file /certs/rootstalk.grinnell.edu.key --cert-file /certs/rootstalk.grinnell.edu.cert --standalone --force
```

# rootstalk-static

This project is used to deploy `rootstalk-static` to Grinnell's `static` server using 'docker-compose'.

## Deploying this Site

As of 10-Jun-2020, this site is intended to be deployed using my [docker-traefik2-acme-host](https://github.com/McFateM/docker-traefik2-acme-host) approach.  A `docker container run` command is no longer used to launch [the site](https://static.grinnell.edu/) on Grinnell College's `static.Grinnell.edu` server.

### Now Using Docker-Compose

With the introduction of [Traefik v2.x](https://traefik.io) this site now relies solely on files, most importantly `docker-compose.yml`, and a single `docker-compose up -d` command, to execute a one-time application launch. On Grinnell's `static.grinnell.edu` server, after the host has been initialized (see [README.md](https://github.com/McFateM/docker-traefik2-acme-host)), the whole command sequence, executed as _administrator_, looked like this:

```
cd ~/host
git clone https://github.com/DigitalGrinnell/rootstalk-static
cd rootstalk-static
docker-compose up -d; docker-compose logs
```

Since the above commands have already been run once, there should be no need to do it again. However, the pertinent portions of the process can now be specified like so:

```
cd ~/host/rootstalk-static
git pull  # assumes the git remote is origin -> https://github.com/DigitalGrinnell/rootstalk-static
docker-compose up -d; docker-compose logs
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
