---
description: 用于访问内部服务
---

# YukiVPN

## 注意

本条目与境外互联网访问无关。

如果你正在寻找突破GFW防火墙或类似的文章，请不要继续阅读此文章。

YukiVPN的设立仅用于访问内部网络，你不能也无法将其用于访问互联网。

## 摘要

YukiVPN旨在为传输大文件提供点对点的支持。

由于公网IP的稀缺，维护高带宽的服务将大幅增加维护成本。因此，与YukiVPN建立点对点的连接，是最为有效的选择。

除去访问YukiCloud外，你还可以使用YukiVPN访问内部服务且无需繁琐的密码输入。你还会发现访问这些内部服务要比平时自互联网访问快的多。

以下介绍两种连接到YukiVPN的方式，以方便YukiGeek成员使用。

## 点对点连接

> 注意：点对点模式仅支持桌面或路由器连接，如需在移动设备使用，请参考下文建立中继隧道连接。

### YukiVPN启动器

#### 这是什么？

YukiVPN的点对点模式部署步骤过于复杂。为了方便非技术极客也能方便地使用YukiVPN的点对点模式，Creeper23456开发了一套启动器，以方便任何人都能方便地连接上YukiVPN。

#### 下载

根据你所使用的设备不同，选择你设备所能运行的文件包。

