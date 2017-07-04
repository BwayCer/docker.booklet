docker run 之命名空間命令詳述
=======


## 前言


Linux 的命名空間（Namespace）是一種內核級別環境隔離的方法。
這不是項新技術，在以前有個「chroot」的系統調用，通過修改根目錄把用戶「jail（監禁）」到一個特定目錄下，
在此內部的文件系統無法訪問外部的內容，形成簡單的隔離模式。
Linux 命名空間在此基礎上，提供了對 UTS、IPC、Mount、PID、Network、User 等的隔離機制。



## 頁籤


* [程序識別碼設定](#程序識別碼設定)
* [UNIX 分時系統設定](#unix-分時系統設定)
* [程序間通訊設定](#程序間通訊設定)
* [網路設定](#網路設定)
* [參考](#參考)



#### 程序識別碼設定


> 程序識別碼（Process ID（PID））。


```
docker run options:
      --pid string   使用「PID」命名空間。
                     設定容器的程序識別碼（PID 或 Process）命名空間模式：
                     `container:<name|id>`： 加入其他容器的 PID 命名空間；
                     `host`： use the host's PID namespace inside the container。
```

容器預設啟用「PID」命名空間``。

「PID」命名空間在隔離的容器視角下，把程序（process）識別碼替換成 UNIX 系統中初始程序（init）傳統的 `1` 號，
又以「Mount」命名空間替換文件系統的掛載，改變了由文件系統下讀取資料的指令（像 `ps`, `top` 會去讀 `/proc` 下的文件），
徹底在容器視角下，移除原系統的其他程序，也將所有容器內的操作限制在這被隔離的空間。

在容器內的程序一號是會被 Linux 特殊對待，它會忽略任何訊號的預設行為。
所以除非程式特別設計，否則像「SIGINT」或「SIGTERM」的訊號將無法終止它。

讀者可能會想共享宿主主機上的程序（process namespace）給被隔離的容器，基本上這是可行的，
但希望這是為程序除錯（如執行 `strace` or `gdb` 指令）才被使用。


範例一，在容器內執行 `htop` 指令：


創建 Dockerfile:

```
FROM alpine:latest
RUN apk add --update htop && rm -rf /var/cache/apk/*
CMD ["htop"]
```

建立（Build） Dockerfile 並把鏡像命名（tag）為 `myhtop`：

```
$ docker build -t myhtop .
```

嘗試以下指令在容器內執行 `htop` 指令：

```
$ docker run -it --rm --pid=host myhtop
```


範例二，把其他容器的程序（PID namespace）加入到容器內除錯監測：


啟動一個運行 redis 伺服器的容器：

```
$ docker run --name my-redis -d redis
```

藉以另一個容器來監測（Debug） redis 伺服器容器，以 `strace` 指令跟踪一個進程的系統調用或信號產生的情況：

```
$ docker run --it --pid=container:my-redis bash
$ strace -p 1
```


### UNIX 分時系統設定


> UNIX 分時系統（UNIX Timesharing System（UTS））。


```
docker run options:
      --uts string   使用「UTS」命名空間。
                     `host`： 使用與宿主主機相同的命名空間。
```


「UTS」命名空間可設定 `hostname` 和 `domain` 在隔離環境內而不影響系統。

預設情況下，所有容器（包含使用 `--network=host` 選項的容器）都有自己的「UTS」命名空間。
`--uts=` 選項值設為 `host` 表示容器與宿主主機使用相同的「UTS」命名空間，
當宿主主機的主機名改變時容器也會一起被改變。

注意！當使用 `--uts=host` 時，`--hostname` 選項標籤將不具作用。


除了「UTS」命名空間共享外，更進階的使用是從容器去改變宿主主機的主機名。（官方無範例）



### 程序間通訊設定


> 程序間通訊（Interprocess Communication（IPC））。


```
docker run options:
      --ipc string   IPC namespace to use
      or
      --ipc=""       Set the IPC mode for the container,
                     'container:<name|id>': reuses another container's IPC namespace
                     'host': use the host's IPC namespace inside the container
```


容器預設啟用「IPC」命名空間。

「IPC」（POSIX/SysV（規範標準））命名空間對於被命名的
共享記憶體資料（memory segments）、信號號誌（semaphores）和訊息佇列（message queues）提供隔離。


共享記憶體資料（memory segments）能以記憶體的速度來加快溝通的效率，而不用透過管道（pipe）或網路棧。
Shared memory segments are used to accelerate inter-process communication at memory speed, rather than through pipes or through the network stack.
Shared memory is commonly used by databases and custom-built (typically C/OpenMPI, C++/using boost libraries) high performance applications for scientific computing and financial services industries. If these types of applications are broken into multiple containers, you might need to share the IPC mechanisms of the containers.


> 默认情况下，所有容器都开启了IPC命名空间。
>
> IPC（POSIX/SysV IPC）命名空间提供了相互隔离的命名共享内存、信号灯变量和消息队列。
>
> 共享内存可以提高进程数据的交互速度。共享内存一般用在数据库和高性能应用（C/OpenMPI、C++/using boost libraries）上或者金融服务上。
  如果需要容器中部署上述类型的应用，那么就应该在多个容器直接使用共享内存了。



### 網路設定


```
docker run options:
      --dns value                   Set custom DNS servers (default [])

      --dns=[]           : Set custom dns servers for the container
      --network="bridge" : Connect a container to a network
                            'bridge': create a network stack on the default Docker bridge
                            'none': no networking
                            'container:<name|id>': reuse another container's network stack
                            'host': use the Docker host network stack
                            '<network-name>|<network-id>': connect to a user-defined network
      --network-alias=[] : Add network-scoped alias for the container
      --add-host=""      : Add a line to /etc/hosts (host:IP)
      --mac-address=""   : Sets the container's Ethernet device's MAC address
      --ip=""            : Sets the container's Ethernet device's IPv4 address
      --ip6=""           : Sets the container's Ethernet device's IPv6 address
      --link-local-ip=[] : Sets one or more container's Ethernet device's link local IPv4/IPv6 addresses
```


預設情況下，容器會被接上網路並可通過網路溝通，但可藉由 `docker run --network none` 操作符停用網路，
而只以共享磁碟（file？）及標準輸入輸出與容器溝通。

公開的通訊埠（ports）和與其他容器的連結（linking to other containers？）只適用於預設的橋接模式（bridge）。
與其他容器的連結這項功能是被遺留的特徵，
You should always prefer using Docker network drivers over linking
（但讀者應該寧願要使用網路驅動程式（network drivers）更勝於連結功能）。

預設情況下，容器會使用與宿主主機相同的域名系統（DNS），但可藉由 `--dns` 覆蓋預設值。

預設情況下，MAC 位址是由容器的 IP 位址生成，但可藉由 `--mac-address` 選項設定 MAC 位址（格式範例： `12:34:56:78:9a:bc`）。
注意！程式不會檢查讀者所設定的是否為唯一位址。



有效的 `--network` 網路參數值：

  * none           ： 無網路環境。
  * bridge（預設） ： Connect the container to the bridge via veth interfaces.
  * host           ： Use the host's network stack inside the container.
  * container      ： <name|id> Use the network stack of another container, specified via its *name* or *id*.
  * NETWORK        ： Connects the container to a user created network (using `docker network create` command)


##### none：

`--network none` 指令將停用對外訪問的接口。雖然容器有啟用 `loopback` 接口（interface），但它並沒有與外部溝通的能力。


##### bridge：


網路參數值 `--network bridge` 為預設設定，將在宿主主機架一座橋，通常名為 `docker0`，
 A bridge is setup on the host, commonly named docker0, and a pair of veth interfaces will be created for the container. One side of the veth pair will remain on the host attached to the bridge while the other side of the pair will be placed inside the container’s namespaces in addition to the loopback interface. An IP address will be allocated for containers on the bridge’s network and traffic will be routed though this bridge to the container.

Containers can communicate via their IP addresses by default. To communicate by name, they must be linked.




` docker run ` 命令首先是在指定的鏡像上創建新的可讀寫容器層（container layer），然後開始執行指定的命令。
這命令相當於執行 API 的 ` /containers/create ` 加上 ` /containers/(id)/start `。
先創建容器，然後使用 ` docker start ` 將暫停狀態的容器重新啟動並保持停止前的狀態。
` docker start ` 是用於暫停狀態的容器，可以使用 ` docker ps -a ` 命令查看所有已創建的容器。


## 參考


* [Docker run reference - Docker](http://docs.docker.com/engine/reference/run/)
* [docker run - Docker](http://docs.docker.com/engine/reference/commandline/run/)
* [Docker run 命令的使用方法 - DockOne.io](http://dockone.io/article/152)
* [Docker基础技术：Linux Namespace（上） | 酷 壳 - CoolShell.cn](http://coolshell.cn/articles/17010.html)
* [Docker基础技术：Linux Namespace（下） | 酷 壳 - CoolShell.cn](http://coolshell.cn/articles/17029.html)
* [A Tutorial for Isolating Your System with Linux Namespaces 繁中簡單翻譯 – Baby Deer's Learing Notes](http://littlebambideer.wordpress.com/2015/08/19/a-tutorial-for-isolating-your-system-with-linux-namespaces)
* [阿旺的 Linux 開竅手冊-process 程序](http://wanggen.myweb.hinet.net/ach6/ach6.html)
* [linux - What's a UTS namespace? - Unix & Linux Stack Exchange](http://unix.stackexchange.com/questions/183717/whats-a-uts-namespace)

