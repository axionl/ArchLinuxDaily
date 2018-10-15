# Gitea

## 介绍

> Git with a cup of tea, painless self-hosted git service

一盏茶的时间，让您拥有自建 git 服务

## 项目链接

> **Github Repository**: https://github.com/go-gitea/gitea

> **Website**: https://gitea.io

## 安装

包名: `gitea`

~~可以添加 Arch CN 源之后使用 pacman 安装~~ (现已加入 `community-testing`)，也可
以通过 AUR Helper 安装。

## 配置

配置文件所在目录: `/etc/gitea/app.ini`

鉴于项目提供了优秀的 Web 初始化界面，在此就不重新贴配置，但需要注意的是数据库用
户和密码会明文写在配置文件中，所以需要**限定可读权限**。

## 测试

```bash
$ sudo coredns  # 默认使用 Corefile 作为配置文件

# 新开一个终端会话
$ sudo netstat -nutlp | grep coredns  # 检查端口是否正常监听
$ dig baidu.com @127.0.0.1 -p 53  # 检查 DNS 解析是否成功
```

## 后续

```bash
$ sudo systemctl enable --now coredns  # 开启 systemd 服务并允许开机启动
$ sudo nano /etc/resolvconf.conf  # 编辑 /etc/resolvconf.conf 并取消 name_servers=127.0.0.1 注释
$ sudo resolvconf -u # 重新生成 /etc/resolv.conf 文件
```

## 其他问题

- 有时 NetworkManager 会尝试修改 `/etc/resolv.conf` 文件，如有需要可以使用如下命
  令防止其修改：

```bash
$ sudo chattr +i /etc/resolv.conf  # 需要修改的时候使用 -i
```

- 53 端口（或其他端口）可能已经被 dnsmasq 占用，停止其服务或者更改端口
- forward 与 proxy 的区别在于前者不会访问缓存，也即 cache
