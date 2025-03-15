# ScheduleMasterCore

ScheduleMasterCore 是一个基于.NET Core 3.1构建的开源分布式任务调度系统，支持跨平台多节点部署运行。

## 项目简介 / Project Introduction

本项目是一个功能强大的任务调度平台，提供简单易用的 Web UI 界面，支持分布式多节点运行，可以满足企业级的各种定时任务需求。系统采用Master-Worker架构设计，支持多节点集群部署，确保任务调度的高可用性和可扩展性。

## 主要特性 / Key Features

### 基础功能 / Basic Features
- 💻 简易的 Web UI 操作界面
  - 响应式设计，支持移动端访问
  - 任务创建、编辑、删除等基本操作
  - 实时任务状态监控
  - 运行日志实时查看

- 🔄 任务动态管理
  - 在线任务编辑
  - 动态任务调度
  - 任务暂停/恢复
  - 手动触发执行

- ⚡ 高可用性支持
  - 多节点自动容错
  - 任务自动重试
  - 节点健康检查
  - 负载均衡调度

### 高级特性 / Advanced Features
- ⚙️ 自定义参数配置
  - 支持任务级别参数配置
  - 支持全局系统参数设置
  - 参数加密存储
  - 动态参数注入

- 📧 邮件告警通知
  - 任务执行异常通知
  - 节点状态异常通知
  - 自定义告警规则
  - 支持多接收人配置

- 🔗 任务依赖关系
  - 支持任务链配置
  - 可视化依赖关系图
  - 智能任务编排
  - 依赖任务状态监控

- 🔌 任务隔离（热插拔）
  - 独立程序集运行环境
  - 动态程序集加载/卸载
  - 任务运行时隔离
  - 资源占用监控

### 扩展功能 / Extended Features
- 🌐 HTTP任务支持
  - 支持GET/POST/PUT/DELETE等请求方式
  - 自定义请求头和请求体
  - 支持文件上传
  - 响应结果验证

- ⏰ 延时任务
  - 支持相对时间和绝对时间设置
  - 精确的延时控制
  - 自动重试机制
  - 任务执行状态跟踪

- 📝 全链路日志记录
  - 详细的任务执行日志
  - 系统运行日志
  - 用户操作审计
  - 日志实时查看和下载

- 🔐 用户访问控制
  - 基于角色的权限管理
  - 细粒度的操作权限控制
  - 用户行为审计
  - 登录安全控制

## 系统架构 / System Architecture

### 技术栈 / Technology Stack
- 后端框架：.NET Core 3.1
- 数据库支持：
  - MySQL 5.7+
  - SQL Server 2012+
  - PostgreSQL 9.5+
- 前端框架：Bootstrap + jQuery
- 任务调度：Quartz.NET

### 系统组件 / Components
- Master节点：负责任务调度和分发
- Worker节点：执行具体任务
- Web管理界面：提供用户操作界面
- 任务执行器：支持多种任务类型执行

## 快速开始 / Quick Start

### 环境要求 / Prerequisites

- .NET Core 3.1 SDK
- 数据库（支持以下任一种）：
  - SQL Server 2012+
  - PostgreSQL 9.5+
  - MySQL 5.7+
- 操作系统：
  - Windows 7/10/11
  - Linux (Ubuntu 16.04+, CentOS 7+)
  - macOS 10.13+

### 安装步骤 / Installation Steps

1. 获取源码
```bash
git clone https://github.com/hey-hoho/ScheduleMasterCore.git
cd ScheduleMasterCore
```

2. 修改配置
- 复制 `appsettings.json.template` 到 `appsettings.json`
- 修改数据库连接信息
- 配置节点信息

3. 编译运行
```bash
dotnet restore
dotnet build
dotnet run
```

### 配置说明 / Configuration

在 `appsettings.json` 中配置数据库连接：

```json
{
  "DbConnector": {
    "Provider": "sqlserver", // 可选值：sqlserver、postgresql、mysql
    "ConnectionString": "your_connection_string"
  },
  "NodeSetting": {
    "IdentityName": "master-node",
    "Role": "master",    // master或worker
    "Protocol": "http",
    "IP": "localhost",
    "Port": 30000
  },
  "AppSettings": {
    "AdminDefaultPwd": "111111"  // 默认管理员密码
  }
}
```

### 任务开发指南 / Task Development Guide

#### 1. 程序集任务
```csharp
public class MyTask : ITask
{
    public TaskResult Execute(TaskContext context)
    {
        // 实现任务逻辑
        return TaskResult.Success();
    }
}
```

#### 2. HTTP任务
- 支持RESTful接口调用
- 支持自定义请求头和参数
- 支持返回值校验

#### 3. 延时任务
- 支持基于时间的延时
- 支持基于条件的延时
- 支持任务重试策略

## 注意事项 / Notes

