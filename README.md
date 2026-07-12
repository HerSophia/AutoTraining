# AutoTraining — Map Training Automation / 地图训练自动化

> Version 1.0.0 ｜ Target game / 适配游戏：Esports Manager 2026 Steam retail patch 1.0.1 (Unity 6000.3.12f1 / IL2CPP metadata v39) ｜ License: MIT

**Language / 语言**: [English](#english) ｜ [中文](#中文)

---

<a name="english"></a>

## English

### Table of Contents

- [What this mod does](#what-this-mod-does)
- [Interface](#interface)
- [Prerequisite](#prerequisite)
- [How to install](#how-to-install)
- [Configuration](#configuration)
- [First-use advice](#first-use-advice)
- [Compatibility](#compatibility)

AutoTraining schedules map training automatically for each player team. You set an intent per map, and the plugin picks maps for later training after the game settles each day's map training, using the game's own interface.

This mod manages only map training. It does not change attribute or psychology training, does not touch AI teams, and does not writesaves directly.

### What this mod does

Each map can be set to one of four tiers:

| Tier | Meaning |
| --- | --- |
| Unset | The plugin does not consider it. If the team has no positive setting at all, the plugin stays dormant and leaves your original training plan untouched. |
| Focus / train most | Takes training slots first; suits first-pick maps and long-term main maps. |
| Maintenance / train a little | Fills remaining slots after focus maps take theirs. |
| Disabled | Never auto-selected. |

Defaults: focus target 85%, maintenance target 70%, minimum hold period 7 game days. The same set of maps will not swap back and forth every day within the hold period, but if the current plan includes a disabled or non-trainable map, it is corrected immediately. The slot limit is fixed at 3 maps.

When there is no trainable focus or maintenance map, the plugin does not clear your original plan.

### Interface

The plugin embeds an "auto training" section inside the game's native map training page; enter that page to see it, no extra key needed. In the section you can:

- View the current team, date, and the proficiency and selection state of all 8 maps;
- Switch each map's tier;
- Toggle automation for this team;
- Adjust the focus target, maintenance target, and hold period;
- Fill from BP suggestion (set the first-pick map to focus and the first-ban map to disabled);
- Apply now (bypassing the hold period).

If the embedded injection fails, it falls back to a standalone floating panel (toggled with F8 by default, draggable). The config key `[UI] PanelMode` selects the display: `Native` (embedded, default), `Imgui` (floating panel), or `Off` (no panel, automation only).

### Prerequisite

This mod requires the **EM.Framework** shared framework to be installed first. Without it, this mod cannot load.

### How to install

1. Make sure BepInEx is installed and the game starts normally.
2. Install EM.Framework first: copy the whole `EM.Framework/` folder into the game's `BepInEx/plugins/`.
3. Copy this mod's `AutoTraining/` folder into `BepInEx/plugins/`. It contains only `AutoTraining.dll`.
4. Start the game. In `BepInEx/LogOutput.log` you should see EM.Framework load first, then this mod load and print a ready line.

After installation, `BepInEx/plugins/` looks roughly like this:

```text
plugins/
  EM.Framework/     the shared framework (prerequisite)
  AutoTraining/     this mod (only AutoTraining.dll)
```

### Configuration

Config file `BepInEx/config/com.esportsmanager.modkit.autotraining.cfg`:

- `[General] Enabled`: master switch. If false at startup, no hook is installed.
- `[General] VerboseLogging`: log every skip and no-change reason.
- `[UI] PanelMode`, `ToggleKey`, and so on: interface settings.

Each team's map intent is saved in `BepInEx/config/em-autotraining/profiles.json`, grouped by the game's team ID, written with a temp file plus atomic replace, keeping a backup copy.

Note: the same team ID shares this config across different saves. If you want two campaigns to use different strategies, adjust or back up this JSON before switching saves.

### First-use advice

The map training plan is state that the game persists in the save. On first use, verify in a separate sandbox save: first set no positive tier and confirm the original plan is unchanged after a day advances; then gradually configure a focus map and verify apply-now, disabled maps not being selected, the three-slot limit, and so on. If anything goeswrong, set `Enabled=false` in the config and restart; this does not clear the game's current training selection.

### Compatibility

- This mod targets a specific game version: Unity 6000.3.12f1, IL2CPP metadata v39.
- On load it runs a compatibility self-check. On a mismatch it refuses to install the training-write hook and warns only.
- An interface injection failure affects only the panel display; the map training automation itself always works independently.

---

<a name="中文"></a>

## 中文

### 目录

- [这个 Mod 做什么](#这个-mod-做什么)
- [界面](#界面)
- [前置依赖](#前置依赖)
- [如何安装](#如何安装)
- [配置](#配置)
- [首次使用建议](#首次使用建议)
- [兼容性](#兼容性)

AutoTraining 为每个玩家队自动安排地图训练。你为每张地图设定意图，插件在游戏每天的地图训练结算完成后，用游戏自己的接口为后续训练挑选地图。

本 Mod 只管理地图训练，不改属性训练和心理训练，不改 AI 队，不直接写存档。

### 这个 Mod 做什么

每张地图可以设为四档：

| 档位 | 含义 |
| --- | --- |
| 未配置 | 插件不把它当候选。如果全队没有任何正向配置，插件保持休眠，完全不动你的原版训练计划。 |
| 核心 / 练最多 | 优先占用训练槽，适合首选图和长期主战图。 |
| 维护 / 稍微练 | 在核心图占槽后填剩余槽。 |
| 禁练 | 永远不会被自动选中。 |

默认核心目标 85%、维护目标 70%、最短持有期 7 个游戏日。同一套地图在持有期内不会每天来回换，但如果当前计划里混进了禁练或不可训练的地图，会立即修正。训练槽上限固定为 3 张图。

在没有任何可训练的核心或维护地图时，插件不会清空你的原版计划。

### 界面

插件在游戏原生的地图训练页里内嵌一个「自动训练」区块，进入该页面即可看到，不需要额外按键。区块里可以：

- 查看当前队伍、日期、8 张地图的熟练度和已选状态；
- 逐张地图切换档位；
- 开关本队的自动化；
- 微调核心目标、维护目标和持有期；
- 按 BP 填充建议（把首选图设为核心、首禁图设为禁练）；
- 立即应用（跳过持有期限制）。

如果内嵌注入失败，会自动降级到一个独立的悬浮面板（默认 F8 开关，可拖动）。可通过配置项 `[UI] PanelMode` 选择显示方式：`Native`（内嵌，默认）、`Imgui`（悬浮面板）、`Off`（不显示面板，只保留自动化）。

### 前置依赖

本 Mod 需要先安装 **EM.Framework** 共享框架。没有它，本 Mod 无法加载。

### 如何安装

1. 确认已安装 BepInEx，且游戏能正常启动。
2. 先安装 EM.Framework：把 `EM.Framework/` 目录整体拷进游戏的 `BepInEx/plugins/`。
3. 把本 Mod 的 `AutoTraining/` 目录拷进 `BepInEx/plugins/`。这个目录里只有 `AutoTraining.dll` 一个文件。
4. 启动游戏。查看 `BepInEx/LogOutput.log`，应能看到 EM.Framework 先加载，本 Mod 随后加载并打出就绪日志。

安装后 `BepInEx/plugins/` 下的结构大致如此：

```text
plugins/
  EM.Framework/     共享框架（前置依赖）
  AutoTraining/     本 Mod（只含 AutoTraining.dll）
```

### 配置

配置文件 `BepInEx/config/com.esportsmanager.modkit.autotraining.cfg`：

- `[General] Enabled`：总开关。启动时为 false 则不安装钩子。
- `[General] VerboseLogging`：记录所有跳过和未变更的原因。
- `[UI] PanelMode`、`ToggleKey` 等：界面设置。

每个队伍的地图意图保存在 `BepInEx/config/em-autotraining/profiles.json`，按游戏的队伍 ID 分组，写入时采用临时文件加原子替换，并保留一份备份。

注意：同一个队伍 ID 在不同存档里会共享这份配置。如果你希望两个战役用不同策略，请在切换存档前调整或备份这个 JSON。

### 首次使用建议

地图训练计划是会随游戏存档保存的状态。首次使用请在一个独立的沙箱存档里验证：先不配置任何正向档位，确认日推进后原版计划不变；再逐步配置核心图、验证立即应用、禁练图不入选、三槽上限等行为。遇到异常，先在配置文件里设 `Enabled=false` 并重启，这不会清空游戏当前的训练选择。

### 兼容性

- 本 Mod 只针对特定游戏版本：Unity 6000.3.12f1，IL2CPP metadata v39。
- 加载时会做兼容自检。不匹配时拒绝安装训练写入钩子，只警告。
- 界面注入失败只影响面板显示，地图训练自动化本身始终独立工作。
