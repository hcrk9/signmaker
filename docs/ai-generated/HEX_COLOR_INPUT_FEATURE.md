# HEX 颜色输入功能实现说明

## 功能概述

为设置面板中的"背景色·前景色设定"部分添加了十六进制颜色值快速输入功能，用户可以通过文本框直接输入 HEX 颜色值，或通过颜色选择器选取颜色，两者实时同步。

## 实现细节

### 1. HTML 结构调整

在每个颜色设置组中，为每个 `<input type="color">` 添加了对应的 `<input type="text">`：

```html
<div class="controlRow" id="enterColorPreference">
    <label>进站区域</label>
    <input type="color" name="" id="enterZoneBgColor">
    <input type="text" id="enterZoneBgColorHex" placeholder="#ffffff" maxlength="7">
    <span>·</span>
    <input type="color" name="" id="enterZoneColor">
    <input type="text" id="enterZoneColorHex" placeholder="#000000" maxlength="7">
</div>
```

**新增的 HEX 输入框 ID：**
- `enterZoneBgColorHex` / `enterZoneColorHex` - 进站区域
- `exitZoneBgColorHex` / `exitZoneColorHex` - 出站区域
- `facilityZoneBgColorHex` / `facilityZoneColorHex` - 设施区域
- `basicZoneBgColorHex` / `basicZoneColorHex` - 基础区域

### 2. CSS 样式

为 HEX 输入框添加了专用样式（[global.css](file://t:/Documents/GitHub/signmaker/global.css)）：

```css
/* HEX color input specific styling */
.controlRow input[type="text"][id$="Hex"] {
    width: 60px;
    flex: none;
    text-align: center;
    font-family: monospace;
    font-size: 11px;
}

.controlRow input[type="text"][id$="Hex"].invalid {
    border-color: var(--color-danger);
    background-color: rgba(255, 77, 79, 0.05);
}
```

**样式特点：**
- 固定宽度 60px，确保布局紧凑
- 居中对齐，便于阅读
- 使用等宽字体（monospace），方便查看 HEX 值
- 较小的字号（11px），节省空间
- 无效输入时显示红色边框和淡红色背景

### 3. JavaScript 功能实现

#### 核心函数

**setupHexColorInputs()**
- 初始化所有 HEX 输入框与颜色选择器的绑定
- 设置初始值为颜色选择器的当前值（大写格式）
- 监听颜色选择器的变化，同步更新 HEX 输入框
- 监听 HEX 输入框的变化，验证并更新颜色选择器

**validateAndUpdateColor(hexInput, colorInput)**
- 验证 HEX 颜色值的合法性
- 支持 3 位和 6 位 HEX 值（如 `#FFF` 或 `#FFFFFF`）
- 支持带或不带 `#` 前缀的输入
- 自动将 3 位 HEX 扩展为 6 位
- 无效输入时添加 `.invalid` 类显示错误状态
- 有效输入时触发预览更新

**syncColorToHex(colorInputId)**
- 当颜色选择器改变时，同步更新对应的 HEX 输入框
- 清除无效状态标记

#### 事件监听

1. **颜色选择器 → HEX 输入框**
   ```javascript
   colorInput.addEventListener('input', () => {
       hexInput.value = colorInput.value.toUpperCase();
       hexInput.classList.remove('invalid');
   });
   ```

2. **HEX 输入框 → 颜色选择器**
   - `blur` 事件：失去焦点时验证并更新
   - `keydown` 事件：按下 Enter 键时验证并更新
   - `input` 事件：输入时清除无效状态

### 4. 输入验证规则

**合法的 HEX 颜色格式：**
- ✅ `#FFF` - 3 位简写
- ✅ `#fff` - 3 位小写
- ✅ `#FFFFFF` - 6 位完整
- ✅ `#ffffff` - 6 位小写
- ✅ `FFF` - 不带 # 的 3 位
- ✅ `FFFFFF` - 不带 # 的 6 位

**非法的 HEX 颜色格式：**
- ❌ `#GGG` - 包含非十六进制字符
- ❌ `#FFFF` - 4 位（不支持）
- ❌ `#FFFFFFFF` - 8 位（不支持透明度）
- ❌ `red` - 颜色名称（不支持）

**验证后的处理：**
- 合法输入：自动规范化为大写 6 位格式（如 `#FFF` → `#FFFFFF`）
- 非法输入：输入框边框变红，背景变为淡红色，不更新颜色选择器

### 5. 双向同步流程

#### 场景 1：使用颜色选择器
1. 用户点击颜色选择器选取颜色
2. 颜色选择器的值改变
3. 触发 `input` 事件
4. HEX 输入框自动更新为大写的 HEX 值
5. 清除无效状态
6. 触发预览画布更新

#### 场景 2：使用 HEX 输入框
1. 用户在 HEX 输入框中输入颜色值（如 `#FFD701`）
2. 按下 Enter 键或输入框失去焦点
3. 触发验证函数
4. 如果合法：
   - 更新颜色选择器的值
   - 规范化 HEX 值为大写格式
   - 清除无效状态
   - 触发预览画布更新
5. 如果非法：
   - 添加 `.invalid` 类
   - 显示红色边框和背景
   - 不更新颜色选择器
   - 不触发预览更新

### 6. 默认值

HEX 输入框的初始值与 `defaultStyles` 常量保持一致：

```javascript
const defaultStyles = {
    enterBgColor: '#ffffff',
    enterColor: '#000000',
    exitBgColor: '#FFD701',
    exitColor: '#000000',
    facilityBgColor: '#333F48',
    facilityColor: '#ffffff',
    basicBgColor: '#ffffff',
    basicColor: '#000000'
};
```

### 7. 用户体验优化

1. **实时反馈**：颜色选择器改变时，HEX 输入框立即更新
2. **灵活输入**：支持多种 HEX 格式，自动规范化
3. **视觉提示**：非法输入时有明显的红色警告
4. **便捷操作**：支持 Enter 键快速确认
5. **容错处理**：自动添加缺失的 `#` 前缀
6. **智能扩展**：自动将 3 位 HEX 扩展为 6 位

## 测试建议

1. **基本功能测试**
   - 通过颜色选择器改变颜色，检查 HEX 输入框是否同步更新
   - 在 HEX 输入框中输入合法颜色值，检查颜色选择器是否同步更新
   - 检查预览画布是否实时更新

2. **输入验证测试**
   - 输入 `#FFF`，应自动转换为 `#FFFFFF`
   - 输入 `fff`，应自动转换为 `#FFFFFF`
   - 输入 `#GGG`，应显示红色错误状态
   - 输入空值，应显示红色错误状态

3. **边界情况测试**
   - 输入不带 `#` 的颜色值（如 `FFD701`）
   - 输入小写字母（如 `#ffd701`）
   - 输入混合大小写（如 `#FfD701`）
   - 在输入过程中修改为非法值，然后改回合法值

4. **重置功能测试**
   - 修改颜色后点击"重置设定"，检查 HEX 输入框是否恢复默认值
   - 检查颜色选择器是否同步恢复

5. **保存/取消测试**
   - 修改颜色后保存，刷新页面检查是否保持
   - 修改颜色后取消，检查是否恢复到打开前的状态

## 注意事项

1. **不支持的颜色格式**：RGB、HSL、颜色名称等格式不被支持，仅支持 HEX 格式
2. **不支持透明度**：8 位 HEX（带 alpha 通道）不被支持
3. **大小写规范**：内部统一使用大写格式显示，但接受任何大小写输入
4. **浏览器兼容性**：使用了现代 CSS 选择器和正则表达式，需要现代浏览器支持
