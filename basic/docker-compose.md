# Docker-compose
* 幫忙一次執行很多image的腳本

##  Install 

* [參考](https://docs.docker.com/compose/install/)
1. Run this command to download the current stable release of Docker Compose:

```
$ sudo curl -L "https://github.com/docker/compose/releases/download/1.24.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

2. Apply executable permissions to the binary:

```
$ sudo chmod +x /usr/local/bin/docker-compose
```

* Add more image 

```
$ sudo docker-compose scale wordporess=3 db=2
```
