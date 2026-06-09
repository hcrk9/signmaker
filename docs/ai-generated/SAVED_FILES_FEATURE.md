# 标牌保存与管理功能实现说明

## 功能概述

实现了基于浏览器 localStorage 的标牌保存与管理功能，用户可以将标牌保存到浏览器本地存储中，并随时查看、打开或删除已保存的标牌。

## 主要功能

### 1. 保存按钮改造

#### HTML 结构修改
将原来的单一保存按钮改为下拉菜单形式（[index.html](file://t:/Documents/GitHub/signmaker/index.html#L227-L241)）：

```html
<div class="saveWrapper">
    <button id="btnSave">保存</button>
    <div class="saveDropdown" id="saveDropdown">
        <button id="btnSaveToFile">保存到本地</button>
        <button id="btnSaveToBrowser">保存到浏览器内</button>
    </div>
</div>
```

#### 交互逻辑
- 点击"保存"按钮 → 显示下拉菜单
- 点击菜单外部 → 关闭下拉菜单
- 选择"保存到本地" → 下载 JSON 文件（原有功能）
- 选择"保存到浏览器内" → 保存到 localStorage（新功能）

### 2. 已保存文件面板

#### 触发机制
- 点击左上角的 `#savedFileBtn` 按钮显示面板
- 再次点击隐藏面板

#### 互斥显示逻辑
三个面板互斥显示：
- `.componentsPanel` - 组件面板
- `.settingsPanel` - 设置面板
- `.savedFilePanel` - 已保存文件面板

当显示其中一个时，其他两个自动隐藏。

#### UI 结构
遵循 [index.html](file://t:/Documents/GitHub/signmaker/index.html#L27-L33) 的结构：

```html
<div class="savedFilePanel">
    <h2>已保存文件 Saved Files</h2>
    <div class="savedFileList">
        <span id="noSavedFile">暂无保存的标牌。</span>
        <!-- 动态生成的卡片 -->
    </div>
</div>
```

#### 卡片内容
每个 `.savedFileCard` 包含：
- **标牌名称** (`.savedFileTitle`)
- **保存时间** (`.savedFileDateTime`) - 格式：YYYY/MM/DD HH:MM:SS
- **打开按钮** (`.savedFileOpenBtn`) - 加载标牌到画布
- **删除按钮** (`.savedFileDeleteBtn`) - 从 localStorage 删除

### 3. 数据存储规范

#### localStorage Key
```javascript
'signmaker_saved_files'
```

#### 数据结构
```javascript
[
  {
    id: "1234567890",           // 唯一标识（时间戳字符串）
    name: "未命名标牌",          // 标牌名称
    timestamp: 1234567890,      // 保存时间戳
    data: { ... }               // 序列化的画布数据
  },
  ...
]
```

#### 存储限制
- 最多保存 50 个标牌
- 超出限制时自动删除最旧的标牌

### 4. 核心函数

#### saveCanvasToBrowser()
将当前画布保存到 localStorage：
```javascript
function saveCanvasToBrowser() {
    const data = serializeCanvas();
    const savedFiles = getSavedFiles();
    
    const newFile = {
        id: Date.now().toString(),
        name: data.projectName || '未命名标牌',
        timestamp: Date.now(),
        data: data
    };
    
    savedFiles.unshift(newFile);
    
    if (savedFiles.length > 50) {
        savedFiles.pop();
    }
    
    localStorage.setItem('signmaker_saved_files', JSON.stringify(savedFiles));
    alert(`已成功保存到浏览器！\n标牌名称：${newFile.name}`);
}
```

#### getSavedFiles()
从 localStorage 读取已保存的文件列表：
```javascript
function getSavedFiles() {
    const stored = localStorage.getItem('signmaker_saved_files');
    return stored ? JSON.parse(stored) : [];
}
```

#### renderSavedFilesList()
渲染已保存文件列表：
- 清空现有列表
- 若无文件，显示提示文本
- 若有文件，为每个文件创建卡片

#### createSavedFileCard(file)
创建单个文件卡片：
- 生成 HTML 结构
- 格式化显示时间
- 绑定"打开"和"删除"按钮事件

#### loadCanvasFromBrowser(fileId)
从 localStorage 加载标牌：
- 查找对应的文件数据
- 弹出确认对话框
- 调用 `deserializeCanvas()` 还原画布
- 关闭面板，显示组件面板

#### deleteSavedFile(fileId)
删除指定的标牌：
- 从数组中过滤掉该文件
- 更新 localStorage
- 重新渲染列表

### 5. CSS 样式

#### 保存下拉菜单
参考导出下拉菜单的样式（[global.css](file://t:/Documents/GitHub/signmaker/global.css)）：
```css
.saveWrapper {
    position: relative;
}

.saveDropdown {
    position: absolute;
    top: 100%;
    right: 0;
    background: var(--bg-canvas-container);
    border: 1px solid var(--border-color-light);
    box-shadow: 2px 2px 8px rgba(0, 0, 0, 0.2);
    display: none;
    flex-flow: column nowrap;
    z-index: 20;
    margin-top: 4px;
}

.saveDropdown.show {
    display: flex;
}
```

#### 已保存文件面板
```css
.savedFilePanel {
    display: none;
    flex-flow: column nowrap;
    gap: 12px;
    flex: 1;
    min-height: 0;
    overflow-y: auto;
    width: 410px;
}

.savedFilePanel.active {
    display: flex;
}

.savedFileCard {
    display: flex;
    flex-flow: column nowrap;
    gap: 6px;
    padding: 12px;
    border: 2px solid var(--border-color-default);
    border-radius: 4px;
    background: var(--bg-panel);
    transition: all 0.2s ease;
}

.savedFileCard:hover {
    background: var(--bg-item-hover);
    border-color: var(--border-color-dark);
}

.savedFileDeleteBtn:hover {
    background: var(--color-danger) !important;
    color: var(--text-light) !important;
}
```

### 6. 用户操作流程

#### 保存标牌到浏览器
1. 完成标牌设计
2. 点击右下角"保存"按钮
3. 在下拉菜单中选择"保存到浏览器内"
4. 看到成功提示："已成功保存到浏览器！标牌名称：XXX"

#### 查看已保存的标牌
1. 点击左上角的"已保存文件"图标
2. 显示已保存文件面板
3. 浏览所有保存的标牌列表

#### 打开已保存的标牌
1. 在已保存文件面板中找到目标标牌
2. 点击"打开"按钮
3. 确认对话框："确定要打开标牌"XXX"吗？当前画布的内容将会被替换。"
4. 点击"确定"，标牌加载到画布
5. 自动关闭面板，返回组件面板

#### 删除已保存的标牌
1. 在已保存文件面板中找到目标标牌
2. 点击"删除"按钮
3. 确认对话框："确定要删除标牌"XXX"吗？"
4. 点击"确定"，标牌从列表中移除

### 7. 特性与优势

#### ✅ 便捷性
- 无需下载文件即可保存多个版本
- 快速切换不同的标牌设计
- 一键打开之前保存的工作

#### ✅ 安全性
- 二次确认防止误操作
- 删除操作有明确提示
- 打开标牌前警告会替换当前内容

#### ✅ 用户体验
- 清晰的时间戳显示
- 悬停效果提供视觉反馈
- 删除按钮红色高亮警示
- 空状态友好提示

#### ✅ 性能优化
- 限制最多 50 个文件，避免 localStorage 溢出
- 最新的文件排在前面
- 滚动条样式与整体风格一致

### 8. 边界情况处理

#### localStorage 不可用
- 浏览器禁用 localStorage 时会抛出错误
- 建议添加 try-catch 处理（未来改进）

#### 存储空间不足
- 超过 50 个文件时自动删除最旧的
- localStorage 总容量通常为 5-10MB

#### 数据损坏
- JSON 解析失败时返回空数组
- 找不到文件时显示错误提示

#### 跨标签页/会话
- localStorage 在同一浏览器所有标签页共享
- 关闭浏览器后数据仍然保留
- 清除浏览器数据会丢失所有保存的标牌

### 9. 测试建议

1. **基本功能测试**
   - 保存标牌到浏览器
   - 打开已保存文件面板
   - 检查列表是否正确显示

2. **打开功能测试**
   - 点击"打开"按钮
   - 确认对话框出现
   - 检查画布是否正确加载

3. **删除功能测试**
   - 点击"删除"按钮
   - 确认对话框出现
   - 检查列表是否更新

4. **互斥显示测试**
   - 打开已保存文件面板
   - 尝试打开设置面板
   - 检查是否正确切换

5. **边界情况测试**
   - 保存空画布
   - 保存 50+ 个标牌
   - 刷新页面后检查数据是否保持

6. **UI 测试**
   - 检查卡片悬停效果
   - 检查删除按钮红色高亮
   - 检查空状态提示

### 10. 注意事项

1. **数据持久性**
   - localStorage 数据在清除浏览器数据时会丢失
   - 重要标牌建议同时保存到本地文件

2. **存储空间**
   - 每个标牌的 JSON 数据大小不一
   - 复杂标牌可能占用较多空间
   - 定期清理不需要的标牌

3. **兼容性**
   - 需要现代浏览器支持 localStorage
   - IE11 及以下版本不支持

4. **隐私安全**
   - 数据存储在用户本地浏览器
   - 不会上传到服务器
   - 不同设备之间不共享

### 11. 未来改进建议

1. **搜索功能**：添加搜索框快速查找标牌
2. **重命名功能**：允许用户修改标牌名称
3. **缩略图预览**：显示标牌的缩略图
4. **分类标签**：支持对标牌进行分类
5. **导出全部**：一键导出所有保存的标牌
6. **导入功能**：支持从 JSON 文件导入到浏览器存储
7. **自动保存**：定时自动保存到浏览器
8. **版本历史**：保留同一标牌的多个版本
