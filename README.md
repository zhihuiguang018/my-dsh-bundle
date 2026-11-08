# my-dsh-bundle

把一套常用的 DeepSeek Harness（DSH）Web 界面插件打成一个聚合包，一条命令装齐。

> 适用内核：DSH `>= 0.2.1-rc.2`（`dsh.engines.dsh`）
> 包内 8 个插件均为独立作者的第三方插件，本包只做聚合与挂载，不修改它们的源码。

---

## 包含什么

| 插件 | 版本 | 作用 | 许可证 |
|---|---|---|---|
| `@anysearch/anysearch-dsh` | 0.1.8 | 网页搜索与抓取后端（AnySearch provider + 工具） | MIT |
| `@linxin666/dsh-client-ui-skin-center` | 0.4.8 | 皮肤中心：主题切换、试穿、自定义配色、背景模糊 | Apache-2.0 |
| `@linxin666/dsh-client-ui-market` | 0.4.8 | 创意工坊：在 GUI 内浏览并一键安装皮肤 / 宠物 / 插件 / 预设 | BSD-3-Clause |
| `dsh-all-usage` | 1.2.1 | 用量看板：按模型 / 供应商 / 工作区统计，缓存命中率、余额、CSV 导出 | MIT |
| `dsh-better-reasoning-effort` | 0.5.4 | 逐模型声明思考档位与图片输入支持，在官方「模型」页内直接编辑 | MIT |
| `dsh-better-sidebar` | 0.28.0 | 双工作台：文件树、可编辑编辑器、Git（含只读提交图）、任务拓扑、侧聊、底部多终端 | MIT |
| `dsh-pet` | 0.3.7 | 桌宠（蓝色大肥鱼），跟随模型活动，可拖拽互动 | MIT |
| `dsh-pocket` | 2.12.0 | 手机扫码远程访问本机 DSH（局域网 + 公网，实时同屏） | GPL-2.0 |

### 不包含（按需求排除）

下面这些在我本机有装，但**故意没有打进本包**：

- `dsh-purge` —— 本机专用，以 git 源码形式安装
- `dsh-workbuddy-connect` —— 绑定 WorkBuddy 桌面端登录态，不适合分发
- `@opencode2dsh/dsh-plugin` —— 绑定 OpenCode 免费额度来源，且当前线路不可用

---

## 安装

### 桌面版（DSH Desktop，profile 由 App 管理）

桌面版的 profile 写入走插件市场自己的 pnpm 通道。先进入 profile 目录再安装：

```powershell
cd "$env:USERPROFILE\.dsh\profiles\desktop"
pnpm add https://github.com/zhuihuiguang018/my-dsh-bundle
```

然后把聚合包名加入 `package.json` 的 `dsh.profile.bundles`：

```json
"bundles": [
  "@deepseek-ai/dsh-base",
  "@deepseek-ai/dsh-web-app",
  "my-dsh-bundle"
]
```

重开 DSH。

### 其他 profile（Web 版 / 自建 profile）

```sh
dsh plugin --profile web add https://github.com/zhuihuiguang018/my-dsh-bundle
```

`dsh plugin add` 会自动把聚合包挂进 `dsh.profile.bundles`，无需手改。

### 注意

- 本包的行 id 都带 `my-` 前缀，**可以和插件的独立安装并存**，不会撞名。
- 但**同一个插件不要同时用聚合包和独立安装各挂一次**，否则会出现两份界面（例如两个桌宠、两个皮肤中心）。选了本包，就把独立安装的行去掉。
- `cordis.patch.yml` 里有一行 `- id: web` 把宿主的搜索/抓取后端指向 AnySearch。如果你不想要这个行为，删掉那三行即可。

---

## 卸载

```powershell
# 桌面版
cd "$env:USERPROFILE\.dsh\profiles\desktop"
pnpm remove my-dsh-bundle
```

再从 `dsh.profile.bundles` 里删除 `my-dsh-bundle` 这一行，重开 DSH。

只想去掉其中某一个插件：在你的 profile 的 `cordis.patch.yml` 里用对应 id 写覆盖，例如：

```yaml
- id: my-dsh-pet
  disabled: true
```

---

## 版本说明

包内依赖全部锁定为具体版本（非 `^` 范围），保证换机器安装时行为一致。要升级某个插件，改 `package.json` 里对应版本号后重新 `pnpm install`。

仓库里带了 `pnpm-workspace.yaml`，关键是 `autoInstallPeers: false`：DSH 的宿主包（`@deepseek-ai/dsh-*`）由 DSH 应用本体提供，不该从 npm 拉。若你在本仓库里直接 `pnpm install` 调试，没有这项配置会因为拉到注册表上的旧版宿主包而失败。

如果内核升级到 `0.2.2+`，部分插件可能需要更新到更高版本才能继续兼容；届时按报错提示逐个升级即可。

---

## 许可证

本聚合包自身以 MIT 发布（见 `LICENSE`）。包内各插件的许可证与来源见 `THIRD_PARTY_NOTICES.md`；其中 `dsh-pocket` 为 GPL-2.0，本包仅以依赖形式引用，未内联其源码。
