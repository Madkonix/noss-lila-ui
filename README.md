# Docker images ready to run lila

This repository contains **Dockerfiles** to build containers ready to run [lila](https://github.com/ornicar/lila).

> This repo uses [noss](https://github.com/madkonix/noss) as an "engine" providing the tools and dependencies required.

## Installation ##

After installing [Docker](https://www.docker.com) and getting [lila](https://github.com/ornicar/lila)'s sources, you can pull an image from the [Docker Hub Registry](https://hub.docker.com/r/madkonix/noss-lila-ui):
```
docker pull madkonix/noss-lila-ui:alpine-dev
```
Or you can build an image yourself from its Dockerfile:
```
docker build -t madkonix/noss-lila-ui:alpine-dev github.com/madkonix/noss-lila-ui.git#:alpine
```

## Usage ##

To successfully run the image, you will need to have mongo and redis servers both accessible on localhost.

There are various ways to achieve this, the two I will be focusing on here suppose mongo and redis will also be containers.

> Both methods outlined below assume there is no mongo or redis server already running. If there is please stop them first.

### Manual method ###

Bring up mongo and redis containers (note the `-d` to run them detached, in the background):
```
docker run -it -d --network host mongo
docker run -it -d --network host redis
```
Then run the image pulled/compiled in the installation step, while providing the path to the sources (note the `--rm` will get the container deleted upon exit - the mounted source files will still be there):
```
docker run -it --rm -v /path/to/lila:/root/lila --network host madkonix/noss-lila-ui:alpine-dev
```
When the image is done compiling and starting the ui, you should see a line like:
```
[info] p.c.server.AkkaHttpServer - Listening for HTTP on /[0:0:0:0:0:0:0:0]:9663
```
You can then go to [http://127.0.0.1:9663](http://127.0.0.1:9663) in a browser to access the web interface.

### Using a Compose file ###
You can edit the file docker-compose.yml provided along with the Dockerfile, the only required change is to edit the sources folder path. Then from within the same folder containing the compose file, run:
```
docker-compose up -d
```
You can check the progression logs with:
```
docker-compose logs -f lila-ui
```
When the image is done compiling sources and starting you should see a line like:
```
lila-ui    | [info] p.c.server.AkkaHttpServer - Listening for HTTP on /[0:0:0:0:0:0:0:0]:9663
```
You can then go to [http://127.0.0.1:9663](http://127.0.0.1:9663) in a browser to access the web interface.

## Contribution policy ##

Contributions via GitHub pull requests are gladly accepted from their original author. Along with any pull requests, please state that the contribution is your original work and that you license the work to the project under the project's open source license. Whether or not you state this explicitly, by submitting any copyrighted material via pull request, email, or other means you agree to license the material under the project's open source license and warrant that you have the legal authority to do so.

## License ##

This code is open source and licensed under the [AGPL-3.0]("https://www.gnu.org/licenses/agpl-3.0.txt").
