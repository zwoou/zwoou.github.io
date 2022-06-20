# vmware的使用

## 网络配置 

[https://blog.csdn.net/zhang33565417/article/details/97779579](https://blog.csdn.net/zhang33565417/article/details/97779579)

## NAT模式

![vmware02](https://gitee.com/zwoou//picgo/raw/master/pic//vmware02.png)



## Bridged（桥接模式）

桥接模式就是将主机网卡与虚拟机虚拟的网卡利用虚拟网桥进行通信.在桥接的作用下，类似于把物理主机虚拟为一个交换机，所有桥接设置的虚拟机连接到这个交换机的一个接口上，物理主机也同样插在这个交换机当中，所以所有桥接下的网卡与网卡都是交换模式的，相互可以访问而不干扰。在桥接模式下，虚拟机ip地址需要与主机在同一个网段，如果需要联网，则网关与DNS需要与主机网卡一致。其网络结构如下图所示：

![vmware01](https://gitee.com/zwoou//picgo/raw/master/pic//vmware01.png)

![20220620172942](https://s2.loli.net/2022/06/20/8fXBZriNcGmDMUz.png)