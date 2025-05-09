[![Auto Update Worker](https://github.com/XWF8188/Auto_update-Interface/actions/workflows/Confusion.yml/badge.svg)](https://github.com/XWF8188/Auto_update-Interface/actions/workflows/Confusion.yml)

---

# 自动同步 `B-W-P` 项目的最新 `worker.js` 文件。

## 🚀 快速开始（适合工作流）

1. 新建github仓库:把`B-W-P`项目代码同步到仓库。

2. 配置`github Actions`: 在仓库目录下创建`.github/workflows`文件夹，并创建`Confusion.yml`文件。
3. 无需其他配置，GitHub 默认的 `GITHUB_TOKEN` 权限即可推送更新。

可随官方自动更新的工作流，可在CF上部署：
```
name: Auto Update Worker

on:
  push:
    branches:
      - main
  schedule:
    - cron: "0 1 * * *" # 每天凌晨1点运行
  workflow_dispatch:
    inputs:
      force_update:
        description: '是否强制更新（忽略版本检查）'
        required: false
        default: 'false'

permissions:
  contents: write

jobs:
  update:
    runs-on: ubuntu-latest
    steps:
      - name: 检出仓库
        uses: actions/checkout@v4

      - name: 设置环境
        run: |
          echo "REPO_URL=https://api.github.com/repos/bia-pain-bache/BPB-Worker-Panel/releases" >> $GITHUB_ENV
          echo "TARGET_FILE=worker.zip" >> $GITHUB_ENV

      - name: 检查并更新 Worker
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }} # 使用 GitHub Token 认证
        run: |
          # 日志函数
          log() { echo "[$(date +'%Y-%m-%d %H:%M:%S')] $1"; }

          log "开始检查更新..."

          # 获取本地版本
          LOCAL_VERSION=$(cat version.txt 2>/dev/null || echo "")
          log "本地版本: ${LOCAL_VERSION:-无}"

          # 获取最新 Release
          log "获取最新 Release 信息..."
          RESPONSE=$(curl -s --retry 3 -H "Authorization: token $GITHUB_TOKEN" -H "Accept: application/vnd.github.v3+json" "$REPO_URL")
          if [ $? -ne 0 ]; then
            log "ERROR: 无法访问 GitHub API"
            exit 1
          fi

          TAG_NAME=$(echo "$RESPONSE" | jq -r '.[0].tag_name')
          DOWNLOAD_URL=$(echo "$RESPONSE" | jq -r '.[0].assets[] | select(.name == "'"$TARGET_FILE"'") | .browser_download_url')

          if [ -z "$DOWNLOAD_URL" ] || [ "$DOWNLOAD_URL" == "null" ]; then
            log "ERROR: 未找到 $TARGET_FILE"
            exit 1
          fi
          log "最新版本: $TAG_NAME"

          # 判断是否需要更新
          FORCE_UPDATE=${{ github.event.inputs.force_update || 'false' }}
          if [ "$LOCAL_VERSION" = "$TAG_NAME" ] && [ "$FORCE_UPDATE" != "true" ]; then
            log "已是最新版本，无需更新"
            exit 0
          fi

          # 下载并更新
          log "下载 $TARGET_FILE..."
          wget -q -O "$TARGET_FILE" "$DOWNLOAD_URL"
          log "解压 $TARGET_FILE..."
          unzip -o "$TARGET_FILE"
          rm "$TARGET_FILE"
          echo "$TAG_NAME" > version.txt
          log "更新完成，新版本: $TAG_NAME"

      - name: 提交更改
        if: success() # 仅在更新成功时提交
        uses: stefanzweifel/git-auto-commit-action@v5
        with:
          commit_message: "🔄 自动同步 Worker 版本: ${{ steps.check_update.outputs.tag_name || '未知' }}"
          commit_author: "github-actions[bot] <github-actions[bot]@users.noreply.github.com>"
```

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

## 📂 目录结构

/
├── _worker.js         
├── version.txt        
├── LICENSE                    
├── README.md          
└── .github/
    └── workflows/
        └── Confusion.yml

---

## ⚙️ 配置说明

- 无需手动设置 `Token`：默认使用 `GitHub` 提供的 `GITHUB_TOKEN` 进行权限认证。
- 如需修改同步源：编辑 `.github/workflows/Confusion.yml`，修改源仓库地址即可。

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
| **UUID**  | `UUID`，[在线生成](https://1024tools.com/uuid)  |
| **TR_PASS**  | 默认要修改的密码  |
| **PROXY_IP**  | 来源于网络分享：`proxy.xxxxxxxx.tk`、`edgetunnel.anycast.eu.org`、`ts.hpc.tw`、`cdn.xn--b6gac.eu.org`、`cdn-all.xn--b6gac.eu.org`、`bestproxy.onecf.eu.org` [CMLiussss提供](https://t.me/CMLiussss_channel/84)、 [IPDB 提供](https://ipdb.030101.xyz/bestproxy/)、[nslookup.io提供](https://www.nslookup.io/domains/bpb.yousef.isegaro.com/dns-records/)|
| **SUB_PATH**  | 订阅的URI  |
| **FALLBACK**  | 默认修改为`example.com` |
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
1. win 电脑下载 IP优选工具/[CF优选官方IP[win电脑版].7z](https://github.com/XWF8188/Auto_update-Interface/blob/main/IP优选工具/CF优选官方IP%5Bwin电脑版%5D.7z)，解压后，退出代理，运行本软件。
2. 下载[CloudflareScanner](https://github.com/bia-pain-bache/Cloudflare-Clean-IP-Scanner/releases/tag/v2.2.5)，解压后，退出代理，运行本软件。
