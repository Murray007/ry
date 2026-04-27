## 项目概述

**若依管理系统 (RuoYi-Vue)** v3.9.2 — 基于 Spring Boot + Vue 前后端分离的 Java 快速开发平台。

**核心技术栈：**
- 后端：Spring Boot 4.0.3 + Spring Security + MyBatis + Redis + JWT + MySQL
- 前端：Vue 2.6.12 + Element UI 2.15.14 + Vuex + Vue Router + Axios + ECharts
- Java 版本：JDK 17

### 目录结构与模块作用

```
RuoYi-Vue/
├── ruoyi-admin/       -- 主启动模块（Web 入口 & Controller 层）
├── ruoyi-framework/   -- 框架核心模块（安全/配置/AOP/拦截器）
├── ruoyi-system/      -- 系统业务模块（用户/角色/菜单等 Service+Mapper）
├── ruoyi-quartz/      -- 定时任务模块（Quartz 调度）
├── ruoyi-generator/   -- 代码生成模块（Velocity 模板引擎）
├── ruoyi-common/      -- 公共工具模块（注解/常量/异常/工具类/实体）
├── ruoyi-ui/          -- 前端 Vue 模块（独立 npm 项目）
├── sql/               -- 数据库初始化脚本
├── bin/               -- Windows 批处理脚本
├── doc/               -- 项目文档
├── ry.bat / ry.sh     -- 启动脚本
└── pom.xml            -- Maven 父 POM
```

**模块依赖关系：**
```
ruoyi-admin → ruoyi-framework → ruoyi-system → ruoyi-common
ruoyi-admin → ruoyi-quartz → ruoyi-common
ruoyi-admin → ruoyi-generator → ruoyi-common
```

### 各模块详细说明

| 模块 | 作用 | 关键内容 |
|---|---|---|
| **ruoyi-admin** | 应用唯一启动入口，聚合所有模块 | `RuoYiApplication` 主类；系统管理/监控/通用/工具 4 组 Controller；`application.yml` 核心配置 |
| **ruoyi-common** | 最底层公共模块，被所有模块依赖 | 9 个自定义注解、7 个常量类、8 个枚举、核心领域模型（`BaseEntity`/`AjaxResult`/`R<T>`）、异常体系、XSS/防盗链过滤器、Excel/文件/IP/加密等工具类 |
| **ruoyi-framework** | 框架核心，安全与基础设施 | Spring Security 配置、JWT 过滤器、Token 服务、AOP 切面（日志/数据权限/数据源/限流）、全局异常处理、Druid/Redis/MyBatis/线程池等配置 |
| **ruoyi-system** | 系统核心业务 | 16 个 Mapper + Service：用户、角色、菜单、部门、岗位、字典、参数、通知公告(含已读)、操作日志、登录日志、在线用户等 |
| **ruoyi-quartz** | 定时任务调度 | 基于 Quartz 的任务管理（增删改查/Cron 表达式）、任务日志、并发控制 |
| **ruoyi-generator** | 代码生成器 | 导入数据库表→生成前后端代码；21 套 Velocity 模板（Controller/Service/Mapper/Domain/Vue 页面/API/SQL），支持 Vue2/Vue3/TypeScript |
| **ruoyi-ui** | 前端项目 | 106 个 Vue 组件、75 个 JS 模块；布局系统、路由守卫、权限指令、全局组件（字典标签/编辑器/文件上传/图标选择等）、API 层、Vuex 状态管理 |

### 数据库结构（20 张核心表）

| 分类 | 数据表 | 说明 |
|---|---|---|
| 组织架构 | `sys_dept` / `sys_user` / `sys_post` / `sys_role` | 部门/用户/岗位/角色 |
| 权限控制 | `sys_menu` / `sys_user_role` / `sys_role_menu` / `sys_role_dept` / `sys_user_post` | 菜单/用户-角色/角色-菜单/角色-部门/用户-岗位关联 |
| 系统配置 | `sys_dict_type` / `sys_dict_data` / `sys_config` | 字典类型/字典数据/参数配置 |
| 日志审计 | `sys_oper_log` / `sys_logininfor` | 操作日志/登录日志 |
| 定时任务 | `sys_job` / `sys_job_log` | 任务调度/任务日志 |
| 通知公告 | `sys_notice` / `sys_notice_read` | 通知/已读记录 |
| 代码生成 | `gen_table` / `gen_table_column` | 代码生成表/字段配置 |
| Quartz | 11 张 `QRTZ_*` 表 | Quartz 调度器内部表 |

### 内置功能清单

| 序号 | 功能 | 说明 |
|---|---|---|
| 1 | **用户管理** | 用户增删改查、授权、重置密码、导入导出 |
| 2 | **部门管理** | 组织机构树结构，支持数据权限 |
| 3 | **岗位管理** | 用户所属职务配置 |
| 4 | **菜单管理** | 系统菜单、操作权限、按钮权限标识 |
| 5 | **角色管理** | 角色菜单权限分配、数据范围权限划分 |
| 6 | **字典管理** | 固定数据维护 |
| 7 | **参数管理** | 系统动态参数配置 |
| 8 | **通知公告** | 通知发布与已读追踪 |
| 9 | **操作日志** | 系统操作记录与异常信息查询 |
| 10 | **登录日志** | 登录记录与异常检测 |
| 11 | **在线用户** | 活跃用户监控与强退 |
| 12 | **定时任务** | 在线管理任务调度及执行日志 |
| 13 | **代码生成** | 一键生成前后端 CRUD 代码 |
| 14 | **系统接口** | SpringDoc/Swagger API 文档自动生成 |
| 15 | **服务监控** | CPU/内存/磁盘/JVM 实时监控 |
| 16 | **缓存监控** | Redis 缓存查询与命令统计 |
| 17 | **在线构建器** | 拖拽表单生成 HTML 代码 |
| 18 | **连接池监视** | Druid 数据库连接池状态与 SQL 分析 |

### 安全特性

- **JWT 认证**：基于 Token 的无状态认证，支持多终端
- **Spring Security**：方法级权限控制
- **数据权限**：`@DataScope` 注解按部门过滤数据
- **接口限流**：`@RateLimiter` 注解防刷
- **防重复提交**：`@RepeatSubmit` 注解
- **XSS 防护**：XSS 过滤器 + 校验注解
- **数据脱敏**：`@Sensitive` 注解（姓名/身份证/手机号等）
- **密码安全**：登录失败锁定、RSA 加密传输
- **防盗链**：`RefererFilter`

---

## 开发

```bash
# 克隆项目
git clone https://gitee.com/y_project/RuoYi-Vue

# 进入项目目录
cd ruoyi-ui

# 安装依赖
npm install

# 建议不要直接使用 cnpm 安装依赖，会有各种诡异的 bug。可以通过如下操作解决 npm 下载速度慢的问题
npm install --registry=https://registry.npmmirror.com

# 启动服务
npm run dev
```

浏览器访问 http://localhost:80

## 发布

```bash
# 构建测试环境
npm run build:stage

# 构建生产环境
npm run build:prod
```