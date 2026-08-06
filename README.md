# SagaFont-VF - ColorOS 16 全局沙加体可变字重字体模块

基于 [MFGA (MakeFontsGreatAgain)](https://github.com/Numbersf/MakeFontsGreatAgain) 修改的全局字体模块，适配 ColorOS 16，将系统中英文字体统一显示为「沙加体」，并支持 **wght 轴 100~900 无级字重调节**。

## 与 1.0.x 的区别

1.0.x 使用单一静态字重，系统字重调节无效。本版本内置**可变字体** `Shajia-VF.ttf`（wght 轴 100~900，默认 400），通过 `fonts.xml` 的 `<axis tag="wght">` 声明注册可变字重族：

- 在 **ColorOS 16 系统设置 → 字体粗细** 中拖动滑块，可无级驱动沙加体的 wght 轴（100→16px、400→32px、900→59px 笔画厚度单调递增）。
- 系统设置界面等应用请求的 medium/semibold 档（500/600）实际渲染为 450/550，避免设置界面显示过粗。
- 无需手动放置字体文件，`Shajia-VF.ttf` 已内置到模块 `system/fonts/`。

## 字体覆盖范围

- `fonts.xml`：主字体族（sans-serif、serif、默认 fallback、lang="zh"/zh-Hans/zh-Hant/ja/ko）全部引用 `SagaSans.ttf`（即内置的 Shajia-VF），每个字重均声明 wght 轴，中英文统一显示为沙加体。
- `zdigit`/`zdigit-for-medium` 数字族统一改为 `SagaSans.ttf`，确保数字全局应用沙加体。
- 其余 Unicode 覆盖字体（emoji、符号等）保留原逻辑，避免全局字体错乱。
- `customize.sh`：安装时将内置 VF 应用到 ColorOS 系统 UI 直接加载的 `SysFont-Regular.ttf`、`SysSans-En-Regular.ttf`、`Roboto-Regular.ttf`、`Roboto-Italic.ttf`、`RobotoFlex-Regular.ttf`，确保状态栏、锁屏等系统组件的数字与英文也显示沙加体（缺省文件以符号链接指向 VF，不重复占空间）。

## 使用前检查（重要）

- 本模块基于 MFGA，在 ColorOS 上获得最佳效果需要 **在系统设置-字体 中启用 Roboto**。
- 需要 Magisk 28.0+ 或 KernelSU 11986+。
- 若已安装旧版 SagaFont（模块 ID 为 SagaFont）或旧版 MFGA（Colorfontsproject），本模块安装时会自动标记移除旧模块。
- 模块 ID 已更新为 `SagaFont-VF`，与旧版不会互相覆盖。

## 模块结构

- `system/fonts/SagaSans.ttf`：沙加体可变字体本体（Shajia-VF，wght 100~900）。
- `fonts.xml`：全局字体族声明，全部文本字重指向 SagaSans.ttf 并带 wght 轴。
- `customize.sh` / `search_dirs.sh`：安装时应用系统 UI 字体并同步系统字体配置。
- `recolor_glyph.sh` / `unicode_filter.sh`：主字体上色与 Unicode 过滤工具。
- `webroot/`：KernelSU WebUI。
