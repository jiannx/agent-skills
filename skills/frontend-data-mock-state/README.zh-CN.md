# frontend-data-mock-state

## 定位

这是“前端数据层、Mock 策略与状态管理” skill，负责 API 结构、query/mutation 组织、adapter、mock-first 开发和状态归属设计。

## 主要职责

- API client 设计
- query / mutation / adapter 设计
- transport model 与 domain model 分离
- local state、URL state、server state、global state 边界设计
- mock-first 开发
- mock 到真实接口的切换策略

## 适合场景

- 接口接入
- TanStack Query 结构设计
- 状态管理梳理
- 后端未完成时的前端独立开发

## 不适合场景

- 只做设计稿还原
- 只做路由权限设计
- 只做发布和监控建设

## 推荐使用方式

- adapter 尽量靠近 feature 模块
- mock 和 real service 的切换要显式、可控
- 如果任务同时涉及权限和路由，可组合 `frontend-routing-permission`
