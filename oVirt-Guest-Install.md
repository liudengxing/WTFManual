### 安装ghost主机

ghost主机需要支持**虚拟化**才可以添加到主机列表中，出现问题可以先检查是否支持虚拟化，可能会报其他错误。



直接在 主机 ——添加 中写好IP，Name，Root Password即可添加




### 报错及解决
- 错误：An error has occurred during installation of Host ovirtlocal4: Failed to execute stage 'Setup validation': Cannot locate ovirt-host package, possible cause is incorrect channels.

ovirt的包找不到，就是没有ovirt软件源

添加ovirt官方软件源即可

```
yum install http://resources.ovirt.org/pub/yum-repo/ovirt-release43.rpm
```



- 出现/dev/run/vdsm空间不足（low disk free space） 说明内存的空间不够，加钱即可解决



- 报错 An error has occurred during installation of Host 31.172: Yum [u'1:net-snmp-5.7.2-37.el7.x86_64 requires libmysqlclient.so.18()(64bit)'].

  安装要求的软件即可

  ```
  yum install libmysqlclient.so.18
  ```

- yum安装软件时报错libmysqlclient.so.18()(64bit)

```
# wget http://www.percona.com/redir/downloads/Percona-XtraDB-Cluster/5.5.37-25.10/RPM/rhel6/x86_64/Percona-XtraDB-Cluster-shared-55-5.5.37-25.10.756.el6.x86_64.rpm
# rpm -ivh Percona-XtraDB-Cluster-shared-55-5.5.37-25.10.756.el6.x86_64.rpm
```