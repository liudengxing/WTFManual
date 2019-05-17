# oVirt安装手册

本文主要参考[oVirt官方文档](<https://www.ovirt.org/documentation/install-guide/chap-Installing_oVirt.html>)

### Host主机安装oVirt-engine

~~确认主机CPU已开启虚拟化~~  
1. 首先，添加oVirt官方源

    ```
    yum install http://resources.ovirt.org/pub/yum-repo/ovirt-release43.rpm
    ```

2. 更新

    ```
    yum update
    ```

3. 安装ovirt-engine

    ```
    yum install ovirt-engine
    ```

4. 配置engine  

   ```
   engine-setup
   ```

   一路全部默认即可，只有一点需要注意

   ```
   Host fully qualified DNS name of this server [*autodetected host name*]:
   ```

   在虚拟机上，获取到的默认获取到的信息会是错误的，所以我们自己给定一个网址，并在host中配置ip对应关系即可。  

5. 安装成功后在非本地主机打开web时需要将之前输入的网址与ip写入hosts中后即可访问  
