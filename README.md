# Loon 插件

## SideStore 本地回环

利用 Loon 原生的 `include-tun` 和 `[IP Rewrite] reflect`，将目的地址为 `10.7.0.1` 的 IPv4 数据包交换源、目的 IP 后回注本机协议栈，用于 SideStore 的本机设备通信。

插件不包含代理节点、JavaScript、MitM、Apple 账号或配对文件。其他目的地址的流量继续由原有 Loon 配置处理。

### 订阅地址

把下方链接添加到 Loon 的**插件**列表并启用，不要作为节点订阅或整份主配置导入：

```text
https://raw.githubusercontent.com/ShiinaWong/loon-plugins/main/SideStore.plugin
```

该链接跟随 `main` 分支，后续更新可以沿用同一地址。

### 使用条件

- Loon **Build 1007 或以上**。请在 Loon 的“关于”页面检查 Build 编号；旧版本不保证识别这两个配置项。
- SideStore 使用的设备通信目标地址为 **`10.7.0.1`**。若地址不同，本插件不能直接匹配，应先核实实际地址。
- SideStore 本身的账号、配对文件和签名配置已经完成。
- 首次测试连接 Wi-Fi，停止其他 VPN，仅启用 Loon。

### 首次验证

1. 在 Loon 导入并启用插件，重新连接 Loon，使路由配置生效。
2. 打开 SideStore；合体版从 LiveContainer 内进入 SideStore。
3. 执行 **Refresh All**，等待 SideStore 明确报告刷新成功，并核对到期时间。
4. 如果失败，记录具体错误；停用本插件后可恢复原来的 LocalDevVPN 使用方式。仅显示 VPN 已连接不代表续签成功。

插件只提供网络回环，不负责自动续签，也不会替代账号登录或设备配对。当前完成了配置静态检查；尚未在用户设备上完成端到端续签验证。

### 插件内容

```ini
[General]
include-tun = 10.7.0.1/32

[IP Rewrite]
10.7.0.1 = reflect
```

`[IP Rewrite]` 只支持放在插件中。命中的数据包先执行回环，不再经过普通规则、策略、HTTP Rewrite、脚本或 MitM。该插件不更改 IPv6 VIF 设置。

### 官方依据

- [Loon 插件文档：IP Rewrite 与 reflect](https://nsloon.app/docs/Plugin/)
- [Loon 通用配置：include-tun](https://nsloon.app/docs/General/)
- [LiveContainer + SideStore 安装与刷新说明](https://livecontainer.github.io/zh-CN/docs/installation/lc_sidestore)

整理日期：2026-11-11。
