---
name: prototype-demo-generator
description: 当用户需要把水厂需求文档、用户文本指令、Figma 链接、Figma 链接清单、UI图、原型图、页面截图、背景图或粗略想法生成并持续改进可预览的水厂数字孪生前端演示系统时使用。适用于智慧水务、水厂大屏、工艺流程、水质监测、设备运行、能耗分析、告警中心、UI图驱动高保真还原和多轮反馈改版场景。
---

# 水厂 UI 图驱动演示生成 Skill

使用本 Skill 将水厂相关需求文档、用户文本指令、Figma 设计、UI 图/原型图、截图、导出图或素材转换成可预览、可演示、可持续改进的前端 Demo。

核心定位：**UI 图驱动高保真生成**，不是根据“水厂风格”重新设计页面。

术语说明：本 Skill 中“UI 图”包含 Figma 设计图、Figma 导出图、页面截图、历史输入中称为“原型图”的图片，以及弹窗、展开态、选中态等状态图。`analysis/prototype-page-map.md` 等历史文件名可以保留，但文件内容必须登记 UI 图、Figma Frame、页面和状态的对应关系。

## 优先级

生成优先级：

1. 当前轮用户明确文本指令。
2. 历史对话中已确认且未被覆盖的用户规则。
3. Figma 设计源、Figma 节点上下文、UI 图/原型图、页面截图、Figma 导出图或背景图。
4. 已映射到 UI 图承载区域的需求文档操作逻辑、数据要求、指标含义和演示重点。
5. 水厂页面级模板。
6. 公司 UI 设计规范 `references/design-spec.md`。

冲突处理：

- 当前轮用户明确指令优先于所有材料，并记录到 `analysis/user-instructions.md`。
- 历史有效用户规则优先于 Figma/UI 图、需求文档、模板和设计规范。
- Figma/UI 图优先于需求文档、页面模板和设计规范。
- 需求文档主要补充操作逻辑、数据要求、指标含义、字段口径、状态规则和演示重点，不得改变 UI 图中的布局、视觉样式、模块数量、信息密度和页面结构。
- 没有对应需求文档的 Figma 页面、UI 图、弹窗、展开态、选中态、详情态和子页面也必须生成或登记为状态实现。
- 无法对应到 UI 页面、区域、组件、入口或状态的需求，不得自由实现，必须登记为未映射需求。

## 典型输入

- 当前轮用户文本指令、历史对话反馈、修改意见和确认规则。
- 需求文档。
- Figma 链接、Figma 节点链接、Figma 源文件或 Figma 导出包。
- `figma-links.txt`、`figma-links.md`、`figma-links.csv` 或其他 Figma 链接清单。
- UI 图/原型图、页面截图或 Figma 导出图。
- 背景图、地图、厂区鸟瞰图、三维模型图、logo、图标、装饰线条和设备素材。
- 页面清单、业务背景、客户演示重点和期望页面数量。

## 需要读取的参考文件

按任务需要读取以下文件：

- `references/input-workflow.md`：用户指令、输入分析、Figma/UI 图、需求映射、页面拆分和资产识别。
- `references/design-spec.md`：公司 UI 提供的设计规范，只作为视觉和通用组件依据，不写入生成经验。
- `references/implementation-spec.md`：输出结构、Vue/Vite、ECharts、交互、字体、指标项、点位和适配实现规则。
- `references/validation-checklist.md`：生成后的截图、视觉、交互、图表、点位和交付验收。

## 必须产出的中间文件

当输入包含 Figma、UI 图/原型图、截图、需求文档或用户文本指令时，必须先产出分析文件，再写主要页面代码：

- `analysis/user-instructions.md`：当前轮用户指令、历史有效规则、被覆盖规则和多轮修改记录。
- `analysis/figma-link-map.md`：当输入包含 Figma 链接清单时，登记所有链接、页面名、node-id 和解析状态。
- `analysis/prototype-page-map.md`：登记每张 UI 图、原型图、Figma Frame、页面和状态对应关系。
- `analysis/page-scope.md`：判断本次应生成的主页面、子页面、弹窗、展开态和切换态范围。
- `analysis/requirement-prototype-alignment.md`：登记需求与 UI 图区域/组件/交互的映射，未映射需求不得实现。
- `analysis/assets-inventory.md`：登记 Figma 和用户素材资产。
- `analysis/pages/<page-id>.md`：逐页拆解布局、组件、资产、数据和交互。
- `analysis/pages/<page-id>-visual-details.md`：逐页拆解视觉细节、点位、字体和状态样式。
- `analysis/typography-scale.md`：逐页登记顶部导航、面板、卡片、指标、列表、图表、标签和弹窗的字体尺寸、行高、字重和缩放比例。
- `analysis/chart-spec.md`：登记每个图表的类型、横坐标、纵坐标、单位、图例显示规则、tooltip、指标切换和数据来源。
- `analysis/interaction-logic.md`：登记导航、菜单、标签、详情、弹窗和筛选等操作逻辑。
- `analysis/responsive-adaptation.md`：登记缩放策略、多视口黑边检查、点位校准和内容安全区。
- `analysis/visual-check-report.md`：生成后记录截图验收结果和剩余差异。

