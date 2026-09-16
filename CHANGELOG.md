# Changelog

## 0.3.0

- **BREAKING removed**: `mapParseStageText(stage, parseError)`（连同内部 `stageSteps`/`STAGE_TOTAL`）——其阶段词表属于已退役的 6 段流水线（`preparing`/`converting`/`popo_normalize`/`indexing`），与现行 9 阶段（`source_prep`/…/`figure_describe`/`fts`/`vectors`/`graph`）不匹配，继续使用只会得到永远对不上的「步骤x/6」假进度。仓内与文档已全量移除（README 同步删 3 处登记，含一处失真记载：组件依赖表声称 `DocumentParsedWorkspace.vue` 使用它，实际无 import）
  - 影响面：仅直接 `import { mapParseStageText } from '@angineer/docs-ui'` 的消费方会构建报错；阶段抽屉/进度条走组件内部逻辑，不受影响（0.2.4 的 `figure_describe` 阶段抽屉修复仍然有效）
  - 迁移：进度文案请直接使用后端 `parse_stage` 字段自行映射，或按现行 9 阶段词表自建映射

## 0.2.4

- fix: 解析阶段词表补齐 `figure_describe`——阶段抽屉此前整行不渲染该阶段（状态/耗时/错误都看不到，也没有它的启动按钮），PDF_Viewer 进度条会显示原始英文 key 且分母写死 8（现为 9）
- fix: `xlsx` 依赖换成 SheetJS CDN `0.20.3` tarball（npm 版 `0.18.5` 无修复，CVE-2023-30533 / CVE-2024-22363），与 monorepo 对齐

## 0.2.3

- perf: package.json 声明 sideEffects 仅样式文件，组件模块可被 bundler tree-shake（消费方知识库关键路径实测 -895KB）
- feat: 新增 ./composables/useKnowledgeParse 子路径导出，消费方可按需导入解析 composable
- perf: PDF_Viewer 与 workspace 预览链路更新（同步 monorepo 至 2026-09-12）

## 0.2.2

- fix: 去除 package.json 的 UTF-8 BOM（0.2.1 之前发布出去的包内 manifest 带 BOM；pnpm ≥10 读本地目录/git 形式的依赖时用严格 JSON.parse，会直接报 `Unexpected token '' … is not valid JSON` —— DredgeAI 升级到 pnpm 11 时实测踩到）

## 0.2.1

- feat: npm registry 正式上架（@angineer/docs-ui），smartree 依赖以 ^0.1.1 从 registry 解析（不再依赖 git 安装）

## 0.2.0

- refactor: 移除内置 SmartTree 组件，改为依赖 @angineer/smartree（^0.1.1），与 monorepo 结构统一，SmartTree 维护单一代码来源
- 对消费方：安装 @angineer/docs-ui 时会自动安装 @angineer/smartree，对外组件 API 不变

## 0.1.9

- feat: PDF 高亮定位统一居中——引用跳转、PDF 内点击、右侧索引树三个入口的 bbox 定位行为一致（同步 monorepo 0.2.16）

## 0.1.8

- feat: 引用定位逻辑共享化（useKnowledgeCitation 统一引用定位链路）
- fix: PDF 索引树懒加载修复（PDFParsedWorkspace 结构数据未全量加载时的处理）
- 同步 monorepo 0.2.14 改动

## 0.1.7

- feat: 溯源切换 activeHighlightId 时 bbox 纵向居中（centerActiveHighlight，DredgeAI 溯源需求）

## 0.1.6

- feat: PDF_Viewer 原生全文搜索回退（调用方未传 searchText/textContent 时逐页用 pdf.js 文本内容搜索）
- feat: PDF_Viewer 新增 title prop（替代“原文”标签显示文档标题）
- feat: 知识树库根虚拟节点、引用定位归一化、检索结果公式渲染等 monorepo 近期改动

## 0.1.5

- fix: PDF 搜索命中按文本+行距解析目标高亮块，修复编辑版/解析版 markdown 行号错位导致的错页定位
- fix: 文档加载时用 pdf.js 预取全部页真实高度种入虚拟布局，跳页/bbox 定位不再依赖估算收敛
- fix: 目标页未渲染时先跳页、渲染后按真实几何再次精确定位（一次点击完成两步）
- fix: 修复虚拟占位高度被 flex 容器压缩导致整篇不可滚动、需多次点击才到位的问题
- fix: 引用跳转只高亮目标块，不再展开整个章节
- chore: 清理 PDF 查看器损坏字符与调试日志

## 0.1.4

- feat: 流式加载 Loading aborted 静默降级为全量加载，并缓存失败源避免重复尝试
- chore: 移除文档切换/卸载调试日志

## 0.1.3

- feat: PDF_Viewer 支持 pdfPageRange 子集渲染（绝对页码、越界吸附）
- feat: 新增 pdf-loaded 加载完成事件

## 0.1.2

- feat: PDF 高亮悬停显示原文（hover 高亮命中项时展示对应原文片段）

## 0.1.1

- 同步 AnGIneer monorepo 最新代码：
  - 新增 citationTarget 引用目标归一化工具与测试；
  - 新增 highlightGroup 章节组高亮工具与测试；
  - PDF 查看器 / 解析工作区 / 知识树 / 知识引用等改进。
