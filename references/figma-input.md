# Figma 输入规范

本文件用于指导 Agent 在用户提供 Figma 链接、Figma 节点链接、Figma 源文件或 Figma 导出包时，如何把 Figma 作为高保真原型来源接入生成流程。

Figma 输入的目标不是绕过 skill，而是为 `prototype-page-map.md`、逐页原型分析、视觉细节分析和资产清单提供更准确的数据。

## 优先级

当 Figma 数据可用时，输入优先级如下：

1. Figma 节点结构、样式、尺寸、坐标、效果和资产。
2. Figma 导出的整页截图或 PNG/JPG 原型图。
3. 用户提供的独立素材目录或资产包。
4. 需求文档中的业务说明。
5. 页面级模板。
6. 通用设计规范。

如果 Figma 数据与截图估算冲突，优先采用 Figma 数据；截图用于检查整体视觉观感和最终还原效果。

如果 Figma 数据与设计规范冲突，必须以 Figma 数据为准。设计规范只用于补充 Figma 中缺失或无法判断的细节。

## 推荐输入形式

优先使用：

- 具体 Frame 或页面节点链接。
- 一张大屏一个 Figma Frame。
- 能对应页面名称的 Frame 命名，例如 `A1_场景总览`、`A2_运行监测`。

也可以使用：

- Figma 文件链接。
- `.fig` 源文件。
- Figma 导出包。
- Figma 导出的整页 PNG/JPG。
- `figma-links.txt`、`figma-links.md`、`figma-links.csv` 或其他包含多个 Figma 链接的清单文件。

如果只有文件级链接、无法判断具体 Frame，应先识别页面列表和主画板，再建立页面映射。

## Figma 链接清单

当输入目录或用户附件中存在以下文件时，必须优先读取：

- `figma-links.txt`
- `figma-links.md`
- `figma-links.csv`
- `links.txt`
- `links.md`
- 其他名称中包含 `figma` 和 `link` 的文本清单

Agent 必须：

1. 读取清单文件全文。
2. 提取所有 Figma URL，包括 `figma.com/design/`、`figma.com/file/`、`figma.com/proto/` 等链接。
3. 保留链接前后的页面名称、编号和备注，例如 `A1 场景总览`。
4. 判断每个链接是否包含 `node-id`。
5. 判断每个链接是单 Frame 链接、文件级链接、原型预览链接还是导出资源链接。
6. 为每条链接登记解析状态。
7. 产出 `analysis/figma-link-map.md`。
8. 将每条有效链接同步登记到 `analysis/prototype-page-map.md`。

不得只解析第一条链接。除非用户明确限定页面范围，否则清单中的每个 Figma 链接都必须被登记和处理。

推荐清单格式：

```text
A1 场景总览：https://www.figma.com/design/xxx?node-id=...
A2 运行监测：https://www.figma.com/design/xxx?node-id=...
A3 水质监测：https://www.figma.com/design/xxx?node-id=...
A4 水量监测：https://www.figma.com/design/xxx?node-id=...
```

也支持一行一个链接；如果没有页面名称，应从 Figma Frame 名称、文件名或需求文档中补全页面名。

### 链接清单输出模板

```markdown
# Figma 链接映射

## 来源文件

- 文件：
- 链接总数：
- 有效链接：
- 无 node-id 链接：
- 暂缓链接：

## 链接列表

| 序号 | 页面名称 | 原始链接 | 链接类型 | node-id | 状态类型 | 解析状态 | 备注 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 1 |  |  |  |  |  |  |  |

## 处理规则

- 带 node-id 的链接：优先按单页面或状态 Frame 解析。
- 无 node-id 的文件级链接：先识别文件中的页面和主 Frame，再建立页面映射；无法识别时向用户说明需要具体 Frame 链接。
- proto 链接：优先提取其对应节点；无法提取节点时作为截图/视觉参考处理。
- 重复链接：保留一条主记录，其他记录写入备注。
```

## 必须提取的内容

### 页面和节点

