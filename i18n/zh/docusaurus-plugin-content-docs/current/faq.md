---
sidebar_position: 17
sidebar_label: 常见问题
title: "常见问题"
---

本文包含了用户常见的 Harvester 问题。

### 如何通过 SSH 登录到 Harvester 节点？
```shell
$ ssh rancher@node-ip
```

### Harvester 仪表盘的默认登录用户名和密码是什么？
```shell
username: admin
password: # 首次登录时会提示你设置默认密码
```

### 如何访问 Harvester 集群的 kubeconfig 文件？

选项 1：你可以从 Harvester 仪表盘的支持页面下载 kubeconfig 文件。
![harvester-kubeconfig.png](/img/v1.2/harvester-kubeconfig.png)

选项 2：你可以从其中一个 Harvester 管理节点获取 kubeconfig 文件。例如：
```shell
$ sudo su
$ cat /etc/rancher/rke2/rke2.yaml
```

### 如何在运行的虚拟机上安装 qemu-guest-agent
```shell
# cloud-init 只会执行一次。使用以下命令添加 cloud-init 配置后请重新启动它。
$ cloud-init clean --logs --reboot
```
[https://cloudinit.readthedocs.io/en/latest/reference/cli.html#clean](https://cloudinit.readthedocs.io/en/latest/reference/cli.html#clean)

### 如何重置管理员密码？

如果你忘记了管理员密码，请通过命令行进行重置。SSH 到其中一个管理节点并运行以下命令：
```shell
# 切换到 root 并运行：
$ kubectl  -n cattle-system exec $(kubectl --kubeconfig $KUBECONFIG -n cattle-system get pods -l app=rancher --no-headers | head -1 | awk '{ print $1 }') -c rancher -- reset-password
New password for default administrator (user-xxxxx):
<new_password>
```

### 我添加了一个带分区的磁盘。为什么没有被检测到？

从 Harvester v1.0.2 开始，我们不再支持添加其他分区磁盘，因此请务必先删除所有分区（例如，使用 `fdisk`）。
