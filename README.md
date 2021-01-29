[<img src="https://hotio.dev/img/unpackerr.png" alt="logo" height="130" width="130">](https://github.com/davidnewhall/unpackerr)

[![GitHub Source](https://img.shields.io/badge/github-source-ffb64c?style=flat-square&logo=github&logoColor=white&labelColor=757575)](https://github.com/hotio/unpackerr)
[![GitHub Registry](https://img.shields.io/badge/github-registry-ffb64c?style=flat-square&logo=github&logoColor=white&labelColor=757575)](https://github.com/orgs/hotio/packages/container/package/unpackerr)
[![Docker Pulls](https://img.shields.io/docker/pulls/hotio/unpackerr?color=ffb64c&style=flat-square&label=pulls&logo=docker&logoColor=white&labelColor=757575)](https://hub.docker.com/r/hotio/unpackerr)
[![Discord](https://img.shields.io/discord/610068305893523457?style=flat-square&color=ffb64c&label=discord&logo=discord&logoColor=white&labelColor=757575)](https://hotio.dev/discord)
[![Upstream](https://img.shields.io/badge/upstream-project-ffb64c?style=flat-square&labelColor=757575)](https://github.com/davidnewhall/unpackerr)
[![Website](https://img.shields.io/badge/website-hotio.dev-ffb64c?style=flat-square&labelColor=757575)](https://hotio.dev/containers/unpackerr)

## Starting the container

CLI:

```shell
docker run --rm \
    --name unpackerr \
    -e PUID=1000 \
    -e PGID=1000 \
    -e UMASK=002 \
    -e TZ="Etc/UTC" \
    -e ARGS="" \
    -e DEBUG="no" \
    -e UN_SONARR_0_URL="http://sonarr:8989" \
    -e UN_SONARR_0_API_KEY="<yourapikey>" \
    -e UN_SONARR_0_PATHS_0="/downloads" \
    -e UN_RADARR_0_URL="http://radarr:7878" \
    -e UN_RADARR_0_API_KEY="<yourapikey>" \
    -e UN_RADARR_0_PATHS_0="/downloads" \
    -e UN_LIDARR_0_URL="http://lidarr:8686" \
    -e UN_LIDARR_0_API_KEY="<yourapikey>" \
    -e UN_LIDARR_0_PATHS_0="/downloads" \
    -v /<host_folder_config>:/config \
    -v /<host_folder_downloads>:/downloads \
    hotio/unpackerr
```

Compose:

```yaml
version: "3.7"

services:
  unpackerr:
    container_name: unpackerr
    image: hotio/unpackerr
    environment:
      - PUID=1000
      - PGID=1000
      - UMASK=002
      - TZ=Etc/UTC
      - ARGS
      - DEBUG=no
      - UN_SONARR_0_URL=http://sonarr:8989
      - UN_SONARR_0_API_KEY=<yourapikey>
      - UN_SONARR_0_PATHS_0=/downloads
      - UN_RADARR_0_URL=http://radarr:7878
      - UN_RADARR_0_API_KEY=<yourapikey>
      - UN_RADARR_0_PATHS_0=/downloads
      - UN_LIDARR_0_URL=http://lidarr:8686
      - UN_LIDARR_0_API_KEY=<yourapikey>
      - UN_LIDARR_0_PATHS_0=/downloads
    volumes:
      - /<host_folder_config>:/config
      - /<host_folder_downloads>:/downloads
```

For all available unpackerr (`UN_...`) environment variables, visit the [upstream project](https://github.com/davidnewhall/unpackerr#docker-env-variables) their documentation.

## Tags

| Tag                | Upstream                      | Version | Build |
| -------------------|-------------------------------|---------|-------|
| `release` (latest) | GitHub releases               | ![version](https://img.shields.io/badge/dynamic/json?color=f5f5f5&style=flat-square&label=&query=%24.version&url=https%3A%2F%2Fraw.githubusercontent.com%2Fhotio%2Funpackerr%2Frelease%2FVERSION.json) | ![build](https://img.shields.io/github/workflow/status/hotio/unpackerr/build/release?style=flat-square&label=) |
| `testing`          | GitHub pre-releases           | ![version](https://img.shields.io/badge/dynamic/json?color=f5f5f5&style=flat-square&label=&query=%24.version&url=https%3A%2F%2Fraw.githubusercontent.com%2Fhotio%2Funpackerr%2Ftesting%2FVERSION.json) | ![build](https://img.shields.io/github/workflow/status/hotio/unpackerr/build/testing?style=flat-square&label=) |
| `nightly`          | Every commit to master branch | ![version](https://img.shields.io/badge/dynamic/json?color=f5f5f5&style=flat-square&label=&query=%24.version&url=https%3A%2F%2Fraw.githubusercontent.com%2Fhotio%2Funpackerr%2Fnightly%2FVERSION.json) | ![build](https://img.shields.io/github/workflow/status/hotio/unpackerr/build/nightly?style=flat-square&label=) |

You can also find tags that reference a commit or version number.

## Configuration

You can use docker environment variables or a configuration file that should be stored in `/config/app/unpackerr.conf`. Don't forget to mount your volume where Unpackerr should look to find your downloads. You should use the same volume as is used in the Sonarr/Radarr/Lidarr containers. More advanced configuration methods are possible too, but take a look at the [upstream](https://github.com/davidnewhall/unpackerr) project page for more info on that.

## Executing your own scripts

If you have a need to do additional stuff when the container starts or stops, you can mount your script with the volume `/docker/host/my-script.sh:/etc/cont-init.d/99-my-script` to execute your script on container start or `/docker/host/my-script.sh:/etc/cont-finish.d/99-my-script` to execute it when the container stops. An example script can be seen below.

```shell
#!/usr/bin/with-contenv bash

echo "Hello, this is me, your script."
```

## Troubleshooting a problem

By default all output is redirected to `/dev/null`, so you won't see anything from the application when using `docker logs`. Most applications write everything to a log file too. If you do want to see this output with `docker logs`, you can set `DEBUG` to `yes`.
