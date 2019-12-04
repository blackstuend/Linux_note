# Doker Learning 

## 更新新版kernal
*  [參考](http://blog.itist.tw/2016/03/how-to-upgrade-newest-kernel-on-centos-7.html)

```
# rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-2.el7.elrepo.noarch.rpm
# rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
# yum -y --enablerepo=elrepo-kernel install kernel-ml
#reboot
```
##  Docker install(Centos)

```
# yum install docker -y
# systemctl start docker
```



## Docker command

```

# dokcer tag test:0.1 blackfloat/test:0.1
# docker serach centos //搜尋關於centos 的images
# docker pull centos:7 //拉下來
# docker -d //d = deamon 背景執行
# docker images //顯示docker 現在已安裝的images
# docker ps |查看現在執行的status
# docker run -i -t centos 7 /bin/bash //-i =互動 -t = terminal 進入會有終端機 
# docke start (name)
# docker exec -it name
# docker attach (name)
# docker stop (name)
# docker run -it --rm centos:7 /bin/bash //if use --rm this command ,when you exit your docker container,your docker container will be done.

```
* 關機後docker內的contanier還繼續存在

```
# docker run -itd --restart always centos:7
```

* 跳出docker 按住ctrl+p +q 會讓container 的status 還是 up的

## docker 將境像儲存成images

```
# docker commit (docker_process_id) (new_image_name):(version)
```

## docker del all container(stop)

```
# docker container prune
```

## docker del one containter or del one images

```
# docker rm -f (containter-name)
# docker rmi -f (images-name)
# docker rm -f $(docker ps -q -a)
```
## docker login and push images to cloud
 
```
# docker login
# docker push blackfloat/test:0.1
```
## docker mounted
```
# docker run -itd -p 8080:80 -v /mydata/usr/local/apache2/htocs/ httpd
# docker run -itd -volumes-from {{contanier id}} -p 8081:80 httpd //接著上面運用voulumes 將上面掛載的位子 接著掛載到下個contaniner
```

## docker add enviroment variable

````
# $ docker run --name mysql -e MYSQL_ROOT_PASSWORD=123456 -d mysql // -e = add enviroment variable
````

## docker pull 

```
# docker blackfloat/test:0.1
```

## docker registry 

* 幫助我們用內定的網路就可以傳docker image到特定的電腦而不用透過docker hub，速度更快
* [網站](https://hub.docker.com/_/registry)
* 先安裝registry
```
# docker pull registry
```

* run regisrty

```
# docker run -d -p 5000:5000 --restart always --name registry registry:2
```

* tag the image and push the image on the registry 

```
#  docker tag ubuntu 127.0.0.1:5000/ubuntu
# docker  127.0.0.1:5000/ubuntupush
```
* 再用另一台pull
* 要先加入security
```
# gedit /etc/docker/deamon.json
// 加上下面這行
{
  "insecure-registries" : ["192.168.56.101:5000"]
}
* docker pull 192.168.56.101:5000/ubuntu
```

* 查看registry目前上傳的Images
```
# curl -X GET http://127.0.0.1:5000/v2/_catalog
```


* 將docker 

````
# cat export_busybox.tar | docker import - busybox1
````

* Docker 查詢 volumes的位址

```
# docker volumes ls  //查看volumes 目前有哪些Container在儲存volumes
# docker volumes inspect 
```
