---
description: 云存储服务
---

# YukiCloud

## 总览

YukiCloud基于群晖DSM套件，为所有YukiGeek许可下的成员提供云存储服务。

YukiCloud的云存储当前基于Creeper23456的个人存储中心，并通过虚拟化共享空间。

## 文件浏览

### File Station

在登录到YukiCloud桌面后，桌面上将显示名为File Station的应用程序。

此时只需要点击即可使用File Station浏览文件，侧边为你当前可用的共享文件夹。

[DSM知识中心](https://kb.synology.com/zh-cn/search?sources%5B%5D=tutorial\&sources%5B%5D=help\&os\_versions%5B%5D=2\&services%5B%5D=File\_Station)

有关DSM的基础操作，你可以点击此处查阅DSM的文档，此处不多赘述。

### FTP、SFTP与SMB、AFP

你需要连接上[YukiVPN](../jin-jie/yukivpn.md)才能使用此类协议访问DSM。

在连接并获取到IP地址后，你可以访问`192.168.234.1`以访问所有的资源。

### 重要：有关大文件传输

大文件无法在**没有连接**上[YukiVPN](../jin-jie/yukivpn.md)的情况下上传。

由于CDN处在境外的特性，对于长时间的大文件传输，将会因为连接不稳定出现传输失败的状况。

超过50MB的文件，请连接上[YukiVPN](../jin-jie/yukivpn.md)后通过YukiCloud桌面的File Station或FTP等协议上传。

## 协同办公

YukiCloud基于DSM所提供的群晖Office套件，为协同办公提供了新的解决方案。

相比起第三方工具，YukiCloud所提供的Office套件数据更加安全，且允许多端协同工作。

在YukiCloud桌面，点击左上角的应用启动器图标，即可找到Synology Office套件中的Drive套件。在Drive中，你可以轻松创建一个新的Office文档，并开始协同工作。

更多详情，请查阅：[Synology Office知识中心](https://kb.synology.com/zh-cn/DSM/help/Spreadsheet/office\_general?version=7)
