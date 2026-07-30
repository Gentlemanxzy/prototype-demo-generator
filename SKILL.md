---
name: prototype-demo-generator
description: 当用户需要把水厂需求文档、原型图、背景图或粗略想法生成可预览的水厂数字孪生前端演示系统时使用。适用于智慧水务、水厂大屏、工艺流程、水质监测、设备运行、能耗分析和告警中心等场景，尤其适合根据原型图进行高保真还原。
---

# 水厂原型驱动演示生成 Skill

当用户希望把水厂相关的需求文档、原型图、页面截图、Figma 导出图或背景素材，转换成一套可预览、可向客户演示的前端页面时，使用本 Skill。

本 Skill 的核心定位是：**原型驱动高保真生成**，而不是根据“水厂风格”重新设计一套页面。

## 目标

生成一套成熟、完整、可运行的水厂业务演示系统。输出结果应适合产品经理向客户讲解，并尽量高保真还原用户提供的原型页面。

## 最高优先级原则

生成优先级如下：

1. 用户提供的 Figma 设计源、Figma 节点上下文、原型图、页面截图、Figma 导出图或背景图。
2. 用户提供的需求文档、业务说明、字段表和演示重点。
3. 水厂页面级模板。
4. 通用水厂数字孪生设计规范。

如果原型图和行业模板存在冲突，必须优先还原原型图。

如果原型图和通用设计规范存在冲突，必须以原型图为准。设计规范只作为缺少原型细节时的补充参考，不得覆盖原型中的颜色、透明度、面板层级、字号、间距、阴影、发光、导航样式和组件形态。

如果原型图和需求文档存在冲突，优先保留原型图的布局、视觉层级和模块关系；需求文档用于补充业务文案、指标、数据和交互细节；如果有原型图却没有提供的需求文档的，按原型图来模拟。

如果需求文档中的内容无法对应到原型图、Figma Frame、页面模块、可点击区域或已有组件，不得直接新增到页面中。必须先登记到 `analysis/requirement-prototype-alignment.md` 的“未映射需求”中，并在最终回复中说明。除非用户明确确认，否则不要因为需求文档而自由新增原型中没有的模块、卡片、图表、工具栏、弹窗或页面。

禁止因为“水厂大屏常见风格”而重排、删减或重新设计原型中的主要页面结构。

## 典型输入

用户可能会提供以下材料：

- 需求文档。
- Figma 链接、Figma 节点链接、Figma 源文件或 Figma 导出包。
- `figma-links.txt`、`figma-links.md`、`figma-links.csv` 或其他包含多个 Figma 链接的清单文件。
- 原型图、页面截图或 Figma 导出图。
- 水厂厂区背景图、地图、三维模型图、设备图标、logo 或装饰素材。
- 页面清单。
- 水厂业务背景。
- 客户演示重点。
- 已有模板或参考页面。
- 希望生成的页面数量。
- 是否需要登录页、多页面路由、图表、流程图、地图或设备状态面板。

如果信息不完整，先基于水厂行业常见场景进行合理补全。只有当缺失信息会影响系统方向或原型对应关系时，才向用户提问。

## 必须产出的中间文件

当输入包含原型图、截图或 Figma 导出图时，必须在生成项目内产出以下分析文件，再开始主要页面实现：

- `analysis/prototype-page-map.md`：逐张登记原型图、页面类型、实现页面、状态类型和是否必须还原。
- `analysis/figma-link-map.md`：当输入包含 Figma 链接清单时，逐条登记链接、页面名、node-id、状态类型和解析结果。
- `analysis/requirement-prototype-alignment.md`：逐条登记需求文档内容与原型页面、区域、组件和交互的对应关系；未对应的需求不得直接实现。
- `analysis/assets-inventory.md`：登记用户单独提供的资产包；如果没有单独资产，再登记从原型图中识别出的可复用视觉内容。
- `analysis/pages/<page-id>.md`：逐页拆解布局、组件、资产、数据和交互。
- `analysis/pages/<page-id>-visual-details.md`：逐页拆解背景遮罩、半透明面板、边框、阴影、发光、字号、间距、图表和状态样式。
- `analysis/interaction-logic.md`：登记顶部导航、业务菜单、底图标签、详情面板、弹窗和筛选等操作逻辑。
- `analysis/responsive-adaptation.md`：登记业务画布缩放策略、多视口黑边检查、点位坐标校准和内容安全区。
- `analysis/visual-check-report.md`：生成后逐页记录截图验收结果和剩余差异。

未登记到 `prototype-page-map.md` 的原型图不得被忽略；如果确实不实现，必须在最终回复中说明原因。

## 工作流程

### 第一阶段：Figma 输入提取

如果用户提供 Figma 链接、Figma 节点链接、Figma 源文件或 Figma 导出包，先按 `references/figma-input.md` 提取页面结构、视觉样式、截图和资产，再进入原型理解阶段。

