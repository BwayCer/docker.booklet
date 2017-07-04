圖說邏輯
=======


## 前言


我學習 Git 版本控制程式也是在搞懂「紀錄區」和「本地與遠端」的差異後，才開始突飛猛進並對指令能舉一反三。
「讀書先讀序文；學習先學原理。」
這是好的開始，雖然筆者對硬體一竅不通，讀完後仍不懂其技術的實作，但確實瞭解「倉庫」、「映像檔」、「容器」的觀念，並加快理解指令的操作意義。

筆者自爆對硬體是一竅不通，為了不誤人子弟，建議讀者還是再一次閱讀參考資料為佳。



## 頁籤


* [大綱 / 簡介](#大綱--簡介)
* [虛擬技術](#虛擬技術)
* [核心概念](#核心概念)
  * [碼頭工人系統](#碼頭工人系統)
  * [倉庫](#倉庫)
  * [映像檔](#映像檔)
  * [容器](#容器)
* [參考](#參考)



## 大綱 / 簡介


文章的大綱或文章內容物的簡介...



## 名詞概念


* Hub
  * 名稱： 倉儲中心。
  【
  [參考1](http://tw.dictionary.search.yahoo.com/search?p=hub)、
  [參考2](http://terms.naer.edu.tw/search/?q=hub)
  】
* Registry
  * 名稱： 註冊管理處（理解上可當成倉儲中心看待）。
  【
  [參考1](http://tw.dictionary.search.yahoo.com/search?p=registry)、
  [參考2](http://terms.naer.edu.tw/search/?q=registry)
  】
* Repository
  * 名稱： 倉庫。
  【
  [參考1](http://tw.dictionary.search.yahoo.com/search?p=repository)、
  [參考2](http://terms.naer.edu.tw/search/?q=repository)
  】
* image
  * 名稱： 鏡像。
* container
  * 名稱： 容器。




## 虛擬技術


碼頭工人是一項新的虛擬技術，可以在一台主機上同時執行數百個不同的虛擬應用程式。
傳統的虛擬主機技術是利用硬體的功能來模擬，
而這套輕量的虛擬技術，一般稱之為容器（Container），則是以基於 Linux 核心的作業系統建立多個虛擬的實例（Instances），有獨立的函式庫及獨立的溝通殼層（Shell），並彼此隔離。

容器與虛擬機的對照表：

 虛擬技術 | 開機載入速度 | 硬碟容 | 最大運行主機數量

而且彼此完全隔離，不就解決了現有的難題嗎?

* 虛擬技術：
  *

## 核心概念


碼頭工人的核心概念：

  * 碼頭工人系統（Docker I/O）
  * 倉庫（Repository）
  * 映像檔（Image）
  * 容器（Container）


### 碼頭工人系統


用 Linux 的系統管理的技術，建立出可把文件檔相互隔離的空間，藉此實現在相同主機運行不同作業系統的虛擬技術。

由於是基於 Linux 所實現的虛擬技術，故欲建立的作業系統也必須是基於 Linux 的作業系統。



### 倉庫


如同 GitHub 一般，用於存放映像檔的倉庫。
[Docker Hub](http://hub.docker.com/)
是官方建立的公有倉庫，也能建立專屬的私有倉庫。



### 映像檔


映像檔是個只讀層，紀錄新增或更動的程式包（package）。
其映像檔間可串聯組合，可視為一個獨有的作業系統。

映像檔被儲存於 ` /var/lib/docker/aufs ` 目錄下。



### 容器


` 容器 = 映像檔的集合 + 一個讀寫層 `



## 參考


* [參考資料](http://bwaycer.github.io)

* 虛擬技術：

  * [虚拟化与Docker 谁会死在路上?来看看东方通怎么说 IT经理世界](http://www.ceocio.com.cn/it/news/2016-07-13/178514.shtml)

  * [docker与虚拟机性能比较 - cbl709的专栏 - 博客频道](http://blog.csdn.net/cbl709/article/details/43955687)

* 核心概念：

  * [10张图带你深入理解Docker容器和镜像 - DockOne.io](http://dockone.io/article/783)

  * [淺談輕量化的虛擬技術 - Docker容器](http://www.cc.ntu.edu.tw/chinese/epaper/0036/20160321_3611.html)

