# OpenWrt 网络检测脚本

适用于 OpenWrt 的命令行网络检测脚本。

## 安装依赖

```sh
apk add bash curl jq bc bind-dig coreutils-od coreutils-timeout
```

## 运行

IPv4 检测（一行命令）：

```sh
curl -fsSL https://raw.githubusercontent.com/mino354e67/aaaaaaaafsf/main/ipquality-openwrt.sh -o /tmp/ipquality-openwrt.sh && bash /tmp/ipquality-openwrt.sh -4 -p
```

IPv6 检测（下载过脚本后）：

```sh
bash /tmp/ipquality-openwrt.sh -6 -p
```

删除临时脚本：

```sh
rm -f /tmp/ipquality-openwrt.sh
```

## 隐私说明

已移除使用量统计、广告和在线报告上传功能。IP 信誉、地理位置及流媒体检测仍需访问相关第三方服务。

## 来源与许可

本项目基于 [xykt/IPQuality](https://github.com/xykt/IPQuality) 的 2026-09-16 版本修改，遵循 GNU AGPL-3.0 许可证，详见 [LICENSE](LICENSE)。
