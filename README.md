# egern-ippure-proxy

让 IP 纯净度检测站 ippure.com 及其检测接口走代理的规则集。

## 为什么不止一个域名

ippure.com 页面除了自己的 `api.ippure.com`，还会并发请求第三方 IP 查询接口来交叉校验：
`ipinfo.io`（含 v6）、`myip.ipip.net`、`ipapi.co`、`cf.999831.xyz`、`api.123169.xyz`，
以及 FingerprintJS 的 `fpjs.dev` / `m1.openfpcdn.io`。只代理主域，第三方那几条会带出真实出口 IP，测出来的纯净度是掺假的。

## 文件

- `ippure.list` — Surge / Egern / mihomo 通用 classical 格式，无策略列
- `ippure.yaml` — Egern 原生规则集（domain_suffix_set）
- `ippure-singbox.json` — sing-box rule-set（source 格式）

## 用法

Egern（规则集里引用）：
```yaml
rule_sets:
  - name: ippure
    url: https://cdn.jsdelivr.net/gh/wukris1106/egern-ippure-proxy@main/ippure.yaml
rules:
  - rule_set: ippure
    policy: Proxy
```

Surge / mihomo：
```
RULE-SET,https://cdn.jsdelivr.net/gh/wukris1106/egern-ippure-proxy@main/ippure.list,PROXY
```

sing-box：
```json
"route": {
  "rule_set": [
    {"type": "remote", "tag": "ippure", "url": "https://cdn.jsdelivr.net/gh/wukris1106/egern-ippure-proxy@main/ippure-singbox.json", "format": "source"}
  ],
  "rules": [{"rule_set": ["ippure"], "outbound": "proxy"}]
}
```

## 直连即可用的两条链接

- https://raw.githubusercontent.com/wukris1106/egern-ippure-proxy/main/ippure.list
- https://cdn.jsdelivr.net/gh/wukris1106/egern-ippure-proxy@main/ippure.list
