# ChemAI 领域词汇表（CONTEXT.md）

> 单一事实源的领域术语表。Agent 编码、写文档、起变量名时必须与本表一致。
> 术语描述参考产品设计文档（课程资料 Part 4：`第三节课/课程资料/4.产品设计/`，本仓库对应 `docs/设计文件/`）。

---

## 一、核心实体（Core Entities）

| 术语 | 中文 | 一句话定义 |
|---|---|---|
| Student | 学生 | 系统的主要学习者，通过学号登录，产生练习、错题与诊断数据 |
| Class | 班级 | 教师授课与学生归属的基本单位，学情面板与考试发放的范围 |
| Teacher | 教师 | 平台的核心操作者，负责出题、审核、考试管理、学情查看与诊断 |
| Parent | 家长 | 通过绑定码与学生关联，接收周报与消息，查看学习概览 |
| School | 学校 | 组织级实体，批量开通教师/班级账号的管理单位 |
| Grade | 年级 | 学段内按年份划分的层级，决定知识点与题目难度范围 |
| Account | 账号 | 登录主体，与角色（teacher/student/parent/admin）绑定，一个账号对应一种角色 |

## 二、学习概念（Learning Concepts）

| 术语 | 中文 | 一句话定义 |
|---|---|---|
| Barrier Type | 障碍类型 | 学生做错题的根因分类，三种：概念理解（concept）、审题（reading）、表述（expression） |
| — 概念理解型（concept） | 概念理解障碍 | 化学概念本身没掌握导致的错误 |
| — 审题障碍型（reading） | 审题障碍 | 读懂题目信息的能力不足导致的错误 |
| — 表述障碍型（expression） | 表述障碍 | 会做但写不出规范答案导致的错误 |
| Barrier Distribution | 障碍分布 | 一个学生/班级在三种障碍类型上的占比结构，用于指导教学干预 |
| Knowledge Point | 知识点 | 课程知识图谱的最小节点，题目、诊断、薄弱分析都以它为索引 |
| Diagnosis | 诊断 | 引擎对学生错误数据的分析结果，输出障碍类型与薄弱知识点 |
| Adaptive Practice | 自适应练习 | 根据诊断结果动态生成/推送的个性化练习任务 |

## 三、内容实体（Content Entities）

| 术语 | 中文 | 一句话定义 |
|---|---|---|
| Exam Paper | 试卷 | 一次考试/练习的题目集合载体，具有状态机（见考试状态） |
| Question | 题目 | 最小可考核单元，含题干、类型、难度、知识点标注与审核状态 |
| Question Set / Bank | 题库 | 题目的组织集合，支持按知识点/难度/类型筛选与复用 |
| Weekly Report | 周报 | 按周生成的学生学习摘要，推送给家长端 |

## 四、诊断概念（Diagnosis Concepts）

| 术语 | 中文 | 一句话定义 |
|---|---|---|
| Misconception Category | 迷思概念类别 | 学生持有的错误化学认知的分类，共 6 类：化学平衡、氧化还原、摩尔计算、有机化学、化学用语、物构知识 |
| Barrier Type vs Misconception Category | 障碍类型 × 迷思概念 | 两者**正交**：障碍类型回答"怎么错"（认知/审题/表述），迷思概念回答"错在哪"（具体化学内容） |

## 五、题目与考试概念（Question & Exam Concepts）

| 术语 | 中文 | 一句话定义 |
|---|---|---|
| Question Type | 题目类型 | 题目的作答形态，8 种：single_choice（单选）/ multi_choice（多选）/ true_false（判断）/ fill_blank（填空）/ short_answer（简答）/ essay（论述）/ calculation（计算）/ experiment（实验） |
| Difficulty | 难度 | 题目难度等级，1-5 整数，与四维审核中的"难度匹配"维度对应 |
| Four-Dimension Review | 四维审核 | AI 生成内容的强制审核：科学性、难度匹配、知识点覆盖、区分度 |
| Exam State | 考试状态 | 试卷生命周期状态机：`draft → published → in_progress → grading → completed → archived`，任一状态可转 `cancelled` |

## 六、Agent 概念（Agent Concepts）

| 术语 | 中文 | 一句话定义 |
|---|---|---|
| Intent | 意图 | 对用户输入的目标分类：`chat`（对话问答）或 `navigate`（页面导航跳转） |
| Single Agent | 单 Agent | 基于 LangGraph `create_react_agent` 构建的单一智能体，通过工具扩展能力 |
| Tool | 工具 | Agent 可调用的函数单元（查询题库、生成练习、查学情等） |
| Persona | 角色 | Agent 对话人格，四种：teacher（教师助手）/ tutor（学生助教）/ parent（家长顾问）/ admin（管理） |
| Guard State | 护栏状态 | Agent 输出的安全与合规状态标记，决定内容是否可下发 |
| Gateway | 网关 | 用户消息进入 Agent 前的统一入口，负责意图识别、角色路由与护栏前置 |

## 七、OCR 概念（OCR Concepts）

| 术语 | 中文 | 一句话定义 |
|---|---|---|
| Upload Session | 上传会话 | 一次答题卡/试卷图片上传的事务单元，跟踪文件与识别进度 |
| Preview | 预览 | OCR 识别结果的可校对展示，教师确认后才会入库 |
| Exam Import | 试卷导入 | 通过 OCR 将纸质试卷转为结构化题目的流程 |
| Grading | 判卷 | 对学生作答图片的识别与判分流程 |
| Fallback | 降级 | OCR 服务异常/低置信时转人工或备用通道的处理策略 |
| Task Polling | 任务轮询 | 前端对异步 OCR 任务状态的定时查询机制 |
