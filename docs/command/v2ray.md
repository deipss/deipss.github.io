---
layout: default
title: v2ray
parent: Command

---

# 1. 高级路由配套

- 教程 https://toutyrater.github.io/

## 1.1. 黑名单（PAC代理模式）
```json

  {
    "port": "",
    "outboundTag": "proxy",
    "ip": [],
    "domain": [
      "#以下三行是GitHub网站，为了不影响下载速度走代理",
      "github.com",
      "githubassets.com",
      "githubusercontent.com"
    ],
    "protocol": []
  },
  {
    "type": "field",
    "outboundTag": "block",
    "domain": [
      "#阻止CrxMouse鼠标手势收集上网数据",
      "mousegesturesapi.com",
      "#下一行广告管理平台网址，在ProductivityTab（原iChrome）浏览器插件页面显示",
      "cf-se.com"
    ]
  },
  {
    "type": "field",
    "port": "",
    "outboundTag": "direct",
    "ip": [
      "geoip:private",
      "geoip:cn"
    ],
    "domain": [
      "bitwarden.com",
      "bitwarden.net",
      "gravatar.com",
      "gstatic.com",
      "baiyunju.cc",
      "letsencrypt.org",
      "adblockplus.org",
      "safesugar.net",
      "#下两行谷歌广告",
      "googleads.g.doubleclick.net",
      "adservice.google.com",
      "#【以下全部是geo预定义域名列表】",
      "#下一行包含所有私有域名",
      "geosite:private",
      "#下一行包含常见大陆站点域名和CNNIC管理的大陆域名，即geolocation-cn和tld-cn的合集",
      "geosite:cn",
      "#下一行包含所有Adobe旗下域名",
      "geosite:adobe",
      "#下一行包含所有Adobe正版激活域名",
      "geosite:adobe-activation",
      "#下一行包含所有微软旗下域名",
      "geosite:microsoft",
      "#下一行包含微软msn相关域名少数与上一行微软列表重复",
      "geosite:msn",
      "#下一行包含所有苹果旗下域名",
      "geosite:apple",
      "#下一行包含所有广告平台、提供商域名",
      "geosite:category-ads-all",
      "#下一行包含可直连访问谷歌网址，需要替换为加强版GEO文件，如已手动更新为加强版GEO文件，删除此行前面的#号使其生效",
      "#geosite:google-cn",
      "#下一行包含可直连访问苹果网址，需要替换为加强版GEO文件，如已手动更新为加强版GEO文件，删除此行前面的#号使其生效",
      "#geosite:apple-cn"
    ],
    "protocol": []
  },
  {
    "type": "field",
    "port": "0-65535",
    "outboundTag": "proxy"
  }
]
```



## 1.2. 全局代理

