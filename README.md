# ⚡ GitHub 加速节点检测（GhProxy 可用性监测）

一个**零依赖、单文件**的网页工具，用于批量检测公共 GitHub 加速节点（ghproxy 类前缀代理）的**连通性、延迟与下载速度**，并一键生成加速链接。

> 用法原理：`加速链接 = 加速前缀 + 原始 GitHub 链接`，参考 [gh-proxy 官方文档](https://gh-proxy.com/docs/github-accelerator)。
> 例：`https://ghfast.top/https://github.com/git/git/archive/refs/tags/v2.43.0.tar.gz`

## ✨ 功能

- **自动全量检测**：打开页面即并发检测所有节点
- **三级状态判定**
  - 🟢 **可用** —— 代理返回 2xx 且内容校验通过（完全验证）
  - 🟡 **可达** —— 服务器有响应但未开启 CORS，无法在浏览器内验证内容
  - 🔴 **异常** —— 显示具体原因（HTTP 404 / 429 / 超时 / DNS 失败等）
- **延迟测量**：毫秒级延迟 + 颜色分级（绿 <400ms / 黄 <1s / 红 ≥1s）+ 延迟条形图
- **单节点测速**：流式下载固定 Release 归档（最长 20 秒），实时显示进度与平均速度
- **链接加速转换**：粘贴任意 GitHub 链接（releases / raw / archive / blob / gist 等），按延迟排序生成可用节点的加速链接
- **一键复制**：按当前排序复制所有「可用」节点前缀
- **排序与持久化**：默认顺序 / 按延迟排序；检测目标、测速文件、排序、主题均存入 localStorage
- **亮 / 暗 / 跟随浏览器** 三态主题切换，防首帧闪烁

## 🔍 检测原理

浏览器直接请求 `代理地址 + 目标文件`（默认拉取 `github/gitignore` 仓库的 README）：

1. 先发**带 CORS** 的请求 —— 可读取 HTTP 状态码并校验内容 → 判定「可用 / 异常」
2. 若被 CORS 拦截，退回 **no-cors** 探测 —— 仅能确认服务器是否可达 → 标记「可达」

> 注意：「可达」≠ 坏了，只是该节点未开放 CORS，浏览器无法进一步验证；实际效果请在下载工具中确认。
> 所有延迟与速度均为**本机网络视角**，不同地区 / 运营商结果可能不同。

## 🚀 使用

无需构建，任选其一：

```bash
# 直接用浏览器打开
index.html

# 或起个本地静态服务
python -m http.server 8642
# 访问 http://127.0.0.1:8642
```

## 📡 内置节点（27 个）

<details>
<summary>展开节点列表</summary>

<!-- 节点以 index.html 中 SERVICES 数组为准 -->

| # | 节点 | # | 节点 |
|---|---|---|---|
| 1 | `https://ghm.078465.xyz/` | 15 | `https://github.boki.moe/` |
| 2 | `https://github.geekery.cn/` | 16 | `https://gh.chjina.com/` |
| 3 | `https://gh.catmak.name/` | 17 | `https://git.yylx.win/` |
| 4 | `https://hub.glowp.xyz/` | 18 | `https://gh-proxy.com/` |
| 5 | `https://gh.monlor.com/` | 19 | `https://gh-proxy.net/` |
| 6 | `https://g.blfrp.cn/` | 20 | `https://ghproxy.vip/` |
| 7 | `https://ghfast.top/` | 21 | `https://gh.404cafe.fun/` |
| 8 | `https://gh-proxy.org/` | 22 | `https://tvv.tw/` |
| 9 | `https://proxy.gitwarp.top/` | 23 | `https://gh.noki.eu.org/` |
| 10 | `https://fastgit.cc/` | 24 | `https://gh.noki.icu/` |
| 11 | `https://gh.zwy.one/` | 25 | `https://github.xxlab.tech/` |
| 12 | `https://wget.la/` | 26 | `https://gh.sixyin.com/` |
| 13 | `https://gh.dpik.top/` | 27 | `https://gh.ddlc.top/` |
| 14 | `https://github.ednovas.xyz/` | | |

</details>

节点收录自社区公开汇总（如 [hunshcn/gh-proxy#116](https://github.com/hunshcn/gh-proxy/issues/116)）并经连通性与内容双重核验。公共节点时效性强，如需增删请编辑 `index.html` 中的 `SERVICES` 数组。

## ⚠️ 免责声明

本工具仅检测公开资源的加速节点可用性；请合理使用公共加速服务，遵守各服务方的使用条款。
