倉儲中心指令
=======


## 頁籤


* [pull](#pull)
* [search](#search)
* [參考](#參考)



## pull


```
Usage:  docker pull [OPTIONS] NAME[:TAG|@DIGEST]

從倉儲中心（registry）下載鏡像，可指定標籤或整個倉庫。

Options:
  -a, --all-tags                Download all tagged images in the repository
      --disable-content-trust   略過鏡像檔驗證。（預設為 ` true `）
      --help                    顯示使用提示。
```


關於代理主機配置請參閱
[docker pull # Proxy configuration](http://docs.docker.com/engine/reference/commandline/pull/#proxy-configuration)
。

關於變更倉儲中心（registry）位置請參閱
[docker pull # Pulling from a different registry](http://docs.docker.com/engine/reference/commandline/pull/#pulling-from-a-different-registry)
。


##### 演示：


若沒指定標籤，預設使用 ` :latest `。


```
$ docker pull debian

Using default tag: latest
latest: Pulling from library/debian
fdd5d7827f33: Pull complete
a3ed95caeb02: Pull complete
Digest: sha256:e7d38b3517548a1c71e41bffe9c8ae6d6d29546ce46bf62159837aad072c90aa
Status: Downloaded newer image for debian:latest
```


以數位文摘方式下載鏡像：

以此方式可鎖定下載鏡像版本，避免更新版本而導致程式錯誤。


```
$ docker pull ubuntu:14.04

14.04: Pulling from library/ubuntu
5a132a7e7af1: Pull complete
fd2731e4c50c: Pull complete
28a2f68d1120: Pull complete
a3ed95caeb02: Pull complete
Digest: sha256:45b23dee08af5e43a7fea6c4cf9c25ccf269ee113168c19722f87876677c5cb2
Status: Downloaded newer image for ubuntu:14.04
Docker prints the digest of the image after the pull has finished. In the example above, the digest of the image is:

# 相同於

$ docker pull ubuntu@sha256:45b23dee08af5e43a7fea6c4cf9c25ccf269ee113168c19722f87876677c5cb2

sha256:45b23dee08af5e43a7fea6c4cf9c25ccf269ee113168c19722f87876677c5cb2: Pulling from library/ubuntu
5a132a7e7af1: Already exists
fd2731e4c50c: Already exists
28a2f68d1120: Already exists
a3ed95caeb02: Already exists
Digest: sha256:45b23dee08af5e43a7fea6c4cf9c25ccf269ee113168c19722f87876677c5cb2
Status: Downloaded newer image for ubuntu@sha256:45b23dee08af5e43a7fea6c4cf9c25ccf269ee113168c19722f87876677c5cb2
```



下載倉庫裡所有鏡像：

鏡像的新舊版本皆儲放於倉庫 ` repository ` 當中，以 ` -a, --all-tags ` 命令可下載倉庫內全部的鏡像。

```
$ docker pull --all-tags fedora

Pulling repository fedora
ad57ef8d78d7: Download complete
105182bb5e8b: Download complete
511136ea3c5a: Download complete
73bd853d2ea5: Download complete
....

Status: Downloaded newer image for fedora

$ docker images fedora

REPOSITORY   TAG         IMAGE ID        CREATED      SIZE
fedora       rawhide     ad57ef8d78d7    5 days ago   359.3 MB
fedora       20          105182bb5e8b    5 days ago   372.7 MB
fedora       heisenbug   105182bb5e8b    5 days ago   372.7 MB
fedora       latest      105182bb5e8b    5 days ago   372.7 MB
```


## search


```
Usage:  docker search [OPTIONS] TERM

搜尋倉儲中心裡的鏡像。

Options:
  -f, --filter value   依據篩選條件輸出。（無預設）
                       - is-automated=(true|false)
                       - is-official=(true|false)
                       - stars=<number> - 不可低於多少星星數量。
      --help           顯示使用提示。
      --limit int      限制輸出的項目比數。（預設 25）
      --no-trunc       「Don't truncate」，不要限縮顯示完整內容。
```

##### 設定篩選條件寫法：


* 單個： ` -f "key=value" `、` -f key=value `。
* 多個： ` -f key1=value1 -f key2=value2 `、` -f key1=value1,key2=value2 `。



##### 演示：


```
$ sudo docker search -f stars=10 node
NAME                        DESCRIPTION                            STARS   OFFICIAL   AUTOMATED
node                        Node.js is a JavaScript-based...       3168    [OK]
nodered/node-red-docker     Node-RED Docker images.                36                 [OK]
strongloop/node             StrongLoop, Node.js, and tools.        33                 [OK]
bitnami/node                Bitnami Node.js Docker Image           17                 [OK]

$ sudo docker search --limit 5 node,js
NAME                        DESCRIPTION                            STARS   OFFICIAL   AUTOMATED
node                        Node.js is a JavaScript-based...       3168    [OK]
iojs                        io.js is an npm compatible...          122     [OK]
mongo-express               Web-based MongoDB admin interface...   90      [OK]
pizak/auto-deploy-node-js   simple auto deployed node js app       1                  [OK]
cvjt/node-js-docker         Triad CDP test ground                  1                  [OK]
```


* NAME         : 名稱。
* DESCRIPTION  : 描述。
* STARS        : 星星數。
* OFFICIAL     : 是否由官方所建立。
* AUTOMATED    : 與程式檔位置建立自動更新。



## 參考


* [docker pull - Docker](http://docs.docker.com/engine/reference/commandline/pull/)
* [docker search - Docker](http://docs.docker.com/engine/reference/commandline/search/)
* [程式扎記: Docker Practice - Repository](http://puremonkey2010.blogspot.tw/2015/05/docker-practice-repository.html)

