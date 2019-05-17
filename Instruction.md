# KVM管理常用指令

```
virsh list  #显示正在运行的虚拟机  
virsh list –all  #显示所有虚拟机  
virsh dumpxml vm-name      # 查看kvm虚拟机配置文件  
virsh start centos   #启动虚拟机centos  
virsh shutdown centos  #关闭虚拟机  
virsh reboot centos  #重启虚拟机  
virsh destroy centos  #强制关闭虚拟机  
virsh suspend centos  #挂起虚拟机  
virsh resume centos  #恢复虚拟机  
virsh undefine centos  #删除虚拟机配置文件（不影响磁盘文件）  
virsh autostart centos  #设置虚拟机开机自启  
virsh autostart --disable centos  #取消虚拟机开机自启  

virsh dumpxml vm-name      # 查看kvm虚拟机配置文件
```



### 修改虚拟机配置信息

直接通过vim命令修改 

`im /etc/libvirt/qemu/centos.xml`

通过virsh命令修改

`virsh edit centos`



### 克隆虚拟机

```
# 暂停原始虚拟机
virsh shutdown centos
virt-clone -o centos -n centos.112 -f /home/vms/centos.112.qcow2 -m 00:00:00:00:00:01
virt-clone -o centos -n centos.112 --file /home/vms/centos.112.qcow2 --nonsparse
```

`virt-clone` 参数介绍

- `--version` 查看版本。
- `-h，--help` 查看帮助信息。
- `--connect=URI` 连接到虚拟机管理程序 libvirt 的URI。
- `-o 原始虚拟机名称` 原始虚拟机名称，必须为关闭或者暂停状态。
- `-n 新虚拟机名称` --name 新虚拟机名称。
- `--auto-clone` 从原来的虚拟机配置自动生成克隆名称和存储路径。
- `-u NEW_UUID, --uuid=NEW_UUID` 克隆虚拟机的新的UUID，默认值是一个随机生成的UUID。
- `-m NEW_MAC, --mac=NEW_MAC` 设置一个新的mac地址，默认为随机生成 MAC。
- `-f NEW_DISKFILE, --file=NEW_DISKFILE` 为新客户机使用新的磁盘镜像文件地址。
- `--force-copy=TARGET` 强制复制设备。
- `--nonsparse` 不使用稀疏文件复制磁盘映像。



### 通过镜像创建虚拟机

创建虚拟机镜像文件

```
# 复制第一次安装的干净系统镜像，作为基础镜像文件，
# 后面创建虚拟机使用这个基础镜像
cp /home/vms/centos.88.qcow2 /home/vms/centos7.base.qcow2

# 使用基础镜像文件，创建新的虚拟机镜像
cp /home/vms/centos7.base.qcow2 /home/vms/centos7.113.qcow2
```

创建虚拟机配置文件

```
# 复制第一次安装的干净系统镜像，作为基础配置文件。
virsh dumpxml centos.88 > /home/vms/centos7.base.xml

# 使用基础虚拟机镜像配置文件，创建新的虚拟机配置文件
cp /home/vms/centos7.base.xml /home/vms/centos7.113.xml

# 编辑新虚拟机配置文件
vi /home/vms/centos7.113.xml
```

主要是修改虚拟机文件名，UUID，镜像地址和网卡地址，其中 UUID 在 Linux 下可以使用 `uuidgen` 命令生成

```
<domain type='kvm'>
  <name>centos7.113</name>
  <uuid>1e86167a-33a9-4ce8-929e-58013fbf9122</uuid>
  <devices>
    <disk type='file' device='disk'>
      <source file='/home/vms/centos7.113.img'/>
    </disk>
    <interface type='bridge'>
      <mac address='00:00:00:00:00:04'/>
    </interface>    
    </devices>
</domain>
virsh define /home/vms/centos7.113.xml
# Domain centos.113 defined from /home/vms/centos7.113.xml
```



### 动态更改CPU数量与内存大小

动态调整，如果超过给虚拟机分配的最大内存，需要重启虚拟机。

```
virsh list --all
#  Id    名称                         状态
# ----------------------------------------------------
#  2     test                     running

# 更改CPU
virsh setvcpus test  --maximum 4 --config
# 更改内存
virsh setmaxmem test  1048576 --config
# 查看信息
virsh dominfo test 
```

