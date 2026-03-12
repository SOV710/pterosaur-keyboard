# 3x5/3x6 快速配置指南

本文档提供快速配置步骤，帮助你将项目从默认的 4x5 布局修改为 3x5 或 3x6 布局。

## 快速步骤

### 1. 修改布局参数

编辑 `src/dactyl_keyboard/dactyl.clj`，在文件开头找到以下行：

```clojure
(def nrows 4)
(def ncols 5)
```

#### 对于 3x5 布局：

```clojure
(def nrows 3)  ; 改为 3 行
(def ncols 5)  ; 保持 5 列
```

#### 对于 3x6 布局：

```clojure
(def nrows 3)  ; 改为 3 行
(def ncols 6)  ; 改为 6 列
```

### 2. 调整相关参数

由于行数变化，你可能需要调整以下参数以获得更好的人体工程学：

```clojure
; 前后倾斜控制（根据行数调整）
(def centerrow (- nrows 2.5))  ; 对于 3 行，结果是 0.5

; 如果改为 6 列，可能需要调整中心列
(def centercol 2)              ; 对于 5 列保持 2，6 列可能改为 3

; 整体高度（可选调整）
(def keyboard-z-offset 23.5)   ; 根据个人喜好调整
```

### 3. 轨迹球选项

如果你不需要轨迹球，可以禁用：

```clojure
(def trackball-enabled false)  ; 禁用轨迹球
```

禁用后，拇指区会恢复完整的按键布局。

### 4. 热插拔座类型

选择你要使用的热插拔座类型：

```clojure
; 使用 Kailh 官方热插拔座（推荐）
(def printed-hotswap? false)

; 或使用 3D 打印热插拔座
(def printed-hotswap? true)
```

### 5. 生成模型

```bash
# 确保在项目根目录
cd ~/proj/tractyl-manuform-keyboard

# 生成 .scad 文件
lein generate

# 等待生成完成...
```

### 6. 检查生成的文件

```bash
ls things/
# 应该看到：
# - right.scad
# - left.scad
# - right-plate.scad
# - left-plate.scad
# - 其他部件...
```

### 7. 转换为 STL

```bash
# 单个文件转换
openscad -o things/right.stl things/right.scad

# 或批量转换（后台运行）
openscad -o things/right.stl things/right.scad &
openscad -o things/left.stl things/left.scad &
openscad -o things/right-plate.stl things/right-plate.scad &
openscad -o things/left-plate.stl things/left-plate.scad &

# 等待所有任务完成
wait
```

## 3x5 和 3x6 的对比

| 特性 | 3x5 | 3x6 |
|------|-----|-----|
| 每侧主键区按键数 | 15 | 18 |
| 拇指区（估计） | 4 | 4 |
| 总按键数（双手） | 38 | 44 |
| 宽度 | 较窄 | 较宽 |
| 适合人群 | 小手，极简主义 | 中等手型，需要更多按键 |
| 层数需求 | 更多层 | 较少层 |
| 打印时间 | 稍短 | 稍长 |

## 3x5 布局建议

**优点：**
- 更紧凑，手指移动距离更短
- 打印材料和时间更少
- 极简美学

**缺点：**
- 需要更多按键层来访问所有功能
- 可能需要复杂的按键映射（Combo, Mod-Tap 等）
- 不适合需要大量符号输入的场景

**适用场景：**
- 编程（配合良好的 QMK keymap）
- 文字处理
- 追求极简体验

## 3x6 布局建议

**优点：**
- 更多按键，减少层切换
- 更接近传统键盘布局
- 常用符号可以直接访问

**缺点：**
- 稍宽，可能增加手指移动
- 打印时间和材料更多

**适用场景：**
- 需要经常输入符号（编程、数据输入）
- 从传统键盘过渡
- 希望减少学习曲线

## 拇指区布局

原项目的拇指区设计为 4 键配置（启用轨迹球时为 3 键 + 轨迹球）。

你可以在代码中找到拇指键的定义：

```clojure
; 搜索 "thumb" 相关的定义
; 主要在以下函数中：
; - thumb-tl-place
; - thumb-mr-place
; - thumb-br-place
; - thumb-bl-place
; - thumb-tr-place
```

如果你想要自定义拇指区，需要修改这些函数的位置参数，这需要一定的 Clojure 和 3D 建模知识。

## 创建补丁文件（可选）

如果你想像项目中 4x6.patch 那样保存你的配置：

```bash
# 1. 确保当前 dactyl.clj 是原始状态
git checkout src/dactyl_keyboard/dactyl.clj

# 2. 修改为你的 3x5 或 3x6 配置

# 3. 创建补丁
git diff src/dactyl_keyboard/dactyl.clj > 3x5.patch
# 或
git diff src/dactyl_keyboard/dactyl.clj > 3x6.patch

# 4. 以后可以通过补丁应用配置
git checkout src/dactyl_keyboard/dactyl.clj
patch -p1 < 3x5.patch
lein generate
```

