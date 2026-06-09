# 设置面板功能实现说明

## 功能概述

已成功实现了一个功能完整的设置面板（Settings Panel），允许用户自定义标牌的字体、字重和颜色样式。

## 主要功能

### 1. 交互逻辑

- **默认状态**：设置面板默认隐藏
- **触发机制**：点击 `#settingsBtn`（设置图标按钮）切换设置面板的显示/隐藏状态
- **互斥显示**：
  - 设置面板显示时，组件面板（`#categoriesList` 和 `#componentsList`）自动隐藏
  - 组件面板显示时，设置面板自动隐藏
- **退出方式**：只能通过点击"取消修改"或"保存修改"按钮退出设置页面

### 2. 样式自定义

#### 字体·字重设定
- **大号文本**：对应 `.largeText` 类元素（数字）
  - 字体选择器：`#largeTextFont`
  - 字重选择器：`#largeTextWeight`（100-900，步长100）
  
- **中号文本**：对应 `.textCN` 类元素（中文）
  - 字体选择器：`#medTextFont`
  - 字重选择器：`#medTextWeight`（100-900，步长100）
  
- **小号文本**：对应 `.textEN` 类元素（西文）
  - 字体选择器：`#smallTextFont`
  - 字重选择器：`#smallTextWeight`（100-900，步长100）

#### 背景色·前景色设定
- **进站区域**：`#enterZoneBgColor` / `#enterZoneColor`
- **出站区域**：`#exitZoneBgColor` / `#exitZoneColor`
- **设施区域**：`#facilityZoneBgColor` / `#facilityZoneColor`
- **基础区域**：`#basicZoneBgColor` / `#basicZoneColor`

### 3. 预览画布

- **位置**：`.stylePreviewCanvas`
- **缩放**：使用 `transform: scale(0.5)` 缩小为实际画布的 0.5 倍
- **实时更新**：任何输入框的修改都会立即反映在预览画布中
- **包含示例**：
  - 进站区域示例（白色背景）
  - 出站区域示例（黄色背景）
  - 设施区域示例（深色背景）

### 4. 应用逻辑

#### 重置设定（`#btnResetSettings`）
- 弹出确认对话框
- 将所有样式设置恢复到初始默认状态
- 清除 localStorage 中保存的自定义样式
- 更新输入框显示默认值
- 实时更新预览画布
- **注意**：不会重置语言设置

#### 取消修改（`#btnCancelSettings`）
- 关闭设置面板
- 恢复到打开设置面板前的样式状态
- 不保存任何更改

#### 保存修改（`#btnSaveSettings`）
- 将当前输入框中的所有设置应用到主画布 `#canvas`
- 将设置保存到 `localStorage`（键名：`customStyles`）
- 关闭设置面板
- 下次打开页面时自动加载保存的样式

### 5. 技术实现

#### CSS 变量系统
使用 CSS 自定义属性实现样式动态更新：

```css
/* 字体变量 */
--large-text-font: 'Noto Sans';
--large-text-weight: 500;
--med-text-font: 'IBM Plex Sans SC';
--med-text-weight: 400;
--small-text-font: 'Geist';
--small-text-weight: 400;

/* 颜色变量 */
--enter-bg-color: #ffffff;
--enter-color: #000000;
--exit-bg-color: #FFD701;
--exit-color: #000000;
--facility-bg-color: #333F48;
--facility-color: #ffffff;
--basic-bg-color: #ffffff;
--basic-color: #000000;
```

#### JavaScript 核心函数

1. **getCurrentCustomStyles()**：获取当前画布的自定义样式
2. **getSettingsFromInputs()**：从输入框读取当前设置
3. **applyCustomStyles(settings)**：将设置应用到主画布
4. **initializePreview()**：初始化预览画布
5. **loadCurrentStylesToInputs()**：将当前样式加载到输入框
6. **setupPreviewListeners()**：设置预览实时更新监听器
7. **updatePreviewFromInputs()**：根据输入更新预览画布
8. **loadSavedStyles()**：从 localStorage 加载保存的样式

### 6. 文件修改清单

#### HTML (index.html)
- 修复了重复的 ID（`enterColorPreference` → `exitColorPreference`, `facilityColorPreference`, `basicColorPreference`）
- 为所有输入框添加了唯一的 ID
- 为按钮添加了 ID（`btnCancelSettings`, `btnSaveSettings`）
- 修复了多余的引号（`<div class="textBox"">` → `<div class="textBox">`）

#### CSS (global.css)
- 添加 `.settingsPanel.active` 和 `.componentsPanel.hidden` 控制显示/隐藏
- 添加 `.settingsBtnWrapper` 及其子元素样式
- 添加 `.settingsGroup` 及相关标题样式
- 添加 `.stylePreviewCanvas` 及其子元素的完整样式
- 更新 `.zone[data-category]` 使用 CSS 变量
- 更新 `.largeText`, `.textCN`, `.textEN` 使用 CSS 变量

#### JavaScript (index.html script)
- 添加完整的设置面板交互逻辑（约 320 行代码）
- 实现预览实时更新功能
- 实现 localStorage 持久化存储

### 7. 注意事项

1. **通用设定部分**："显示语言"和"页面配色"目前仅保留 UI，未实现具体功能
2. **字体名称**：用户需要输入准确的字体名称（如 "Noto Sans", "IBM Plex Sans SC", "Geist"）
3. **颜色格式**：使用十六进制颜色值（如 #ffffff）
4. **浏览器兼容性**：使用了 CSS 变量和 localStorage，需要现代浏览器支持
5. **初始值**：首次使用时会加载默认样式，后续使用会加载保存的自定义样式

### 8. 使用流程

1. 点击左上角的设置图标按钮
2. 在设置面板中调整字体、字重和颜色
3. 观察上方预览画布的实时变化
4. 如需恢复到初始状态，点击“重置设定”按钮（会弹出确认对话框）
5. 满意后点击“保存修改”按钮
6. 如需放弃更改，点击“取消修改”按钮

## 测试建议

1. 测试设置面板的显示/隐藏切换
2. 测试预览画布的实时更新
3. 测试重置功能（点击重置按钮，确认对话框，检查是否恢复到默认样式）
4. 测试保存功能（刷新页面后检查样式是否保持）
5. 测试取消功能（修改后取消，检查是否恢复原状）
6. 测试不同字体和颜色的组合效果
