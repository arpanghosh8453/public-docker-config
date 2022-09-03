
# Docker containers configuration

## [Uptime-kuma](https://github.com/louislam/uptime-kuma)
- **Docker-run**
```docker
docker run -d --restart=unless-stopped -p 7384:3001 -v /home/arpan/docker-containers/uptime-kuma:/app/data --name uptime-kuma louislam/uptime-kuma:1
```
- **Docker-compose**
```docker
version: '3.3'
services:
    uptime-kuma:
        restart: unless-stopped
        ports:
            - '7384:3001'
        volumes:
            - '/home/arpan/docker-containers/uptime-kuma:/app/data'
        container_name: uptime-kuma
        image: 'louislam/uptime-kuma:1'
```
---
## [Navidrome](https://www.navidrome.org/docs/installation/docker/)
- **Docker-run**
```docker
docker run -d --restart unless-stopped --volume /home/arpan/docker-containers/navidrome:/data --volume /home/arpan/DATA:/music:ro -p 4533:4533 --name navidrome deluan/navidrome:latest
```
- **Docker-compose**
```docker
version: '3.3'
services:
    navidrome:
        restart: unless-stopped
        volumes:
            - '/home/arpan/docker-containers/navidrome:/data'
            - '/home/arpan/DATA:/music:ro'
        ports:
            - '4533:4533'
        container_name: navidrome
        image: 'deluan/navidrome:latest'
```
---
## [Grafana](https://grafana.com/docs/grafana/latest/setup-grafana/installation/docker/)
- **Docker-run**
```docker
docker run -d --restart unless-stopped --volume /home/arpan/docker-containers/grafana:/var/lib/grafana -p 3000:3000 --name grafana grafana/grafana:latest
```
- **Docker-compose**
```docker
version: '3.3'
services:
    grafana:
        restart: unless-stopped
        volumes:
            - '/home/arpan/docker-containers/grafana:/var/lib/grafana'
        ports:
            - '3000:3000'
        container_name: grafana
        image: 'grafana/grafana:latest'
```
- **Notes**
```bash
sudo chown -R 472:472 /home/arpan/docker-containers/grafana
```
## [Logger-dozzle](https://dozzle.dev/)
- **Docker-run**
```docker
docker run --name logger-dozzle --detach --volume=/var/run/docker.sock:/var/run/docker.sock -p 8083:8080 amir20/dozzle:latest
```
- **Docker-compose**
```docker
version: '3.3'
services:
    dozzle:
        container_name: logger-dozzle
        volumes:
            - '/var/run/docker.sock:/var/run/docker.sock'
        ports:
            - '8083:8080'
        image: 'amir20/dozzle:latest'
```
## [Influxdb](https://hub.docker.com/_/influxdb/tags?page=1&name=1.8)
- **Pre-configure**
```docker
docker run --rm influxdb:1.8 influxd config > /home/arpan/docker-containers/influxdb/influxdb.conf
```
- **Docker-run**
```docker
docker run -d --restart unless-stopped --name influxdb -p 8086:8086 --volume /home/arpan/docker-containers/influxdb:/var/lib/influxdb --volume /home/arpan/docker-containers/influxdb/influxdb.conf:/etc/influxdb/influxdb.conf influxdb:1.8
```
- **Docker-compose**
```docker
version: '3.3'
services:
    influxdb:
        restart: unless-stopped
        container_name: influxdb
        ports:
            - '8086:8086'
        volumes:
            - '/home/arpan/docker-containers/influxdb:/var/lib/influxdb'
            - '/home/arpan/docker-containers/influxdb/influxdb.conf:/etc/influxdb/influxdb.conf'
        image: 'influxdb:1.8'
```
## [Portainer](https://hub.docker.com/r/portainer/portainer-ce)
- **Docker-run**
```docker
docker run -d -p 9000:9000 --name=portainer --restart=unless-stopped -v /var/run/docker.sock:/var/run/docker.sock -v /home/arpan/docker-containers/portainer:/data portainer/portainer-ce:latest
```
- **Docker-compose**
```docker
version: '3.3'
services:
    portainer-ce:
        ports:
            - '9000:9000'
        container_name: portainer
        restart: unless-stopped
        volumes:
            - '/var/run/docker.sock:/var/run/docker.sock'
            - '/home/arpan/docker-containers/portainer:/data'
        image: 'portainer/portainer-ce:latest'
```
## [Code-server](https://hub.docker.com/r/codercom/code-server)
- **Docker-run**
```docker
docker run -d --restart unless-stopped -it --name code-server -p 8123:8080 -v /home/arpan/docker-containers/code-server:/home/coder/.config -v /home/arpan:/home/coder/project -u "$(id -u):$(id -g)" -e "DOCKER_USER=$USER" codercom/code-server:latest
```
- **Docker-compose**
```docker
version: '3.3'
services:
    code-server:
        restart: unless-stopped
        container_name: code-server
        ports:
            - '8123:8080'
        volumes:
            - '/home/arpan/docker-containers/code-server:/home/coder/.config'
            - '/home/arpan:/home/coder/project'
        environment:
            - DOCKER_USER=$USER
        image: 'codercom/code-server:latest'
```
## [Filebrowser](https://hub.docker.com/r/filebrowser/filebrowser)
- **Docker-run**
```docker
docker run -d --restart unless-stopped --name filebrowser -v /home/arpan:/srv -v /home/arpan/docker-containers/filebrowser:/database -e PUID=$(id -u) -e PGID=$(id -g) -p 8080:80 filebrowser/filebrowser:s6
```
- **Docker-compose**
```docker
version: '3.3'
services:
    filebrowser:
        restart: unless-stopped
        container_name: filebrowser
        volumes:
            - '/home/arpan:/srv'
            - '/home/arpan/docker-containers/filebrowser:/database'
        environment:
            - PUID=1000
            - PGID=1000
        ports:
            - '8080:80'
        image: 'filebrowser/filebrowser:s6'
```
## [Duplicati](https://hub.docker.com/r/linuxserver/duplicati)
- **Docker-compose**
```docker
---
version: "2.1"
services:
  duplicati:
    image: lscr.io/linuxserver/duplicati:latest
    container_name: duplicati
    environment:
      - PUID=0
      - PGID=0
      - TZ=India/Kolkata
    volumes:
      - /home/arpan/docker-containers/duplicati:/config
      - /home/arpan/DATA/duplicati_backups:/backups
      - /home/arpan:/source
    ports:
      - 8200:8200
    restart: unless-stopped                                                       
```
## [qBittorrent]([https://hub.docker.com/r/filebrowser/filebrowser](https://hotio.dev/containers/qbittorrent/))
- **Docker-compose**
```docker
version: "2.1"
services:
  qbittorrent:
    container_name: qbittorrent
    image: hotio/qbittorrent
    ports:
      - 8282:8080
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=India/Kolkata
    volumes:
      - /home/arpan/qbittorrent/config:/config
      - /home/arpan/qbittorrent/downloads:/downloads
    restart: unless-stopped

```
## [Trilium](https://hub.docker.com/r/zadam/trilium)
- **Docker-compose**
```docker
version: '2.1'
services:
  trilium:
    container_name: trilium
    image: zadam/trilium:latest
    restart: unless-stopped
    environment:
      - TRILIUM_DATA_DIR=/data
    ports:
      - "8844:8080"
    volumes:
      - /home/arpan/trilium:/data

```

## [Pi-gallary](https://github.com/bpatrik/pigallery2/blob/master/docker/README.md)
- **Docker-compose**
```docker
version: '3'
services:
  pigallery2:
    image: bpatrik/pigallery2:latest
    container_name: pigallery2
    environment:
      - NODE_ENV=production
    volumes:
      - "/home/arpan/docker-containers/pigallery/config:/app/data/config"
      - "/home/arpan/docker-containers/pigallery/db-data:/app/data/db"
      - "/home/arpan/docker-containers/pigallery/tmp:/app/data/tmp"
      - "/home/arpan/DATA/Pictures:/app/data/images:ro"
    ports:
      - 16000:80
    restart: unless-stopped

```

