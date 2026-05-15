# 禁止通行 / No Passing — Level Editor

> 红灯停 · 绿灯行 / Red Light, Green Light

一个浏览器端的关卡编辑器，导出 JSON 给 Godot 游戏加载。单 HTML 文件，无需安装、无需联网。

A browser-based level editor that exports JSON for the Godot game to load. Single HTML file, no install, no network needed.

---

## 快速开始 / Quick Start

1. 双击 `no-passing-editor.html` 在浏览器打开 / Double-click `no-passing-editor.html` to open in any browser
2. 右上角点 `EN` 或 `中` 切换语言（默认英文）/ Top-right `EN` / `中` toggles language (English default)
3. 左侧选工具 → 点格子放置 → 点 `EXPORT JSON` 导出 / Pick a tool → click cells → `EXPORT JSON`

---

## 界面分区 / UI Sections

### 1. 工具栏 Tools

放置在关卡里的元素。点击切换当前工具，或按数字快捷键。

The elements you place into the level. Click to switch tool, or press the number key.

| 工具 Tool | 键 Key | 作用 Function |
|---|---|---|
| Erase 擦除 | `0` | 清除一格的内容。或者拖动右键直接擦。Removes content from a cell. Right-click drag also erases. |
| Platform 平台 | `1` | 标准实心方块，玩家能站上去。The basic solid block — player can stand on it. |
| Spike 尖刺 | `2` | 陷阱。玩家碰到 = 死，回检查点。Trap. Player touch = death, respawn at last checkpoint. |
| Start 起点 | `3` | 玩家出生位置。**每关只能有一个**——再放新的会清掉旧的。Player spawn point. **Only one per level** — placing a new one removes the old. |
| Goal 终点 | `4` | 通关位置。**每关只能有一个**。Win point. **Only one per level**. |
| Checkpoint 检查点 | `5` | 玩家碰到后，下次死亡会从这里复活。Player touch = respawn point updated. |
| Moving Plat. 移动平台 | `6` | 自动左右移动的平台。红灯时停止。Auto horizontal-moving platform. Stops on red light. |
| Light Zone 红绿灯区域 | `7` | 视觉提示标记——纯装饰，告诉玩家这里有红绿灯规则。**红绿灯效果是全局的**，不是只有这个区域才生效。Visual decoration to mark where the red-light rule "feels" present. **The light is global** — it applies everywhere, not just inside this zone. |

### 2. 网格 Grid

控制关卡尺寸（以瓦片为单位）。

Controls level dimensions in tiles.

- **Width / 宽度**：横向格子数 (10–200) / horizontal tile count
- **Height / 高度**：纵向格子数 (8–100) / vertical tile count
- **Apply Size / 应用尺寸**：点击后重建网格。**已放置的内容会保留**（在新尺寸范围内的部分）。Rebuilds the grid. **Existing placements are preserved** within the new bounds.

### 3. 红绿灯 Traffic Light

每关独立配置的红绿灯节奏。所有数值单位是秒。

Per-level traffic light timing. All values in seconds.

- **Green duration / 绿灯时长**：自由活动的时间。玩家可以自由跑跳，移动平台正常运行。Free movement window. Player can run/jump, moving platforms run normally.
- **Yellow warning / 黄灯预警**：红灯来临前的预警窗口。玩家**仍然可以移动**，但要准备停下。设为 0 = 没有预警，红灯瞬间到。Warning window before red. Player can **still move**, but should prepare to stop. Set to 0 for no warning.
- **Red duration / 红灯时长**：冻结时间。玩家移动 = 回检查点，移动平台暂停。Freeze window. Player movement = respawn, platforms paused.

设计建议 / Design tips:
- 短绿灯 + 长红灯 = 紧张 / Short green + long red = tense
- 长绿灯 + 短红灯 = 节奏温和 / Long green + short red = gentle pacing
- 黄灯建议保留 0.5–1 秒，给玩家反应时间 / Keep yellow at 0.5–1s for fair reaction time

### 4. 关卡信息 Level Info

- **Name / 名称**：导出文件名 + JSON 内部的关卡标识。建议用 `level_01`, `level_02` 这种格式。Filename and internal level ID. Suggest `level_01`, `level_02` etc.

### 5. 操作 Actions

- **EXPORT JSON / 导出 JSON**：弹窗显示完整 JSON。可以复制（粘贴分享）或下载（保存到本地）。如果没放起点/终点会先提示。Opens a modal with the full JSON. Copy (to share) or download (to save locally). Warns if Start/Goal missing.
- **IMPORT JSON / 导入 JSON**：粘贴别人分享的 JSON 进来加载。会覆盖当前编辑内容。Paste someone's exported JSON to load it. Overwrites current edit.
- **CLEAR ALL / 清空**：清空整个网格。会先确认。Clears everything. Asks first.

---

## 操作方式 / Controls

### 鼠标 Mouse

| 操作 Action | 效果 Effect |
|---|---|
| 左键点击 Left click | 放置当前工具 / Place current tool |
| 右键点击 Right click | 擦除一格 / Erase one cell |
| 左键按住拖动 Left drag | 连续放置（画长条平台超快）/ Paint continuously — fast for long platforms |
| 右键按住拖动 Right drag | 连续擦除 / Erase continuously |

拖动时如果鼠标移动太快，编辑器会**自动用直线连接**两个采样点之间的所有格子，不会漏。

If your mouse moves faster than the browser can sample, the editor **interpolates a line** between samples so no cells are skipped.

### 触摸 Touch

直接按住屏幕拖动放置。和鼠标一样。

Press and drag on touchscreen. Same as mouse drag.

