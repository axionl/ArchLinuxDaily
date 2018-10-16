# Haveged

## 介绍

> Entropy harvesting daemon using CPU timings

haveged 提供一个基于 HAVEGE 算法的随机数生成器

## 项目链接

> **Github Repository**: https://github.com/jirka-h/haveged

## 检查

当你出现以下状况时候可以检查当前系统的熵是否足够：

- sddm 需要较长时间才能启动，摇晃鼠标等操作可以明显减少等待时间
- 热点网速较慢（非性能限制）
- 开机 ssh 服务启动缓慢

```bash
$ cat /proc/sys/kernel/random/entropy_avail
3805 // 如果此值低于 1000 则需要安装此包
```

## 安装

```bash
$ sudo pacman -S haveged
$ sudo systemctl enable --now haveged // 添加开机启动并立即运行
```
