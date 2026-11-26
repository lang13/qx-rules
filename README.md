# QX Rules

自用的 Quantumult X 规则集。

## 番茄小说 / 红果短剧 去广告

规则来源：[liidolidol/fanqie-hongguo-adblock](https://github.com/liidolidol/fanqie-hongguo-adblock) v1.6.0（2026-11-22），已由 Surge 语法转写为 Quantumult X 语法。

| 文件 | 用途 |
|---|---|
| `fanqie.list` | 分流列表（78 条域名 / IP 级拦截，不需要证书） |
| `fanqie.rewrite.conf` | 重写规则（12 条 URL 路径级拦截，必须配合 MITM） |
| `fanqie-qx.conf` | 三段合一的总览版，供手动粘贴进主配置文件，**不能直接作为引用资源** |

### 用法一：引用（推荐）

分流：

```
https://raw.githubusercontent.com/lang13/qx-rules/main/fanqie.list
```

重写：

```
https://raw.githubusercontent.com/lang13/qx-rules/main/fanqie.rewrite.conf
```

在 QX 的「分流」「重写」页面右上角分别新建引用，粘贴上面的地址即可。

### 用法二：并入主配置

把 `fanqie-qx.conf` 里的三段内容分别粘贴到主配置文件的 `[filter_local]`、`[rewrite_local]`、`[mitm]` 段落中。

注意：带方括号的段头只能出现在主配置文件里，**被引用的远程资源文件不能带段头**，否则 QX 会报 `Invalid Line [rewrite_local]`。

### MITM

重写规则依赖以下域名解密，需在 QX 的「MitM」页手动添加，并确认证书已信任：

```
*.pangolin-sdk-toutiao.com, *.pstatp.com, *.pglstatp-toutiao.com, *.snssdk.com, *.byteimg.com, *.ixigua.com, *.fqnovelvod.com, *.qznovelvod.com, gurd.snssdk.com
```

### 注意事项

- `host-suffix, bytedance.com, reject` 是全域拦截，会影响抖音、头条等其他字节系 App。只用番茄小说的话建议注释掉这一行。
- 末尾的直连兜底（`fqnovel.com` / `snssdk.com` / `zijieapi.com` / `ixigua.com` / `pstatp.com`）不要删，删了正文、封面图、听书会被一起拦掉。
- 修改后需重连 QX 才生效。

## 免责声明

规则仅用于个人网络调试与学习研究。使用前请自行评估风险，遵守相关服务条款。
