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

**v20260607.1**
* 现已同步重庆轨道线路信息。
* 添加了出站、设施种类组件（部分图标来自Wikimedia Common），以及更多进站组件。
* 重写了左栏信息部分。
* 一些样式更新和功能改善。

## To-do / Roadmap

- [x] 一个可拖拽放置组件的“标牌”画布
- [x] 使用属性菜单方便的修改组件属性
- [x] 支持导出能用的SVG文件
- [x] 解决缩放导致画布与描边出现空隙的问题
- [x] 制作所有分区的导视组件
- [x] 支持移动分区位置
- [x] 合并区域的字宽、对齐设置为区域设置，原标题改为label；高级选项新增固定宽度（选中后出现输入框）选项，文字间距输入框
- [ ] 属性面板的调整项应该有记忆，而不是每次都回到默认项。
- [ ] 支持自定义画布宽度
- [ ] 找个地方放下版本号
- [ ] 设定面板——自定义背景色、前景色、字体、字重等参数
- [ ] 按照重庆轨道GK导视标准调节默认样式，并制作各个标牌样式
- [ ] 制作“预设标牌”功能，支持使用预制标牌json快速排版
- [ ] 支持上传本地图标
- [ ] 导出面板重置——支持调整输出图片大小
- [ ] 暗色模式支持，多语言支持
- [ ] 完成Addon支持 - 制作一个Sample Addon（申通）
- [ ] 写一个操作指南
- [ ] 支持手机端操作
- [ ] v1.0 发布
- [ ] 制作更多Addon - JR / MTR / ...
