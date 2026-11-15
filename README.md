# mino-releases

Mino 桌面端安装包的**唯一收口仓**：构建 + 发布同仓，GitHub Releases 即官网
`mino.baby/downloads/` 与自动更新 feed `download.mino.baby/dsh-desk/feeds/` 的上游
（服务器 cron 按资产清单+digest 镜像到 /var/www/mino-releases）。

- **构建**：Actions → `mac-build` / `win-build` → Run workflow。
  公仓吃免费 runner；产品源码不公开——deploy key（mino-agent 只读部署密钥）
  checkout `694410194/mino-agent` 的 BASE+format-patch 备份，与公开上游重建产品树。
  secrets：`MAC_BUILD_SSH_KEY`（私钥）、`MAC_BUILD_KNOWN_HOSTS`。
- **发布**：同一 dispatch 勾选 `publish` + 填 `version` 即可——构建成功后
  publish 作业把产物改名（dmg/exe→`Mino-Setup-<v>.<ext>`，zip/blockmap/yml 保原名，
  stable→nightly 通道复制）传 `v<version>` release，逐件核 GitHub digest 后才公开。
  已发布版本只能追加新资产，不覆盖既有安装包。
- 本仓还承载 minosvc 服务端二进制通道（`svc-*` tag，手动部署，与 v* 产品线互不干扰）。
- **新仓 workflow 不索引坑**：若 Actions 列表为空（`total_count:0`），改 workflow
  文件任意一行推送即强制重索引（GitHub 已知 bug，空 commit 无效）。
- Windows 本机备胎链：mino-deploy `scripts/build-win-x64-release.sh` + 树外 worktree。