如果用户提供 `figma-links.txt`、`figma-links.md`、`figma-links.csv` 或其他链接清单文件，必须先读取该文件，提取其中所有 Figma URL，并按 `references/figma-input.md` 产出 `analysis/figma-link-map.md`。不得只解析第一条链接或只挑选部分链接；除非用户明确限定范围，否则清单中的每个链接都必须登记到 `analysis/figma-link-map.md` 和 `analysis/prototype-page-map.md`。

必须优先使用 Figma 中可获得的真实数据：

- 页面 Frame 名称、节点层级、尺寸、坐标和约束。
- 文本内容、字号、字重、颜色和行高。
- 填充、透明度、描边、阴影、模糊和效果。
- 图片、logo、图标、装饰线和背景资产。
- 组件状态、弹窗、展开态和选中态。

如果 Figma 数据与 PNG/JPG 截图估算冲突，优先采用 Figma 数据；截图用于视觉对照和最终验收。如果只能读取截图或导出图，再按图片方式估算。

### 第二阶段：原型理解

1. 阅读所有需求文档、说明文字和页面清单。
2. 阅读所有 Figma 提取结果、原型图片、截图、Figma 导出图和背景素材。
3. 按 `references/prototype-page-map.md` 产出 `analysis/prototype-page-map.md`，为每张原型图建立页面或状态对应关系。
4. 区分“主页面原型”和“展开态、弹窗态、选中态、轨迹态、详情态”等状态原型。
5. 按 `references/prototype-analysis.md` 为每个需要实现的页面产出逐页分析文件。

必须先完成原型拆解，再开始写页面代码。

### 第三阶段：需求与原型对齐

如果用户提供需求文档，必须按 `references/requirement-prototype-alignment.md` 产出 `analysis/requirement-prototype-alignment.md`。

必须逐条判断需求内容能否落到：

- Figma Frame 或原型页面。
- 原型中的顶部导航、左侧菜单、中央主视觉、右侧面板、底部区域、弹窗或标签。
- 原型中的已有图表、指标卡、列表、表格、详情区域或交互状态。

只有能明确对应到原型承载位置的需求，才允许进入页面实现。无法对应的需求只能进入“未映射需求”或“待确认需求”，不得自行新增模块承载。

### 第四阶段：视觉细节分析

按 `references/visual-detail-analysis.md` 为每个需要实现的页面产出 `analysis/pages/<page-id>-visual-details.md`。

必须分析并记录：

- 背景图亮度、遮罩颜色、遮罩透明度和渐变压暗方式。
- 顶部导航、左侧菜单、左右面板、弹窗、标签和图表容器的透明度、底色、边框、阴影、发光和层级。
- 标题、指标数字、列表、表格、单位、状态文字的字号、颜色、字重和密度。
- 面板标题栏、内容区、角标、装饰线、分割线和选中态的具体样式。

如果视觉细节与 `references/design-spec.md` 不一致，必须以原型图视觉细节为准。不得因为设计规范中定义了默认 GlassPanel、TopNav 或颜色 token，就把原型中的面板透明度、导航样式和组件层级改成通用样式。

### 第五阶段：操作逻辑分析

按 `references/interaction-logic-analysis.md` 产出 `analysis/interaction-logic.md`。

必须分析并登记：

- 顶部导航的菜单项、当前选中态、hover 态和点击切换目标。
- 左侧业务菜单、底图标签、右侧详情、弹窗、筛选和列表行的交互关系。
- 运行监测等页面中每个底图标签的唯一 ID、显示名称、点击后的详情标题、指标、图表和状态变化。
- 哪些交互来自需求文档，哪些来自原型图，哪些是为演示补充的模拟逻辑；来自需求文档的交互必须已在 `analysis/requirement-prototype-alignment.md` 中完成映射。
- 使用模拟数据时，必须在界面合适位置标注“模拟数据”或“Mock 数据”，避免冒充真实采集值。

顶部导航和底图标签不得只做静态展示。除非用户明确要求静态截图页，否则可点击元素必须有可演示的切换反馈。

### 第六阶段：资源识别

先检查用户是否单独提供了资产包或素材目录。

如果用户单独提供了资产，必须优先登记并复用这些资产：

- 背景图片。
- logo。
- 图标。
- 地图素材。
- 水厂模型或厂区鸟瞰图。
- 装饰线条。
- 设备示意图。
- 业务截图。

如果用户没有单独提供资产，才从原型图中识别可复用视觉内容，并记录哪些内容只能近似复刻、哪些内容需要裁切自原型截图。

按 `references/assets-inventory.md` 产出 `analysis/assets-inventory.md`。如果存在明确视觉资产，不应使用纯 CSS 渐变、通用插画或泛化图形替代。

### 第七阶段：页面计划

根据原型分析结果和页面级模板生成页面计划，明确：

- 页面名称。
- 页面类型。
- 业务画布尺寸。
- 组件树。
- 主要布局坐标和区域比例。
- 需要复用的视觉资源。
- Mock 数据结构。
- 必要交互。

