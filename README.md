# IPQuality for OpenWrt (self-use)

在 OpenWrt 上运行的 IPQuality 定制版，基于 [xykt/IPQuality](https://github.com/xykt/IPQuality) `ip.sh` 的 2026-09-16 版本修改。源项目及本衍生版本按 GNU AGPL-3.0 授权；详见仓库根目录 `LICENSE`。

> 状态：已做源代码静态检查；**尚未在你的 NanoPi R3S / OpenWrt 25.12.5 真实设备上执行完整检测**。

## 与原版的差别

- **禁用访问次数统计**（不请求 `hits.xykt.de`）。
- **不上传完整报告**（去掉 `upload.check.place` 调用），也不生成在线报告链接。
- 删除广告/赞助商下载和显示代码；不清屏，保留 SSH 终端中的历史输出。
- 不进行 Google 网络预检；删除原版交互菜单与在线自动升级 Bash 的提示。
- 不自动安装或升级软件包；检测必要命令是否存在，缺失时报错退出。
- 将 BusyBox 不接受的 `sleep 0.1` 改为 `sleep 1`；使用 `coreutils-od` 提供原版需要的 `od`。
- 对 DNS 黑名单查询并发数由 50 降到 4，以减少对路由器的瞬时资源压力。
- 隐私模式默认开启，不提供通过命令行恢复在线上传的开关。

**网络隐私边界：** 这不是离线检测。为了保留原版风险评分、ASN、流媒体和邮件连通性功能，脚本仍然请求 ipinfo.check.place（原作者维护的数据库聚合接口）、其他公开 IP 数据库、原版仓库中的 cookies/iso3166/dnsbl 参考资料以及目标流媒体/邮件服务。它们能够观察到相应请求和来源 IP。这里禁用的只是**额外统计、广告内容下载和完整汇总报告上传**。某些检测服务仍使用 POST 请求完成其正常 API 查询（例如 Disney+），不能理解为完全禁止向外发送任何数据。若你需要完全不接触原作者服务器，不能直接使用此版的完整风险数据库功能。

## 环境

目标：OpenWrt 25.12.5 / ARM64（R3S）。脚本需要 Bash 4+；不是 POSIX `sh` 脚本。

OpenWrt 25.12 使用 `apk`，首次安装依赖（一行）：

```sh
apk add bash curl jq bc bind-dig coreutils-od
```

确认其余工具存在（一行）：

```sh
for x in bash curl jq bc nc dig ip od timeout xargs; do command -v "$x" || echo "缺少: $x"; done
```

只要有一项缺失，应先根据报错处理；不要使用原版的 `-y` 自动安装选项。

## 运行

把 `ipquality-openwrt.sh` 放到路由器 `/tmp/ipquality-openwrt.sh`，然后执行：

```sh
bash /tmp/ipquality-openwrt.sh -4 -p
```

`-4` 仅测 IPv4；`-p` 隐私模式（此版已默认开启，保留此参数兼容原版）。只测 IPv6：`-6`。输出 JSON：`-4 -j`。查看参数：`-h`。

本仓库是公开仓库。可在 R3S 上直接运行以下一行命令（先确认依赖已安装，并阅读脚本内容）：

```sh
curl -fsSL https://raw.githubusercontent.com/mino354e67/aaaaaaaafsf/main/ipquality-openwrt.sh -o /tmp/ipquality-openwrt.sh && bash /tmp/ipquality-openwrt.sh -4 -p
```

公开仓库可以被搜索、克隆和重新传播；随机仓库名不提供访问控制。

脚本仅检测**路由器自身**的出站网络；如果默认路由经过 WireGuard/代理，结果可能是隧道/代理出口，而非韩国宽带原生出口。运行时会产生大量外部 HTTPS 和 DNS 请求，建议在不影响远程管理的时段测试。

## 清理

```sh
rm -f /tmp/ipquality-openwrt.sh
```

此命令不会卸载已安装的系统依赖。如需清理软件包，应先确认其他服务是否也在使用，避免影响路由器运行。

## 原版出处与许可

- 上游：https://github.com/xykt/IPQuality
- 基于上游 2026-09-16 版；保留原始许可 `LICENSE`。
- 本仓库为独立公开副本，原先 `sgproxy` 的独立分支不受影响。
