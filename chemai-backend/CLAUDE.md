# ChemAI Backend — 项目行为准则（CLAUDE.md）

> 本文件是 AI 助手在本仓库工作的最高行为约束。与工具链相关的流程见文末「标准开发流程」。

---

## 一、项目背景

**ChemAI（智辅化学）** 是一个 AI 驱动的中学化学教学辅助平台，目标用户为中国初中和高中化学教师、学生及家长。

核心功能：

- **AI Agent 对话系统**：教师/学生/家长多角色对话，支持意图识别（chat/navigate）与工具调用
- **出题工作台**：AI 生成、手动录入、OCR 导入三种出题模式，题库管理与考试生命周期管理
- **四维审核引擎**：AI 生成内容须经科学性/难度匹配/知识点覆盖/区分度四维审核后方可下发
- **障碍诊断引擎**：对学生错误进行障碍类型诊断（概念理解/审题/表述）
- **题库管理与考试生命周期**：试卷/题目/题库 CRUD，考试状态机管理
- **学生练习与错题本**：自适应练习、错题归档与复习
- **家长端**：周报、消息与学习情况概览

技术栈：

| 层 | 选型 |
|---|---|
| 运行时 | Python 3.11+ |
| Web 框架 | FastAPI |
| ORM / 数据库 | SQLAlchemy + SQLite |
| 向量库 | ChromaDB |
| AI 编排 | LangGraph（单 Agent，`create_react_agent`） |
| LLM 提供商 | 主力 MiMo-V2.5；备选 DashScope 通义千问；第三后备 DeepSeek（全部走 OpenAI 兼容 API） |
| 前端 | Vanilla JS + Vue 3（CDN 引入，无构建步骤） |

---

## 二、行为准则（4 条核心原则）

### 1. 先思考再编码（Think Before Coding）

- 开始实现前**明确陈述假设**；有不确定的地方直接发问，不要私自做决定
- 如果存在多种解读方式，全部列出
- 需求不明立即停止，指明困惑点并寻求澄清

### 2. 简单优先（Simplicity First）

- 只写解决问题所需的最少代码，拒绝过度工程
- 不为"单次使用"的代码写抽象层，不为不存在的灵活性增加复杂度
- 不为不可能发生的场景编写异常处理

### 3. 手术式修改（Surgical Changes）

- 只触及必须修改的代码，匹配已有代码风格
- 不擅自"优化"周边代码；发现无关问题只指出，不直接改
- 每一行改动的代码都必须能追溯到具体需求

### 4. 目标驱动执行（Goal-Driven Execution）

- 将任务转化为可验证的目标（"添加校验"→"为非法输入写测试并通过"）
- 多步骤任务先列出简要计划：步骤 → 验证方式
- 循环直至验证通过，不留未验证的完成声明

---

## 三、项目特定规范

- 所有代码注释和文档使用**中文**
- Python 代码遵循 **PEP 8**
- 数据库模型统一使用 **SQLAlchemy ORM**，禁止裸 SQL 拼接
- API 遵循 **RESTful** 设计（资源名词复数、正确使用 HTTP 方法与状态码）
- 使用 **TDD**（测试驱动开发）：先写测试（RED），再实现（GREEN），后重构
- 化学方程式使用 **LaTeX 格式**，以 `$...$` 包裹，如 `$CH_4 + 2O_2 \xrightarrow{\text{点燃}} CO_2 + 2H_2O$`
- **所有 AI 生成内容必须经审核引擎校验后方可输出**——任何绕过审核直接返回 AI 输出的代码都是错误的

---

## 四、标准开发流程（工具链）

| 层 | 工具与流程 |
|---|---|
| 思考层 | `/office-hours` `/plan-ceo-review` `/plan-eng-review`（Gstack） |
| 规格层 | `/opsx:propose` → `/opsx:apply` → `/opsx:archive`（OpenSpec） |
| 实现层 | 写测试(RED) → Claude 生成(GREEN) → 重构（TDD） |
| 质量层 | L1 单元 → L2 集成 → L3 Golden → 基线对比（Evals） |
| 流程层 | `/review` `/cso` `/qa` `/ship` `/retro` `/investigate`（Gstack） |
| 骨架层 | branch commit tag merge revert push（Git） |
