# frontend-routing-permission

## 定位

这是“前端路由与权限系统” skill，负责导航结构、URL 状态、路由边界、认证授权逻辑和访问控制策略。

## 主要职责

- 路由层级设计
- layout 与 route ownership 设计
- URL 状态设计
- 登录态、未登录跳转、路由守卫
- 权限模型设计，例如角色、能力、策略、租户级权限
- hidden、disabled、read-only、forbidden 等访问状态设计

## 适合场景

- 菜单和页面访问控制
- 登录态与权限判断
- 多租户、多角色、多能力点产品
- 路由结构混乱，需要重新梳理

## 不适合场景

- 单纯 UI 还原
- 单纯组件抽象
- 单纯测试覆盖补充

## 推荐使用方式

- 路由和权限通常一起设计，不建议完全割裂处理
- 大访问边界放在 route level，小权限点放在 component 或 action level
- 如果任务同时涉及数据和权限，可以再组合 `frontend-data-mock-state`
