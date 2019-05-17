# NET模式网络配置

Linux下KVM的NET网络连接配置流程



首先配置虚拟机中的网络信息

```
 vi /etc/sysconfig/network-scripts/ifcfg-eth0
```

`ONBOOT=no` 

改为

`ONBOOT=yes`

然后设置JVM的NET模式（NET模式是KVM安装后的默认模式）

`virsh net-edit default`

`virsh net-list --all`



![NET模式list](..\img\NET模式list.png)

重新加载虚拟网络并激活配置：

`virsh net-define /etc/libvirt/qemu/networks/default.xml`

设置自动启动

`virsh net-auto-start default`

启动网络

`virsh net-start default`

修改sysctl.conf中的参数以允许ip转发

`vi /etc/sysctl.conf`

新增/修改

`net.ipv4.ip_forward=1`

通过`susctl -p`查看修改结果