# QX Rules

自用的 Quantumult X 规则集。

## 番茄小说 / 红果短剧 去广告

规则来源：[liidolidol/fanqie-hongguo-adblock](https://github.com/liidolidol/fanqie-hongguo-adblock) v1.6.0（2026-11-22），已由 Surge 语法转写为 Quantumult X 语法。

| 文件 | 用途 |
|---|---|
| `fanqie.list` | 分流列表（78 条域名 / IP 级拦截，不需要证书） |
| `fanqie.rewrite.conf` | 重写规则（16 条 URL 路径级拦截，必须配合 MITM） |
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
*.pangolin-sdk-toutiao.com, *.pstatp.com, *.pglstatp-toutiao.com, *.snssdk.com, *.zijieapi.com, *.byteimg.com, *.ixigua.com, *.fqnovelvod.com, *.qznovelvod.com, gurd.snssdk.com
```

`*.zijieapi.com` 不能漏：阅读中途弹出的 6 秒插页广告走 `ad.zijieapi.com/api/ad/v1/banner/`，该域名不在 MITM 名单里时，对应的路径重写规则一条都不会触发。

### 关于阅读中途的 6 秒插页广告

番茄 iOS 的阅读页广告不是普通广告位，而是「阅读流 SDK 在翻页时按概率把整页广告卡片插进正文分页序列」（详见 [aixi134/fanqie-loon-adblock](https://github.com/aixi134/fanqie-loon-adblock) 的 IPA 逆向分析）。因此：

- 广告物料接口是 `ad.zijieapi.com/api/ad/v1/banner|patch|inspire|recall_ads`，本仓库按路径拦截。
- **不要整段屏蔽 `ad.zijieapi.com`**，否则「看视频免广告」「听书领时长」这类官方激励任务会一起失效。
- 拦掉物料后，SDK 可能降级到 `defaultAd` 占位页——屏幕上仍可能出现一页占位内容，这是服务端兜底逻辑，网络层无法消除。
- 想彻底干净只有两条路：改运行时 hook（需越狱）、或走官方免广告（会员 / 本地导入书）。

### 注意事项

- `host-suffix, bytedance.com, reject` 是全域拦截，会影响抖音、头条等其他字节系 App。只用番茄小说的话建议注释掉这一行。
- 末尾的直连兜底（`fqnovel.com` / `snssdk.com` / `zijieapi.com` / `ixigua.com` / `pstatp.com`）不要删，删了正文、封面图、听书会被一起拦掉。
- 修改后需重连 QX 才生效。

## 免责声明

规则仅用于个人网络调试与学习研究。使用前请自行评估风险，遵守相关服务条款。
