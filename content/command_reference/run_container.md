docker run 命令詳述
=======


## 前言


船塢工人把運行進程包覆在被隔離的容器內，而容器是一主機上的進程，這主機可能是本地或者遠端。
當執行 `docker run` 命令將在主機上運行隔離容器，其擁有自己的檔案系統、網路和進程樹。

`docker run` 命令包含 `create` 和 `start`，且又可以重新定義 `build` 設定於鏡像上的配置，這也是其命令有如此多參數的原因。



## 頁籤


* [一般格式](#一般格式)
* [特有選項指令](#特有選項指令)
  * [是否背景運行](#是否背景運行)
  * [命名容器](#命名容器)
  * [紀錄容器識別碼](#紀錄容器識別碼)
  * [命名空間](#命名空間)
* [覆蓋鏡像預設值](#覆蓋鏡像預設值)
* [參考](#參考)
  * [run](#run)



## 一般格式


```
$ docker run [OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]
```

`docker run` 命令會以指定的鏡像為基礎創建一新容器格式。


鏡像能被定義以下配置：

  * 是否背景運行（detached or foreground）。
  * 容器識別碼設置。
  * 網路設置。
  * 設置容器可用的處理器及記憶體。

可藉由 `docker run` 的操作選項設定運行參數，並且可覆蓋掉原先由鏡像配置或預設的參數值。



## 特有選項指令


### 是否背景運行


關於船塢工人的「背景」有特別的形容，叫分離模式（detached mode），反之稱為前景模式（foreground mode）。
但筆者還是以習慣用背景模式或非背景模式稱呼。

```
docker run options:
  -d, --detach   在背景運行容器和顯示容器識別碼。
```

預設是非背景模式的，可藉由 `-d, -d=true` 指定由背景執行。


背景模式：

以背景模式運行的容器需由該容器的根進程（root process）退出時才會退出。
當它暫停時並不會被自動移除，所以 `-d` 與 `--rm` 並無法連用。

舉例：
不要對背景執行的容器使用 `service x start` 指令，如啟動 ngnix 服務器：

```
$ docker run -d -p 80:80 my_image service nginx start
```

nginx 被成功啟動了沒錯，但這是一個失敗的範例，由於根進程（service nginx start）退出（return），依程式設計背景執行的容器也跟著退出（stops）所以其結果就是 nginx 伺服器啟動了但無法被使用。

若是要運行這類的指令可參考以下的替代方案：

```
$ docker run -d -p 80:80 my_image nginx -g 'daemon off;'
```

背景運行的容器不會對 `docker run` 啟動指令所在的命令行進行長監聽（longer listening），所以若要對容器輸入或輸出必需透過網路連線或共享磁碟（shared volumes）方可溝通。

若想重新連接可使用 `docker attach` 指令。


非背景模式：

預設的一般情況 `docker run` 啟動容器進程並連接標準輸入、標準輸出、標準錯誤到控制台，也能虛擬終端設備（TTY），倚靠命令行執行指令。參考以下配置：

```
docker run options:
  -a=[]           : 連接 `STDIN`（標準輸入）、`STDOUT`（標準輸出）、`STDERR`（標準錯誤）等標準串流。
  -t              : Allocate a pseudo-tty
  --sig-proxy=true: Proxy all received signals to the process (non-TTY mode only)
  -i              : Keep STDIN open even if not attached
```

`-a` 指令預設是使用
[全部的標準串流](http://github.com/docker/docker/blob/75a7f4d90cde0295bcfb7213004abce8d4779b75/commands.go#L1797)
。讀者能依需求指定其參數如下例示範：

```
$ docker run -a stdin -a stdout -i -t ubuntu /bin/bash
```

若想進行交互式操作（如： shell），你必須使用 `-i -t, -it` 連用的方式分配終端設備至容器的進程。

當客戶端標準輸出（client standard output）被重定向或通過「piped」的情況下禁止使用 `-t`，如下例：

```
$ echo test | docker run -i busybox cat
```

> Note: 由於虛擬終端設備（pty）實現（implementation）的限制，所以不要把 `-t` 和 `-a stderr` 一起使用。
  在虛擬終端設備模式裡的錯誤訊息只會依照標準輸出方式輸出。


> Note: A process running as PID 1 inside a container is treated specially by Linux: it ignores any signal with the default action. So, the process will not terminate on SIGINT or SIGTERM unless it is coded to do so.



### 命名容器


```
docker run options:
      --name string   替容器命名。
```

若無設定 `--name` 指令，預設會生成隨機文字當容器名稱。
能以名稱簡易地去指定欲操作的容器，此操作方式在一般或背景運行的模式下都適用。


更多相關可閱讀
[容器識別](/content/quick_start/hub_image_container.md#容器識別)
。


> Note: 在預設的網路橋接器（default bridge network）下必須藉由容器名字才能與容器連結通信。



### 紀錄容器識別碼


```
docker run options:
      --cidfile string   把容器識別碼寫入至指定文件
```

容器識別碼如同進程識別碼（PID, process ID）的效用一般，
紀錄容器識別碼就類似一些應用程式會把進程識別碼寫入「PID file」文件，有利於透過程式繼續後續自動化的操作。


### 命名空間


船塢工人的技術精髓莫過於容器的隔離，而這技術是依賴 Linux 的命名空間（Namespace）來實現。
Linux 命名空間提供了對 UTS、IPC、Mount、PID、Network、User 等的隔離機制，
這也牽涉了 `docker run` 指令中 `PID`、`UTS`、`IPC` 及網路的設定，
若想認識更多請閱讀
[docker run 之命名空間命令詳述](./docker_run_namespace.md)
。



## run


```
Usage:  docker run [OPTIONS] IMAGE [COMMAND] [ARG...]

Run a command in a new container

Options:
      --add-host value              Add a custom host-to-IP mapping (host:ip) (default [])
  -a, --attach value                Attach to STDIN, STDOUT or STDERR (default [])
      --blkio-weight value          Block IO (relative weight), between 10 and 1000
      --blkio-weight-device value   Block IO weight (relative device weight) (default [])
      --cap-add value               Add Linux capabilities (default [])
      --cap-drop value              Drop Linux capabilities (default [])
      --cgroup-parent string        Optional parent cgroup for the container
      --cidfile string              Write the container ID to the file
      --cpu-percent int             CPU percent (Windows only)
      --cpu-period int              Limit CPU CFS (Completely Fair Scheduler) period
      --cpu-quota int               Limit CPU CFS (Completely Fair Scheduler) quota
  -c, --cpu-shares int              CPU shares (relative weight)
      --cpuset-cpus string          CPUs in which to allow execution (0-3, 0,1)
      --cpuset-mems string          MEMs in which to allow execution (0-3, 0,1)
  -d, --detach                      Run container in background and print container ID
      --detach-keys string          Override the key sequence for detaching a container
      --device value                Add a host device to the container (default [])
      --device-read-bps value       Limit read rate (bytes per second) from a device (default [])
      --device-read-iops value      Limit read rate (IO per second) from a device (default [])
      --device-write-bps value      Limit write rate (bytes per second) to a device (default [])
      --device-write-iops value     Limit write rate (IO per second) to a device (default [])
      --disable-content-trust       Skip image verification (default true)
      --dns value                   Set custom DNS servers (default [])
      --dns-opt value               Set DNS options (default [])
      --dns-search value            Set custom DNS search domains (default [])
      --entrypoint string           Overwrite the default ENTRYPOINT of the image
  -e, --env value                   Set environment variables (default [])
      --env-file value              Read in a file of environment variables (default [])
      --expose value                Expose a port or a range of ports (default [])
      --group-add value             Add additional groups to join (default [])
      --health-cmd string           Command to run to check health
      --health-interval duration    Time between running the check
      --health-retries int          Consecutive failures needed to report unhealthy
      --health-timeout duration     Maximum time to allow one check to run
      --help                        顯示使用提示。
  -h, --hostname string             Container host name
  -i, --interactive                 Keep STDIN open even if not attached
      --io-maxbandwidth string      Maximum IO bandwidth limit for the system drive (Windows only)
                                    (Windows only). The format is `<number><unit>`.
                                    Unit is optional and can be `b` (bytes per second),
                                    `k` (kilobytes per second), `m` (megabytes per second),
                                    or `g` (gigabytes per second). If you omit the unit,
                                    the system uses bytes per second.
                                    --io-maxbandwidth and --io-maxiops are mutually exclusive options.
      --io-maxiops uint             Maximum IOps limit for the system drive (Windows only)
      --ip string                   Container IPv4 address (e.g. 172.30.100.104)
      --ip6 string                  Container IPv6 address (e.g. 2001:db8::33)
      --ipc string                  IPC namespace to use
      --isolation string            Container isolation technology
      --kernel-memory string        Kernel memory limit
  -l, --label value                 Set meta data on a container (default [])
      --label-file value            Read in a line delimited file of labels (default [])
      --link value                  Add link to another container (default [])
      --link-local-ip value         Container IPv4/IPv6 link-local addresses (default [])
      --log-driver string           Logging driver for the container
      --log-opt value               Log driver options (default [])
      --mac-address string          Container MAC address (e.g. 92:d0:c6:0a:29:33)
  -m, --memory string               Memory limit
      --memory-reservation string   Memory soft limit
      --memory-swap string          Swap limit equal to memory plus swap: '-1' to enable unlimited swap
      --memory-swappiness int       Tune container memory swappiness (0 to 100) (default -1).
      --name string                 Assign a name to the container
      --network-alias value         Add network-scoped alias for the container (default [])
      --network string              Connect a container to a network
                                    'bridge': create a network stack on the default Docker bridge
                                    'none': no networking
                                    'container:<name|id>': reuse another container's network stack
                                    'host': use the Docker host network stack
                                    '<network-name>|<network-id>': connect to a user-defined network
      --no-healthcheck              Disable any container-specified HEALTHCHECK
      --oom-kill-disable            Disable OOM Killer
      --oom-score-adj int           Tune host's OOM preferences (-1000 to 1000)
      --pid string                  PID namespace to use
      --pids-limit int              Tune container pids limit (set -1 for unlimited)
      --privileged                  Give extended privileges to this container
  -p, --publish value               Publish a container's port(s) to the host (default [])
  -P, --publish-all                 Publish all exposed ports to random ports
      --read-only                   Mount the container's root filesystem as read only
      --restart string              Restart policy to apply when a container exits (default "no")
                                    Possible values are : no, on-failure[:max-retry], always, unless-stopped
      --rm                          Automatically remove the container when it exits
      --runtime string              Runtime to use for this container
      --security-opt value          Security Options (default [])
      --shm-size string             Size of /dev/shm, default value is 64MB.
                                    The format is `<number><unit>`. `number` must be greater than `0`.
                                    Unit is optional and can be `b` (bytes), `k` (kilobytes), `m` (megabytes),
                                    or `g` (gigabytes). If you omit the unit, the system uses bytes.
      --sig-proxy                   Proxy received signals to the process (default true)
      --stop-signal string          Signal to stop a container, SIGTERM by default (default "SIGTERM")
      --storage-opt value           Storage driver options for the container (default [])
      --sysctl value                Sysctl options (default map[])
      --tmpfs value                 Mount a tmpfs directory (default [])
  -t, --tty                         Allocate a pseudo-TTY
      --ulimit value                Ulimit options (default [])
  -u, --user string                 Username or UID (format: <name|uid>[:<group|gid>])
      --userns string               User namespace to use
                                    'host': Use the Docker host user namespace
                                    '': Use the Docker daemon user namespace specified by `--userns-remap` option.
      --uts string                  UTS namespace to use
  -v, --volume value                Bind mount a volume (default []). The format
                                    is `[host-src:]container-dest[:<options>]`.
                                    The comma-delimited `options` are [rw|ro],
                                    [z|Z], [[r]shared|[r]slave|[r]private], and
                                    [nocopy]. The 'host-src' is an absolute path
                                    or a name value.
      --volume-driver string        Optional volume driver for the container
      --volumes-from value          Mount volumes from the specified container(s) (default [])
  -w, --workdir string              Working directory inside the container
```


` docker run ` 命令首先是在指定的鏡像上創建新的可讀寫容器層（container layer），然後開始執行指定的命令。
這命令相當於執行 API 的 ` /containers/create ` 加上 ` /containers/(id)/start `。
先創建容器，然後使用 ` docker start ` 將暫停狀態的容器重新啟動並保持停止前的狀態。
` docker start ` 是用於暫停狀態的容器，可以使用 ` docker ps -a ` 命令查看所有已創建的容器。


## 參考


* [Docker run reference - Docker](http://docs.docker.com/engine/reference/run/)
* [docker run - Docker](http://docs.docker.com/engine/reference/commandline/run/)
* [Use the Docker Engine command-line - Docker](http://docs.docker.com/engine/reference/commandline/cli/)
* [Docker run 命令的使用方法 - DockOne.io](http://dockone.io/article/152)
* [[轉載整理]What is tty? @ 十年磨一劍 - 痞客邦 PIXNET](http://flykof.pixnet.net/blog/post/24277709)
* [英文中的 tele- 這個字首](http://blogs.teachersammy.com/Blogs/entry/meaning-of-English-word-prefix-tele)
* [Docker基础技术：Linux Namespace（上） | 酷 壳 - CoolShell.cn](http://coolshell.cn/articles/17010.html)
* [Docker基础技术：Linux Namespace（下） | 酷 壳 - CoolShell.cn](http://coolshell.cn/articles/17029.html)

