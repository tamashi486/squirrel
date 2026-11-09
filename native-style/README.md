# 鼠须管 · 原生风格补丁

基于 [rime/squirrel](https://github.com/rime/squirrel) 1.1.2，让候选框尽量接近 macOS 原生拼音输入法的观感。所有新增选项默认关闭，不开启时行为与上游一致。

## 新增 `style/` 选项

| 选项 | 默认 | 作用 |
|---|---|---|
| `capsule` | `false` | 胶囊候选条：内边距不再随 `corner_radius` 膨胀，圆角自动截到半高；选中项画成离边缘 `hilited_inset` 的胶囊；`border_color` 变为 0.5pt 发丝描边。仅横排 `linear` 生效。 |
| `hilited_inset` | `3` | 胶囊模式下选中项距面板边缘的距离（pt）。选中项内左右留白 = `border_width - hilited_inset`，候选间距为其两倍。 |
| `material` | `hud` | `translucency: true` 时的磨砂材质：`popover` / `menu` / `hud` / `sidebar` / `sheet` / `tooltip` / `header` / `titlebar` / `window` / `under_window` / `content` / `selection`。 |
| `max_width` | `0`（关） | 按宽度显示候选：候选条固定宽度，放得下几个显示几个（最多 `menu/page_size` 个）。翻页、数字键选词、方向键、鼠标都由前端按「当前可见页」处理；翻页键从方案的 `key_binder` 中读取。 |
| `status_badge` | `false` | 切换中英文等状态时，在光标下方弹出强调色胶囊（「中」/「A」），代替在候选框里显示文字；开始打字即消失。全角标签自动转半角。 |
| `status_badge_font_point` | `12` | 状态胶囊的字号，高度约为字号 × 1.6。 |

完整示例见 [`squirrel.custom.yaml`](squirrel.custom.yaml)（放到 `~/Library/Rime/` 后重新部署）。注意鼠须管的颜色格式是 `0xAABBGGRR`。

## 构建与安装

```sh
bash ./action-install.sh          # 下载预编译 librime / Sparkle
DEVELOPER_DIR=/Applications/Xcode.app/Contents/Developer make release
```

最简单的安装方式是只替换已安装版本的主程序（保留其中的 librime 及 lua 等插件）：

```sh
sudo cp build/Build/Products/Release/Squirrel.app/Contents/MacOS/Squirrel \
  "/Library/Input Methods/Squirrel.app/Contents/MacOS/Squirrel"
sudo codesign --force --deep --sign - "/Library/Input Methods/Squirrel.app"
killall Squirrel
```

自编译版本会被官方自动更新覆盖，建议关闭自动更新：

```sh
defaults write im.rime.inputmethod.Squirrel SUEnableAutomaticChecks -bool false
```