### 1. 任务开发注意事项
- 任务程序集应尽量减少外部依赖
- 确保任务代码的幂等性
- 合理使用自定义参数
- 注意异常处理和日志记录
- 避免死循环和资源泄露

### 2. 部署注意事项
- 确保所有节点时间同步
- 合理配置任务执行超时时间
- 建议使用独立的数据库实例
- 配置适当的日志级别
- 定期备份数据库

### 3. 性能优化建议
- 合理设置任务并发数
- 避免过于频繁的任务调度
- 及时清理历史日志数据
- 使用合适的任务分片策略
- 监控系统资源使用情况

## 常见问题 / FAQ

1. Q: 如何实现任务的高可用？
   A: 系统通过多节点部署和自动故障转移实现高可用。

2. Q: 如何处理任务执行失败？
   A: 系统提供重试机制和异常通知功能。

3. Q: 是否支持任务分片？
   A: 支持，可以通过自定义参数实现任务分片。

## 贡献指南 / Contributing

1. Fork 项目
2. 创建功能分支
3. 提交代码
4. 创建 Pull Request

## 开源协议 / License

本项目采用 MIT 协议开源。

## 联系方式 / Contact

如有问题可以通过以下方式联系作者：
- QQ：591310381
- 邮箱：591310381@qq.com
- GitHub：[hey-hoho](https://github.com/hey-hoho)

## 更新日志 / Changelog

### v1.0.0 (2024-03-15)
- 初始版本发布
- 支持基础任务调度功能
- 实现分布式部署支持

## 代码库结构 / Code Structure

### 项目组织 / Project Organization

```
ScheduleMasterCore/
├── Hos.ScheduleMaster.Base/        # 基础类库，包含核心接口和基础设施
├── Hos.ScheduleMaster.Core/        # 核心业务逻辑实现
├── Hos.ScheduleMaster.Web/         # Web管理界面
├── Hos.ScheduleMaster.QuartzHost/  # Quartz调度器宿主
├── Hos.ScheduleMaster.Demo/        # 示例项目
├── Hos.ScheduleMaster.xUnitTest/   # 单元测试项目
└── ScheduleMasterCore.sln          # 解决方案文件
```

### 核心项目结构 / Core Project Structure

#### Hos.ScheduleMaster.Core/
```
Core/
├── Common/           # 通用工具类和辅助方法
├── Dto/             # 数据传输对象
├── EntityFramework/ # EF Core实体配置
├── Interface/       # 接口定义
├── Log/            # 日志相关实现
├── Models/         # 数据模型
├── Repository/     # 数据访问层
├── Services/       # 业务服务层
└── Migrations/     # 数据库迁移文件
```

#### Hos.ScheduleMaster.Web/
```
Web/
├── ApiControllers/  # API控制器
├── AppStart/       # 应用程序启动配置
├── Controllers/    # MVC控制器
├── Extension/      # 扩展方法
├── Filters/       # 过滤器
├── Views/         # 视图文件
│   ├── Schedule/  # 调度任务视图
│   ├── Console/   # 控制台视图
│   └── Account/   # 账户管理视图
├── wwwroot/       # 静态资源文件
│   ├── js/       # JavaScript文件
│   ├── css/      # 样式文件
│   └── lib/      # 第三方库
└── Startup.cs     # 应用程序配置
```

### 主要命名空间 / Main Namespaces

- `Hos.ScheduleMaster.Core.Models`: 核心数据模型
- `Hos.ScheduleMaster.Core.Services`: 业务服务实现
- `Hos.ScheduleMaster.Core.Interface`: 核心接口定义
- `Hos.ScheduleMaster.Core.Common`: 通用工具类
- `Hos.ScheduleMaster.Web.Controllers`: Web控制器
- `Hos.ScheduleMaster.Web.ApiControllers`: API接口

### 关键文件说明 / Key Files

- `ScheduleContext.cs`: 任务上下文类，包含任务执行的核心信息
- `ServiceResponseMessage.cs`: 统一的服务响应消息格式
- `ConfigurationCache.cs`: 系统配置缓存
- `Program.cs`: 应用程序入口
- `Startup.cs`: 应用程序启动配置
- `appsettings.json`: 应用程序配置文件

### 开发规范 / Development Standards

1. 命名规范
   - 类名：PascalCase
   - 方法名：PascalCase
   - 变量名：camelCase
   - 常量：UPPER_CASE
   - 接口：以I开头，如ITask

2. 代码组织
   - 按功能模块划分目录
   - 相关的类放在同一命名空间
   - 遵循依赖注入原则
   - 使用接口进行解耦

3. 文档规范
   - 类和公共方法必须添加文档注释
   - 复杂的业务逻辑需要添加说明
   - 配置项需要添加示例和说明

4. 数据库规范
   - 表名使用下划线命名法
   - 主键统一使用自增ID
   - 必须包含创建时间和更新时间字段
   - 关键字段添加索引

---
Powered by HOHO 