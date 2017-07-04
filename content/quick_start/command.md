鑫佑贈送指令
=======


## 頁籤


* [大綱 / 簡介](#大綱--簡介)
* [正文](#正文)
* [參考](#參考)



## 正文


```
安裝docker

yum install docker
systemctl start docker
systemctl enable docker.service
```


```
 --安裝 docker 並啟動
yum install docker
systemctl start docker
systemctl enable docker.service

--docker搜尋映像檔
docker search 關鍵字

--安裝lamp
docker pull dockerfiles/centos-lamp
--安裝php52
docker pull docker.io/seti/php52

--新增一個虛擬機
docker run -d -p 49001:80 dockerfiles/centos-lamp

docker run -t -i ubuntu:12.04 /bin/bash

docker run -t -i -e PATH=/usr/lib/gradle-1.7/bin:$PATH 映像檔 /bin/bash

docker run -d -P --name web-php5.2 -v /var/www/html/tmp d76ad224c58c

docker run -t -i --privileged=true -p 49002:80 --name andenhud_backend -v /var/www/html/andenhud_backend:/var/www/html 4bd4f4eec837 /bin/bash

--啟動容器
docker start 容器

--進入容器
docker exec -ti 容器 /bin/bash

--檢查正在執行的映像檔
docker ps
docker ps -a

--目前已下載的docker
docker images

--移除映像檔
docker rm

--移除容器
docker rmi

--查看container數目
docker info
 搜尋要安裝的 mariadb

docker search mariadb

安裝 docker-mariadb

docker pull mariadb

設定本機 xxxx port 對應到 此 docker-mariadb 的 port

docker run --name mariadb1 -p 3307:3306 -e MYSQL_ROOT_PASSWORD=ex\!\#\%aq -d mariadb

docker run --name mariadb2 -p 3308:3306 -e MYSQL_ROOT_PASSWORD=ex\!\#\%aq -d mariadb

再來就在本機連入docker的mariadb

mysql -h xxx.xxx.xxx.xxx -P 3307 -uroot -p     =>   連入 mariadb1

mysql -h xxx.xxx.xxx.xxx -P 3308 -uroot -p     =>   連入 mariadb2

附註：-h xxx.xxx.xxx.xxx 請輸入主機的外部或內部IP

docekr-maraidb master-slave架構

--------------------------------------------------------------------------------------------------------------------------------------

照上面先建立好兩個 mariadb 容器

要分別進入容器設定 master & slave

這邊假設

name:mariadb1, prot 3307 是 master,

name:mariadb2, prot 3308 是 slave

先進入 master

docker exec -ti mariadb1 bash

可能沒有vim所以必須安裝

如果環境是ubuntu:

apt-get update

apt-get install vim



然後 docker-mariadb 設定擋在/etc/mysql/my.cnf

vim /etc/mysql/my.cnf

在 [mysqld] 下 加入：

#id不能重複

server-id=1
log-bin=mysql-bin



離開 docker - mariadb1

在主機上重啟mariadb1

docker restart mariadb1

同上在 docker - mariadb2 內的 my.cnf  [mysqld] 加入：

#id不能重複

server-id=2
#設定要排除的table
#replicate-ignore-table=andenhud_db.orders_tw
#設定要同步的table
#replicate-do-table=andenhud_db.orders_tw
#設定要同步的db
#binlog-do-db=andenhud_db
#唯讀
read_only
log-bin=mysql-bin
```

