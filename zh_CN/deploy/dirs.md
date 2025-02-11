# 目录结构

不同安装方式得到的 EMQX 其目录结构会有所不同，具体如下:

| 描述                        | 使用 ZIP 压缩包安装      | 使用二进制包安装              |
| --------------------------- | ------------------------ | ----------------------------- |
| 配置文件目录                | `./etc`                  | `/etc/emqx/etc`               |
| 数据文件                    | `./data`                 | `/var/lib/emqx/data`          |
| 日志文件                    | `./log`                  | `/var/log/emqx`               |
| 启动相关的脚本              | `./releases`             | `/usr/lib/emqx/releases`      |
| 可执行文件目录              | `./bin`                  | `/usr/lib/emqx/bin`           |
| Erlang 代码                 | `./lib`                  | `/usr/lib/emqx/lib`           |
| Erlang 虚拟机文件           | `./erts-*`               | `/usr/lib/emqx/erts-*`        |
| 插件                        | `./plugins`              | `/usr/lib/emqx/plugins`       |

这些目录都是在安装时就创建好的。
除了 `etc`、`data`、`log` 和 `plugins`，其他的目录通常都是静态的，只在升级或重装时才会改变。

## bin 目录

**emqx、emqx.cmd**

EMQX 的可执行文件，具体使用可以查看 [基本命令](../admin/cli.md)。

**emqx_ctl、emqx_ctl.cmd**

EMQX 管理命令的可执行文件，具体使用可以查看  [管理命令 CLI](../admin/cli.md)。

## etc 目录

EMQX 通过 `etc` 目录下配置文件进行设置，主要配置文件包括:

* `emqx.conf`：EMQX 的主配置文件。
* `vm.args`：Erlang 虚拟机的运行参数。
* `certs/`：X.509 的密钥和证书文件。这些文件被用于 EMQX 的 SSL/TLS 监听器。
  也被用与与外部系统集成时建立 SSL/TLS 连接。

## data 目录

EMQX 将运行数据存储在 `data` 目录下，请确保 EMQX 具有该目录下所有文件的读写权限。
:::: tip Tip
在 EMQX 的文档中，这个目录通常被称作 `data_dir`。
::::

data 目录中主要的文件包括:

**configs/app.*.config**

EMQX 读取 `etc/emqx.conf` 和 `data/configs/cluster-override.conf` `data/configs/local-override.conf` 中的配置后，转换为 Erlang 原生配置文件格式，并在运行时读取其中的配置。


**mnesia**

Mnesia 数据库是 Erlang 内置的一个分布式 DBMS，可以直接存储 Erlang 的各种数据结构。

EMQX 使用 Mnesia 数据库存储自身运行数据，例如告警记录、规则引擎已创建的资源和规则、Dashbaord 用户信息等数据，这些数据都将被存储在 `mnesia` 目录下，因此一旦删除该目录，将导致 EMQX 丢失所有业务数据。

可以通过 `emqx_ctl mnesia` 命令查询 EMQX 中 Mnesia 数据库的系统信息，具体请查看 [管理命令 CLI](../admin/cli.md)。

**trace**

EMQX 支对指定 ClientID 或 Topic 或 IP 实时过滤日志，用于调试和排查错误。具体请查看 [日志追踪](../observability/tracer.md)

## log 目录

**emqx.log.***

EMQX 运行时产生的日志文件，具体请查看 [日志与追踪](../observability/log.md)。

**erlang.log.***

以 `emqx start` 方式后台启动 EMQX 时，控制台日志的副本文件。
