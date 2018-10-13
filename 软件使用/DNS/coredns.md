# CoreDNS

## 介绍

> CoreDNS is a DNS server that chains plugins

可快速高效的搭建属于自己的 DNS 服务器，并且包含多种插件。

## 项目链接

> **Github Repository**: https://github.com/coredns/coredns

> **Website**: https://coredns.io

## 安装

包名: `coredns`

可以添加 Arch CN 源之后使用 pacman 安装，也可以通过 AUR Helper 安装。

## 配置

配置文件所在目录: `/etc/coredns/Corefile`

```json
// 默认监听在 53 端口，请务必保持对应防火墙规则开启

.:53 {
    log // 解析记录
    cache // 缓存解析结果
    proxy . 8.8.8.8 // 转发未命中请求至上游 DNS
    forward example.org 1.1.1.1 // 直接转发请求
}
```

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
