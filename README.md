# Cloudflare 优选域名和 IP
## 一、 数据源
1. 每 12 小时（UTC+0）自动构建
2. **bestcf-domain.txt** 源采用 [CMLiussss（优选域名）](https://cf.090227.xyz)、[VPS789（优选 CNAME 域名）](https://vps789.com/cfip/?remarks=domain) 和[微测网（CloudFlare 优选 Cname 域名）](https://www.wetest.vip/page/cloudflare/cname.html)组合
3. **cmcc-ip.txt** 源采用 [CMLiussss（移动优选 IP）](https://cf.090227.xyz/cmcc)（IPv4 & IPv6）、[VPS789（移动优选 IP）](https://vps789.com/cfip/)（IPv4）、[CloudFlareYes（移动优选 IP）](https://stock.hostmonit.com/CloudFlareYes)（IPv4 & IPv6）和[微测网（移动优选 IP）](https://www.wetest.vip/page/cloudflare/address_v4.html)（IPv4 & IPv6）组合
4. **cucc-ip.txt** 源采用 [CMLiussss（联通优选 IP）](https://cf.090227.xyz/cu)（IPv4）、[VPS789（联通优选 IP）](https://vps789.com/cfip/)（IPv4）、[CloudFlareYes（联通优选 IP）](https://stock.hostmonit.com/CloudFlareYes)（IPv4 & IPv6）和[微测网（联通优选 IP）](https://www.wetest.vip/page/cloudflare/address_v4.html)（IPv4 & IPv6）组合
5. **ctcc-ip.txt** 源采用 [CMLiussss（电信优选 IP）](https://cf.090227.xyz/ct)（IPv4）、[VPS789（电信优选 IP）](https://vps789.com/cfip/)（IPv4）、[CloudFlareYes（电信优选 IP）](https://stock.hostmonit.com/CloudFlareYes)（IPv4 & IPv6）和[微测网（电信优选 IP）](https://www.wetest.vip/page/cloudflare/address_v4.html)（IPv4 & IPv6）组合
6. **bestcf-ip.txt** 源采用 [VPS789（CF 优选 IP）](https://vps789.com/cfip/)（IPv4）、[CloudflareSpeedTest（Cloudflare 优选 IP 测速数据）](https://ip.164746.xyz)（IPv4）、[IPDB（CF 优选官方 IP 服务）](https://ipdb.030101.xyz/bestcfv4/)（IPv4 & IPv6）组合
7. **proxy-ip.txt**（反代 IP）源采用 [IPDB（CF 优选官方反代 IP 服务）](https://ipdb.030101.xyz/bestproxy/)（IPv4）

## 二、 使用方法
### 1. 使用说明
先使用 [`fallback` 自动回退](https://wiki.metacubex.one/config/proxy-groups/fallback/)策略优先选择各个优选 IP 来源的第一个节点（质量最高），然后使用 [`url-test` 自动选择](https://wiki.metacubex.one/config/proxy-groups/url-test/)策略供用户选择使用
### 2. [mihomo 内核](https://github.com/MetaCubeX/mihomo)

- 注：以下只是节选，请酌情套用

```yaml
proxy-providers:
  🆓 免费订阅:
    type: http
    url: "https://example.com&clash"
    path: ./providers/free.yaml
    interval: 43200
    health-check:
      enable: true
      url: https://www.gstatic.com/generate_204
      interval: 600

proxy-groups:
  - {name: 节点选择, type: select, proxies: [移动优选节点 IPv4, 移动优选节点 IPv6, 联通优选节点 IPv4, 联通优选节点 IPv6, 电信优选节点 IPv4, 电信优选节点 IPv6, CF 优选 IP 节点, CF 优选域名节点], icon: "https://github.com/DustinWin/ruleset_geodata/releases/download/icons/proxy.png"}
  - {name: 移动优选节点 IPv4, type: url-test, tolerance: 100, proxies: [移动优选节点 CMLiu_IPv4, 移动优选节点 VPS789_IPv4, 移动优选节点 CFYes_IPv4, 移动优选节点 WeTest_IPv4], icon: "https://github.com/DustinWin/ruleset_geodata/releases/download/icons/cmcc.png"}
  - {name: 移动优选节点 CMLiu_IPv4, type: fallback, use: [🆓 免费订阅], filter: "(?i)(CMCC-IPv4_CMLiu)", hidden: true, icon: "https://github.com/DustinWin/ruleset_geodata/releases/download/icons/cmliu.png"}
  - {name: 移动优选节点 VPS789_IPv4, type: fallback, use: [🆓 免费订阅], filter: "(?i)(CMCC-IPv4_VPS789)", hidden: true, icon: "https://github.com/DustinWin/ruleset_geodata/releases/download/icons/vps789.png"}
  - {name: 移动优选节点 CFYes_IPv4, type: fallback, use: [🆓 免费订阅], filter: "(?i)(CMCC-IPv4_CFYes)", hidden: true, icon: "https://github.com/DustinWin/ruleset_geodata/releases/download/icons/cloudflareyes.png"}
  - {name: 移动优选节点 WeTest_IPv4, type: fallback, use: [🆓 免费订阅], filter: "(?i)(CMCC-IPv4_WeTest)", hidden: true, icon: "https://github.com/DustinWin/ruleset_geodata/releases/download/icons/wetest.png"}
  - {name: 移动优选节点 IPv6, type: url-test, tolerance: 100, proxies: [移动优选节点 CMLiu_IPv6, 移动优选节点 CFYes_IPv6, 移动优选节点 WeTest_IPv6], icon: "https://github.com/DustinWin/ruleset_geodata/releases/download/icons/cmcc.png"}
  - {name: 移动优选节点 CMLiu_IPv6, type: fallback, use: [🆓 免费订阅], filter: "(?i)(CMCC-IPv6_CMLiu)", hidden: true, icon: "https://github.com/DustinWin/ruleset_geodata/releases/download/icons/cmliu.png"}
  - {name: 移动优选节点 CFYes_IPv6, type: fallback, use: [🆓 免费订阅], filter: "(?i)(CMCC-IPv6_CFYes)", hidden: true, icon: "https://github.com/DustinWin/ruleset_geodata/releases/download/icons/cloudflareyes.png"}
  - {name: 移动优选节点 WeTest_IPv6, type: fallback, use: [🆓 免费订阅], filter: "(?i)(CMCC-IPv6_WeTest)", hidden: true, icon: "https://github.com/DustinWin/ruleset_geodata/releases/download/icons/wetest.png"}
  - {name: 联通优选节点 IPv4, type: url-test, tolerance: 100, proxies: [联通优选节点 CMLiu_IPv4, 联通优选节点 VPS789_IPv4, 联通优选节点 CFYes_IPv4, 联通优选节点 WeTest_IPv4], icon: "https://github.com/DustinWin/ruleset_geodata/releases/download/icons/cucc.png"}
  - {name: 联通优选节点 CMLiu_IPv4, type: fallback, use: [🆓 免费订阅], filter: "(?i)(CUCC-IPv4_CMLiu)", hidden: true, icon: "https://github.com/DustinWin/ruleset_geodata/releases/download/icons/cmliu.png"}
  - {name: 联通优选节点 VPS789_IPv4, type: fallback, use: [🆓 免费订阅], filter: "(?i)(CUCC-IPv4_VPS789)", hidden: true, icon: "https://github.com/DustinWin/ruleset_geodata/releases/download/icons/vps789.png"}
  - {name: 联通优选节点 CFYes_IPv4, type: fallback, use: [🆓 免费订阅], filter: "(?i)(CUCC-IPv4_CFYes)", hidden: true, icon: "https://github.com/DustinWin/ruleset_geodata/releases/download/icons/cloudflareyes.png"}
  - {name: 联通优选节点 WeTest_IPv4, type: fallback, use: [🆓 免费订阅], filter: "(?i)(CUCC-IPv4_WeTest)", hidden: true, icon: "https://github.com/DustinWin/ruleset_geodata/releases/download/icons/wetest.png"}
  - {name: 联通优选节点 IPv6, type: url-test, tolerance: 100, proxies: [联通优选节点 CFYes_IPv6, 联通优选节点 WeTest_IPv6], icon: "https://github.com/DustinWin/ruleset_geodata/releases/download/icons/cucc.png"}
  - {name: 联通优选节点 CFYes_IPv6, type: fallback, use: [🆓 免费订阅], filter: "(?i)(CUCC-IPv6_CFYes)", hidden: true, icon: "https://github.com/DustinWin/ruleset_geodata/releases/download/icons/cloudflareyes.png"}
  - {name: 联通优选节点 WeTest_IPv6, type: fallback, use: [🆓 免费订阅], filter: "(?i)(CUCC-IPv6_WeTest)", hidden: true, icon: "https://github.com/DustinWin/ruleset_geodata/releases/download/icons/wetest.png"}
  - {name: 电信优选节点 IPv4, type: url-test, tolerance: 100, proxies: [电信优选节点 CMLiu_IPv4, 电信优选节点 VPS789_IPv4, 电信优选节点 CFYes_IPv4, 电信优选节点 WeTest_IPv4], icon: "https://github.com/DustinWin/ruleset_geodata/releases/download/icons/ctcc.png"}
  - {name: 电信优选节点 CMLiu_IPv4, type: fallback, use: [🆓 免费订阅], filter: "(?i)(CTCC-IPv4_CMLiu)", hidden: true, icon: "https://github.com/DustinWin/ruleset_geodata/releases/download/icons/cmliu.png"}
  - {name: 电信优选节点 VPS789_IPv4, type: fallback, use: [🆓 免费订阅], filter: "(?i)(CTCC-IPv4_VPS789)", hidden: true, icon: "https://github.com/DustinWin/ruleset_geodata/releases/download/icons/vps789.png"}
  - {name: 电信优选节点 CFYes_IPv4, type: fallback, use: [🆓 免费订阅], filter: "(?i)(CTCC-IPv4_CFYes)", hidden: true, icon: "https://github.com/DustinWin/ruleset_geodata/releases/download/icons/cloudflareyes.png"}
  - {name: 电信优选节点 WeTest_IPv4, type: fallback, use: [🆓 免费订阅], filter: "(?i)(CTCC-IPv4_WeTest)", hidden: true, icon: "https://github.com/DustinWin/ruleset_geodata/releases/download/icons/wetest.png"}
  - {name: 电信优选节点 IPv6, type: url-test, tolerance: 100, proxies: [电信优选节点 CFYes_IPv6, 电信优选节点 WeTest_IPv6], icon: "https://github.com/DustinWin/ruleset_geodata/releases/download/icons/ctcc.png"}
  - {name: 电信优选节点 CFYes_IPv6, type: fallback, use: [🆓 免费订阅], filter: "(?i)(CTCC-IPv6_CFYes)", hidden: true, icon: "https://github.com/DustinWin/ruleset_geodata/releases/download/icons/cloudflareyes.png"}
  - {name: 电信优选节点 WeTest_IPv6, type: fallback, use: [🆓 免费订阅], filter: "(?i)(CTCC-IPv6_WeTest)", hidden: true, icon: "https://github.com/DustinWin/ruleset_geodata/releases/download/icons/wetest.png"}
  - {name: CF 优选 IP 节点, type: url-test, tolerance: 100, proxies: [CF 优选节点 VPS789, CF 优选节点 CFSpeedTest, CF 优选节点 IPDB_IPv4, CF 优选节点 IPDB_IPv6], icon: "https://github.com/DustinWin/ruleset_geodata/releases/download/icons/cfip.png"}
  - {name: CF 优选节点 VPS789, type: fallback, use: [🆓 免费订阅], filter: "(?i)(CF-IPv4_VPS789)", hidden: true, icon: "https://github.com/DustinWin/ruleset_geodata/releases/download/icons/vps789.png"}
  - {name: CF 优选节点 CFSpeedTest, type: fallback, use: [🆓 免费订阅], filter: "(?i)(CF-IPv4_CFSpeedTest)", hidden: true, icon: "https://github.com/DustinWin/ruleset_geodata/releases/download/icons/cloudflare.png"}
  - {name: CF 优选节点 IPDB_IPv4, type: fallback, use: [🆓 免费订阅], filter: "(?i)(CF-IPv4_IPDB)", hidden: true, icon: "https://github.com/DustinWin/ruleset_geodata/releases/download/icons/ipdb.png"}
  - {name: CF 优选节点 IPDB_IPv6, type: fallback, use: [🆓 免费订阅], filter: "(?i)(CF-IPv6_IPDB)", hidden: true, icon: "https://github.com/DustinWin/ruleset_geodata/releases/download/icons/ipdb.png"}
  - {name: CF 优选域名节点, type: url-test, tolerance: 100, use: [🆓 免费订阅], filter: "[.+.]", icon: "https://github.com/DustinWin/ruleset_geodata/releases/download/icons/cfdomain.png"}
```

### 3. [sing-boxr 内核](https://github.com/reF1nd/sing-box)
- 注：以下只是节选，请酌情套用

```json
{
  "providers": [
    {
      "tag": "🆓 免费订阅",
      "type": "remote",
      "url": "https://example.com&sb",
      "path": "./providers/free.json",
      "update_interval": "12h",
      "health_check": {
        "enabled": true,
        "url": "https://www.gstatic.com/generate_204"
      }
    }
  ],
  "outbounds": [
    { "tag": "节点选择", "type": "selector", "outbounds": [ "移动优选节点 IPv4", "移动优选节点 IPv6", "联通优选节点 IPv4", "联通优选节点 IPv6", "电信优选节点 IPv4", "电信优选节点 IPv6", "CF 优选 IP 节点", "CF 优选域名节点" ] },
    { "tag": "移动优选节点 IPv4", "type": "urltest", "tolerance": 100, "outbounds": [ "移动优选节点 CMLiu_IPv4", "移动优选节点 VPS789_IPv4", "移动优选节点 CFYes_IPv4", "移动优选节点 WeTest_IPv4" ] },
    { "tag": "移动优选节点 CMLiu_IPv4", "type": "urltest", "providers": [ "🆓 免费订阅" ], "include": "(?i)(CMCC-IPv4_CMLiu)", "fallback": { "enabled": true, "max_delay": "400ms" } },
    { "tag": "移动优选节点 VPS789_IPv4", "type": "urltest", "providers": [ "🆓 免费订阅" ], "include": "(?i)(CMCC-IPv4_VPS789)", "fallback": { "enabled": true, "max_delay": "400ms" } },
    { "tag": "移动优选节点 CFYes_IPv4", "type": "urltest", "providers": [ "🆓 免费订阅" ], "include": "(?i)(CMCC-IPv4_CFYes)", "fallback": { "enabled": true, "max_delay": "400ms" } },
    { "tag": "移动优选节点 WeTest_IPv4", "type": "urltest", "providers": [ "🆓 免费订阅" ], "include": "(?i)(CMCC-IPv4_WeTest)", "fallback": { "enabled": true, "max_delay": "400ms" } },
    { "tag": "移动优选节点 IPv6", "type": "urltest", "tolerance": 100, "outbounds": [ "移动优选节点 CMLiu_IPv6", "移动优选节点 CFYes_IPv6", "移动优选节点 WeTest_IPv6" ] },
    { "tag": "移动优选节点 CMLiu_IPv6", "type": "urltest", "providers": [ "🆓 免费订阅" ], "include": "(?i)(CMCC-IPv6_CMLiu)", "fallback": { "enabled": true, "max_delay": "400ms" } },
    { "tag": "移动优选节点 CFYes_IPv6", "type": "urltest", "providers": [ "🆓 免费订阅" ], "include": "(?i)(CMCC-IPv6_CFYes)", "fallback": { "enabled": true, "max_delay": "400ms" } },
    { "tag": "移动优选节点 WeTest_IPv6", "type": "urltest", "providers": [ "🆓 免费订阅" ], "include": "(?i)(CMCC-IPv6_WeTest)", "fallback": { "enabled": true, "max_delay": "400ms" } },
    { "tag": "联通优选节点 IPv4", "type": "urltest", "tolerance": 100, "outbounds": [ "联通优选节点 CMLiu_IPv4", "联通优选节点 VPS789_IPv4", "联通优选节点 CFYes_IPv4", "联通优选节点 WeTest_IPv4" ] },
    { "tag": "联通优选节点 CMLiu_IPv4", "type": "urltest", "providers": [ "🆓 免费订阅" ], "include": "(?i)(CUCC-IPv4_CMLiu)", "fallback": { "enabled": true, "max_delay": "400ms" } },
    { "tag": "联通优选节点 VPS789_IPv4", "type": "urltest", "providers": [ "🆓 免费订阅" ], "include": "(?i)(CUCC-IPv4_VPS789)", "fallback": { "enabled": true, "max_delay": "400ms" } },
    { "tag": "联通优选节点 CFYes_IPv4", "type": "urltest", "providers": [ "🆓 免费订阅" ], "include": "(?i)(CUCC-IPv4_CFYes)", "fallback": { "enabled": true, "max_delay": "400ms" } },
    { "tag": "联通优选节点 WeTest_IPv4", "type": "urltest", "providers": [ "🆓 免费订阅" ], "include": "(?i)(CUCC-IPv4_WeTest)", "fallback": { "enabled": true, "max_delay": "400ms" } },
    { "tag": "联通优选节点 IPv6", "type": "urltest", "tolerance": 100, "outbounds": [ "联通优选节点 CFYes_IPv6", "联通优选节点 WeTest_IPv6" ] },
    { "tag": "联通优选节点 CFYes_IPv6", "type": "urltest", "providers": [ "🆓 免费订阅" ], "include": "(?i)(CUCC-IPv6_CFYes)", "fallback": { "enabled": true, "max_delay": "400ms" } },
    { "tag": "联通优选节点 WeTest_IPv6", "type": "urltest", "providers": [ "🆓 免费订阅" ], "include": "(?i)(CUCC-IPv6_WeTest)", "fallback": { "enabled": true, "max_delay": "400ms" } },
    { "tag": "电信优选节点 IPv4", "type": "urltest", "tolerance": 100, "outbounds": [ "电信优选节点 CMLiu_IPv4", "电信优选节点 VPS789_IPv4", "电信优选节点 CFYes_IPv4", "电信优选节点 WeTest_IPv4" ] },
    { "tag": "电信优选节点 CMLiu_IPv4", "type": "urltest", "providers": [ "🆓 免费订阅" ], "include": "(?i)(CTCC-IPv4_CMLiu)", "fallback": { "enabled": true, "max_delay": "400ms" } },
    { "tag": "电信优选节点 VPS789_IPv4", "type": "urltest", "providers": [ "🆓 免费订阅" ], "include": "(?i)(CTCC-IPv4_VPS789)", "fallback": { "enabled": true, "max_delay": "400ms" } },
    { "tag": "电信优选节点 CFYes_IPv4", "type": "urltest", "providers": [ "🆓 免费订阅" ], "include": "(?i)(CTCC-IPv4_CFYes)", "fallback": { "enabled": true, "max_delay": "400ms" } },
    { "tag": "电信优选节点 WeTest_IPv4", "type": "urltest", "providers": [ "🆓 免费订阅" ], "include": "(?i)(CTCC-IPv4_WeTest)", "fallback": { "enabled": true, "max_delay": "400ms" } },
    { "tag": "电信优选节点 IPv6", "type": "urltest", "tolerance": 100, "outbounds": [ "电信优选节点 CFYes_IPv6", "电信优选节点 WeTest_IPv6" ] },
    { "tag": "电信优选节点 CFYes_IPv6", "type": "urltest", "providers": [ "🆓 免费订阅" ], "include": "(?i)(CTCC-IPv6_CFYes)", "fallback": { "enabled": true, "max_delay": "400ms" } },
    { "tag": "电信优选节点 WeTest_IPv6", "type": "urltest", "providers": [ "🆓 免费订阅" ], "include": "(?i)(CTCC-IPv6_WeTest)", "fallback": { "enabled": true, "max_delay": "400ms" } },
    { "tag": "CF 优选 IP 节点", "type": "urltest", "tolerance": 100, "outbounds": [ "CF 优选节点 VPS789", "CF 优选节点 CFSpeedTest", "CF 优选节点 IPDB_IPv4", "CF 优选节点 IPDB_IPv6" ] },
    { "tag": "CF 优选节点 VPS789", "type": "urltest", "providers": [ "🆓 免费订阅" ], "include": "(?i)(CF-IPv4_VPS789)", "fallback": { "enabled": true, "max_delay": "400ms" } },
    { "tag": "CF 优选节点 CFSpeedTest", "type": "urltest", "providers": [ "🆓 免费订阅" ], "include": "(?i)(CF-IPv4_CFSpeedTest)", "fallback": { "enabled": true, "max_delay": "400ms" } },
    { "tag": "CF 优选节点 IPDB_IPv4", "type": "urltest", "providers": [ "🆓 免费订阅" ], "include": "(?i)(CF-IPv4_IPDB)", "fallback": { "enabled": true, "max_delay": "400ms" } },
    { "tag": "CF 优选节点 IPDB_IPv6", "type": "urltest", "providers": [ "🆓 免费订阅" ], "include": "(?i)(CF-IPv6_IPDB)", "fallback": { "enabled": true, "max_delay": "400ms" } },
    { "tag": "CF 优选域名节点", "type": "urltest", "tolerance": 100, "providers": [ "🆓 免费订阅" ], "include": "[.+.]" }
  ]
}
```

**推荐设置**
1. 进入 [zashboard 面板](https://github.com/Zephyruso/zashboard) → 代理 → 代理设置 → 管理隐藏代理组，隐藏不必要显示的代理组
2. 进入 zashboard 面板 → 设置 → 图标，设置“自定义图标”，可参考 [icon 文件](https://github.com/DustinWin/ruleset_geodata/releases/tag/icons)