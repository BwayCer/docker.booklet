命令行使用方式
=======


## 頁籤


* [幫助訊息](#幫助訊息)
* [選項參數](#選項參數)
  * [參數值表達式](#參數值表達式)
  * [簡化指令](#簡化指令)
  * [布林值參數值](#布林值參數值)
  * [複用選項參數](#複用選項參數)
* [參考](#參考)



## 幫助訊息


如需解說指令使用可以 `-h, --help` 旗標來顯示幫助訊息：

```
$ docker run --help

Usage: docker [OPTIONS] COMMAND [arg...]
       docker [ --help | -v | --version ]

A self-sufficient runtime for containers.

Options:

  --config=~/.docker              Location of client config files
  -D, --debug                     Enable debug mode
  -H, --host=[]                   Daemon socket(s) to connect to
  -h, --help                      Print usage
  -l, --log-level=info            Set the logging level
  --tls                           Use TLS; implied by --tlsverify
  --tlscacert=~/.docker/ca.pem    Trust certs signed only by this CA
  --tlscert=~/.docker/cert.pem    Path to TLS certificate file
  --tlskey=~/.docker/key.pem      Path to TLS key file
  --tlsverify                     Use TLS and verify the remote
  -v, --version                   Print version information and quit

Commands:
    attach    Attach to a running container
    # […]

Run 'docker COMMAND --help' for more information on a command.
```


## 選項參數


### 參數值表達式


參數值表達式是以簡化的文字來表達其代表的意義，可歸類為下列幾項：

  * `-flag=""`： 參數值為文字類型（String），單次賦值。
  * `-flag=0`： 參數值為數字類型，單次賦值。
  * `-flag=[]`： 參數值類型未知，複用選項，可使用多次以達成賦值的完整。



### 簡化指令


若為單一字符的旗標，其選項可合併使用，如下例：

```
$ docker run -i -t --name test busybox sh
```

相同於

```
$ docker run -it --name test busybox sh
```



### 布林值參數值


如 `-d=false` 格式即為有布林值的選項。
此選項類型若無使用該旗標則使用系統預設值；
若使用該旗標但不設定布林參數值，則默認為 `true`。


以 `docker build` 的 `--rm` 為例，其預設值為 `true`，故僅下旗標並不會與原先有何差異，如下例：

```
$ docker build .
$ docker build --rm .
$ docker build --rm=true .
```

以上三者作用皆相同，或許 `--rm=false` 才是其真正需求。



### 複用選項參數


在船塢工人裡常用 `-a=[]` 表示其為複用選項。
此項目是一多選值，該旗標可被使用多次來達成賦值的完整。
如下例：

```
$ docker run -a stdin -a stdout -i -t ubuntu /bin/bash
$ docker run -a stdin -a stdout -a stderr ubuntu /bin/ls
```

而有些時候，複用選項可用更複雜的數值方式賦值，以 `-v` 為例：

```
$ docker run -v /host:/container example/mysql
```

或以 `search -f` 為例：

```
$ docker search -f is-automated=true,stars=10 node
```



## 參考


* [Use the Docker Engine command-line - Docker](http://docs.docker.com/engine/reference/commandline/cli/)

