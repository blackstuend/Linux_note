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
```
## docker login and push images to cloud
 
```
# docker login
# docker push blackfloat/test:0.1
```
## docker pull 

```
# docker blackfloat/test:0.1
```