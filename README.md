# <h1 align="center"> [![Auto Update _worker.js](https://github.com/XWF8188/Auto-update-B/actions/workflows/update_worker.yml/badge.svg)](https://github.com/XWF8188/Auto-update-B/actions/workflows/update_worker.yml)

# ♻️ 自动同步 `B-W-P` 项目的最新 `worker.js` 文件。

## 🧩 快速开始（适合工作流）

1. 新建github仓库:把`B-W-P`项目代码同步到仓库。

2. 配置`github Actions`: 在仓库目录下创建`.github/workflows`文件夹，并创建[update_worker.yml](https://github.com/XWF8188/Auto-update-B/blob/main/创建仓库源码.js)文件。
3. 无需其他配置，GitHub 默认的 `GITHUB_TOKEN` 权限即可推送更新。
4. 你可以手动点击 `Run workflow`，也可以等待每天定时自动检查。

---

## 📖 功能介绍

- 每天自动检查 `bia-pain-bache/B-W-P` 仓库的最新 `Release`
- 支持选择更新正式版或预发布版本：通过手动触发或 `update_type.txt` 文件配置 `1`是正式版, `0`是测试版本。
- 自动下载最新版本的 `worker.js`
- 重命名为 `\_worker.js`
- 同步更新本地 `version.txt`
- 自动提交并推送到本仓库
- 如果 `update_type.txt` 文件不存在，将自动创建并默认设置为更新正式版。
- 更新成功后，自动复用或创建 GitHub Issue 进行通知。

---

## 🖇 工作流程

> `GitHub Actions` 会每日 00:00（UTC 时间）自动运行：

1. 检查 `update_type.txt` 文件：如果文件不存在，会自动创建并写入 `1`（表示正式版）。
2. 根据 `update_type.txt` 或手动输入确定更新类型（正式版或预发布版）。
3. 获取上游仓库的最新 Release 版本号（根据所选类型）。
4. 比较本地 `version.txt` 的记录。
5. 若版本不同，则自动下载并替换 `\_worker.js`。
6. 更新 `version.txt`。
7. 自动提交并推送到主分支（main）。
8. 如果 `update_type.txt` 文件是自动创建的，也会一并提交到仓库。
9. 如果更新成功并提交了更改，工作流会首先查找一个名为 `_worker.js 自动更新通知` 且带有 `auto-update-status-issue` 标签的现有 Issue。如果找到，则在该 Issue 下添加一条评论，包含更新时间、版本类型和版本号；如果未找到，则创建一个新的 Issue 并添加该标签。

> 若版本一致，则不执行任何操作。

---

## ⚙️ 配置说明

- 无需手动设置 `Token`：默认使用 `GitHub` 提供的 `GITHUB_TOKEN` 进行权限认证。
- 如需修改同步源：编辑 `.github/workflows/update_worker.yml`，修改源仓库地址即可。
- **更新类型配置（`update_type.txt`）：**
  - 在仓库根目录下创建或修改 `update_type.txt` 文件。
  - 文件内容为 `1`：表示定时任务将更新到**最新正式发布版本**。
  - 文件内容为 `0`：表示定时任务将更新到**最新预发布版本**。
  - 如果 `update_type.txt` 文件不存在，工作流会自动创建它并默认设置为 `1`（正式版）。
  - 手动触发时，您可以通过 GitHub Actions 界面选择更新类型，此选择将覆盖 `update_type.txt` 的设置。
- **更新成功通知：**
  - 工作流在成功更新并提交代码后，会尝试复用一个特定的 GitHub Issue 进行通知。
  - 该 Issue 的标题统一为 `_worker.js 自动更新通知`，并带有 `auto-update-status-issue` 标签。
  - 如果该 Issue 已存在，新的更新信息将作为评论添加到该 Issue 中，这样可以保持通知集中在一个地方。
  - 如果该 Issue 不存在，工作流会创建一个新的 Issue。
  - 您可以通过关注该仓库的 Issue 动态来接收通知。

---

## ⚖️ 开源协议

- 本项目使用 `MIT License` 开源。

- 您可以自由地使用、复制、修改和分发本项目，前提是附带原始许可证声明。

---

## 📣 特别说明

- 本仓库同步的内容来源于 [B-W-P](https://github.com/bia-pain-bache)。
- 原项目版权归原作者所有，本项目仅用于自动同步更新，不对原内容进行修改。

---

## 🔐 变量的使用(v3.2.0 及以上版本)

|变量|值|选填|
|:---:|:---:|:---:|
|**UUID**|`VLESS UUID`，[UUID生成](https://1024tools.com/uuid)，用于生成 VLESS 节点配置|:heavy_check_mark:|
|**TR_PASS**|`Trojan Password`，[密码生成](https://password.github.net.cn/) ，用于生成 Trojan 节点配置|:heavy_check_mark:|
|**PROXY_IP**| 来源于网络分享：`ts.hpc.tw`[🔍Check](https://ip.weibreeze.us.kg/ts.hpc.tw?)、`cdn.xn--b6gac.eu.org`[🔍Check](https://ip.weibreeze.us.kg/cdn.xn--b6gac.eu.org?)、`cdn-all.xn--b6gac.eu.org`[🔍Check](https://ip.weibreeze.us.kg/cdn-all.xn--b6gac.eu.org?)、`bestproxy.onecf.eu.org`[🔍Check](https://ip.weibreeze.us.kg/bestproxy.onecf.eu.org?)、 [CMLiussss提供](https://t.me/CMLiussss_channel/84)、 [IPDB 提供](https://ipdb.030101.xyz/bestproxy/)、[nslookup.io提供](https://www.nslookup.io/domains/bpb.yousef.isegaro.com/dns-records/)、 [cdn-xx-b6gac.acu.org](https://www.nslookup.io/domains/cdn.xn--b6gac.eu.org/dns-records/)|:heavy_check_mark:|
|**SUB_PATH**|订阅的`URI`|:x:|
|**FALLBACK**|后退域默认修改为`example.com` |:x:|
|**DOH_URL**|核心`DOH`|:x:|

---

|类型|KV命名空间|必填| 
|:---:|:---:|:---:|
|**名称**|`kv`（必须小写)|:heavy_check_mark:|
|**值**|自定义名字（随意）|:heavy_check_mark:|

1. 本存储库`main`主线默认为自动升级为最新版本。
2. 代理IP/域名(`Proxy IP/Domains`)的变量区别：
   - v3.2.0 及以上版本为`PROXY_IP`。
   - v3.1.4 及以下版本为`PROXYIP`。
3. `/panel`，部署成功后，在 url 后面增加`/panel`来进行访问面板，访问面板修改的密码将会保存在`kv`之中。
4. 面板设置要👀注意以下区别：
   - v3.2.0 及以上版本，`优选IP/域名（Clean IP/Domains）`用回车键`ENTER`分隔。
   - v3.1.4 及以下版本，`优选IP/域名（Clean IP/Domains）`用英文逗号`,`分隔。

---

## 🅿️ 常用IP优选获取方式
- Clean IP/优选 IP：[地址一](https://www.wetest.vip/page/cloudflare/address_v4.html) [地址二](https://ipdb.030101.xyz/bestcf/) [地址三](https://mrxn.net/BESTCFDOMAIN)

---
## 🅿️ 自选IP优选工具使用
1. 跳转链接下载: [IP优选[win电脑版].7z](https://github.com/XWF8188/Auto-update-B/blob/main/IP优选工具/CF优选官方IP%5Bwin电脑版%5D.7z)，解压后，退出代理，运行本软件。
2. 跳转链接下载: [IP优选[CloudflareScanner]](https://github.com/bia-pain-bache/Cloudflare-Clean-IP-Scanner/releases/tag/v2.2.5)，解压后，退出代理，运行本软件。
