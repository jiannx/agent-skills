# 前端 Skill 中文总览

本文档用于快速说明当前前端 skill 体系的职责划分、适用场景和推荐使用方式。

## Skill 结构

当前前端 skill 体系由 1 个主 skill 和 6 个子 skill 组成：

- `frontend-product-development`
- `frontend-bootstrap-architecture`
- `frontend-routing-permission`
- `frontend-design-implementation`
- `frontend-component-feature-system`
- `frontend-data-mock-state`
- `frontend-quality-operations`

## 1. frontend-product-development

定位：前端产品交付总控 skill。

主要职责：
- 判断任务是否属于跨多个前端领域的复杂任务。
- 决定应该启用哪些子 skill。
- 汇总多个子 skill 的建议，形成统一的实施路径。
- 控制任务范围，避免小任务被整套大规范机械套用。

适合场景：
- 同时涉及架构、设计、数据、测试、发布等多个方面。
- 当前还不确定应该走哪条实现路径。
- 需要把多个前端子领域方案整理成一份统一交付计划。

不适合场景：
- 只改一个页面样式。
- 只处理一个接口接入。
- 只做一个路由或一个权限点的小范围调整。

## 2. frontend-bootstrap-architecture

定位：前端项目初始化与基础架构 skill。

主要职责：
- 新项目初始化。
- 已有项目结构审查。
- 框架选择，例如 React、Next.js、Vite、Vue。
- 目录结构、模块边界、共享层设计。
- 环境配置基线设计。
- 工程基础设施初始化，如 lint、format、类型检查、测试基线。
- 国际化基础设施设计，例如翻译资源组织、locale 加载策略、格式化能力基线。

适合场景：
- 新项目启动。
- 现有项目前期架构梳理。
- 需要决定框架、构建方式、基础目录结构。

## 3. frontend-routing-permission

定位：路由与权限系统 skill。

主要职责：
- 路由层级设计。
- URL 状态设计。
- layout 与 route ownership 设计。
- 认证与授权边界设计。
- 权限模型设计，例如角色、能力、策略、租户级权限。
- hidden、disabled、read-only、forbidden 等访问状态区分。

适合场景：
- 登录态与页面访问控制。
- 菜单权限、按钮权限、字段权限。
- 多租户、多角色、多能力点产品。

## 4. frontend-design-implementation

定位：设计稿落地与 UI 实现 skill。

主要职责：
- 根据 Figma、Sketch、截图或现有产品实现 UI。
- 响应式和移动端适配。
- 基于 theme、token、语义变量、组件变体的样式落地。
- 交互一致性控制。
- 设计比对、截图校验和设计验收说明。
- 国际化 UI 适配，例如长文本、RTL、日期数字货币格式显示。

适合场景：
- 设计稿转代码。
- 页面视觉实现。
- 样式统一与设计修正。
- 设计还原和截图比对。

## 5. frontend-component-feature-system

定位：组件系统与 feature 模块结构 skill。

主要职责：
- 组件系统分层：基础组件、组合组件、领域组件。
- 公共组件抽离策略。
- feature-first 目录结构设计。
- 共享抽象边界设计。
- 重复页面结构、表单、列表模式抽象。

适合场景：
- 设计系统建设。
- 公共组件体系建设。
- feature 模块结构改造。
- 多页面重复结构重构。

## 6. frontend-data-mock-state

定位：数据层、mock 策略与状态管理 skill。

主要职责：
- API client 设计。
- query / mutation / adapter 设计。
- transport model 与 domain model 分离。
- local state、URL state、server state、global state 的边界设计。
- mock-first 开发。
- mock 到真实接口的切换策略。
- 国际化相关的数据边界，例如 locale-sensitive API、格式化归属、翻译资源加载边界。

适合场景：
- 接口接入。
- TanStack Query 结构设计。
- 状态管理梳理。
- 后端未完成时的前端独立开发。

## 7. frontend-quality-operations

定位：质量保障与交付运维 skill。

主要职责：
- 测试策略设计。
- 性能检查。
- 可观测性建设。
- 发布策略。
- 上线验证、灰度发布、回滚方案。
- 生产可交付性检查。

适合场景：
- 功能准备上线。
- 需要补测试覆盖。
- 引入埋点、错误监控、性能监控。
- 需要制定发布、灰度、回滚策略。

## 推荐使用方式

按任务类型选择 skill：

- 新项目、框架选择、架构初始化：`frontend-bootstrap-architecture`
- 路由、鉴权、访问控制：`frontend-routing-permission`
- 设计稿实现、响应式、样式统一：`frontend-design-implementation`
- 组件抽象、feature 模块设计：`frontend-component-feature-system`
- 接口、状态、mock、adapter：`frontend-data-mock-state`
- 测试、监控、发布、回滚：`frontend-quality-operations`

国际化相关任务通常需要组合使用：

- 国际化基础设施、locale 策略、翻译资源组织：`frontend-bootstrap-architecture`
- 文案展示、长文本适配、RTL、格式化显示：`frontend-design-implementation`
- locale-sensitive API、格式化边界、翻译资源与状态归属：`frontend-data-mock-state`

如果任务同时跨越多个领域，先使用 `frontend-product-development` 作为总控入口，再组合使用相关子 skill。

## 使用原则

- 小任务优先使用子 skill，不要默认启用主 skill。
- 主 skill 适合做分流、协调和汇总，不适合承担所有实现细节。
- 子 skill 负责具体执行标准，主 skill 负责跨域一致性。
- 如果本地项目已经存在，优先读取现有结构和约定，再决定采用哪些 skill。