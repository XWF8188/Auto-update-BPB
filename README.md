[![Auto Update _worker.js](https://github.com/XWF8188/Auto-update-B/actions/workflows/update_worker.yml/badge.svg)](https://github.com/XWF8188/Auto-update-B/actions/workflows/update_worker.yml)
# 自动同步 `B-W-P` 项目的最新 `worker.js` 文件。

## 🚀 快速开始（适合工作流）

1. 新建github仓库:把`B-W-P`项目代码同步到仓库。

2. 配置`github Actions`: 在仓库目录下创建`.github/workflows`文件夹，并创建[update_worker.yml](https://github.com/XWF8188/Auto-update-B/blob/main/创建仓库源码.js)文件。
3. 无需其他配置，GitHub 默认的 `GITHUB_TOKEN` 权限即可推送更新。
4. 你可以手动点击 Run workflow，也可以等待每天定时自动检查。

---

## 功能介绍

- 每天自动检查 `bia-pain-bache/B-W-P` 仓库的最新 `Release`
- 自动下载最新版本的 `worker.js`
- 重命名为 `_worker.js`
- 同步更新本地 `version.txt`
- 自动提交并推送到本仓库

## 工作流程

`GitHub Actions` 会每日 01:00（UTC 时间）自动运行：

1. 获取上游仓库的最新 `Release 版本号`
2. 比较本地 `version.txt` 的记录
3. 若版本不同，则自动下载并替换 `_worker.js`
4. 更新 `version.txt`
5. 自动提交并推送到主分支`（main）`

> 若版本一致，则不执行任何操作。

---

## ⚙️ 配置说明

- 无需手动设置 `Token`：默认使用 `GitHub` 提供的 `GITHUB_TOKEN` 进行权限认证。
- 如需修改同步源：编辑 `.github/workflows/update_worker.yml`，修改源仓库地址即可。

---

## 📜 开源协议

本项目使用 `GPL-3.0 许可证` 开源。

您可以自由地使用、复制、修改和分发本项目，前提是附带原始许可证声明。

---

## 📢 特别说明

- 本仓库同步的内容来源于 [B-W-P](https://github.com/bia-pain-bache)。
- 原项目版权归原作者所有，本项目仅用于自动同步更新，不对原内容进行修改。

---

## 🔐 变量的使用

| 变量  | 值 |
| :-------------: | :-------------: |
| **UUID**  | `VLESS UUID`，[在线生成](https://1024tools.com/uuid)  |
| **TR_PASS**  | `Trojan` 密码  |
| **PROXY_IP**  | 来源于网络分享：`proxy.xxxxxxxx.tk`、`edgetunnel.anycast.eu.org`、`ts.hpc.tw`、`cdn.xn--b6gac.eu.org`、`cdn-all.xn--b6gac.eu.org`、`bestproxy.onecf.eu.org` [CMLiussss提供](https://t.me/CMLiussss_channel/84)、 [IPDB 提供](https://ipdb.030101.xyz/bestproxy/)、[nslookup.io提供](https://www.nslookup.io/domains/bpb.yousef.isegaro.com/dns-records/)|
| **SUB_PATH**  | 订阅的`URI`  |
| **FALLBACK**  | 后退域默认修改为`example.com` |
| **DOH_URL**  | 核心DOH |

---

| KV命名空间  | 类型 |
| :-------------: | :-------------: |
| **kv（必须小写）**  | 名称  |
| **自定义名字（随意）**  | 值  |

- `/panel`，部署成功后，在 url 后面增加`/panel`来进行访问面板，访问面板修改的密码将会保存在`kv`对里。

---

## ℹ️ 常用IP优选获取方式
- cleanIP/优选IP：[地址一](https://www.wetest.vip/page/cloudflare/address_v4.html) [地址二](https://ipdb.030101.xyz/bestcf/) [地址三](https://mrxn.net/BESTCFDOMAIN)

---
## 🅿️ 自选IP优选工具的使用
1. win 电脑下载 IP优选工具/[CF优选官方IP[win电脑版].7z](https://github.com/XWF8188/Auto-update-B/blob/main/IP优选工具/CF优选官方IP%5Bwin电脑版%5D.7z)，解压后，退出代理，运行本软件。
2. 下载[CloudflareScanner](https://github.com/bia-pain-bache/Cloudflare-Clean-IP-Scanner/releases/tag/v2.2.5)，解压后，退出代理，运行本软件。
