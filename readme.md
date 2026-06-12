# Sign Maker by Chitose.City

这是一个基于HTML + JS的导视标牌生成器，目前只支持桌面端使用。

操作逻辑类似于Auto Layout + 组件库，如果使用过Figma应该挺容易习惯的。

**fork前注意：源代码写得很烂。**

A HTML + JS based viewfinding signages maker. Currently only for desktop.

The idea is inspired by Auto Layout & Components Library, so if you used Figma this should be pretty easy to use.

**Warning before you fork: Really bad codes ahead.**

## 功能 Features

* 通过组件系统直观的制作、编辑导视标牌。
* 内置光照，吊杆效果，模拟现实导视标牌效果。
* 导出标牌为PNG / SVG格式图片。
* 通过Add-on系统，制作你想象中的导视标牌。（制作中）

* Use the Component system to make & edit signage.
* Built-in lighting & hanger effect, simulating real-life signages.
* Export your signs as PNG or SVG file.
* Build your dream signages with the Add-on system. (WIP)

## 更新 Update log

### v20260612.1

* 添加了分割线组件。Added Divider component.
* 一些样式修正。General Fix.

### v20260611.2

* 添加了自定义图标功能，可以上传SVG格式文件作为图标。 Added Custom Icon (SVG) Upload feature.
* 设定菜单现在支持调整光照效果和导出位图缩放倍率。New settings for adjusting lighting effect & export bitmap scale.

### v20260611.1

* 添加了预设标牌功能，你可以快速的创建符合标准的标牌。Added Preset Signs feature.
* 修复导出时CSS filter失效问题。Fixed CSS filter export issue.
* 画布内图标现在使用SVG渲染，实现了更好的导出效果。Icons in canvas are now SVGs instead of img.

### v20260610.1

* 完整适配了TypeB标牌。Supported TypeB Signage.
* 支持在signs.json配置创建自定义HTML内容。Added CustomHTML parameter in signs.json.

### v20260609.1

* 添加了文字间距设置。Letter spacing now can be configured.
* 新增固定宽度选项，适用于组件和区域。Static width for components & zones.
* 支持自定义画布宽度。Custom canvas width.
* 新增设定面板-样式自定义部分。New settings panel with style customization.
* 修改了描边逻辑为svg，添加了描边渐变效果。Borders are now rendered with SVG, supporting gradients.

## To-do / Roadmap

- [x] 一个可拖拽放置组件的“标牌”画布
- [x] 使用属性菜单方便的修改组件属性
- [x] 支持导出能用的SVG文件
- [x] 制作所有分区的导视组件
- [x] 解决缩放导致画布与描边出现空隙的问题
- [x] 合并字宽、对齐设置为区域设置，添加字间距设置；高级选项新增固定宽度选项
- [x] 支持自定义画布宽度
- [x] 设定面板——自定义背景色、前景色、字体、字重等参数
- [x] 支持保存json到浏览器内
- [x] 按照重庆轨道GK导视标准调节默认样式，并制作各个标牌样式
- [x] 制作“预设标牌”功能，支持使用预制标牌json快速排版
- [x] 设置菜单支持调整输出PNG大小
- [x] 设置菜单光照设置
- [x] 导出保存菜单互斥
- [x] 支持上传本地图标
- [x] 添加分割线组件
- [ ] 支持纵向复制画布（分行效果）
- [ ] 暗色模式支持，多语言支持
- [ ] 完成Addon支持 - 制作一个Sample Addon（申通）
- [ ] 写一个操作指南
- [ ] 支持手机端操作
- [ ] v1.0 发布
- [ ] 制作更多Addon - JR / MTR / ...