[Windows 64位](https://assets.cnklp.cn/geekhouse/stream/bin/YukiVPN/windows\_x64\_build.zip)

[macOS Intel芯片](https://assets.cnklp.cn/geekhouse/stream/bin/YukiVPN/macos\_x64\_build.zip)

[macOS Apple芯片](https://assets.cnklp.cn/geekhouse/stream/bin/YukiVPN/macos\_arm64\_build.zip)

[Linux 64位](https://assets.cnklp.cn/geekhouse/stream/bin/YukiVPN/linux\_x64\_build.zip)

[Linux armhf芯片](https://assets.cnklp.cn/geekhouse/stream/bin/YukiVPN/linux\_arm\_build.zip)

[Linux arm64芯片](https://assets.cnklp.cn/geekhouse/stream/bin/YukiVPN/linux\_arm64\_build.zip)

在完成下载后，你还需要安装OpenVPN、TAP驱动程序作为运行VPN的必要依赖。

对于未列出的设备类型，你可以在YukiGit寻找Creeper23456的代码仓库，并自行拉取与编译。

#### 安装OpenVPN

对于Linux用户，请参照你的发行版所提供的OpenVPN包

对于Windows用户，运行压缩包内所含的`openvpn_installer.msi`与`tap-windows-installer.exe`即可

对于macOS用户，你需要安装`Homebrew`，随后使用brew指令安装OpenVPN软件包。

#### 启动YukiVPN启动器

对于Unix系用户，使用sudo执行`start.sh`即可。

对于Windows，直接双击运行`start.bat`即可。

#### 疑难解答

（等待补充）

### 手动部署

#### 前提

对于出于各类原因不希望使用YukiVPN启动器的技术人员来说，手动部署更易于排查错误与检查连接性问题。

#### FRP

YukiVPN的点对点连接依赖于frp与OpenVPN。以下为frpc的配置：

```
[common]
server_addr = vpn.yukigeek.cn
server_port = 7000
token = YukiGeek
log_file = ./frpc.log
nat_hole_stun_server = stun.voipstunt.com:3478
protocol = kcp

[openvpn]
type = xtcp
role = visitor
sk = YukiGeek
protocol = kcp
server_name = openvpn
bind_addr = 0.0.0.0
bind_port = 1194
```

请注意，除去`log_file`键可以被编辑外，编辑其他的文本可能会导致无法连接。

该frp服务器被设定为仅允许`xtcp`流量通过。如需进行内网穿透，请联系YukiGeek技术支持单独讨论。

#### OpenVPN

YukiVPN使用TUN模式，使用TAP模式将无法连接至服务器。

DHCP服务器将会为每个用户分配一个处在`192.168.234.0/24`网段的地址。每个帐号最多可登录3台设备。

OpenVPN配置文件参见 中继隧道>OpenVPN配置

将OpenVPN配置中的`remote`字段修改为`127.0.0.1`即可通过点对点连接访问YukiVPN。

## 中继隧道

当点对点连接出于网络环境限制无法成功建立时，可考虑使用隧道连接。

YukiVPN的中转隧道提供3Mbps的转发带宽，这相当于只有300kb/s的下行与上行速度。因此除非必要，请使用点对点连接。

### OpenVPN配置

```
dev tun
tls-client
remote vpn.yukigeek.cn 1194
pull
proto tcp-client
script-security 2
comp-lzo
reneg-sec 0
cipher AES-256-CBC
auth SHA512
auth-user-pass
<ca>
-----BEGIN CERTIFICATE-----
MIIDgDCCAmigAwIBAgIUbVg4WnyyOEiM/Q+LlccCtHygc7EwDQYJKoZIhvcNAQEL
BQAwUTELMAkGA1UEBhMCVFcxDzANBgNVBAcMBlRhaXBlaTEWMBQGA1UECgwNU3lu
b2xvZ3kgSW5jLjEZMBcGA1UEAwwQU3lub2xvZ3kgSW5jLiBDQTAeFw0yMzA4MTcw
NDM1NDJaFw0yNDA4MTcwNDM1NDJaMFExCzAJBgNVBAYTAlRXMQ8wDQYDVQQHDAZU
YWlwZWkxFjAUBgNVBAoMDVN5bm9sb2d5IEluYy4xGTAXBgNVBAMMEFN5bm9sb2d5
IEluYy4gQ0EwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQC/6XU7Agf7
WhZNYXBjTfibgei3YnjfHfAGRqq/45/fcFLz/QCkL4qt3pIS/3EZIEGq4tcbB2vT
qAuR2W5zD4KSJ7ezUP2tCdWsURWB6aO/3YZbTSDhzloTit5CcNiWT4oW8KH9EISH
gfbuGnzr6LCEFpgeE2Xz8y1HId1CVjK/eZn1P9vj8t6w6XAnhlFy+lcRvRuKduP6
pUJzAbg8jp5Xy+A8osp3Eg/ODgkURiX4sszcXBt6xMH4rojcv57MzGyr1LIBu2Ty
QFN0+pmlBd5IZzQKe7DYrdEm/wzY5ZOnIIK1D2XtBTnviqOZPrDsWJMCwSmb8f4G
V0cnok8dOdmRAgMBAAGjUDBOMB0GA1UdDgQWBBTv+gYpPpkWiuCmMKK3T6rmYy8o
ODAfBgNVHSMEGDAWgBTv+gYpPpkWiuCmMKK3T6rmYy8oODAMBgNVHRMEBTADAQH/
MA0GCSqGSIb3DQEBCwUAA4IBAQBt79OhlCV6dQpQEM9ZY0Ka0bvu5AqQrRJjdO8i
dQuTlzHPJWnXF5hIqom9yZStmA8GIcMHQgxKNJK5yC7J4S+FvJtOCYlskYYevCyZ
eKCuZEvL/qp4w68MNCbeoQCN+rP9vWE76phc4s7FdFxGiQUYrOM0MwgBJTeqIfJ2
1BkoWlyT3CMjckTr3LxM78WM+C+0dXcztu4BiCKjKZcQ7H5VURPVRxrHtdTBY/cp
bdPXWPrkc1XsSNSUJZ2Ok0cZTO5mP+ztx1Q/jiylADUOpm22oWcrQoAyLPac7TN9
tpcZAsxo9RyAMJJlB4BHv5agk6tLfd3cEnV/Znn/VuvOnQKb
-----END CERTIFICATE-----

</ca>
```

新建一个文件，并复制该配置文件到空的新文件中，将后缀改为`.ovpn`即可在OpenVPN注册中继服务器的连接信息。