## QMK 配置调整

### 矩阵大小

在 QMK 的 `config.h` 中：

```c
// 3x5 配置
#define MATRIX_ROWS 6     // 每侧 3 行，共 6 行
#define MATRIX_COLS 10    // 每侧 5 列，共 10 列

// 3x6 配置
#define MATRIX_ROWS 6     // 每侧 3 行，共 6 行
#define MATRIX_COLS 12    // 每侧 6 列，共 12 列
```

### 引脚定义

#### 3x5 配置：

```c
// 行引脚（3 行）
#define MATRIX_ROW_PINS { D4, D5, D6 }

// 列引脚（5 列）
#define MATRIX_COL_PINS { A0, A1, A2, A3, F4 }
```

#### 3x6 配置：

```c
// 行引脚（3 行）
#define MATRIX_ROW_PINS { D4, D5, D6 }

// 列引脚（6 列）- 需要添加一个引脚
#define MATRIX_COL_PINS { A0, A1, A2, A3, F4, F5 }
```

### 布局宏

在键盘定义文件（如 `3x5.h`）中定义物理布局：

```c
// 3x5 示例
#define LAYOUT_3x5( \
    L00, L01, L02, L03, L04,    R00, R01, R02, R03, R04, \
    L10, L11, L12, L13, L14,    R10, R11, R12, R13, R14, \
    L20, L21, L22, L23, L24,    R20, R21, R22, R23, R24, \
              L30, L31,              R30, R31             \
) { \
    { L00, L01, L02, L03, L04 }, \
    { L10, L11, L12, L13, L14 }, \
    { L20, L21, L22, L23, L24 }, \
    { KC_NO, KC_NO, KC_NO, L30, L31 }, \
    { R00, R01, R02, R03, R04 }, \
    { R10, R11, R12, R13, R14 }, \
    { R20, R21, R22, R23, R24 }, \
    { KC_NO, KC_NO, KC_NO, R30, R31 }  \
}
```

根据你实际的拇指区布局调整 L30, L31, R30, R31 的位置。

## 测试和调试

### 1. 预览 3D 模型

```bash
# 使用 OpenSCAD GUI 预览
openscad things/right.scad &

# 在 OpenSCAD 中：
# - F5: 快速预览
# - F6: 完整渲染
# - 旋转视角检查所有细节
```

### 2. 打印测试件

建议先打印一个小的测试部件：

```bash
# 只打印一个键位测试间距
# 或打印 tent-foot 等小部件测试打印质量
```

### 3. 使用测试文件

项目提供了 `things/right-test.scad`，可以用来预览键盘布局和手部模型：

```bash
openscad things/right-test.scad &
```

在这个文件中，你可以看到键位和手指的匹配情况，帮助你调整参数。

## 常见调整

### 键位间距太紧

增加间距：

```clojure
(def extra-width 2.5)    ; 增加这个值，例如 3.0
(def extra-height 1.0)   ; 增加这个值，例如 1.5
```

### 键盘太平坦或太弯曲

调整曲率：

```clojure
(def α (/ π 8))    ; 列弯曲，增加数值增加弯曲
(def β (/ π 26))   ; 行弯曲，增加数值增加弯曲
```

### 键盘整体高度

```clojure
(def keyboard-z-offset 23.5)  ; 增加提高，减少降低
```

### 倾角

```clojure
(def tenting-angle (deg2rad 20))  ; 增加数值增加倾斜
```

## 推荐工作流程

1. **小改动测试：** 每次只修改一两个参数
2. **快速预览：** 使用 `lein auto generate` 和 OpenSCAD 的自动重载
3. **打印测试：** 打印小部件或单个键位测试手感
4. **记录调整：** 在注释中记录每次修改的原因和效果
5. **完整打印：** 确认参数后再打印完整键盘

## 进一步定制

如果你想要更深入的定制（如修改拇指区布局、添加额外的键、改变整体形状），你需要：

1. 学习 Clojure 基础语法
2. 理解 scad-clj 库的用法
3. 研究 `dactyl.clj` 中的函数定义
4. 使用 `right-test.scad` 进行可视化调试

参考资源：
- [Clojure 文档](https://clojure.org/guides/getting_started)
- [scad-clj GitHub](https://github.com/farrellm/scad-clj)
- [OpenSCAD 教程](https://openscad.org/documentation.html)

## 预计时间消耗

### 配置和生成模型

- 修改配置：5-10 分钟
- 生成 .scad：1-2 分钟
- 预览和调整：30-60 分钟（多次迭代）
- 导出 STL：每个文件 5-15 分钟

### 打印

- 测试件：1-3 小时
- 完整键盘（3x5）：约 50-60 小时
- 完整键盘（3x6）：约 60-70 小时

### 布线和组装

- 焊接：每侧 8-12 小时
- 组装：2-4 小时
- 调试：2-6 小时

**总计：** 约 1-2 周（包括打印和等待部件）

---

祝你定制顺利！