页面计划必须只包含已在 `analysis/prototype-page-map.md`、`analysis/pages/<page-id>.md`、`analysis/interaction-logic.md` 或 `analysis/requirement-prototype-alignment.md` 中登记过的模块和交互。未登记内容不得进入实现。

如果页面包含底图点位、地图标签、设备标签或工艺节点，必须按 `references/responsive-adaptation.md` 记录原型坐标、相对百分比、背景图显示方式和适配后的校准策略。

页面级模板位于：

- `templates/water-plant/overview.md`
- `templates/water-plant/operation-monitor.md`
- `templates/water-plant/water-quality.md`
- `templates/water-plant/water-volume.md`
- `templates/water-plant/equipment-management.md`
- `templates/water-plant/electricity-consumption.md`
- `templates/water-plant/chemical-consumption.md`
- `templates/water-plant/security-camera.md`
- `templates/water-plant/access-control.md`
- `templates/water-plant/personnel-management.md`
- `templates/water-plant/interaction-states.md`

### 第八阶段：代码生成

严格根据原型分析、需求与原型对齐结果、视觉细节分析、操作逻辑分析、适配策略和页面计划实现。

禁止：

- 未经理由自行调整主布局。
- 用统一通用 GlassPanel 样式套所有面板。
- 用设计规范默认色值覆盖原型图中的真实色彩关系。
- 用通用字体、默认字号或大号数字样式替代原型中的顶部导航、右侧卡片、标题和指标文字。
- 自行新增原型中不存在的大模块。
- 为了承载需求文档中未映射的内容而新增原型没有的模块、卡片、图表、弹窗、工具栏或页面。
- 自行新增原型和需求文档中未出现的三维工具栏，例如主视角、空间剖切、测量、气候调整、时间回溯。
- 删除原型中的主要区域。
- 合并多个原型页面为一个泛化页面。
- 使用通用后台布局替代大屏原型布局。
- 用纯装饰内容替代业务内容。

### 第九阶段：视觉验收和迭代

生成完成后必须启动预览并截图。

按 `references/visual-check.md` 和 `references/acceptance-checklist.md` 检查：

- 页面是否填满视口。
- 是否还原原型布局。
- 主要模块数量是否一致。
- 页面是否只实现已映射需求，是否存在未映射需求被自由发挥到界面中。
- 背景、面板、颜色、字号和信息密度是否贴近原型。
- 半透明面板、遮罩、边框、发光、阴影和标题栏层级是否符合 `analysis/pages/<page-id>-visual-details.md`。
- 顶部导航、左侧业务菜单和底图标签切换是否符合 `analysis/interaction-logic.md`。
- 多视口下是否无黑边、点位不偏移、右侧卡片不溢出，是否符合 `analysis/responsive-adaptation.md`。
- 交互和 Mock 数据是否支撑演示。

按 `references/visual-check.md` 产出 `analysis/visual-check-report.md`。如果任何主页面出现明显黑边、核心区域裁切、主要模块缺失、状态图未实现或布局不像原型，应继续修改并重新截图检查。

## 默认技术栈

如果用户没有指定技术栈，默认使用：

- Vue
- TypeScript
- Vite
- Tailwind CSS
- ECharts
- Three.js

数据采用 Mock 数据，保留对后端接口的兼容。

如果项目已有技术栈，应优先沿用现有项目结构和依赖。

如果调用方明确要求纯静态页面，则优先使用：

- HTML
- CSS
- JavaScript
- 内置 SVG、CSS 图形或轻量 Canvas 图表

纯静态模式下，不使用 Vue、React、Vite、Tailwind CSS、ECharts、Three.js 等需要额外安装依赖或启动构建服务的技术。

## 设计原则

- 生成真实可用的系统页面，不生成营销落地页。
- 页面第一屏要适合客户演示。
- 除非原型图明确没有顶部导航栏，否则每个业务页面都必须保留顶部导航栏或平台顶栏，包含平台名称、一级导航、当前页面、时间和用户/状态信息。
- 原型图是最高优先级输入，设计规范只用于补充细节。
- 原型图与设计规范冲突时，以原型图为准；设计规范不得覆盖原型中的透明度、颜色、面板层级、字号、间距和组件形态。
- 大屏页面以 `1920 × 1080` 为常用业务画布基准；如果用户上传了原型图，优先保持原型图画面比例和布局关系。
- 预览页面应填满浏览器视口，不出现四周黑边；需要保持大屏比例时使用 cover 缩放策略，核心内容放在安全区。
- 视觉风格应成熟、稳定、可信，避免过度科幻和纯装饰效果。
- 所有核心模块都应有真实感数据，避免空白占位。
- 图表、指标、表格和状态信息要服务于业务讲解。
- 中文文案应专业、自然，贴近水务行业表达。

## 最终回复要求

完成后向用户说明：

- 已生成或修改的内容。
- 主要页面。
- 本地预览地址。
- 做出的关键假设。
- 未覆盖或后续可增强的部分。
