# Tractyl Manuform 构建指南目录

本目录包含构建 Tractyl Manuform 键盘的中文指南和工具。

## 📚 文档索引

### 1. 主要构建指南

**[build-guide-cn.md](build-guide-cn.md)** - 完整的中文构建指南

这是最全面的指南，涵盖：
- 环境配置（Java、Leiningen、OpenSCAD）
- 3x5/3x6 布局修改
- 3D 模型生成和打印
- 硬件方案和物料清单
- 焊接布线详细步骤
- QMK 固件配置
- 组装流程
- 常见问题解答

**适合：** 所有用户，特别是初次构建者

---

### 2. 快速配置指南

**[quick-start-3x5-3x6.md](quick-start-3x5-3x6.md)** - 3x5/3x6 布局快速配置

专注于将默认 4x5 布局修改为 3x5 或 3x6 布局的快速步骤：
- 关键参数修改
- 3x5 vs 3x6 对比
- QMK 矩阵配置
- 常见调整技巧
- 推荐工作流程

**适合：** 已了解基础流程，需要快速修改布局的用户

---

### 3. 环境配置脚本

**[setup-gentoo.sh](setup-gentoo.sh)** - Gentoo Linux 自动配置脚本

自动安装和配置所有依赖：
- Java（OpenJDK）
- Leiningen（Clojure 构建工具）
- OpenSCAD（3D 建模）
- QMK 工具链（可选）
- Leiningen 镜像配置（加速下载）
- 项目克隆和测试

**使用方法：**

```bash
cd ~/proj/tractyl-manuform-keyboard/guide
./setup-gentoo.sh
```

**适合：** Gentoo Linux 用户，快速配置开发环境

---

### 4. PCB 方案说明

**[pcb-alternatives.md](pcb-alternatives.md)** - PCB 方案和替代方案详解

详细说明本项目的布线方案和 PCB 替代选择：
- 手工布线详细方案和引脚分配
- 柔性 PCB 设计指南
- 使用现成 PCB 的方案
- 各方案对比和推荐
- PCB 设计资源和工具

**适合：** 想要了解 PCB 方案或考虑自己设计 PCB 的用户

---

## 🚀 快速开始

### 第一次构建？

1. **阅读主指南：** [build-guide-cn.md](build-guide-cn.md)
2. **运行配置脚本：** `./setup-gentoo.sh`（Gentoo 用户）
3. **修改布局：** 参考 [quick-start-3x5-3x6.md](quick-start-3x5-3x6.md)
4. **生成模型：** `lein generate`
5. **打印和组装：** 按照主指南进行

### 已经熟悉流程？

1. **修改配置：** `vim src/dactyl_keyboard/dactyl.clj`
2. **生成模型：** `lein generate`
3. **预览：** `openscad things/right.scad`
4. **导出 STL：** `openscad -o things/right.stl things/right.scad`

---

## 📝 文档结构

```
guide/
├── README.md                    # 本文件 - 目录和索引
├── build-guide-cn.md            # 完整构建指南（中文）
├── quick-start-3x5-3x6.md       # 3x5/3x6 快速配置
├── pcb-alternatives.md          # PCB 方案和替代方案
└── setup-gentoo.sh              # Gentoo 环境配置脚本
```

---

## 🔗 相关资源

### 项目相关

- **原项目 README：** [../README.md](../README.md)（英文，含详细图片）
- **原项目仓库：** [https://github.com/noahprince22/tractyl-manuform-keyboard](https://github.com/noahprince22/tractyl-manuform-keyboard)
- **Noah 的 QMK Fork（轨迹球）：** [https://github.com/noahprince22/qmk_firmware/tree/trackball](https://github.com/noahprince22/qmk_firmware/tree/trackball)

### 上游项目

- **Dactyl Manuform：** [https://github.com/tshort/dactyl-keyboard](https://github.com/tshort/dactyl-keyboard)
- **Dactyl：** [https://github.com/adereth/dactyl-keyboard](https://github.com/adereth/dactyl-keyboard)
- **ManuForm：** [https://github.com/jeffgran/ManuForm](https://github.com/jeffgran/ManuForm)

### 工具文档

- **Clojure：** [https://clojure.org/](https://clojure.org/)
- **Leiningen：** [https://leiningen.org/](https://leiningen.org/)
- **OpenSCAD：** [https://openscad.org/](https://openscad.org/)
- **QMK 固件：** [https://docs.qmk.fm/](https://docs.qmk.fm/)

### 社区

- **Reddit - r/MechanicalKeyboards：** [https://reddit.com/r/MechanicalKeyboards](https://reddit.com/r/MechanicalKeyboards)
- **Reddit - r/ErgoMechKeyboards：** [https://reddit.com/r/ErgoMechKeyboards](https://reddit.com/r/ErgoMechKeyboards)
- **Discord - MechKeys：** 搜索 "mechanical keyboards discord"

---

## ❓ 常见问题

### Q: 我的系统不是 Gentoo，能用这些指南吗？

A: 主指南中的大部分内容适用于所有 Linux 发行版（以及 macOS/Windows）。只需要根据你的包管理器调整安装命令：

- **Arch Linux：** `sudo pacman -S jdk-openjdk openscad`
- **Ubuntu/Debian：** `sudo apt install default-jdk openscad`
- **Fedora：** `sudo dnf install java-openjdk openscad`

Leiningen 的安装方式在所有系统上都相同（下载脚本）。

### Q: 我需要哪些硬件知识？

A: 基础的焊接技能和使用万用表的能力。如果你从未焊接过，建议先练习焊接一些简单的套件。

### Q: 整个项目需要多长时间？

A: 根据经验水平：
- **首次构建：** 2-4 周（包括学习、打印、调试）
- **有经验：** 1-2 周
- **纯打印时间：** 约 60 小时
- **焊接组装：** 约 20-40 小时

### Q: 成本是多少？

A:
- **最小化（无轨迹球，使用现有轴和键帽）：** ¥200-300
- **完整版（含轨迹球）：** ¥400-500
- **首次搭建（含所有工具）：** ¥800-1200

### Q: 我可以不用轨迹球吗？

A: 可以！设置 `(def trackball-enabled false)` 即可禁用轨迹球功能。

### Q: 有 PCB 设计文件吗？

A: 本项目是手工布线方案，没有 PCB 文件。如果需要 PCB，可以：
1. 自己设计（使用 KiCad）
2. 寻找社区的 Dactyl PCB 方案
3. 使用现成的分体式键盘 PCB（需要修改外壳）

---

## 📧 反馈和贡献

如有问题、建议或发现错误：

1. **提交 Issue：** 到原项目的 GitHub 仓库
2. **改进文档：** 欢迎提交 Pull Request 改进中文指南
3. **分享经验：** 在社区分享你的构建过程

---

## 📄 许可证

本指南遵循原项目的许可证：

- **源代码：** GNU AGPL v3
- **生成的模型：** CC BY-SA 4.0

---

**祝你构建顺利！享受你的定制人体工学键盘！**
