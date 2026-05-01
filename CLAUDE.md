# CLAUDE.md

本文档为 Claude Code (claude.ai/code) 在此仓库中工作提供指导。

## 项目概述

Scratch 到 HTML 转换器 —— 解析 `.sb3` 文件（Scratch 3.0 项目格式）并生成可在浏览器中独立运行的 HTML 游戏，无需服务器。

## 架构

项目包含两个基于 HTML 的转换器工具：

### `scratch_to_html_converter.html`
- 现代化 UI，支持拖拽上传 `.sb3` 文件
- 使用 **JSZip**（通过 CDN）解压 `.sb3` 压缩包
- 解析 SB3 包中的 `project.json` 并提取素材资源（造型、声音）
- 生成自包含的 HTML 文件，游戏逻辑被转换为 JavaScript
- 支持的积木类型：
  - 运动：移动、旋转、坐标定位、获取位置、弹射
  - 外观：说话/思考气泡、切换造型、大小、特效
  - 事件：绿旗、按键、广播
  - 控制：循环、条件、等待、重复直到
  - 运算：数学运算、比较、随机数
  - 变量：读取/设置
- 局限性：
  - 声音积木未实现（需要 Web Audio API）
  - 克隆功能为基础版本
  - 视频侦测和自定义积木暂不支持

### `htmlifier-offline.html`
- HTMLifier（SheepTester）的离线版本分支
- 包含 Mr. Cringe Kid 的 CSS
- 支持更多积木类型（详见文件中的 `convertBlock` 函数）

## 生成输出结构

两个转换器生成的 HTML 文件包含：
- `<game-container>` 元素，包含舞台和角色
- Scratch 风格的 CSS 样式
- JavaScript 代码：
  - 初始化游戏循环
  - 将 Scratch 积木映射为 JS 函数
  - 处理碰撞检测和边界检查
  - 管理角色状态（位置、方向、造型、变量）

## 开发

这是一个静态 HTML/JS 项目 —— 除了 JSZip 外无需构建系统或依赖。开发方式：

1. 在浏览器中打开任一转换器 HTML 文件
2. 使用 `.sb3` 文件测试转换输出
3. 在浏览器中打开生成的 HTML 验证运行效果

## 测试

手动测试流程：
1. 通过拖拽或文件选择器上传 `.sb3` 文件
2. 下载生成的 HTML
3. 在浏览器中打开输出的 HTML
4. 验证游戏是否可运行（绿旗启动、响应控制）
