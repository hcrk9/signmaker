# 清空画布功能实现说明

## 功能概述

为项目操作栏添加了"清空"按钮，用户可以一键清空画布上的所有内容，包括所有区域（zone）和组件。

## 实现细节

### 1. HTML 结构

在 [index.html](file://t:/Documents/GitHub/signmaker/index.html#L207-L218) 的项目操作栏中已存在清空按钮：

```html
<div class="projectActions">
    <button id="btnClearAll">清空</button>
    <button id="btnSave">保存</button>
    <span class="divider">·</span>
    <div class="exportWrapper">
        <button id="btnExport">导出</button>
        ...
    </div>
</div>
```

### 2. JavaScript 实现

#### 事件绑定

在 DOMContentLoaded 事件中绑定了清空按钮的点击事件（[index.html](file://t:/Documents/GitHub/signmaker/index.html#L2605-L2614)）：

```javascript
// Clear all binding
const btnClearAll = document.getElementById('btnClearAll');
if (btnClearAll) {
    btnClearAll.addEventListener('click', () => {
        if (confirm('确定要清空画布所有内容吗？\n此操作不可撤销。')) {
            clearCanvas();
        }
    });
}
```

**确认对话框：**
- 使用浏览器原生的 `confirm()` 函数
- 提示用户此操作不可撤销
- 用户点击"确定"后执行清空操作
- 用户点击"取消"则不执行任何操作

#### 核心函数：clearCanvas()

位置：[index.html](file://t:/Documents/GitHub/signmaker/index.html#L2095-L2134)

```javascript
function clearCanvas() {
    const canvas = document.getElementById('canvas');
    if (!canvas) return;
    
    // Remove all child elements except hangers and lighting overlay
    const children = Array.from(canvas.children);
    children.forEach(child => {
        // Keep hanger elements and lighting overlay
        if (!child.classList.contains('hanger') && 
            !child.classList.contains('lighting-overlay') &&
            !child.classList.contains('insert-line')) {
            child.remove();
        }
    });
    
    // Clear selection
    selectElement(null);
    
    // Reset property panel
    const container = document.getElementById('featureControls');
    if (container) {
        container.innerHTML = '<span style="color: #5b5b5b;text-align: center;">未选中组件</span>';
    }
    const panelTitle = document.getElementById('propertyPanelTitle');
    if (panelTitle) {
        panelTitle.textContent = '属性面板';
    }
    
    // Disable quick action buttons
    const btnDuplicate = document.getElementById('btnDuplicate');
    const btnDelete = document.getElementById('btnDelete');
    const btnLeft = document.getElementById('btnLeft');
    const btnRight = document.getElementById('btnRight');
    
    if (btnDuplicate) btnDuplicate.disabled = true;
    if (btnDelete) btnDelete.disabled = true;
    if (btnLeft) btnLeft.disabled = true;
    if (btnRight) btnRight.disabled = true;
}
```

### 3. 功能流程

#### 步骤 1：用户点击"清空"按钮
- 触发点击事件监听器
- 弹出确认对话框

#### 步骤 2：用户确认操作
- 点击"确定"：继续执行清空流程
- 点击"取消"：中止操作，不做任何改变

#### 步骤 3：清空画布内容
1. **移除画布子元素**
   - 遍历 `#canvas` 的所有子元素
   - 保留以下特殊元素：
     - `.hanger` - 吊杆元素（左右吊杆）
     - `.lighting-overlay` - 光照效果层
     - `.insert-line` - 拖拽插入指示线
   - 移除其他所有元素（所有 zone 和组件）

2. **清除选中状态**
   - 调用 `selectElement(null)` 清除当前选中的元素
   - 清除元素的高亮边框

3. **重置属性面板**
   - 将属性面板内容重置为"未选中组件"
   - 将属性面板标题重置为"属性面板"

4. **禁用快捷操作按钮**
   - 禁用"复制"按钮（`#btnDuplicate`）
   - 禁用"删除"按钮（`#btnDelete`）
   - 禁用"左移"按钮（`#btnLeft`）
   - 禁用"右移"按钮（`#btnRight`）

### 4. 保留的元素

清空操作会保留以下画布内的特殊元素：

| 元素类名 | 用途 | 原因 |
|---------|------|------|
| `.hanger` | 吊杆装饰 | UI 装饰元素，不属于用户内容 |
| `.lighting-overlay` | 光照效果层 | 视觉效果层，不属于用户内容 |
| `.insert-line` | 拖拽插入指示线 | 临时交互元素，不影响内容 |

### 5. 用户体验设计

#### 安全保护
- ✅ **二次确认**：必须通过确认对话框才能执行清空
- ✅ **明确提示**：告知用户"此操作不可撤销"
- ✅ **可取消**：用户可以随时取消操作

#### 完整清理
- ✅ 清除所有用户添加的内容
- ✅ 清除选中状态
- ✅ 重置属性面板
- ✅ 禁用相关按钮

#### 视觉反馈
- 清空后画布立即变为空白
- 属性面板显示"未选中组件"
- 快捷操作按钮变为禁用状态（灰色）

### 6. 与其他功能的协调

#### 与保存功能的关系
- 清空前建议用户先保存（如果需要）
- 清空后保存会得到一个空白的画布文件

#### 与导入功能的关系
- 清空后可以导入新的项目文件
- 导入会自动替换当前画布内容

#### 与导出功能的关系
- 清空后导出的图片将是空白的
- 建议在清空前导出需要的内容

#### 与设置面板的关系
- 清空操作不会影响自定义样式设置
- 字体、颜色等设置保持不变

### 7. 边界情况处理

#### 空画布时点击清空
- 仍然会弹出确认对话框
- 确认后不会报错，只是再次清空已经是空的画布

#### 有选中元素时清空
- 自动清除选中状态
- 不会出现悬空的选中引用

#### 属性面板打开时清空
- 自动重置属性面板
- 不会显示已删除元素的属性

### 8. 测试建议

1. **基本功能测试**
   - 在有内容的画布上点击"清空"
   - 确认对话框出现
   - 点击"确定"，检查画布是否清空
   - 点击"取消"，检查画布是否保持不变

2. **完整性测试**
   - 添加多个区域和组件
   - 清空后检查是否所有内容都被移除
   - 检查吊杆和光照效果是否保留

3. **UI 状态测试**
   - 选中一个组件后清空
   - 检查选中状态是否清除
   - 检查属性面板是否重置
   - 检查快捷按钮是否禁用

4. **边界情况测试**
   - 在空白画布上点击清空
   - 连续多次点击清空
   - 清空后立即添加新组件

5. **恢复测试**
   - 清空后刷新页面
   - 检查是否能从 localStorage 恢复（如果之前保存过）

## 注意事项

1. **不可撤销**：清空操作没有撤销功能，用户需要谨慎操作
2. **不影响设置**：清空只移除画布内容，不影响自定义样式设置
3. **保留装饰**：吊杆和光照效果等装饰元素会被保留
4. **建议保存**：清空前建议用户先保存重要内容

## 未来改进建议

1. **添加撤销功能**：可以考虑实现简单的撤销机制
2. **快捷键支持**：可以添加键盘快捷键（如 Ctrl+Shift+X）
3. **批量操作提示**：如果画布内容很多，可以显示将要删除的项目数量
4. **自动保存草稿**：清空前自动保存草稿到 localStorage
