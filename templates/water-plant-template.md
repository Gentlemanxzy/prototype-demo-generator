# 水厂页面模板索引

水厂模板已从单一大模板拆分为页面级模板。生成页面时应先根据原型图判断页面类型，再读取对应模板。

## 页面级模板

- `templates/water-plant/overview.md`：场景总览页面。
- `templates/water-plant/operation-monitor.md`：运行监测页面。
- `templates/water-plant/water-quality.md`：水质监测页面。
- `templates/water-plant/water-volume.md`：水量监测页面。
- `templates/water-plant/equipment-management.md`：设备管理页面。
- `templates/water-plant/electricity-consumption.md`：电耗监测页面。
- `templates/water-plant/chemical-consumption.md`：药耗监测页面。
- `templates/water-plant/security-camera.md`：安防摄像头页面。
- `templates/water-plant/access-control.md`：门禁安防页面。
- `templates/water-plant/personnel-management.md`：人员管理页面。
- `templates/water-plant/interaction-states.md`：展开态、选中态、弹窗态、轨迹态等交互状态。

## 使用原则

- 原型图优先于页面模板。
- 页面模板用于补充业务内容、指标、数据和交互，不用于覆盖原型布局。
- 如果原型图已有明确结构，模板只能作为字段和业务逻辑参考。
- 如果没有原型图，可使用页面模板生成完整水厂演示页面。
