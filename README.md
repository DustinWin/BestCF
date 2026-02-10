# Cloudflare 优选域名和 IP
## 数据源
1. 每 12 小时（UTC+0）自动构建
2. **bestcf-domain.txt** 源采用 [CMLiussss（优选域名）](https://cf.090227.xyz)、[VPS789（优选 CNAME 域名）](https://vps789.com/cfip/?remarks=domain) 和[微测网（CloudFlare 优选 Cname 域名）](https://www.wetest.vip/page/cloudflare/cname.html)组合
3. **cmcc-ip.txt** 源采用 [CMLiussss（移动优选 IP）](https://cf.090227.xyz/cmcc)（IPv4 & IPv6）、[VPS789（移动优选 IP）](https://vps789.com/cfip/)（IPv4）、[CloudFlareYes（移动优选 IP）](https://stock.hostmonit.com/CloudFlareYes)（IPv4 & IPv6）和[微测网（移动优选 IP）](https://www.wetest.vip/page/cloudflare/address_v4.html)（IPv4 & IPv6）组合
4. **cucc-ip.txt** 源采用 [CMLiussss（联通优选 IP）](https://cf.090227.xyz/cu)（IPv4）、[VPS789（联通优选 IP）](https://vps789.com/cfip/)（IPv4）、[CloudFlareYes（联通优选 IP）](https://stock.hostmonit.com/CloudFlareYes)（IPv4 & IPv6）和[微测网（联通优选 IP）](https://www.wetest.vip/page/cloudflare/address_v4.html)（IPv4 & IPv6）组合
5. **ctcc-ip.txt** 源采用 [CMLiussss（电信优选 IP）](https://cf.090227.xyz/ct)（IPv4）、[VPS789（电信优选 IP）](https://vps789.com/cfip/)（IPv4）、[CloudFlareYes（电信优选 IP）](https://stock.hostmonit.com/CloudFlareYes)（IPv4 & IPv6）和[微测网（电信优选 IP）](https://www.wetest.vip/page/cloudflare/address_v4.html)（IPv4 & IPv6）组合
6. **bestcf-ip.txt** 源采用 [VPS789（CF 优选 IP）](https://vps789.com/cfip/)（IPv4）、[CloudflareSpeedTest（Cloudflare 优选 IP 测速数据）](https://ip.164746.xyz)（IPv4）、[IPDB（CF 优选官方 IP 服务）](https://ipdb.030101.xyz/bestcfv4/)（IPv4 & IPv6）组合
7. **proxy-ip.txt**（反代 IP）源采用 [IPDB（CF 优选官方反代 IP 服务）](https://ipdb.030101.xyz/bestproxy/)（IPv4）


# 🚀 Cloudflare 优选 IP 自动采集系统 (BestCF)

这是一个专为 Windows 自建运行器（Self-hosted Runner）优化的自动化工具。它通过集成多种数据源，利用 Bash 脚本与 Python 逻辑，实现 Cloudflare 优选 IP 及域名的全天候自动抓取、过滤与发布。

---

## 🌟 项目亮点

* **双环境联动**：结合 Bash 的文本处理能力（`awk`, `sed`, `grep`）与 Python 的逻辑处理能力。
* **深度适配 Windows**：解决了 Windows 系统中常见的 `sort` 冲突、SSL 证书撤销检查、UTF-8 编码乱码等痛点。
* **全自动发布**：数据每 12 小时更新一次，自动推送到 `bestcf` 分支，并同步创建 GitHub Release。
* **CDN 预热**：内置 jsDelivr 缓存刷新逻辑，确保全球边缘节点获取的是最新数据。

---

## 🛠️ 本地部署教程 (Windows)

由于本项目使用了 `runs-on: self-hosted`，您需要将自己的 Windows 电脑或服务器配置为 GitHub 的执行节点。

### 1. 准备工作

确保您的本地环境已安装以下软件：

* **Git for Windows**：提供脚本所需的 `bash`、`curl`、`awk` 等工具。
* **Python 3.x**：用于运行核心过滤脚本 `filter.py`。
* **GitHub Actions Runner**：这是连接本地与 GitHub 的桥梁。

### 2. 配置 Self-hosted Runner

1. 进入您的 GitHub 仓库，点击 **Settings** -> **Actions** -> **Runners**。
2. 点击 **New self-hosted runner**，选择 **Windows**。
3. 按照页面提供的命令，在本地创建一个文件夹（如 `E:\actions-runner`）并运行安装脚本。
4. **关键步骤**：在配置完成后，运行 `run.cmd` 启动运行器。

### 3. 设置权限与密钥

1. **权限**：前往 `Settings -> Actions -> General`，将 **Workflow permissions** 设置为 `Read and write permissions`。
2. **环境变量**：确保 `python` 和 `git` 已添加到系统的环境变量 `PATH` 中。

### 4. 文件结构准备

确保仓库根目录下包含以下文件：

* `.github/workflows/generate_bestcf.yml` 
* `filter.py` (核心过滤脚本)

---

## 📂 自动化流程解析

| 阶段 | 执行动作 | 说明 |
| --- | --- | --- |
| **抓取** | `curl` 多接口获取 | 从 `090227.xyz`、`vps789` 等接口提取 IP 样本 |
| **打标** | `awk` 字符串处理 | 为每个 IP 自动添加运营商后缀（如 `#CMCC-IPv4`） |
| **过滤** | `python filter.py` | 执行去重、测速（如果脚本支持）及国家分类 |
| **推送** | `git push -f` | 强制推送到独立 `bestcf` 分支，保持主干干净 |
| **发布** | `gh-release` | 自动生成 Release 标签，方便用户直接下载 `.txt` 文件 |

---

## ⚠️ 维护与注意事项

> [!IMPORTANT]
> **Windows 路径陷阱**
> 脚本中使用了 `/usr/bin/sort` 而非 `sort`。这是为了强制调用 Git Bash 的排序工具，避免误用 Windows 自带的 `C:\Windows\System32\sort.exe`，后者会导致语法报错。

* **SSL 报错**：脚本中已全局添加 `--ssl-no-revoke` 参数，解决了 Windows 上某些网络环境下证书吊销检查失败的问题。
* **Python 编码**：在 Windows 上运行 Python 务必设置 `PYTHONIOENCODING: utf-8` 环境变量，否则处理中文注释（如：`移动/电信`）时会崩溃。

## 📈 订阅与使用

您可以直接通过 jsDelivr 引用本项目生成的结果：

* **优选域名**：`https://cdn.jsdelivr.net/gh/你的用户名/仓库名@bestcf/bestcf-domain.txt`
* **优选 IP**：`https://cdn.jsdelivr.net/gh/你的用户名/仓库名@bestcf/bestcf-ip.txt`


