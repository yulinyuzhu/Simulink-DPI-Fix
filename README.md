# MATLAB / Simulink Windows 多显示器 Mixed-DPI 启动修复脚本

一个用于缓解 Windows 多显示器、不同缩放比例环境下 MATLAB / Simulink 界面异常放大的启动脚本。

**English version below.**

---

## 问题描述

在 Windows 多显示器环境中，如果主屏和副屏使用不同的缩放比例，例如笔记本内屏使用较高缩放比例，外接显示器使用较低缩放比例，MATLAB 或 Simulink 可能会在启动后出现界面缩放异常。

常见表现包括：

- 工具栏巨大
- 菜单巨大
- Simulink 控件异常放大
- 模型画布区域变得很小
- Windows 的“高 DPI 缩放替代”兼容性选项无效
- 临时关闭副屏后显示恢复正常

这个问题通常与 MATLAB / Simulink 在 mixed-DPI 多显示器环境下的缩放处理有关。部分 Simulink 界面可能还会受到 Qt 相关 DPI 缩放环境变量的影响。

---

## 解决思路

本项目提供一个 Windows 批处理启动脚本。

它的作用是在启动 MATLAB 前，临时清理当前 MATLAB 进程可见的 Qt DPI 缩放相关环境变量，从而避免 MATLAB / Simulink 在启动时继承错误或不合适的缩放配置。

这些修改只影响通过该脚本启动的 MATLAB 进程，不会永久修改 Windows 系统设置，也不会修改 MATLAB 安装文件。

---

## 使用方法

1. 下载本项目中的启动脚本。
2. 根据自己的 MATLAB 安装位置，修改脚本中的 MATLAB 启动路径。
3. 保存脚本。
4. 以后通过该脚本启动 MATLAB，而不是直接双击 MATLAB 原始快捷方式。

如果 MATLAB 安装在非默认路径，需要把脚本中的路径改成实际的 `matlab.exe` 路径。

---

## 适用场景

这个脚本主要适合以下情况：

- 使用 Windows 系统
- 使用多个显示器
- 不同显示器使用不同缩放比例
- MATLAB / Simulink 启动后界面异常巨大
- Simulink 工具栏、菜单或控件被异常放大
- Windows 的高 DPI 兼容性设置无效
- 关闭副屏后 MATLAB / Simulink 显示恢复正常
- 不想通过 Win + P 临时断开显示器来绕过问题

---

## 为什么不建议用 Win + P 关闭副屏？

有些用户会在启动 MATLAB 前临时关闭副屏，让 MATLAB 只看到一个显示器。这样确实可能绕过 DPI 问题，但它会带来新的副作用。

Windows 在切换显示器模式时会重建显示器拓扑。这样可能导致：

- OBS Studio 的 Display Capture 失效
- 显示器编号变化
- 主副屏状态变化
- 显示器坐标变化
- 依赖显示器枚举的软件出现异常

本脚本的目的就是避免通过改变显示器拓扑来解决 MATLAB 的 DPI 问题。

---

## 它不会做什么

这个脚本不会：

- 修改注册表
- 修改 Windows 全局 DPI 设置
- 修改 MATLAB 安装文件
- 修改系统环境变量
- 修改 OBS 设置
- 断开或重连显示器

它只是为 MATLAB 提供一个更干净的 DPI 启动环境。

---

## 已知限制

这个脚本不保证解决所有 MATLAB / Simulink 缩放问题。

它更可能解决的是以下类型的问题：

- Simulink 界面比 MATLAB 主界面缩放更异常
- 工具栏、菜单、控件巨大
- 问题只在多显示器 mixed-DPI 环境下出现
- 关闭高缩放显示器后问题消失

它不一定能解决：

- MATLAB 字体本身太小或太大
- Windows 全局缩放配置错误
- 显卡驱动导致的显示异常
- MATLAB 本身的版本级 UI bug
- 远程桌面、虚拟机、串流软件带来的 DPI 问题

---

## 排查建议

如果脚本没有效果，可以检查以下几点：

1. 确认脚本中的 MATLAB 路径是否正确。
2. 确认 MATLAB 是通过该脚本启动的，而不是通过原始快捷方式启动的。
3. 完全退出所有 MATLAB 进程后再重新启动。
4. 检查 MATLAB 内部是否设置过额外的桌面缩放参数。
5. 尝试更新 MATLAB、显卡驱动和 Windows。
6. 如果问题只出现在某个 MATLAB 版本中，可以考虑向 MathWorks 提交 bug report。

