# JitAI Developer Quick Start & Contribution Guide

Welcome to the JitAI open-source community!

If this is your first time contributing to JitAI, **please do not clone individual sub-projects** (such as jitai-web or jitai-auth) directly. JitAI is a highly modular system, and each sub-project functions as an independent module that cannot run standalone.

This repository (jitai-quickstart) serves as the **unified entry point** for all developers contributing to JitAI, providing:

- 📚 Complete repository relationship documentation
- 🎯 Ready-to-use application templates
- 🚀 One-click automated setup scripts

With these tools, you can quickly set up a complete local development and debugging environment.

> **Tip**: Besides using the setup scripts in this repository, you can also visit the [JitAI official website](https://jit.pro) to download the free [desktop client](https://jit.pro/download), create applications visually, and export them as complete runtime directories.

## Architecture Overview

JitAI's codebase follows a layered architecture design, much like a car composed of an engine, chassis, and various components:

### 1. Core Engine

- **[jitnode](https://github.com/jitai-team/jitnode)** - Runtime Engine
  - The runtime foundation and entry point of JitAI (equivalent to the system's `main` function)
  - Responsible for application loading, request handling, and process management

### 2. Development Framework

- **[open-app](https://github.com/jitai-team/open-app)** - Framework Chassis
  - An "aggregation application" that doesn't contain specific business logic
  - Defines **multiple inheritance relationships** for a set of framework applications
  - Developers only need to inherit from open-app to gain access to all framework application capabilities

### 3. Framework Applications

The following are specific functional modules inherited by open-app:

- **[jitai-web](https://github.com/jitai-team/jitai-web)** - Frontend Interaction Layer
  - Provides various portals, pages, and UI component libraries (charts, dashboards, forms, etc.)

- **[jitai-ai](https://github.com/jitai-team/jitai-ai)** - AI Capabilities Integration
  - Large language model connections, Agent design, AI assistant orchestration, knowledge base integration

- **[jitai-auth](https://github.com/jitai-team/jitai-auth)** - Authentication & Authorization
  - Supports multiple authentication methods: username/password, phone number, third-party login
  - Organization management, RBAC-based permission control, open APIs

- **[jitai-orm](https://github.com/jitai-team/jitai-orm)** - Data Modeling
  - Data model definition, database adapters, JitAI data type implementation

- **[jitai-service](https://github.com/jitai-team/jitai-service)** - Service Orchestration
  - External API integration, model events, custom events, cross-application service calls

- **[jitai-storage](https://github.com/jitai-team/jitai-storage)** - Storage Services
  - File storage abstraction layer (supports local disk/object storage), file templates, cache management

- **[jitai-task](https://github.com/jitai-team/jitai-task)** - Task Scheduling
  - Regular scheduled tasks and data model time-field-based task management

- **[jitai-workflow](https://github.com/jitai-team/jitai-workflow)** - Workflow Engine
  - Approval flow design, approval processing, workflow state management

- **[jitai-pay](https://github.com/jitai-team/jitai-pay)** - Payment Center
  - Integration with mainstream payment channels like WeChat Pay and Alipay

- **[jitai-message](https://github.com/jitai-team/jitai-message)** - Message Notifications
  - SMS and email delivery services

- **[jitai-i18n](https://github.com/jitai-team/jitai-i18n)** - Internationalization Support
  - Multi-language package management, real-time language switching

- **[jitai-commons](https://github.com/jitai-team/jitai-commons)** - Common Modules
  - General-purpose backend utilities and frontend common modules

- **[jitai-docs](https://github.com/jitai-team/jitai-docs)** - Project Documentation
  - JitAI official website content, development guides, API reference manuals, etc.
  - *Note: The documentation repository is not part of the development framework*

## Quick Start

To modify code and see the effects in real-time, you need to assemble all repositories into a unified directory structure following the specifications.

### Step 1: Clone This Repository

```bash
git clone https://github.com/jitai-team/quickstart.git
cd quickstart
```

### Step 2: One-Click Environment Setup

Run the setup script, which will automatically pull the source code for jitnode, open-app, and all framework applications, placing them in the correct directories according to specifications.

**macOS/Linux:**
```bash
./cli.sh init
```

**Windows:**
```powershell
.\cli.ps1 init
```

After execution, you will see the following directory structure alongside the `quickstart` directory:

```plaintext
jitai-workspace/
│
├── system/
│   ├── jitDebugger.py          # Debug entry point
│   ├── bin/
│   │   ├── builder/            # Build tools
│   │   └── jitnode/            # jitnode source code
│   └── pyLibraries/            # Python dependencies directory
│
└── home/
    └── environs/
        └── JED_default/
             └── myteam/
                 ├── open-app/0_0_0/  # open-app source code
                 ├── jitai-web/0_0_0/ # jitai-web source code
                 ├── .../0_0_0/       # Other framework application source code
                 └── MyApp/0_0_0/     # Your application
```

### Step 3: Launch & Debug

#### Option 1: Direct Launch

**macOS/Linux:**
```bash
./cli.sh start
```

**Windows:**
```powershell
.\cli.ps1 start
```

After successful startup, visit `http://127.0.0.1:8000/myteam/Myapp` in your browser to view your application.

#### Option 2: Debug Mode Launch

We recommend using **VS Code / Cursor / Windsurf** or similar IDEs to open the `jitai-workspace` directory and configure `launch.json`:

**macOS/Linux Configuration:**
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
            "pythonPath": "/usr/local/opt/python@3.12/bin/python3",  // Update with your Python executable path
            "justMyCode": true,
            "env": {
                "PYTHONPATH": "/usr/local/opt/python@3.12/lib/python3.12/site-packages" // Update with your Python packages path
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

**Windows Configuration:**
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
            "pythonPath": "C:\\Python312\\python.exe",  // Update with your Python executable path
            "cwd": "${workspaceFolder}",
            "env": {
                "PYTHONPATH": "C:\\Python312\\Lib\\site-packages" // Update with your Python packages path
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

> **Note**: Please update the `pythonPath` and `PYTHONPATH` configurations according to your actual environment.

After launching the debugger, visit `http://127.0.0.1:8080/myteam/Myapp` in your browser to access your application and hit breakpoints for debugging.

## Contribution Guide

Thank you for contributing to JitAI! Before getting started, please read this workflow and the contribution guidelines in the relevant project repositories.

### Contribution Workflow

Using the modification of the "dashboard component" in **jitai-web** as an example, the complete workflow is as follows:

#### 1. Fork and Replace Source Code

Fork the target repository on GitHub (e.g., `jitai-team/jitai-web`), then replace the corresponding local directory with your forked repository content:

```bash
cd jitai-workspace/home/environs/JED_default/myteam/jitai-web/0_0_0
# Backup the original content or directly replace it with your forked repository content
```

#### 2. Modify Code

Make your code modifications in your forked repository.

#### 3. Test Your Changes

Start or restart the service:

```bash
./cli.sh start
# or
./cli.sh restart
```

After successful startup, visit `http://127.0.0.1:8080/myteam/Myapp` to test your changes.

#### 4. Submit a Pull Request

> **Important**: Although all code is under the `jitai-workspace` directory, each application directory (e.g., `jitai-web/0_0_0`) is an independent Git repository.

Navigate to the corresponding application directory:

```bash
cd jitai-workspace/home/environs/JED_default/myteam/jitai-web/0_0_0
```

Create a branch and commit:

```bash
git checkout -b feature/your-feature-name
git add .
git commit -m "Describe your changes"
git push origin feature/your-feature-name
```

Finally, submit a Pull Request to `jitai-team/jitai-web` on GitHub.

## FAQ

### Environment Requirements

**Q: What are the version requirements for Python and Node.js?**

Please ensure your Python version is >= 3.12 and Node.js version is >= 20.

### Getting Help

If you encounter any issues, feel free to seek help through the following channels:

- 📝 [Submit an Issue](https://github.com/jitai-team/quickstart/issues)
- 💬 [Start a Discussion](https://github.com/jitai-team/quickstart/discussions)
- 📖 Check the [Official Documentation](https://jit.pro/docs)


**Happy Coding! 🚀**
