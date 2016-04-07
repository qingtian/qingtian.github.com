---
layout: post
title: mac下的效率工具shuttle介绍
---


使用mac的同学都知道，如果我们想要远程登录某台机器，通常会按如下步骤操作

- 打开Terminal
- 输入ssh XXX（xxx是主机别名，通常还需要在 .ssh/config中预先配置）

这样操作也没啥大问题， 就有一点特别烦人 —— 机器多了记不住那么多别名。

而shuttle就是为了解决此类问题的，当然它也支持快捷执行一些常用的命令（一键切换）。

下面我就着重介绍一下shuttle:

* 安装shuttle
* 配置shuttle
* 使用shuttle

### 1. 安装shuttle

shuttle是一个开源软件，其源码托管在github ( [shuttle](https://github.com/fitztrev/shuttle) ), 通过访问此地址，可以下载最新版本的shuttle.app

### 2. 配置shuttle

运行shuttle.app, 然后可以看到托盘里会出现一个“火箭”的图标，那个就是shuttle。 点击 图标 -> setting -> Edit , 系统会使用文本编辑器打开一个名为：“.shuttle.json”的文件 ， 此文件即为shuttle的配置文件。配置字段描述可见 （[JSON Options](https://github.com/fitztrev/shuttle)）里的JSON Options。以下我针对性介绍一下：

* editor: 使用什么编辑器打开.shuttle.json文件（可选值：default, nano, vi, vim或其他可在终端编辑文件的命令）
* launch_at_login: 是否自动启用shuttle(可选值: true, false)
* terminal: 设置执行命令的默认终端（可选值：Terminal.app, iTerm）
* iTerm_verison: 当terminal参数设置为iTerm时必填（可选值：stable, nightly）
* default_theme: 设置终端主题
* open_in: 命令窗口展示方式（可选值：tab, new）
* show_ssh_config_hosts: 是否解析ssh config，并显示对应的主机到菜单列表中（可选值：true, false）
* ssh_config_ignore_hosts: 在ssh config需要忽略显示在菜单的主机数组（值为主机名）
* ssh_config_ignore_keywords: 在ssh config需要忽略的关键字
 
如果要将~/.ssh/config中的主机显示到菜单中， 可以这样定义主机的Host值：

`Host work/servers/web01`: 表示web01会出现在shuttle的work菜单下servers子菜单下(一种方便的目录层级定义方式)。

也可以使用另外一种方式：

`Host web01`

`# shuttle.name = work/servers/web01`

`HostName user@web01.example.com`

也就是通过“# shuttle.name”开始来定义shuttle的菜单名与层级关系（注意，这个注释必须位于 Host与HostName之前，否则显示的菜单与实际运行的命令会错乱）

除了以上配置项， shuttle还支持自定义命令配置， 这种方式特别适合用常用命令。

自定义命令配置是定义在.shuttle.json中的hosts键值中，其值为一个数组，每个item为一个对象，结构如下：

`{
	"菜单名": xxx (对象或数组)
}`

对象包含字段有：

* cmd: 需要执行的命令
* name: 菜单名
* inTerminal: 命令执行窗口模式（可选值：new, tab, current）
* theme: 终端主题
* title: 终端显示标题(缺失时使用name作为标题)

### 3. 使用shuttle

按照上面介绍的步骤完成安装与配置后， 现在点击shuttle图标就应该可以看到一些菜单了。

![shuttle dropmenu](http://blog.qingtian16265.com/static/img/20160406/shuttle-dropmenu.jpg)

`目录切换`: 里面是平常我使用的常用命令
`netease`: 就是平时需要远程登录的服务器

点击其中的菜单项，将会使用iTerm打开窗口并运行命令（我配置的是iTerm）。

至此shuttle的使用也基本介绍完了， 希望能帮助到大家。

