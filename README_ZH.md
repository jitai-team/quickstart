# JitAI 开发者快速入门与贡献指南

欢迎加入 JitAI 开源社区！

如果你是第一次参与 JitAI 开发，**请不要直接克隆单个子项目**（如 jitai-web 或 jitai-auth）。JitAI 是一套高度模块化的系统，各个子项目作为独立模块，无法单独运行。

本仓库（jitai-quickstart）是所有开发者参与 JitAI 开发的**统一入口**，提供：

- 📚 完整的仓库关系说明
- 🎯 开箱即用的应用模板
- 🚀 一键式自动化装配脚本

通过这些工具，你可以快速搭建完整的本地开发、调试环境。

> **提示**：除了使用本仓库的装配脚本，你也可以访问 [JitAI 官网](https://jit.pro)下载免费的[桌面客户端](https://jit.pro/download)，通过可视化方式创建应用并导出完整的运行目录。

## 架构概览

JitAI 的代码库采用分层架构设计，就像一辆汽车由引擎、底盘和各个零部件组成：

### 1. 核心引擎

- **[jitnode](https://github.com/jitai-team/jitnode)** - 运行时引擎
  - JitAI 的运行时底座和启动入口（相当于系统的 `main` 函数）
  - 负责应用加载、请求处理和进程管理

### 2. 开发框架

- **[open-app](https://github.com/jitai-team/open-app)** - 框架底盘
  - 一个"聚合应用"，本身不包含具体业务逻辑
  - 定义了对一组框架应用的**多继承关系**
  - 开发者只需继承 open-app，即可获得所有框架应用的能力
        

### 3. 框架应用

以下是被 open-app 继承的具体功能模块：

- **[jitai-web](https://github.com/jitai-team/jitai-web)** - 前端交互层
  - 提供多种门户、页面及 UI 组件库（图表、看板、表单等）

- **[jitai-ai](https://github.com/jitai-team/jitai-ai)** - AI 能力集成
  - 大模型连接、Agent 设计、AI 助理编排、知识库集成

- **[jitai-auth](https://github.com/jitai-team/jitai-auth)** - 身份认证与权限
  - 支持账号密码、手机号、第三方登录等多种认证方式
  - 组织架构管理、基于 RBAC 的权限控制、开放 API

- **[jitai-orm](https://github.com/jitai-team/jitai-orm)** - 数据建模
  - 数据模型定义、数据库适配、JitAI 数据类型实现

- **[jitai-service](https://github.com/jitai-team/jitai-service)** - 服务编排
  - 外部 API 集成、模型事件、自定义事件、跨应用服务调用

- **[jitai-storage](https://github.com/jitai-team/jitai-storage)** - 存储服务
  - 文件存储抽象层（支持本地磁盘/对象存储）、文件模板、缓存管理

- **[jitai-task](https://github.com/jitai-team/jitai-task)** - 任务调度
  - 常规定时任务及基于数据模型时间字段的任务管理

- **[jitai-workflow](https://github.com/jitai-team/jitai-workflow)** - 工作流引擎
  - 审批流设计、审批处理、流程状态管理

- **[jitai-pay](https://github.com/jitai-team/jitai-pay)** - 支付中心
  - 集成微信支付、支付宝等主流支付渠道

- **[jitai-message](https://github.com/jitai-team/jitai-message)** - 消息通知
  - 短信、邮件发送服务

- **[jitai-i18n](https://github.com/jitai-team/jitai-i18n)** - 国际化支持
  - 多语言包管理、实时语言切换

- **[jitai-commons](https://github.com/jitai-team/jitai-commons)** - 公共模块
  - 通用的后端工具类和前端公共模块

- **[jitai-docs](https://github.com/jitai-team/jitai-docs)** - 项目文档
  - JitAI 官网内容、开发指南、API 参考手册等
  - *注：文档仓库不属于开发框架的一部分*
    

## 快速开始

为了让你能够修改代码并实时查看效果，需要将所有仓库按照规范组装到统一的目录结构中。

### 步骤 1：克隆本仓库

```bash
git clone https://github.com/jitai-team/quickstart.git
cd quickstart
```

### 步骤 2：一键装配环境

运行装配脚本，它会自动拉取 jitnode、open-app 以及所有框架应用的源码，并按照规范放置到正确的目录中。

**macOS/Linux:**
```bash
./cli.sh init
```

**Windows:**
```powershell
.\cli.ps1 init
```

执行完成后，你将在 `quickstart` 同级目录下看到如下目录结构：

```plaintext
jitai-workspace/
│
├── system/
│   ├── jitDebugger.py          # 调试启动入口
│   ├── bin/
│   │   ├── builder/            # 打包构建工具
│   │   └── jitnode/            # jitnode 源码
│   └── pyLibraries/            # Python 依赖包目录
│
└── home/
    └── environs/
        └── JED_default/
             └── myteam/
                 ├── open-app/0_0_0/  # open-app 源码
                 ├── jitai-web/0_0_0/ # jitai-web 源码
                 ├── .../0_0_0/       # 其他框架应用源码
                 └── MyApp/0_0_0/     # 你的应用
```

### 步骤 3：启动与调试

#### 方式 1：直接启动

**macOS/Linux:**
```bash
./cli.sh start
```

**Windows:**
```powershell
.\cli.ps1 start
```

启动成功后，在浏览器中访问 `http://127.0.0.1:8000/myteam/Myapp` 即可查看你的应用。

#### 方式 2：调试模式启动

推荐使用 **VS Code / Cursor / Windsurf** 等 IDE 打开 `jitai-workspace` 目录，并配置 `launch.json`：

**macOS/Linux 配置：**
```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "JitAI Debug",
            "type": "debugpy",
            "request": "launch",
            "program": "${workspaceFolder}/system/jitDebugger.py",
            "cwd": "${workspaceFolder}",
            "console": "integratedTerminal",
            "pythonPath": "/usr/local/opt/python@3.12/bin/python3",  # 开发者需要填写自己的Python可执行程序所在路径
            "justMyCode": true,
            "env": {
                "PYTHONPATH": "/usr/local/opt/python@3.12/lib/python3.12/site-packages" # 开发者需要填写自己的Python依赖包所在路径
            },
            "stopOnEntry": false,
            "debugOptions": [
                "WaitOnAbnormalExit",
                "WaitOnNormalExit"
            ]
        }
    ]
}
```

**Windows 配置：**
```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "JitAI Debug",
            "type": "python",
            "request": "launch",
            "program": "${workspaceFolder}/system/jitDebugger.py",
            "console": "integratedTerminal",
            "pythonPath": "C:\\Python312\\python.exe",
            "cwd": "${workspaceFolder}",
            "env": {
                "PYTHONPATH": "C:\\Python312\\Lib\\site-packages"
            },
            "stopOnEntry": false,
            "debugOptions": [
                "WaitOnAbnormalExit",
                "WaitOnNormalExit"
            ]
        }
    ]
}
```

> **注意**：请根据你的实际环境修改 `pythonPath` 和 `PYTHONPATH` 配置。

启动调试后，在浏览器中访问 `http://127.0.0.1:8080/myteam/Myapp`，即可访问你的应用并命中断点进行调试。

## 贡献指南

感谢你对 JitAI 的贡献！在开始之前，请先阅读本流程以及相关项目仓库中的贡献指南。

### 贡献流程

以修改 **jitai-web** 中的"看板组件"为例，完整流程如下：

#### 1. Fork 并替换源码

在 GitHub 上 Fork 目标仓库（如 `jitai-team/jitai-web`），然后用你 Fork 的仓库内容替换本地对应目录：

```bash
cd jitai-workspace/home/environs/JED_default/myteam/jitai-web/0_0_0
# 备份原有内容或直接用你 Fork 的仓库内容替换
```

#### 2. 修改代码

在你 Fork 的仓库中进行代码修改。

#### 3. 测试修改

启动或重启服务：

```bash
./cli.sh start
# 或
./cli.sh restart
```

启动成功后，访问 `http://127.0.0.1:8080/myteam/Myapp` 测试你的修改。

#### 4. 提交 Pull Request

> **重要**：虽然所有代码都在 `jitai-workspace` 目录下，但每个应用目录（如 `jitai-web/0_0_0`）都是独立的 Git 仓库。

进入对应的应用目录：

```bash
cd jitai-workspace/home/environs/JED_default/myteam/jitai-web/0_0_0
```

创建分支并提交：

```bash
git checkout -b feature/your-feature-name
git add .
git commit -m "描述你的修改"
git push origin feature/your-feature-name
```

最后，在 GitHub 上向 `jitai-team/jitai-web` 提交 Pull Request。

## 常见问题

### 环境相关

**Q: 对Python以及Node.js的版本有什么要求？**

请确保你的Python版本 >= 3.12，Node.js版本 >= 20。

### 获取帮助

如果遇到问题，欢迎通过以下方式寻求帮助：

- 📝 [提交 Issue](https://github.com/jitai-team/quickstart/issues)
- 💬 [发起 Discussion](https://github.com/jitai-team/quickstart/discussions)
- 📖 查阅 [官方文档](https://jit.pro/docs)


**Happy Coding! 🚀**
