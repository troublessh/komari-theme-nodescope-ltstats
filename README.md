# NodeScope for Komari

LTstats 风格的 Komari 监控主题：近黑底、等宽字体、直角边框、高密度数据呈现。单文件实现，零依赖、零构建，数据每 5 秒刷新。

An LTstats-style theme for [Komari Monitor](https://github.com/komari-monitor/komari): near-black background, monospace type, square corners, dense data-first layout. Single-file, zero dependencies, no build step, 5-second refresh.

## 特性 / Features

- **列表视图**：可排序的密集表格（名称 / CPU 型号 / 核心 / 内存 / 运行时间 / CPU% / 负载 / 内存% / 交换% / 磁盘% / 实时网速 / 累计流量），底部带全舰队 TOTAL / AVG 汇总行，离线节点折叠为一行提示
- **卡片视图**：每台服务器一张卡——使用率条（CPU / 内存 / 磁盘）、负载 1/5/15 分钟、网速与累计流量，以及打开页面后实时采样绘制的 CPU/RAM 趋势火花线；卡片信息含内核版本，顶部 SORT BY 按钮条可按名称、运行时长、CPU%、RAM%、磁盘%、网络上下行一键排序（点已选按钮切换升/降序）
- **详情页**：点击节点名进入——信息卡片（累计流量、内核、CPU、运行时间、容量）+ 指标卡片（均值 / 峰值 / 当前值）+ 内联 SVG 大图（用量、网络、负载；有 Ping 任务时含延迟图），时间范围 1h / 3h / 6h / 12h / 24h
- **总览条**：节点数与在线状态、总核心与总内存、当前总网速、累计总流量
- **指标筛选**：除按名称过滤外，可按 CPU% / RAM% 占用率下限与运行时长下限（天）筛选节点，列表与卡片视图同时生效；排序、名称与指标条件都会写入 URL hash，可直接分享带条件的链接
- 百分比按阈值着色（绿 < 65% ≤ 琥珀 < 85% ≤ 红），支持按名称过滤、排序状态写入 URL hash

## 安装 / Install

**方法一（推荐）**：到 [Releases](https://github.com/troublessh/komari-theme-nodescope-ltstats/releases) 下载 `komari-theme-nodescope-ltstats-v1.0.1.zip`，在 Komari 后台的主题管理里导入，然后切换到 NodeScope。

**方法二（手动打包）**：克隆本仓库后，在仓库根目录执行：

```bash
zip -r komari-theme-nodescope-ltstats-v1.0.1.zip komari-theme.json dist
```

得到同样的主题包，再按方法一导入。

**方法三（放目录）**：把 `komari-theme.json` 和 `dist/` 放到 Komari 数据目录下的 `theme/nodescope/` 文件夹中，后台主题列表里就会出现 NodeScope。

要求：Komari 服务端 ≥ 1.0.5（实测 1.6.2）。主题通过 `/api/public`、`/api/nodes` 与 `/api/rpc2` 读取数据，无需任何额外配置。

## 数据口径说明 / Data notes

Komari 不采集 iowait、steal 和磁盘 IO 速率，原版 LTstats 的这几项在本主题中替换为 Komari 能提供的数据：

- `IOWAIT%` / `STEAL%` 两列 → 合并为 **LOAD**（1 分钟负载，卡片视图显示 1/5/15 分钟三档）
- `IO` 列 → **TRAFFIC**（累计上行 / 下行流量）

历史数据受 Komari 的记录保留时长限制（默认 24 小时），详情页时间范围最大 24h。

## 来源与许可 / Credits & License

- 设计与前端源自 [LTstats](https://github.com/lukastautz/ltstats)（作者 lukastautz，GPL-3.0）的 NodeScope 改版（卡片视图，见 ltstats.oloke.xyz），本仓库是其向 Komari 主题格式的移植，数据层已全部换为 Komari API。
- 本项目以 **GPL-3.0** 发布，见 [LICENSE](LICENSE)。
- 在 Komari 1.6.2 上实测通过（19 个节点，含 IPv6-only 与 NAT 容器节点）。
