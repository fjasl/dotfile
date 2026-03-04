# Fcitx5 输入法与 Emoji 方块问题解决指南

本指南记录了在 Arch Linux 下解决 Fcitx5 皮肤制作、Emoji 显示（方块/豆腐块）以及配置调整的方法。

## 1. Fcitx5 皮肤制作 (ModernDark)
如果您需要备份或迁移皮肤：
- **皮肤路径**: `~/.local/share/fcitx5/themes/ModernDark/`
- **关键文件**: 
    - `theme.conf`: 配色与布局配置。
    - `bar.svg`: 带有 8px 圆角的背景。
    - `highlight.svg`: 带有 6px 圆角的高亮条。

---

## 2. 候选项显示为“方块”（豆腐块/Tofu）
这是由于系统缺少字体或字体回退（Fallback）路径不正确导致的。

### 解决方法 A：安装必要字体
```bash
sudo pacman -S noto-fonts noto-fonts-emoji
```

### 解决方法 B：强制 Emoji 回退配置
创建或修改 `~/.config/fontconfig/conf.d/99-emoji.conf`，添加以下内容，强制系统在任何字体下都优先使用 `Noto Color Emoji`：
```xml
<?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "fonts.dtd">
<fontconfig>
  <alias>
    <family>sans-serif</family>
    <prefer><family>Noto Color Emoji</family></prefer>
  </alias>
</fontconfig>
```

### 解决方法 C：刷新缓存
安装/配置后运行：
```bash
fc-cache -fv
fcitx5 -r
```

---

## 3. 常见交互设置

### 如何让候选项横向排列
1. 在 Fcitx5 配置中进入 **Addons (附加组件)**。
2. 找到 **Classic User Interface (经典用户界面)** -> **Configure**。
3. **取消勾选** `Vertical Candidate List (垂直列表)`。

### 如何移动候选项（翻页）
- **左右/翻页**: 使用键盘上的 **`-`** (减号) 和 **`=`** (等号)。
- **焦点切换**: 使用 **方向键** 或 **Tab** 键。

---

## 4. 维护常用命令
- **重启输入法**: `fcitx5 -r`
- **查看当前 Sans 别名指向**: `fc-match Sans`
- **确认 Emoji 字体**: `fc-match "Noto Color Emoji"`