## 工作流程

1. **用户指令归档**：按 `references/input-workflow.md` 更新 `analysis/user-instructions.md`。
2. **输入分析与页面拆分**：读取 Figma、链接清单、UI 图、截图和需求文档，生成 `analysis/page-scope.md`、`analysis/prototype-page-map.md` 和 `analysis/requirement-prototype-alignment.md`。不得默认只生成一个页面。
3. **资产识别**：登记并复用 Figma/用户提供的背景、logo、图标、装饰线、底图和图片资产。
4. **逐页分析**：生成页面结构、视觉细节、字体比例、图表规格、交互逻辑和适配策略。
5. **通用组件对齐**：当页面包含顶部导航、左侧业务菜单、点位标签、图表卡片、指标卡片或弹窗表格时，优先按当前 UI 图还原；UI 图缺失时参考 `references/design-spec.md`。
6. **页面生成或多轮改进**：按 `references/implementation-spec.md` 实现页面，严格遵循用户指令、分析文件、页面模板、通用组件样式和输出规范。
7. **验收迭代**：按 `references/validation-checklist.md` 截图检查布局、视觉、交互、适配、点位、未映射需求和剩余差异。

## 页面模板

页面级模板位于 `templates/water-plant/`：

- `overview.md`
- `operation-monitor.md`
- `water-quality.md`
- `water-volume.md`
- `equipment-management.md`
- `electricity-consumption.md`
- `chemical-consumption.md`
- `security-camera.md`
- `access-control.md`
- `personnel-management.md`
- `interaction-states.md`

模板只补充业务内容，不得改变 UI 图布局、模块数量和视觉层级。

## 默认技术栈

如果用户没有指定技术栈，默认使用 Vue 3、Vite 和 ECharts。TypeScript、Tailwind CSS、Element Plus、Three.js 只在用户明确要求、现有项目已经使用，或页面确实需要时引入。

如果用户明确要求纯静态页面，则优先使用 HTML、CSS、JavaScript、内置 SVG/CSS 图形或轻量 Canvas 图表，避免引入需要构建服务的依赖。

无论使用 Vue/Vite 还是纯静态实现，交付目录根入口 `workspace/index.html` 都必须能作为用户可点击预览入口打开。

## 关键禁止事项

- 不要根据“水厂大屏常见风格”重新设计页面。
- 不要新增 UI 图中不存在的大模块、卡片、图表、弹窗、工具栏或页面。
- 不要把未映射需求自由发挥到页面中。
- 不要让需求文档覆盖用户明确指令或 Figma/UI 图中的布局、视觉、模块数量和页面结构。
- 不要用通用 GlassPanel、默认字体或大号 KPI 样式覆盖 UI 图/Figma 样式。
- 不要默认使用通用大屏字号体系；没有完成 `analysis/typography-scale.md` 前，不得写主要页面样式。
- 不要新增 UI 图和需求未出现的三维工具栏，例如主视角、空间剖切、测量、气候调整、时间回溯。
- 不要只处理 Figma 链接清单中的第一条链接。
- 不要因为某个 Figma 页面、弹出层、展开态或子页面没有需求文档说明就跳过生成。
- 不要把用户提交的多个页面、多个 Figma 链接、多个 UI 图或多个需求文档合并成一个单页面 Demo。
- 不要用静态 CSS/SVG 装饰图形替代 UI 图或需求中有业务含义的折线图、柱状图、面积图、饼图或组合图。
- 不要交付只能通过开发服务器打开的 `workspace/index.html`。

## 最终回复

完成后说明：

- 已生成或修改的内容。
- 主要页面和本地预览地址。
- 已实现/暂缓的 UI 页面和状态。
- 已采纳的用户指令、被覆盖的旧规则和本轮修改点。
- 未映射需求、关键假设和剩余差异。