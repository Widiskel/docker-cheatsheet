# Docker

## Table Of Contents
- [Docker](#docker)
  - [Table Of Contents](#table-of-contents)
  - [Installing Docker](#installing-docker)
  - [DockerFile](#dockerfile)
    - [DockerFile](#dockerfile-1)
    - [Building DockerFile](#building-dockerfile)
  - [Docker Compose](#docker-compose)
    - [Docker Compose](#docker-compose-1)
    - [Run A Docker Compose](#run-a-docker-compose)
  - [Docker Commit](#docker-commit)
  - [Docker Hub](#docker-hub)
    - [Add Tag To Docker Images](#add-tag-to-docker-images)
    - [Pushing Images To Docker Hub](#pushing-images-to-docker-hub)
    - [Pulling Images From Docker Hub](#pulling-images-from-docker-hub)
  - [Docker Command Cheat Sheet](#docker-command-cheat-sheet)
    - [Docker Compose](#docker-compose-2)
    - [Docker Run Container From Image](#docker-run-container-from-image)
    - [Docker Start Stop Container](#docker-start-stop-container)
    - [Open Shell On Container](#open-shell-on-container)
    - [Install Package On Container](#install-package-on-container)
    - [View Resource Usage Of Container](#view-resource-usage-of-container)
    - [Docker Images](#docker-images)
    - [Copy File Between Host And Container](#copy-file-between-host-and-container)
    - [Log Running Container](#log-running-container)
    - [Docker Hub](#docker-hub-1)
    - [Docker Prune](#docker-prune)
    - [Docker Network](#docker-network)


## Installing Docker
Ada berbagai cara untuk melakukan instalasi docker bergantung pada OS, dan Versi yang digunakan. Akan lebih jelas apabila melihat langsung pada dokumentasi instalasi yang disediakan oleh Docker [Installing Docker](https://docs.docker.com/engine/install/).

## DockerFile
### DockerFile
**Dockerfile** adalah dokumen teks yang berisi *command* yang dapat dipanggil pengguna pada *command-line* untuk membuat sebuah **Docker Image**. 

Berikut adalah beberapa contoh penulisan **DockerFile**
- [DockerFile NextJs](docker/nextjs-dockerfile.md)
  
### Building DockerFile
Setelah kita membuat sebuah **DockerFile**, tahapan selanjutnya pastilah membuat sebuah *docker images* dari **DockerFile** yang kita buat. Untuk melakukan *build image* kita hanya perlu menjalankan perintah
```bash
docker build -t IMAGENAME .
```
kita juga bisa menambahkan berbagai *options* seperti `--platform linux/amd64` dan lainnya, dengan mengeksekusi *command*  diatas, akan tercipta sebuah *docker images*, kita bisa melihatnya dengan mengetikkan perintah `docker images` untuk lebih lengkapnya bisa dilihat pada [Docker build](https://docs.docker.com/reference/cli/docker/image/build/)

## Docker Compose
### Docker Compose
**Docker Compose** adalah *tool* yang membantu kita mendefinisikan dan berbagi aplikasi multi-container. Dengan Compose, kita bisa membuat file YAML untuk mendefinisikan *service* dan dengan hanya satu *command* untuk menjalankan atau menghentikan sebuah *service* biasanya docker compose ditulis pada file dengan nama `docker-compose.yml`.

Berikut adalah beberapa contoh penulisan **Docker Compose File**
- [Kibana & Elastic Search docker-compose.yml](docker/kibana-elasticsearch-docker-compose.md)

**Docker Compose** bisa membaca variabel yang kita tuliskan pada sebuah file `.env` dengan menggunakan anotasi `${NAMA VARIABEL}`. **Docker Compose** juga bisa mengambil resource dari **DockerFile** dengan menggunakan `dockerfile: Dockerfile`.
### Run A Docker Compose
Untuk menjalankan ataupun menghentukan service pada `docker-compose.yml` kita cukup menggunakan *command*
```bash
docker compose up
```
atau 
```bash
docker compose up -d
```
pada *command* diatas ada sebuah flag `-d` yang artinya `detached` agar *service* yang kita jalankan bisa berjalan di *background*. Setelah *command* diatas dieksekusi maka nantinya akan tercipa sebuah *docker container* yang bisa kita lihat dengan mengetikkan *command* `docker ps` atau `docker ps -a`, untuk lebih jelasnya bisa dilihat pada [Overview Docker Compose](https://docs.docker.com/compose/reference/)


## Docker Commit
Ketika kita sudah membuat image dan menjalankan sebuah container, terkadang kita juga melakukan perubahan secara langsung dari dalam *container* yang kita buat, contohnya misalkan kita menginstall nano pada *container* yang sedang berjalan. Sayangnya apabila kita menjalankan *container* lagi dengan *image* ataupun *compose* file yang kita buat, perubahan tersebut akan hilang. Maka dari itu kita perlu menggunakan **Docker Commit**. Dengan menggunakan **Docker Commit**, kita bisa membuat sebuah *image* baru dari *conntainer* kita yang sedang berjalan. Untuk menggunakan **Docker Commit**, kita bisa menjalankan *command*
```bash
docker commit RUNNINGCONTAINERNAMEORID IMAGETAGNAME
```
 
## Docker Hub
Seperti halnya Github yang kita gunakan untuk me*manage repository*, docker juga memiliki **Docker Hub** sebagai sarana bagi kita untuk me*manage docker images / container*. Untuk menggunakan docker hub, kita perlu membuat akun terlebih dahulu pada halaman [Docker Hub](https://hub.docker.com/).

### Add Tag To Docker Images
untuk memberikan tag pada *docker images* yang kita buat, kita bisa menggunakan *command*.
```bash
docker tag IMAGENAME:latest USERNAME/REPONAME:latest
```
### Pushing Images To Docker Hub
Setelah kita memberikan tag pada *docker images* kita bisa melakukan push ke docker hub dengan menggunakan *command*.
```bash
docker push USERNAME/REPONAME:latest
```
### Pulling Images From Docker Hub
Setelah kita melakukan *push* ke docker hub, kita bisa melakukan *pull* untuk mendapatkan *image* terbaru dengan menggunakan *command*.
```bash
docker pull USERNAME/REPONAME:latest
```

## Docker Command Cheat Sheet
### Docker Compose
```bash
# Start Service
docker compose up -d
# Stop Service
docker compose down -d
```
### Docker Run Container From Image
```bash
# Run Container From Image
docker run -d --name CONTAINERNAME -it IMAGENAMENTAG
# Run Container From Image With Port Forwarding
docker run -d --name CONTAINERNAME -p 3001:3000 -it IMAGENAMENTAG
# Run Container From Image With PoRt Forwarding And Environment Variable
docker run -d --name CONTAINERNAME -p 3001:3000 -it IMAGENAMENTAG -e "SOMEENVIRONMENTVARIABLE"
```

### Docker Start Stop Container
```bash
# Start Container
docker start CONTAINERIDORNAME
# Stop Container
docker stop CONTAAINERIDORNAME
```

### Open Shell On Container
```bash
# Open Shell or CLI on running container
docker exec -it CONTAINERNAMEORID /bin/bash
docker exec -it CONTAINERNAMEORID /sh
```

### Install Package On Container
It might be different on different Container OS
```bash
pkg install nano
apt install nano
apk install nano
```

### View Resource Usage Of Container
```bash
docker container stats
```

### Docker Images
```bash
# Build Image
docker build -t IMAGENAME
# Build Image With No Chache
docker build -t IMAGENAME . –no-cache
# List Docker Images
docker images
# Delete Images
docker rmi IMAGENAME
# Delete Unused Images
docker image prune 

```


### Copy File Between Host And Container
```bash
# Copy From Local To Container
docker cp /PATHONLOCAL CONTAINERIDORNAME:/PATHONCONTAINER
# Copy From Container To Local
docker cp CONTAINERIDORNAME:/PATHONCONTAINER /PATHONLOCAL
# Stdout file from container
docker cp CONTAINERIDORNAME:/PATHTOLOGFILEONCONTAINER - | tar x -O | grep "ERROR"
```

### Log Running Container
```bash
# Docker log from running container
docker logs -f CONTAINERIDORNAME
```

### Docker Hub
```bash
# Login into Docker
docker login -u USERNAME
# Publish an image to Docker Hub
docker push USERNAME/IMAGENAME
# Search Hub for an image
docker search IMAGENAME
# Pull an image from a Docker Hub
docker pull IMAGENAME
docker pull USERNAME/IMAGENAME
```

### Docker Prune
```bash
# Delete Unused Docker resource (image, container, volume, network)
docker system prune
docker system prune --all
```

### Docker Network
```bash
docker create network NETWORKNAME
```

For more cheat sheet, you can read from [Docker Labs](https://dockerlabs.collabnix.com/docker/cheatsheet/)