```json
{
  // DNS 配置部分，用于定义域名解析规则和 DNS 服务器
  "dns": {
    // 定义域名和 IP 的映射关系
    "hosts": {
      // 将 geosite:category-ads-all 域名解析到本地回环地址 127.0.0.1，用于屏蔽广告相关域名
      "geosite:category-ads-all": "127.0.0.1",
      // 将 googleapis.cn 域名解析为 googleapis.com
      "domain:googleapis.cn": "googleapis.com",
      // 为 dns.alidns.com 域名指定多个 IP 地址，包括 IPv4 和 IPv6 地址
      "dns.alidns.com": [
        "223.5.5.5",
        "223.6.6.6",
        "2400:3200::1",
        "2400:3200:baba::1"
      ],
      // 为 one.one.one.one 域名指定多个 IP 地址，包括 IPv4 和 IPv6 地址
      "one.one.one.one": [
        "1.1.1.1",
        "1.0.0.1",
        "2606:4700:4700::1111",
        "2606:4700:4700::1001"
      ],
      // 为 dot.pub 域名指定多个 IPv4 地址
      "dot.pub": [
        "1.12.12.12",
        "120.53.53.53"
      ],
      // 为 dns.google 域名指定多个 IP 地址，包括 IPv4 和 IPv6 地址
      "dns.google": [
        "8.8.8.8",
        "8.8.4.4",
        "2001:4860:4860::8888",
        "2001:4860:4860::8844"
      ],
      // 为 dns.quad9.net 域名指定多个 IP 地址，包括 IPv4 和 IPv6 地址
      "dns.quad9.net": [
        "9.9.9.9",
        "149.112.112.112",
        "2620:fe::fe",
        "2620:fe::9"
      ],
      // 为 common.dot.dns.yandex.net 域名指定多个 IP 地址，包括 IPv4 和 IPv6 地址
      "common.dot.dns.yandex.net": [
        "77.88.8.8",
        "77.88.8.1",
        "2a02:6b8::feed:0ff",
        "2a02:6b8:0:1::feed:0ff"
      ]
    },
    // 定义使用的 DNS 服务器
    "servers": [
      // 使用 1.1.1.1 作为默认 DNS 服务器
      "1.1.1.1",
      {
        // 使用 1.1.1.1 作为 DNS 服务器，仅对指定的域名生效
        "address": "1.1.1.1",
        // 对 googleapis.cn 和 gstatic.com 域名使用该 DNS 服务器进行解析
        "domains": [
          "domain:googleapis.cn",
          "domain:gstatic.com"
        ]
      },
      {
        // 使用 223.5.5.5 作为 DNS 服务器
        "address": "223.5.5.5",
        // 对指定的域名使用该 DNS 服务器进行解析
        "domains": [
          "domain:alidns.com",
          "domain:doh.pub",
          "domain:dot.pub",
          "domain:360.cn",
          "domain:onedns.net",
          "geosite:cn"
        ],
        // 期望解析出的 IP 地址范围为中国 IP
        "expectIPs": [
          "geoip:cn"
        ],
        // 当解析失败时，不尝试使用其他 DNS 服务器进行解析
        "skipFallback": true
      }
    ]
  },
  // 入站连接配置，定义接收外部连接的规则
  "inbounds": [
    {
      // 监听本地回环地址 127.0.0.1
      "listen": "127.0.0.1",
      // 监听端口 10808
      "port": 10808,
      // 使用 SOCKS 协议接收连接
      "protocol": "socks",
      // SOCKS 协议的设置
      "settings": {
        // 不进行身份验证
        "auth": "noauth",
        // 允许 UDP 连接
        "udp": true,
        // 用户级别为 8
        "userLevel": 8
      },
      // 嗅探配置，用于识别连接的目标协议
      "sniffing": {
        // 当识别到 HTTP 或 TLS 协议时，覆盖目标地址
        "destOverride": [
          "http",
          "tls"
        ],
        // 启用嗅探功能
        "enabled": true,
        // 仅用于路由，不修改连接
        "routeOnly": false
      },
      // 该入站连接的标签，用于路由规则匹配
      "tag": "socks"
    },
    {
      // 使用 HTTP 协议接收连接
      "protocol": "http",
      // 监听本地回环地址 127.0.0.1
      "listen": "127.0.0.1",
      // 嗅探配置，用于识别连接的目标协议
      "sniffing": {
        // 启用嗅探功能
        "enabled": true,
        // 当识别到 TLS 或 HTTP 协议时，覆盖目标地址
        "destOverride": [
          "tls",
          "http"
        ]
      },
      // 监听端口 10809
      "port": 10809,
      // HTTP 协议的设置
      "settings": {
        // 连接超时时间为 360 秒
        "timeout": 360
      }
    }
  ],
  // 日志配置，定义日志的级别和存储位置
  "log": {
    // 日志级别为 debug，记录详细的调试信息
    "loglevel": "debug",
    // 访问日志的存储路径
    "access": "/Users/deipss/logs/v2ray-core.log",
    // 错误日志的存储路径
    "error": "/Users/deipss/logs/v2ray-core.log"
  },
  // 出站连接配置，定义将流量发送到外部的规则
  "outbounds": [
    {
      // MUX 配置，用于多路复用连接
      "mux": {
        // 并发连接数为 -1，表示不限制
        "concurrency": -1,
        // 禁用 MUX 功能
        "enabled": false,
        // UDP 并发连接数为 8
        "xudpConcurrency": 8,
        // UDP 代理 UDP443 端口的配置，此处为空
        "xudpProxyUDP443": ""
      },
      // 使用 VLESS 协议进行出站连接
      "protocol": "vless",
      // VLESS 协议的设置
      "settings": {
        // 目标服务器的配置
        "vnext": [
          {
            // 目标服务器的地址
            "address": "vmess.yahyav2rayssr.top",
            // 目标服务器的端口
            "port": 443,
            // 用户配置
            "users": [
              {
                // 不使用加密
                "encryption": "none",
                // 流量类型为空
                "flow": "",
                // 用户 ID
                "id": "185eff15-75af-4981-f1bd-8457784f4cba",
                // 用户级别为 8
                "level": 8
              }
            ]
          }
        ]
      },
      // 传输层设置
      "streamSettings": {
        // 使用 TCP 作为传输协议
        "network": "tcp",
        // 使用 TLS 进行安全传输
        "security": "tls",
        // TCP 协议的设置
        "tcpSettings": {
          // 不使用自定义的 TCP 头部
          "header": {
            "type": "none"
          }
        },
        // TLS 协议的设置
        "tlsSettings": {
          // 不允许不安全的连接
          "allowInsecure": false,
          // 使用 Chrome 的指纹
          "fingerprint": "chrome",
          // 目标服务器的名称
          "serverName": "vmess.yahyav2rayssr.top",
          // 不显示详细信息
          "show": false
        }
      },
      // 该出站连接的标签，用于路由规则匹配
      "tag": "proxy"
    },
    {
      // 使用 freedom 协议进行直接连接
      "protocol": "freedom",
      // freedom 协议的设置
      "settings": {
        // 域名解析策略为使用 IP 地址
        "domainStrategy": "UseIP"
      },
      // 该出站连接的标签，用于路由规则匹配
      "tag": "direct"
    },
    {
      // 使用 blackhole 协议进行阻止连接
      "protocol": "blackhole",
      // blackhole 协议的设置
      "settings": {
        // 返回 HTTP 响应
        "response": {
          "type": "http"
        }
      },
      // 该出站连接的标签，用于路由规则匹配
      "tag": "block"
    }
  ],
  // 配置文件的备注信息
  "remarks": "vmess",
  // 路由配置，定义流量的转发规则
  "routing": {
    // 域名解析策略为如果无法匹配域名，则使用 IP 地址进行匹配
    "domainStrategy": "IPIfNonMatch",
    // 路由规则列表
    "rules": [
      {
        // 匹配 IP 地址为 1.1.1.1 且端口为 53 的流量
        "ip": [
          "1.1.1.1"
        ],
        // 将匹配的流量转发到 proxy 出站连接
        "outboundTag": "proxy",
        "port": "53",
        // 规则类型为字段匹配
        "type": "field"
      },
      {
        // 匹配 IP 地址为 223.5.5.5 且端口为 53 的流量
        "ip": [
          "223.5.5.5"
        ],
        // 将匹配的流量转发到 direct 出站连接
        "outboundTag": "direct",
        "port": "53",
        // 规则类型为字段匹配
        "type": "field"
      },
      {
        // 匹配域名 googleapis.cn 和 gstatic.com 的流量
        "domain": [
          "domain:googleapis.cn",
          "domain:gstatic.com"
        ],
        // 将匹配的流量转发到 proxy 出站连接
        "outboundTag": "proxy",
        // 规则类型为字段匹配
        "type": "field"
      },
      {
        // 匹配 UDP 协议且端口为 443 的流量
        "network": "udp",
        // 将匹配的流量转发到 block 出站连接，即阻止该流量
        "outboundTag": "block",
        "port": "443",
        // 规则类型为字段匹配
        "type": "field"
      },
      {
        // 匹配域名 geosite:category-ads-all 的流量
        "domain": [
          "geosite:category-ads-all"
        ],
        // 将匹配的流量转发到 block 出站连接，即阻止该流量
        "outboundTag": "block",
        // 规则类型为字段匹配
        "type": "field"
      },
      {
        // 匹配私有 IP 地址的流量
        "ip": [
          "geoip:private"
        ],
        // 将匹配的流量转发到 direct 出站连接
        "outboundTag": "direct",
        // 规则类型为字段匹配
        "type": "field"
      },
      {
        // 匹配私有域名的流量
        "domain": [
          "geosite:private"
        ],
        // 将匹配的流量转发到 direct 出站连接
        "outboundTag": "direct",
        // 规则类型为字段匹配
        "type": "field"
      },
      {
        // 匹配指定的多个 IP 地址的流量
        "ip": [
          "223.5.5.5",
          "223.6.6.6",
          "2400:3200::1",
          "2400:3200:baba::1",
          "119.29.29.29",
          "1.12.12.12",
          "120.53.53.53",
          "2402:4e00::",
          "2402:4e00:1::",
          "180.76.76.76",
          "2400:da00::6666",
          "114.114.114.114",
          "114.114.115.115",
          "114.114.114.119",
          "114.114.115.119",
          "114.114.114.110",
          "114.114.115.110",
          "180.184.1.1",
          "180.184.2.2",
          "101.226.4.6",
          "218.30.118.6",
          "123.125.81.6",
          "140.207.198.6",
          "1.2.4.8",
          "210.2.4.8",
          "52.80.66.66",
          "117.50.22.22",
          "2400:7fc0:849e:200::4",
          "2404:c2c0:85d8:901::4",
          "117.50.10.10",
          "52.80.52.52",
          "2400:7fc0:849e:200::8",
          "2404:c2c0:85d8:901::8",
          "117.50.60.30",
          "52.80.60.30"
        ],
        // 将匹配的流量转发到 direct 出站连接
        "outboundTag": "direct",
        // 规则类型为字段匹配
        "type": "field"
      },
      {
        // 匹配指定的多个域名的流量
        "domain": [
          "domain:alidns.com",
          "domain:doh.pub",
          "domain:dot.pub",
          "domain:360.cn",
          "domain:onedns.net"
        ],
        // 将匹配的流量转发到 direct 出站连接
        "outboundTag": "direct",
        // 规则类型为字段匹配
        "type": "field"
      },
      {
        // 匹配中国 IP 地址的流量
        "ip": [
          "geoip:cn"
        ],
        // 将匹配的流量转发到 direct 出站连接
        "outboundTag": "direct",
        // 规则类型为字段匹配
        "type": "field"
      },
      {
        // 匹配中国域名的流量
        "domain": [
          "geosite:cn"
        ],
        // 将匹配的流量转发到 direct 出站连接
        "outboundTag": "direct",
        // 规则类型为字段匹配
        "type": "field"
      },
      {
        // 匹配所有端口的流量
        "outboundTag": "proxy",
        "port": "0-65535",
        // 规则类型为字段匹配
        "type": "field"
      }
    ]
  }
}
```