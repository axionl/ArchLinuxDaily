# Gitea

## 介绍

> Git with a cup of tea, painless self-hosted git service

一盏茶的时间，让您拥有自建 git 服务

## 项目链接

> **Github Repository**: https://github.com/go-gitea/gitea

> **Website**: https://gitea.io

## 安装

包名: `gitea`

~~可以添加 Arch CN 源之后使用 pacman 安装~~ (现已加入 ~~`community-testing`~~
`community`)，也可以通过 AUR Helper 安装。

## 配置

配置文件所在目录: `/etc/gitea/app.ini`

鉴于项目提供了优秀的 Web 初始化界面，在此就不重新贴配置，但需要注意的是数据库用
户和密码会明文写在配置文件中，所以需要**限定可读权限**。

### 数据库选择和配置

- 如果所需要存储的代码量比较少的话建议直接使用 `SQLite3` 作为数据库后端，无需配
  置，但后续不便于升级和迁移。

- 而如果需要大量代码的可靠托管服务，则建议使用
  [`PostgreSQL`](https://wiki.archlinux.org/index.php/PostgreSQL) 作为数据库后端
  。

- Gitea 也提供了 [`TiDB`](https://github.com/pingcap/tidb) 的实验性支持，如有能
  力可以自建大规模分布式数据库。

- ~~**`MariaDB` 不是一个好选择**：鉴于 Mariadb 新版本依赖构建过于复杂，ArchLinux
  的 MariaDB 包更新基本处于停滞状态，而目前正在使用的旧版本会导致一些字段过长错
  误，暂无修复方案。~~ 喜大普奔：多年的老 mariadb 已经更新了！

**下面以 PostgreSQL 为例介绍配置：**

如果你已经安装过 PostgreSQL，那么仅需添加一个用户及配置其对应的权限即可，于此不
再赘述。如果你在使用 Btrfs 或者 ZFS 文件系统，需要查看 Wiki 相关目录进行配置。如
果您之前没有安装过，则通过以下步骤进行初始化：

```bash
$ sudo pacman -S postgresql // 安装包
$ sudo -u postgres -i // 切换到 postgres 用户
[postgres] $ initdb --locale en_US.UTF-8 -E UTF8 -D '/var/lib/postgres/data' // 初始化数据库集

// 此时新建一个终端会话或者 exit 退出
$ sudo systemctl enable --now postgresql // 启动服务并允许开机启动

// 服务启动之后继续进入 postgres 用户
[postgres] $ createuser gitea --interactive -W // 以交互方式创建一个账户
// 可以仅允许其添加规则的权限

[postgres] createdb gitea -O gtiea // 创建一个名为 gitea 的 Database 并将 Owner 赋予 gitea

// 如果创建用户或者 Database 错误可以使用 dropuser / dropdb 来删除。如果需要移除用户，则需要先移除用户所有的 Database

// 可选进入 PostgreSQL 查看创建权限和详情，\q 退出，具体用法自行学习
[postgres] psql -d gitea
gitea=# \du
```

**WebUI 配置**

临时启动 `gitea` 服务，并用浏览器访问

```bash
$ sudo gitea web  # 默认使用 app.ini 作为配置文件，监听 0.0.0.0:3000
```

选择数据库类型后填入 Database 名 (`gitea`) 和用户名 (`gitea`) 密码，在最后添加管
理员账户和密码即可正常使用。

其余配置选项例如 SSL 等需要自行注册域名证书和拥有公网 IP，可以利用 `nginx` 或
`caddy` 之类的 web server 进行反向代理端口到本地，并改写 `/etc/gitea/app.ini` 中
的 `HTTP_ADDR` 为本地，避免服务和数据库端口直接暴露在公网。

## 启动

```bash
$ sudo systemctl enable --now gitea
```
