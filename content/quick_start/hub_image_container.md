三大單位
=======


## 前言


以過來人的視角敘述想對後進說的話...



## 頁籤


* [簡介](#簡介)
* [倉儲中心](#倉儲中心)
  * [關於翻譯](#關於翻譯)
  * [倉庫](#倉庫)
* [鏡像](#鏡像)
  * [鏡像識別](#鏡像識別)
  * [數位文摘](#數位文摘)
* [容器](#容器)
  * [容器識別](#容器識別)
* [參考](#參考)



## 簡介


新起的新興的事物倚靠既有的模式，參考其運作、管理、單位 ... 等等分類以快速完善大體系的結構。
我認為船塢工人（docker）即是參考海事的相關模式來完成這系統的邏輯。


船塢工人除了虛擬機的功能外，最大的特色就屬作業系統可自製、儲存和分享，
其有三個大單位： 倉儲中心（Hub）、鏡像（Image）和容器（Container）等。
其中容器是用於虛擬機的環境，而鏡像和倉儲中心是那作業系統和作業系統的轉運樞紐，
其儲放關係為「鏡像放於倉庫（Repository），倉庫貼上用戶識別後堆在倉儲中心，並交由管理處（Registry）管理」。



## 倉儲中心


船塢工人倉儲中心
[Docker Hub](http://hub.docker.com/)
為預設的倉儲中心，絕大多數用來建立基礎環境的鏡像都可在此找到。

若沒有上傳鏡像存放到遠端的需求，則不需申請倉儲中心帳號，但仍可在倉儲中心找尋所需的檔案並下載使用。

此官方的倉儲中心屬公開共享的空間，讀者也可以依需求建立所需的私有倉儲中心。其建立可以參考：

  * [docker/docker-registry - GitHub](http://github.com/docker/docker-registry)
  * [私有倉庫 · 《Docker —— 從入門到實踐­》正體中文版](https://philipzheng.gitbooks.io/docker_practice/content/repository/local_repo.html)



### 關於翻譯


船塢工人使用「Hub」、「Registry」來描述其雲端主機，在用法上看似相同的詞令我很困擾，
例如說：「Pull an image or a repository from a Docker registry」，
咦！為何是由註冊機構下載呢？對於 git 的使用我都直接從 GitHub 下載啊，有何不同呢？

我的見解是說，「Hub」是倉庫（Repository）的儲放位置，而「Registry」則是「Hub」的管理機構。
倉庫可大可小，所以能裝得下倉庫的東西我實在想不出來，於是乎「倉儲中心」挺合適的。
「從倉儲中心下載檔案」也很合理，偏偏船塢工人要到倉儲中心非得經過管理單位，故名為「倉儲中心管理處」，簡稱「倉管處」，但若沒特別強調皆以倉儲中心稱呼。



### 倉庫


註冊成為倉儲中心的用戶後即可建立自己的倉庫（Repository），倉庫用於存放鏡像，
當要指定某鏡像時需提供「廣義的倉庫名」去找尋檔案，其格式像網址般，如下例：

```
<主機名>/<用戶名>/<倉庫名>
```

> 若為官方所建立的鏡像則沒有用戶名（不知是否為絕對及官方是指原著還是船塢工人？）。


倉庫內通常存放相關類型的鏡像，如 `ubuntu` 的倉庫裡有 `16.04`、`14.04` ... 等等的標籤以區分鏡像，其格式如下例：

```
<廣義的倉庫名>:<標籤名>
```


更多重點在 [鏡像](#鏡像) 一節說明。



## 鏡像


鏡像的學習幾乎含括了船塢工人的操作邏輯。


更多關於鏡像、層和內容可檢索貯存的資訊可閱讀
[understand images, containers, and storage drivers](http://docs.docker.com/engine/userguide/storagedriver/imagesandcontainers/)
文章，其也可幫助瞭解鏡像的組成喔！。



### 鏡像識別


鏡像被識別藉由以下方式：

 識別類型        | 範例名稱
:--------        |:--------
識別碼（長）     | sha256:f753707788c5c100f194ce0a73058faae1a457774efcda6c1469544a114f8644
識別碼（短）     | f753707788c5
名稱（標籤）     | ubuntu:16.04
名稱（數位文摘） | ubuntu@sha256:2d44ae143feeb36f4c898d32ed2ab2dffeb3a573d2d892864...(省略)


識別碼可省略 `sha256:`，且最短長度只要與本機其他鏡像識別碼由前往後讀取有數值上的不同即可。

名稱有兩種類型，分為：

  * `<廣義的倉庫名>:<標籤名>`
  * `<廣義的倉庫名>:<數位文摘>`


其名稱猶如其存在的路徑，以標籤名為例：

```
<主機名>/<用戶名>/<倉庫名>:<標籤名>
```

廣義的倉庫名由斜線（/）分隔的名稱所組成，各名稱可包含小寫、數字及分隔符，其分隔符包含逗點（.）、一個或兩個的底線（\_）、一個或兩個的破折號（-），且不能以分隔符開頭或結尾。

標籤名稱可包含大小寫、數字、底線、逗點和破折號，且不能以逗點、破折號開頭，限制最多 128 個字符。

  * 主機名： 倉儲中心的位置，必須符合 DNS 規範，且不能有底線。可省略，預設為
    [Docker Hub](http://hub.docker.com/)
    。
  * 用戶名： 在 [Docker Hub](http://hub.docker.com/) 註冊的名稱。官方所建立的鏡像（如： `ubuntu:16.04`）則沒有用戶名。
  * 倉庫名： 用戶所建立的倉庫。
  * 標籤名： 用於標示同一個倉庫內鏡像的區分。



### 數位文摘


船塢工人 1.10 版本後增加其內容可檢索貯存（content-addressable store）的數位文摘（digest）機制，
以 SHA256 編碼，其涵蓋了鏡像的配置檔和層。


數位文摘格式：

```
sha256:2d44ae143feeb36f4c898d32ed2ab2dffeb3a573d2d8928646dfc9cb7deb1315
```

其使用範例：

```
ubuntu@sha256:2d44ae143feeb36f4c898d32ed2ab2dffeb3a573d2d8928646dfc9cb7deb1315
```

__注意： 數位文摘前綴 `sha256:` 不可被省略。__


數位文摘如同其檔案內容的數位簽章，相同的編碼即表示有著相同的內容，故以此方式更能準確的指定目標鏡像。



## 其組成


```
$ docker pull debian

Using default tag: latest
latest: Pulling from library/debian
fdd5d7827f33: Pull complete
a3ed95caeb02: Pull complete
Digest: sha256:e7d38b3517548a1c71e41bffe9c8ae6d6d29546ce46bf62159837aad072c90aa
Status: Downloaded newer image for debian:latest
```


鏡像是由多個層來組成，如上例，` debian:latest ` 就是包含 ` fdd5d7827f33 `、` a3ed95caeb02 ` 兩層。

層能在鏡像檔間重複使用，用與 ` debian:latest ` 參照相同鏡像的 ` debian:jessie ` 作範例，由於其二者有相同的層，在下載前者時就已將其載入，故後者只需下載鏡像的「metadata」即可。


```
$ docker pull debian:jessie

jessie: Pulling from library/debian
fdd5d7827f33: Already exists
a3ed95caeb02: Already exists
Digest: sha256:a9c958be96d7d40df920e7041608f2f017af81800ca5ad23e327bc402626b58e
Status: Downloaded newer image for debian:jessie


$ docker images

REPOSITORY   TAG      IMAGE ID        CREATED      SIZE
debian       jessie   f50f9524513f    5 days ago   125.1 MB
debian       latest   f50f9524513f    5 days ago   125.1 MB
```



## 容器


### 容器識別


容器被識別藉由下列三種方式：

 識別類型  | 範例名稱
:--------  |:--------
UUID（長） | f78375b1c487e03c9438c729345e54db9d20cfa2ac1fc3494b6eb60872e74778
UUID（短） | f78375b1c487
名稱       | myName_by_self

  * UUID： 通用唯一識別碼。


通用唯一識別碼由船塢工人常駐程式（Docker daemon）產生。

而其名稱可自行設定或使用預設生成的隨機文字。
給予容器可區別其目的的名稱是方便的，也能簡易地去指定欲操作的容器。

> Note: 在預設的網路橋接器（default bridge network）下必須藉由容器名字才能與容器連結通信。


## 參考


* [映像檔名稱基礎 | 全面易懂的Docker指令大全](http://joshhu.gitbooks.io/dockercommands/content/DockerImages/ImageBasic.html)
* [docker pull - Docker](http://docs.docker.com/engine/reference/commandline/pull/)
* [docker tag - Docker](http://docs.docker.com/engine/reference/commandline/tag/)
* [docker images 介绍 – Cizixs Writes Here](http://cizixs.com/2016/04/06/docker-images)
* [Docker run reference - Docker](http://docs.docker.com/engine/reference/run/)

