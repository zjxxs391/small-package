## 介绍

#### 已归档

注意：鉴于运营商借着打击pcdn的理由，大搞nat4（也许是nat444444444...我也不知道以后会有多少个4），打洞基本失效，继续更新已无必要。大陆地区运营商网内、网间限速严重，ipv6也是苟延残喘，跨境倒是速度不错。。。
#### 基于 openwrt master 分支的 natmap 插件

注意：自 **openwrt23.0** 之后，使用 **golang>= 1.20，luci2**，部分插件不兼容。

## 基本功能

### 1.目前支持第三方服务调用功能

#### 1.1.qBittorrent

打洞成功后，自动修改 qBittorrent 的端口号，并配置转发（可选）。
需要配置 qBittorrent 地址、账号、密码用于修改端口。
需要配置 qBittorrent 使用网卡的 IP 用于配置转发，端口填 0，会转发到修改后的端口。

#### 1.2.Transmission

打洞成功后，自动修改 Transmission 的端口号，并配置转发（可选）。
需要配置 Transmission 地址、账号、密码用于修改端口。
需要配置 Transmission 使用网卡的 IP 用于配置转发，端口填 0，会转发到修改后的端口。

#### 1.3.Emby

配合 Emby Connect 使用时，用户登录账号后，会从服务器获取最新的连接地址信息，此模式就是用于配置这些信息的。
需要配置 Emby 地址和 API Key 用于修改连接地址信息。
此模式必须配置转发，默认不更新「外部域」，如果有配置 DDNS，将 DDNS 域名填入外部域后将不需要再次修改。
若没有域名，需要将 IP 填入外部域，可以勾选 「Update host with IP」，若对外提供的是 HTTPS 服务，需要勾选 「Update HTTPS Port」。

#### 1.4.Cloudflare Origin Rules

Cloudflare Origin Rules 可以设置回源端口，配合 DDNS 使用时，可以将 DDNS 域名指向 Cloudflare，然后将回源端口设置为打洞后的端口，这样就可以通过 Cloudflare 的 CDN 加速访问。
需要配置 Cloudflare 的 API Key，邮箱 和 Zone ID，Zone ID 可以在 Cloudflare 的域名首页找到。
API Key 请访问 https://dash.cloudflare.com/profile/api-tokens 复制 Global API Key。
需要先在 Cloudflare 后台的 Rules - Origin Rules 下添加一个 Origin Rules，然后将 Origin Rules 的 Name 填入配置中。
注意：Name 请保持唯一，否则会出现奇怪的问题。

#### 1.5.Cloudflare Redirect Rules

#### 1.6.Cloudflare DDNS

支持调用 Cloudflare DDNS 功能，存储外部 IP 和端口。
支持**AAAA 记录**、**HTTPS 记录**、**SRV 记录**。

### 2.目前支持的通知功能

#### 2.1. Telegram Bot

#### 2.2. PushPlus

#### 2.3. server 酱

#### 2.4. Gotify

### 3.端口转发功能

注意：openwrt 网关部署本插件时，可以直接使用 **natmap 转发** 和 **firewall dnat 转发**。 但在内网设备上部署时，若网关为**ikuai**，可使用 **ikuai 端口映射**，其余情况需要在网关手动设置**端口映射**或**dmz**。

#### 3.1.natmap 转发

支持使用 natmap 转发 tcp 和 udp。

#### 3.2.firewall dnat 转发

支持使用 openwrt 防火墙转发 tcp 和 udp。

#### 3.3.ikuai 端口映射

当前仅支持使用爱快系统作为主路由，可以自动设置主网关爱快系统的端口映射。

### 4.自定义脚本

支持自定义脚本

## 截图展示

![图1](./.img/natmap-1.png)
![图2](./.img/natmap-2.png)

## 使用

### openwrt 编译时添加软件源至 feeds.conf.default 首行，以覆盖 openwrt 内置 luci-app-natmap

```
src-git zzz https://github.com/blueberry-pie-11/openwrt-natmap
```

### 编译源码，尽量使用编译固件而非插件安装

```
./scripts/feeds update -a
./scripts/feeds install -a
make
```

## 本脚本相关功能依据以下代码改写：

1.  https://github.com/EkkoG/luci-app-natmap
2.  https://github.com/EkkoG/openwrt-natmap
3.  https://github.com/loyux/ikuai_local_api
4.  https://github.com/ztc1997/ikuai-bypass
