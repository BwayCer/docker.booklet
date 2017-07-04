指令集
=======


## 前言


此篇主要參考
[官網指令教學](http://docs.docker.com/engine/reference/commandline/)
。
而若讀者想知道船塢工人指令於命令行（command line）中的使用，可閱讀
[命令行使用方式](../command_reference/use_command_line.md)
。



## 頁籤


* [船塢工人命令管理](#船塢工人命令管理)
* [鏡像命令](#鏡像命令)
* [容器命令](#容器命令)
* [倉儲中心命令](#倉儲中心命令)
* [網路與網路連線命令](#網路與網路連線命令)
* [共享資料容量空間命令](#共享資料容量空間命令)
* [Swarm node 命令](#swarm-node-命令)
* [Swarm swarm 命令](#swarm-swarm-命令)
* [Swarm service 命令](#swarm-service-命令)
* [參考](#參考)



## 船塢工人命令管理


* dockerd             ： 啟動船塢工人常駐進程，可影響船塢工人的容器。
* info                ： 顯示系統的資訊。
* inspect             ： 返回容器或鏡像的 low-level 的資訊。
* version             ： 顯示船塢工人的版本。



## 鏡像命令


* build               ： Build an image from a Dockerfile
* commit              ： 以容器被更改的內容創建出新的鏡像。
* [history            ： Show the history of an image。](./image.md#history)
* [images             ： 列出鏡像清單。](./image.md#image)
* import              ： Import the contents from a tarball to create a filesystem image
* load                ： Load an image from a tar archive or STDIN
* [rmi                ： 移除一個或多個鏡像。](./image.md#rmi)
* save                ： Save images to a tar archive
* [tag                ： 標示鏡像到指定的倉庫。](./image.md#tag)



### 容器命令


* attach              ： Attach to a running container
* cp                  ： Copy files/folders from a container to a HOSTDIR or to STDOUT
* [create             ： 創建一個新容器。](./container.md#create)
* diff                ： Inspect changes on a container’s filesystem
* events              ： Get real time events from the server
* exec                ： Run a command in a running container
* export              ： Export a container’s filesystem as a tar archive
* kill                ： Kill a running container
* logs                ： Fetch the logs of a container
* pause               ： Pause all processes within a container
* port                ： List port mappings or a specific mapping for the container
* ps                  ： 列出容器清單。
* rename              ： 更改容器名稱。
* restart             ： Restart a running container
* rm                  ： 移除一個或多個容器。
* [run                ： Run a command in a new container](./container.md#run)
* [start              ： Start one or more stopped containers](./container.md#start)
* stats               ： Display a live stream of container(s) resource usage statistics
* stop                ： Stop a running container
* top                 ： Display the running processes of a container
* unpause             ： Unpause all processes within a container
* update              ： Update configuration of one or more containers
* wait                ： Block until a container stops, then print its exit code



## 倉儲中心命令


* login               ： Register or log in to a Docker registry
* logout              ： Log out from a Docker registry
* [pull               ： 從倉儲中心（registry）下載鏡像，可指定標籤或整個倉庫。](./hub_and_registry.html#pull)
* push                ： Push an image or a repository to a Docker registry
* [search             ： 搜尋倉儲中心裡的鏡像。](./hub_and_registry.html#search)



## 網路與網路連線命令


* network connect     ： Connect a container to a network
* network create      ： Create a new network
* network disconnect  ： Disconnect a container from a network
* network inspect     ： Display information about a network
* network ls          ： Lists all the networks the Engine daemon knows about
* network rm          ： Removes one or more networks



## 共享資料容量空間命令


* volume create       ： Creates a new volume where containers can consume and store data
* volume inspect      ： Display information about a volume
* volume ls           ： Lists all the volumes Docker knows about
* volume rm           ： Remove one or more volumes



## Swarm node 命令


* node promote        ： Promote a node that is pending a promotion to manager
* node demote         ： Demotes an existing manager so that it is no longer a manager
* node inspect        ： Inspect a node in the swarm
* node update         ： Update attributes for a node
* node ps             ： List tasks running on a node
* node ls             ： List nodes in the swarm
* node rm             ： Remove one or more nodes from the swarm



## Swarm swarm 命令


* swarm init          ： Initialize a swarm
* swarm join          ： Join a swarm as a manager node or worker node
* swarm leave         ： Remove the current node from the swarm
* swarm update        ： Update attributes of a swarm
* swarm join-token    ： Display or rotate join tokens



## Swarm service 命令


* service create      ： Create a new service
* service inspect     ： Inspect a service
* service ls          ： List services in the swarm
* service rm          ： Remove a service from the swarm
* service scale       ： Set the number of replicas for the desired state of the service
* service ps          ： List the tasks of a service
* service update      ： Update the attributes of a service



## 參考


* [The Docker commands - Docker](http://docs.docker.com/engine/reference/commandline/)