### 键盘 Keyboard

| 键 Key | 作用 Action |
|---|---|
| `0`–`7` | 切换工具 / Switch tool |

数字键在输入框聚焦时不生效。

Number keys don't fire when an input field is focused.

---

## 元素颜色对照 / Visual Legend

打开编辑器后会看到一个示例关卡。各元素的视觉：

You'll see a demo level when first opened. Visual identification:

- **棕色方块 / Brown block** = Platform 平台
- **红色锯齿 / Red zig-zag** = Spike 尖刺
- **绿色 S / Green S** = Start 起点
- **金色 G / Gold G** = Goal 终点
- **蓝色星标 / Blue star** = Checkpoint 检查点
- **紫色 ↔ / Purple ↔** = Moving Platform 移动平台
- **黄色斜纹 ◉ / Yellow diagonal stripes ◉** = Light Zone 红绿灯区域

---

## 导出的 JSON 格式 / Exported JSON Format

每个关卡是一个独立的 JSON 文件，结构如下：

Each level is a standalone JSON file with this structure:

```json
{
  "version": 1,
  "name": "level_01",
  "grid": {
    "width": 40,
    "height": 20,
    "tile_size": 32
  },
  "traffic_light": {
    "green": 3,
    "yellow": 1,
    "red": 2.5
  },
  "start": { "x": 3, "y": 16 },
  "goal":  { "x": 36, "y": 16 },
  "tiles": [
    { "type": "platform",   "x": 5, "y": 17 },
    { "type": "spike",      "x": 8, "y": 17 },
    { "type": "checkpoint", "x": 12, "y": 16 },
    { "type": "moving",     "x": 20, "y": 15 },
    { "type": "light_zone", "x": 25, "y": 16 }
  ]
}
```

字段说明 / Field reference:

- `version`：JSON 格式版本号。未来如果格式变动会递增。/ JSON schema version. Bumps if the format changes.
- `name`：关卡名 / Level name
- `grid.tile_size`：每个格子的像素大小，固定 32 / Pixel size per tile, fixed 32
- `traffic_light`：见上方说明 / See above
- `start` / `goal`：单独字段，因为每关只能有一个 / Separate fields because they're singletons
- `tiles[]`：所有其他元素的列表，每项 `{type, x, y}`。空格子**不**出现在数组里（稀疏存储）/ Array of everything else, each `{type, x, y}`. Empty cells are **not** stored (sparse).

可用的 `type` 值：`platform`, `spike`, `checkpoint`, `moving`, `light_zone`。

Valid `type` values: `platform`, `spike`, `checkpoint`, `moving`, `light_zone`.

---

## 单例规则 / Singleton Rule

`start` 和 `goal` 是 singleton——**全关卡只能存在一个**。

`start` and `goal` are singletons — **only one allowed per level**.

- 用工具再放一个 = 旧的自动清掉 / Placing again = old one auto-cleared
- 拖动时只在落笔那一格放置，不会跟着鼠标到处跑 / During drag, only placed at the press point — won't follow the cursor

---

## 分享 / Sharing Levels

1. 在编辑器里点 `EXPORT JSON`
2. **复制**：贴到 Discord / 微信 / 邮件给朋友 / **Copy**: paste to Discord / WeChat / email
3. **下载**：得到一个 `.json` 文件，发送即可 / **Download**: get a `.json` file, send it

对方收到后 / The receiver:

1. 打开自己的编辑器 → `IMPORT JSON`
2. 把 JSON 粘贴进去 → `LOAD`
3. 看到完整的关卡，可以继续编辑或直接放进 Godot 跑

---

## 给 Godot 用 / Loading in Godot

把导出的 `.json` 放进 Godot 项目的 `levels/` 目录，然后在 `Main` 场景的 `level_to_load` 字段指向它。`LevelLoader.gd` 会自动读取并搭建关卡。

Put the exported `.json` into your Godot project's `levels/` folder, then point the `Main` scene's `level_to_load` field at it. `LevelLoader.gd` handles the rest.

---

## 常见问题 / FAQ

**Q: 我能手写 JSON 吗？/ Can I hand-edit the JSON?**

可以，格式按上面的就行。导入回编辑器可以可视化检查。

Yes — follow the schema above. Import it back to verify visually.

**Q: 网格能比 200×100 更大吗？/ Can the grid be larger than 200×100?**

代码层面没硬限制，但浏览器会变慢。如果真需要超大关卡，分成多个小关卡更好。

No hard limit in code, but the browser slows down. Split into multiple levels if you really need it.

**Q: 数据保存在哪？刷新会丢吗？/ Where's data saved? Does refresh lose it?**

**会丢**。当前版本不写本地存储，刷新页面 = 回到初始示例关卡。编辑过程中记得定期 `EXPORT` 或 `DOWNLOAD`。

**Yes, refresh loses your work.** Current version doesn't persist anywhere. `EXPORT` or `DOWNLOAD` regularly while editing.

**Q: 红绿灯区域 Light Zone 到底有什么用？/ What does Light Zone actually do?**

游戏里只是**视觉染色**——会跟着当前灯光颜色变。规则上和其他地方完全一样。它存在的意义是给玩家视觉提示"注意这里"。

It's a **visual tint only** in-game — colors shift with the current light. The rule itself applies everywhere globally. The zone exists purely as a visual hint to draw the player's attention.

**Q: 玩家在空中时红灯亮了会被惩罚吗？/ Penalty if airborne when red starts?**

不会。只有**主动按方向键或起跳**才算违规。重力下落不算。

No. Only **pressing direction keys or jumping** counts as violation. Falling from gravity doesn't.
