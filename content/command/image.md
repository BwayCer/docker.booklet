鏡像指令
=======



## 頁籤


* [build](#build)
* [commit](#commit)
* [history](#history)
* [image](#image)
  * [指定顯示的鏡像](#指定顯示的鏡像)
  * [列出完整識別碼](#列出鏡像完整識別碼)
  * [列出數位文摘](#列出鏡像數位文摘)
  * [顯示未標記的鏡像](#顯示未標記的鏡像)
  * [「label」鏡像](#「label」鏡像)
  * [顯示創建時間區間範圍內的鏡像](#顯示創建時間區間範圍內的鏡像)
  * [格式化輸出](#格式化輸出)
* [rmi](#rmi)
  * [強行移除](#強行移除)
  * [以數位文摘移除鏡像](#以數位文摘移除鏡像)
* [tag](#tag)
* [參考](#參考)



## build


```
i
```



## commit


```
Usage:  docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]

由容器內容的改變來創建鏡像。

Options:
  -a, --author string    Author (e.g., "John Hannibal Smith <hannibal@a-team.com>")
  -c, --change value     Apply Dockerfile instruction to the created image (default [])
      --help             顯示使用提示。
  -m, --message string   Commit message
  -p, --pause            Pause container during commit (default true)
```



## history


```
Usage:  docker history [OPTIONS] IMAGE

Show the history of an image

Options:
      --help       顯示使用提示。
  -H, --human      顯示可閱讀的日期時間和檔案大小。（預設 true）
      --no-trunc   「Don't truncate」，不要限縮顯示完整內容。
  -q, --quiet      只顯示鏡像識別碼。
```


```
# 官網範例

$ docker history docker

IMAGE               CREATED             CREATED BY                                      SIZE                COMMENT
3e23a5875458        8 days ago          /bin/sh -c #(nop) ENV LC_ALL=C.UTF-8            0 B
8578938dd170        8 days ago          /bin/sh -c dpkg-reconfigure locales &&    loc   1.245 MB
be51b77efb42        8 days ago          /bin/sh -c apt-get update && apt-get install    338.3 MB
4b137612be55        6 weeks ago         /bin/sh -c #(nop) ADD jessie.tar.xz in /        121 MB
750d58736b4b        6 weeks ago         /bin/sh -c #(nop) MAINTAINER Tianon Gravi <ad   0 B
511136ea3c5a        9 months ago                                                        0 B                 Imported from


# 實測結果

$ docker pull docker
Using default tag: latest
latest: Pulling from library/docker

3690ec4760f9: Pull complete
7601c2ad1cd1: Pull complete
a2fa41e1947e: Pull complete
7ee74a896d83: Pull complete
Digest: sha256:83795a384fd7321e1a58d06c2fa2b86a0f0ce9e4a75c91aa382317b3c453671d
Status: Downloaded newer image for docker:latest


$ docker history docker

IMAGE               CREATED             CREATED BY                                      SIZE                COMMENT
cf693ec9b5c7        4 weeks ago         /bin/sh -c #(nop)  CMD ["sh"]                   0 B
<missing>           4 weeks ago         /bin/sh -c #(nop)  ENTRYPOINT ["docker-entryp   0 B
<missing>           4 weeks ago         /bin/sh -c #(nop) COPY file:399605dc1850a60a5   524 B
<missing>           4 weeks ago         /bin/sh -c set -x  && curl -fSL "https://${DO   98.38 MB
<missing>           4 weeks ago         /bin/sh -c #(nop)  ENV DOCKER_SHA256=626601de   0 B
<missing>           4 weeks ago         /bin/sh -c #(nop)  ENV DOCKER_VERSION=1.12.3    0 B
<missing>           6 weeks ago         /bin/sh -c #(nop)  ENV DOCKER_BUCKET=get.dock   0 B
<missing>           6 weeks ago         /bin/sh -c apk add --no-cache   ca-certificat   1.876 MB
<missing>           6 weeks ago         /bin/sh -c #(nop) ADD file:7afbc23fda8b0b3872   4.803 MB


$ docker rmi --no-prune docker

Untagged: docker:latest
Untagged: docker@sha256:83795a384fd7321e1a58d06c2fa2b86a0f0ce9e4a75c91aa382317b3c453671d
Deleted: sha256:cf693ec9b5c76a8d05bb5e0c38257d5a575b0e1a7af56037cea8f54e9a751cab
Deleted: sha256:aa023e391ad25455b5c075827f7cd8a72742c208bc489c5ce30afe1ad9a6c49b
Deleted: sha256:5b82e43655bace02ebfad49c4783bf9ee370fa3019d969b801dff3f4efd059ac
Deleted: sha256:03ebb67b76d1f316105f825d255c35f97c9e01466910019e1579fd4b6b3a9df2
Deleted: sha256:011b303988d241a4ae28a6b82b0d8262751ef02910f0ae2265cb637504b72e36
```



## image


```
Usage:  docker images [OPTIONS] [REPOSITORY[:TAG]]

列出鏡像清單。

Options:
  -a, --all             顯示所有鏡像。（預設隱藏中間層鏡像）
      --digests         顯示數位文摘。
  -f, --filter value    依據篩選條件輸出。（無預設）
                        - dangling=(true|false) - 顯示未標記的鏡像
                        - label=<key> or label=<key>=<value>
                        - before=<image-expression>
                        - since=<image-expression>
      --format string   Pretty-print images using a Go template
      --help            顯示使用提示。
      --no-trunc        「Don't truncate」，不要限縮顯示完整內容。
  -q, --quiet           只顯示鏡像識別碼。
```

` image-expression `： ` image-name[:tag] `、` image-id `、` image@digest `。


預設情況下只顯示最上層的鏡像。

鏡像是由很多層的組合，而層是可在鏡像間被共用，因此減少消耗硬碟空間，and speed up docker build by allowing each step to be cached。

列出的檔案大小其計算包含鏡像使用的所有層，這也是當運行 ` docker save ` 時壓縮檔的檔案大小。


```
$ docker images

REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
<none>              <none>              63d79010c578        3 days ago          127.2 MB
node                latest              89cd8193c9a1        8 days ago          655.5 MB
docker              latest              cf693ec9b5c7        4 weeks ago         105.1 MB
ubuntu              16.04               f753707788c5        7 weeks ago         127.2 MB
postgres            9                   746b819f315e        4 days ago          213.4 MB
postgres            9.3.5               746b819f315e        4 days ago          213.4 MB
postgres            latest              746b819f315e        4 days ago          213.4 MB
```


* REPOSITORY： 倉庫名。
* TAG： 標籤名。
* IMAGE ID： 鏡像識別碼。
* CREATED： 創建時間。
* SIZE： 檔案大小。


### 指定顯示的鏡像


以指定 ` [REPOSITORY[:TAG]] ` 倉庫和標籤名來限制列出的鏡像。


```
$ docker images java
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
java                8                   308e519aac60        6 days ago          824.5 MB
java                7                   493d82594c15        3 months ago        656.3 MB
java                latest              2711b1d6f3aa        5 months ago        603.9 MB


$ docker images java:8
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
java                8                   308e519aac60        6 days ago          824.5 MB
```


### 列出完整識別碼


```
$ docker images --no-trunc
REPOSITORY     TAG        IMAGE ID                                                                  CREATED          SIZE
<none>         <none>     sha256:63d79010c57885363c219eb1ffd76cd756c2c32fe6b2061791d996fb8633bf82   3 days ago       127.2 MB
node           latest     sha256:89cd8193c9a1c520c69429e919bd8b120273526600a75f14229690222f72a534   8 days ago       655.5 MB
docker         latest     sha256:cf693ec9b5c76a8d05bb5e0c38257d5a575b0e1a7af56037cea8f54e9a751cab   4 weeks ago      105.1 MB
ubuntu         16.04      sha256:f753707788c5c100f194ce0a73058faae1a457774efcda6c1469544a114f8644   7 weeks ago      127.2 MB
```


### 列出數位文摘

當使用 ` push ` 或 ` pull ` 皆會顯示數位文摘，其適用的範圍包含 ` pull `、` create `、` run `、` rmi ` 和 「Dockerfile」的 ` FROM `。


```
$ docker images --digests
REPOSITORY     TAG        DIGEST                                                                    IMAGE ID         CREATED        SIZE
<none>         <none>     <none>                                                                    63d79010c578     3 days ago     127.2 MB
node           latest     sha256:85545ad7f691afc346ac3990d89f447cde06999690007d23f1d13d55efb247d3   89cd8193c9a1     8 days ago     655.5 MB
docker         latest     sha256:83795a384fd7321e1a58d06c2fa2b86a0f0ce9e4a75c91aa382317b3c453671d   cf693ec9b5c7     4 weeks ago    105.1 MB
ubuntu         16.04      sha256:2d44ae143feeb36f4c898d32ed2ab2dffeb3a573d2d8928646dfc9cb7deb1315   f753707788c5     7 weeks ago    127.2 MB
```


The filtering flag (-f or --filter) format is of “key=value”. If there is more than one filter, then pass multiple flags (e.g., --filter "foo=bar" --filter "bif=baz")
後
The currently supported filters are:

dangling (boolean - true or false)


### 顯示未標記的鏡像


```
$ docker images --filter "dangling=true"

REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
<none>              <none>              8abc22fbb042        4 weeks ago         0 B
<none>              <none>              48e5f45168b9        4 weeks ago         2.489 MB
<none>              <none>              dea752e4e117        12 weeks ago        101.4 MB
```


This will display untagged images, that are the leaves of the images tree (not intermediary layers). These images occur when a new build of an image takes the repo:tag away from the image ID, leaving it as <none>:<none> or untagged. A warning will be issued if trying to remove an image when a container is presently using it.


可藉此來批量刪除此類鏡像：


```
$ docker rmi $(docker images -f "dangling=true" -q)

8abc22fbb042
48e5f45168b9
bf747efa0e2f
980fe10e5736
dea752e4e117
511136ea3c5a
```


### 「label」鏡像


```
label (label=<key> or label=<key>=<value>)
```

其他有關的資訊請參考
[docker images # Filtering](http://docs.docker.com/engine/reference/commandline/images/#Filtering)
。


### 顯示創建時間區間範圍內的鏡像


* ` before `： 在指定鏡像創建之前。
* ` since `： 現在起算至指定鏡像創建之間。


```
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED              SIZE
image1              latest              eeae25ada2aa        4 minutes ago        188.3 MB
image2              latest              dea752e4e117        9 minutes ago        188.3 MB
image3              latest              511136ea3c5a        25 minutes ago       188.3 MB


$ docker images --filter "before=image1"
REPOSITORY          TAG                 IMAGE ID            CREATED              SIZE
image2              latest              dea752e4e117        9 minutes ago        188.3 MB
image3              latest              511136ea3c5a        25 minutes ago       188.3 MB


$ docker images --filter "since=image3"
REPOSITORY          TAG                 IMAGE ID            CREATED              SIZE
image1              latest              eeae25ada2aa        4 minutes ago        188.3 MB
image2              latest              dea752e4e117        9 minutes ago        188.3 MB
```


### 格式化輸出


 符號         | 描述
:----         |:----
table         | 顯示欄位名稱。（放於句首）
.ID           | 識別碼。
.Repository   | 倉庫名。
.Tag          | 標籤名。
.Digest       | 數位文摘。
.CreatedSince | 被建立多長時間。
.CreatedAt    | 被建立的日期時間。
.Size         | 檔案大小。


```
$ docker images --format "table {{.Repository}}\t{{.Tag}}\t{{.ID}}\t{{.Size}}\t{{.CreatedAt}}"
REPOSITORY          TAG                 IMAGE ID            SIZE                CREATED AT
<none>              <none>              b6b63b054554        127.2 MB            2016-12-02 22:06:46 +0800 CST
node                latest              89cd8193c9a1        655.5 MB            2016-11-24 03:04:57 +0800 CST
docker              latest              cf693ec9b5c7        105.1 MB            2016-11-01 05:46:21 +0800 CST
ubuntu              16.04               f753707788c5        127.2 MB            2016-10-14 05:13:21 +0800 CST


$ docker images --digests --format "{{.Repository}}:{{.Tag}}\t{{.ID}}\t{{.Digest}}"
<none>:<none>   b6b63b054554    <none>
node:latest     89cd8193c9a1    sha256:85545ad7f691afc346ac3990d89f447cde06999690007d23f1d13d55efb247d3
docker:latest   cf693ec9b5c7    sha256:83795a384fd7321e1a58d06c2fa2b86a0f0ce9e4a75c91aa382317b3c453671d
ubuntu:16.04    f753707788c5    sha256:2d44ae143feeb36f4c898d32ed2ab2dffeb3a573d2d8928646dfc9cb7deb1315
```



## rmi


```
Usage:  docker rmi [OPTIONS] IMAGE [IMAGE...]

移除一個或多個鏡像。

Options:
  -f, --force      強行移除鏡像。
      --help       顯示使用提示。
      --no-prune   Do not delete untagged parents
```


能以完整或最小長度的識別碼（id）、標籤（tag）或者數位文摘（digest）來指定欲移除鏡像。


### 演示：


如果相同鏡像有多個標籤（tag）參照，此時若想移除鏡像，則要把其他標籤參照的鏡像全部移除。

當以標籤移除鏡像時，數位文摘會自動被移除。


```
$ docker images
REPOSITORY           TAG         IMAGE ID          CREATED            SIZE
test1                latest      fd484f19954f      23 seconds ago     7 B (virtual 4.964 MB)
test                 latest      fd484f19954f      23 seconds ago     7 B (virtual 4.964 MB)
test2                latest      fd484f19954f      23 seconds ago     7 B (virtual 4.964 MB)


$ docker rmi fd484f19954f
Error: Conflict, cannot delete image fd484f19954f because it is tagged in multiple repositories,
use -f to force 2013/12/11 05:47:16 Error: failed to remove one or more images


$ docker rmi test1
Untagged: test1:latest
$ docker rmi test2
Untagged: test2:latest


$ docker images
REPOSITORY           TAG         IMAGE ID          CREATED            SIZE
test                 latest      fd484f19954f      23 seconds ago     7 B (virtual 4.964 MB)


$ docker rmi test
Untagged: test:latest
Deleted: fd484f19954f4920da7ff372b5067f5b7ddb2fd3830cecd17b96ea9e286ba5b8
```


### 強行移除


除了上述方法，也能以 ` -f, --force ` 強行移除指定鏡像及其所有參照產生的相同鏡像。


```
$ docker images

REPOSITORY           TAG         IMAGE ID          CREATED            SIZE
test1                latest      fd484f19954f      23 seconds ago     7 B (virtual 4.964 MB)
test                 latest      fd484f19954f      23 seconds ago     7 B (virtual 4.964 MB)
test2                latest      fd484f19954f      23 seconds ago     7 B (virtual 4.964 MB)


$ docker rmi -f fd484f19954f

Untagged: test1:latest
Untagged: test:latest
Untagged: test2:latest
Deleted: fd484f19954f4920da7ff372b5067f5b7ddb2fd3830cecd17b96ea9e286ba5b8
```


### 以數位文摘移除鏡像


```
$ docker images --digests

REPOSITORY                     TAG       DIGEST                                                                    IMAGE ID        CREATED         SIZE
localhost:5000/test/busybox    <none>    sha256:cbbf2f9a99b47fc460d422812b6a5adff7dfee951d8fa2e4a98caa0382cfbdbf   4986bf8c1536    9 weeks ago     2.43 MB


$ docker rmi localhost:5000/test/busybox@sha256:cbbf2f9a99b47fc460d422812b6a5adff7dfee951d8fa2e4a98caa0382cfbdbf

Untagged: localhost:5000/test/busybox@sha256:cbbf2f9a99b47fc460d422812b6a5adff7dfee951d8fa2e4a98caa0382cfbdbf
Deleted: 4986bf8c15363d1c5d15512d5266f8777bfba4974ac56e3270e7760f6f0a8125
Deleted: ea13149945cb6b1e746bf28032f02e9b5a793523481a0a18645fc77ad53c4ea2
Deleted: df7546f9f060a2268024c8a230d8639878585defcc1bc6f79d2728a13957871b
```



## tag


```
Usage:  docker tag IMAGE[:TAG] IMAGE[:TAG]

標示鏡像到指定的倉庫。

Options:
      --help   顯示使用提示。
```


關於命名方式請參考
[鏡像命名]()
。

鏡像會以倉庫名及標籤畫分出群組，並能以群組為單位上傳。



### 演示


```
# 以識別碼指定
docker tag 0e5574283393 fedora/httpd:version1.0

# 以名稱指定
docker tag httpd fedora/httpd:version1.0
# 等同於
docker tag httpd:test fedora/httpd:version1.0.test

# 使用私人倉儲中心
docker tag 0e5574283393 myregistryhost:5000/fedora/httpd:version1.0
```



## 參考


* [docker history - Docker](http://docs.docker.com/engine/reference/commandline/history/)
* [docker images - Docker](http://docs.docker.com/engine/reference/commandline/images/)
* [docker rmi - Docker](http://docs.docker.com/engine/reference/commandline/rmi/)
* [docker tag - Docker](http://docs.docker.com/engine/reference/commandline/tag/)