---

## 推荐文件名

推荐使用：

`matlab_clean_dpi.bat`

也可以使用更描述性的名称，例如：

`start_matlab_without_qt_dpi_scaling.bat`

---

# MATLAB / Simulink Mixed-DPI Startup Fix for Windows

A small Windows launcher script that helps reduce abnormal MATLAB / Simulink UI scaling issues in multi-monitor setups with different display scaling factors.

---

## Problem

On Windows, when multiple monitors use different scaling factors, MATLAB or Simulink may start with incorrect UI scaling.

This often happens when, for example, a laptop display uses a higher scaling factor while an external monitor uses a lower scaling factor.

Common symptoms include:

- Huge toolbar
- Huge menus
- Oversized Simulink controls
- Very small model canvas area
- Windows high-DPI compatibility overrides do not help
- Disconnecting the secondary display makes the UI look normal again

This issue is usually related to how MATLAB / Simulink handles mixed-DPI multi-monitor environments. Some Simulink UI components may also be affected by Qt-related DPI scaling environment variables.

---

## How It Works

This project provides a Windows batch launcher script.

Before starting MATLAB, the script temporarily clears several Qt DPI scaling-related environment variables visible to the MATLAB process. This prevents MATLAB / Simulink from inheriting unsuitable DPI scaling settings at startup.

The change only affects the MATLAB process started by this script. It does not permanently modify Windows system settings or MATLAB installation files.

---

## Usage

1. Download the launcher script from this project.
2. Edit the script and update the MATLAB executable path according to your installation.
3. Save the script.
4. Start MATLAB using this script instead of launching MATLAB directly from the original shortcut.

If MATLAB is installed in a non-default location, make sure the path in the script points to the actual `matlab.exe`.

---

## When to Use This

This script is mainly useful if you have:

- Windows
- Multiple monitors
- Different scaling factors on different monitors
- Oversized MATLAB / Simulink UI after startup
- Oversized Simulink toolbar, menus, or controls
- Ineffective Windows high-DPI compatibility overrides
- Normal MATLAB / Simulink scaling after disconnecting a high-scaling display
- A need to avoid using Win + P to disconnect monitors before launching MATLAB

---

## Why Not Use Win + P?

Some users temporarily disable the secondary monitor before starting MATLAB. This may work around the DPI issue, but it can cause new problems.

When Windows switches display modes, it rebuilds the display topology. This may cause:

- OBS Studio Display Capture to stop working
- Monitor numbering to change
- Main display state to change
- Monitor coordinates to change
- Other applications that depend on display enumeration to behave incorrectly

This script is designed to avoid solving MATLAB's DPI problem by changing the Windows display topology.

---

## What This Script Does Not Do

This script does not:

- Modify the registry
- Modify global Windows DPI settings
- Modify MATLAB installation files
- Modify system environment variables
- Modify OBS settings
- Disconnect or reconnect monitors

It simply gives MATLAB a cleaner DPI startup environment.

---

## Known Limitations

This script does not guarantee a fix for every MATLAB / Simulink scaling problem.

It is more likely to help when:

- Simulink is scaled more incorrectly than the MATLAB main desktop
- Toolbars, menus, or controls are oversized
- The issue only happens in a mixed-DPI multi-monitor setup
- Disconnecting a high-scaling display makes the problem disappear

It may not fix:

- MATLAB fonts being too small or too large
- Incorrect global Windows scaling configuration
- GPU driver display issues
- Version-specific MATLAB UI bugs
- Remote Desktop, virtual machine, or streaming-related DPI problems

---

## Troubleshooting

If the script does not work, check the following:

1. Make sure the MATLAB path in the script is correct.
2. Make sure MATLAB is started through this script, not through the original shortcut.
3. Fully close all MATLAB processes before restarting.
4. Check whether MATLAB has additional internal desktop scaling settings.
5. Try updating MATLAB, the GPU driver, and Windows.
6. If the issue only happens in a specific MATLAB version, consider reporting it to MathWorks.

---

## Recommended File Name

Recommended:

`matlab_clean_dpi.bat`

Alternative:

`start_matlab_without_qt_dpi_scaling.bat`
