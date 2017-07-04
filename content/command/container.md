容器指令
=======



## 頁籤


* [create](#create)
* [run](#run)
* [start](#start)
* [參考](#參考)



## create


```
Usage:  docker create [OPTIONS] IMAGE [COMMAND] [ARG...]

創建一個新容器。

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
      --memory-swappiness int       Tune container memory swappiness (0 to 100) (default -1)
      --name string                 Assign a name to the container
      --network-alias value         Add network-scoped alias for the container (default [])
      --network string              Connect a container to a network (default "default")
                                    'bridge': create a network stack on the default Docker bridge
                                    'none': no networking
                                    'container:<name|id>': reuse another container's network stack
                                    'host': use the Docker host network stack
                                    '<network-name>|<network-id>': connect to a user-defined network
      --no-healthcheck              Disable any container-specified HEALTHCHECK
      --oom-kill-disable            Disable OOM Killer
      --oom-score-adj int           Tune host's OOM preferences (-1000 to 1000)
      --pid string                  PID namespace to use
      --pids-limit int              Tune container pids limit (set -1 for unlimited), kernel >= 4.3
      --privileged                  Give extended privileges to this container
  -p, --publish value               Publish a container's port(s) to the host (default [])
  -P, --publish-all                 Publish all exposed ports to random ports
      --read-only                   Mount the container's root filesystem as read only
      --restart string              Restart policy to apply when a container exits (default "no")
                                    Possible values are: no, on-failure[:max-retry], always, unless-stopped
      --runtime string              Runtime to use for this container
      --security-opt value          Security Options (default [])
      --shm-size string             Size of /dev/shm, default value is 64MB.
                                    The format is `<number><unit>`. `number` must be greater than `0`.
                                    Unit is optional and can be `b` (bytes), `k` (kilobytes), `m` (megabytes),
                                    or `g` (gigabytes). If you omit the unit, the system uses bytes.
      --stop-signal string          Signal to stop a container, SIGTERM by default (default "SIGTERM")
      --storage-opt value           Storage driver options for the container (default [])
      --sysctl value                Sysctl options (default map[])
      --tmpfs value                 Mount a tmpfs directory (default [])
  -t, --tty                         分配一個虛擬電傳設備（pseudo-TTY）。
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


` docker create ` 命令是在指定的鏡像上創建新的可讀寫容器層（container layer），然後執行指定的命令完成啟動前準備。
執行此命令後會顯示容器識別碼，這相似於 ` docker run -d ` 但不包含啟動容器。

更多細節請閱讀
[docker run](#run)
和
[docker run 參考](http://docs.docker.com/engine/userguide/networking/)
。



## 演示


```
$ docker create -t -i fedora bash
6d8af538ec541dd581ebc2a24153a28329acb5268abe5ef868c1f1a261221752


$ docker start -a -i 6d8af538ec5
bash-4.2#
```



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

` docker run ` 可與 ` docker commit ` 命令結合使用以
[改變容器運行的命令](http://docs.docker.com/engine/reference/commandline/commit/)
。

關於額外 ` docker run ` 命令的資訊，請參考
[` docker run ` 參考](http://docs.docker.com/engine/userguide/networking/)
。

關於容器連接網路的資訊，請參考
[船塢工人網路概述](http://docs.docker.com/engine/reference/run/)
。



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



## start


```
Usage:  docker start [OPTIONS] CONTAINER [CONTAINER...]

Start one or more stopped containers

Options:
  -a, --attach               Attach STDOUT/STDERR and forward signals
      --detach-keys string   Override the key sequence for detaching a container
      --help                 顯示使用提示。
  -i, --interactive          Attach container's STDIN
```



## 參考


* [docker run - Docker](http://docs.docker.com/engine/reference/commandline/run/)
* [docker start - Docker](http://docs.docker.com/engine/reference/commandline/start/)