- 文件名。
- 页面名。
- Frame 名。
- Frame 尺寸。
- Frame 坐标。
- 页面层级。
- 主页面、弹窗态、展开态、选中态、详情态、轨迹态等状态关系。

### 布局

- 顶部导航区域。
- 左侧菜单或左侧面板。
- 中央主视觉区域。
- 右侧面板。
- 底部状态栏。
- 弹窗、抽屉、浮层和标签。
- 模块数量、尺寸和位置关系。

### 视觉样式

- 背景图和背景填充。
- 遮罩颜色、透明度和渐变。
- 填充颜色。
- 透明度。
- 描边颜色和透明度。
- 阴影。
- 模糊。
- 发光。
- 圆角。
- 角标、装饰线和分割线。
- 组件状态样式。

### 字体和文本

- 文本内容。
- 字体家族。
- 字号。
- 字重。
- 行高。
- 字间距。
- 文本颜色。
- 对齐方式。
- 文案密度。

### 资产

- 背景图。
- 厂区鸟瞰图。
- 地图。
- logo。
- 图标。
- 设备图片。
- 装饰线条。
- 图表装饰元素。

Figma 返回的远程资产链接如果有时效性，应下载或复制到生成项目的本地资产目录中，并在 `analysis/assets-inventory.md` 中登记来源和用途。

## 输出到分析文件

Figma 提取结果必须进入以下文件：

- `analysis/figma-link-map.md`：当输入包含链接清单时，登记每条 Figma 链接和解析状态。
- `analysis/prototype-page-map.md`：登记每个 Frame、截图和状态对应关系。
- `analysis/pages/<page-id>.md`：登记页面布局、组件、数据和交互。
- `analysis/pages/<page-id>-visual-details.md`：登记颜色、透明度、描边、阴影、发光、字号、间距和状态样式。
- `analysis/assets-inventory.md`：登记从 Figma 提取或导出的图片、logo、图标和装饰资产。

不要把 Figma 数据只作为临时参考。关键尺寸、颜色、透明度和资产必须落到分析文件中，后续代码生成应以这些分析文件为依据。

## 页面映射要求

如果 Figma 文件中存在多个 Frame，应先建立页面映射：

| Figma 页面 | Frame 名称 | 页面类型 | 状态类型 | 实现页面 | 是否必须还原 |
| --- | --- | --- | --- | --- | --- |
|  |  |  |  |  |  |

状态类型包括：

- 主页面。
- 展开态。
- 选中态。
- 弹窗态。
- 详情态。
- 轨迹态。
- hover 态。
- 其他。

未映射的 Frame 不得被直接忽略；如果暂缓实现，必须记录原因。

## 视觉细节要求

使用 Figma 数据填写 `analysis/pages/<page-id>-visual-details.md` 时，优先记录真实值：

- `rgba()` 或 hex 颜色。
- opacity。
- blur。
- box-shadow 或 shadow 参数。
- border/stroke。
- corner radius。
- text size。
- line height。
- spacing。

如果某些 Figma 信息无法直接获取，应结合截图估算，并标注“估算”。

## 代码生成要求

代码生成时：

- 优先使用 Figma 提取的真实尺寸和比例。
- 优先复用 Figma 导出的图片和图标资产。
- 不得用通用 CSS 图形替代 Figma 中已有的关键视觉资产。
- 不得用默认设计规范覆盖 Figma 中明确存在的样式。
- 可以把 Figma 的绝对坐标整理成可维护的组件结构，但不得改变原型的视觉位置关系。

## 验收要求

生成后截图检查时，应同时对照：

- Figma 原始 Frame 或 Figma 导出截图。
- `analysis/pages/<page-id>.md`。
- `analysis/pages/<page-id>-visual-details.md`。
- `analysis/assets-inventory.md`。

重点检查：

- 页面尺寸和比例。
- Frame 对应关系。
- 背景和资产是否复用。
- 半透明面板是否接近 Figma。
- 字号、颜色、边框、阴影和发光是否接近 Figma。
- 弹窗、展开态、选中态等状态是否实现或说明暂缓。
