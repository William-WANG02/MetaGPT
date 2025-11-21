# MetaGPT 项目详细讲解指南

> 本指南旨在帮助学习者深入理解 MetaGPT 项目，为求职面试做好充分准备。

## 目录

1. [项目概述](#1-项目概述)
2. [核心理念](#2-核心理念)
3. [框架架构](#3-框架架构)
4. [核心组件详解](#4-核心组件详解)
5. [代码结构分析](#5-代码结构分析)
6. [实际应用场景](#6-实际应用场景)
7. [关键技术要点](#7-关键技术要点)
8. [面试准备建议](#8-面试准备建议)

---

## 1. 项目概述

### 1.1 什么是 MetaGPT？

MetaGPT 是一个**多智能体（Multi-Agent）协作框架**，它将大语言模型（LLM）组织成一个虚拟软件公司，通过多个 AI 角色的协作来完成复杂任务。

**核心特点：**
- 输入：一句话的需求描述
- 输出：完整的软件项目（包括需求文档、设计文档、代码、测试等）
- 过程：模拟真实软件公司的标准操作流程（SOP）

### 1.2 项目定位

- **技术领域**：大模型应用、多智能体系统、自动化软件工程
- **研究价值**：发表在 ICLR 2024（顶级 AI 会议）
- **实用价值**：可实际用于软件开发、数据分析、研究等场景
- **开源生态**：GitHub 上拥有活跃的社区和持续更新

### 1.3 项目规模

```bash
主要目录结构：
MetaGPT/
├── metagpt/          # 核心框架代码（约 20 个子模块）
├── examples/         # 示例和应用场景
├── tests/            # 测试代码
├── docs/             # 文档
└── config/           # 配置文件
```

**代码规模：** 超过 50,000 行 Python 代码，支持 Python 3.9-3.11

---

## 2. 核心理念

### 2.1 Code = SOP(Team)

这是 MetaGPT 的**核心哲学**：

```
代码 = 标准操作流程（团队）
```

**含义解释：**
- **Team（团队）**：多个具有不同角色的 AI 智能体
- **SOP（标准操作流程）**：软件公司的标准工作流程
- **Code（代码）**：最终产出的高质量代码

**实现方式：**
1. 将软件开发流程形式化为 SOP
2. 将 LLM 赋予不同的角色和职责
3. 通过角色间的协作执行完整的开发流程

### 2.2 软件公司隐喻

MetaGPT 模拟真实的软件公司组织结构：

```
老板（用户）
    ↓ 提出需求
产品经理（ProductManager）
    ↓ 编写 PRD
架构师（Architect）
    ↓ 设计系统架构
工程师（Engineer）
    ↓ 编写代码
测试工程师（QA Engineer）
    ↓ 测试验证
```

**优势：**
- 符合人类对软件开发的认知
- 每个角色职责明确，便于扩展
- 通过 SOP 保证输出质量

---

## 3. 框架架构

### 3.1 整体架构图

```
┌─────────────────────────────────────────────────────────┐
│                      MetaGPT 框架                        │
├─────────────────────────────────────────────────────────┤
│  用户输入：一句话需求                                      │
│  ↓                                                       │
│  ┌──────────────────────────────────────────────────┐  │
│  │  Team（团队）                                       │  │
│  │  - 管理多个 Role                                    │  │
│  │  - 控制执行流程                                     │  │
│  │  - 维护共享 Environment                            │  │
│  └──────────────────────────────────────────────────┘  │
│  ↓                                                       │
│  ┌──────────────────────────────────────────────────┐  │
│  │  Environment（环境）                                │  │
│  │  - 消息传递                                         │  │
│  │  - 角色通信                                         │  │
│  │  - 共享内存                                         │  │
│  └──────────────────────────────────────────────────┘  │
│  ↓                                                       │
│  ┌──────────────────────────────────────────────────┐  │
│  │  Roles（角色集合）                                   │  │
│  │  ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐   │  │
│  │  │  PM    │ │Architect│ │Engineer│ │   QA   │   │  │
│  │  └────────┘ └────────┘ └────────┘ └────────┘   │  │
│  └──────────────────────────────────────────────────┘  │
│  ↓                                                       │
│  ┌──────────────────────────────────────────────────┐  │
│  │  Actions（动作执行）                                 │  │
│  │  WritePRD → WriteDesign → WriteCode → WriteTest  │  │
│  └──────────────────────────────────────────────────┘  │
│  ↓                                                       │
│  ┌──────────────────────────────────────────────────┐  │
│  │  LLM Provider（大模型接口）                          │  │
│  │  OpenAI / Azure / Claude / Local Models           │  │
│  └──────────────────────────────────────────────────┘  │
│  ↓                                                       │
│  最终输出：完整项目代码和文档                              │
└─────────────────────────────────────────────────────────┘
```

### 3.2 核心模块关系

```python
# 模块依赖关系
Team
  └─ Environment
      └─ Roles (ProductManager, Architect, Engineer, etc.)
          └─ Actions (WritePRD, WriteDesign, WriteCode, etc.)
              └─ LLM (OpenAI, Azure, etc.)
```

### 3.3 消息流转机制

```
1. 用户发布需求消息
   ↓
2. ProductManager 订阅需求消息，执行 WritePRD
   ↓
3. Architect 订阅 PRD 消息，执行 WriteDesign
   ↓
4. Engineer 订阅 Design 消息，执行 WriteCode
   ↓
5. QA Engineer 订阅 Code 消息，执行 WriteTest
```

**关键点：**
- 消息驱动（Message-Driven）
- 发布-订阅模式（Publish-Subscribe）
- 异步执行（Async/Await）

---

## 4. 核心组件详解

### 4.1 Team（团队）

**文件位置：** `metagpt/team.py`

**职责：**
- 管理所有角色（Role）
- 控制执行流程和轮次
- 维护共享环境（Environment）

**核心代码解析：**

```python
class Team(BaseModel):
    """团队类：管理多个角色协作"""
    
    env: Optional[Environment] = None  # 共享环境
    investment: float = 10.0           # 投资金额（控制 Token 消耗）
    idea: str = ""                     # 项目需求
    
    def hire(self, roles: list[Role]):
        """雇佣角色到团队"""
        for role in roles:
            role.set_env(self.env)
            self.env.add_role(role)
    
    async def run(self, n_round=5, idea=""):
        """运行团队协作，执行 n_round 轮"""
        self.idea = idea
        # 发布初始需求消息
        self.env.publish_message(Message(content=idea))
        
        for i in range(n_round):
            # 每轮让所有角色执行各自的任务
            await self.env.run()
```

**面试要点：**
- Team 是整个框架的调度中心
- 通过 `investment` 控制成本
- 支持序列化存储和恢复（用于中断后继续执行）

### 4.2 Role（角色）

**文件位置：** `metagpt/roles/role.py`

**核心概念：**
- 每个 Role 代表一个职能角色（如产品经理、工程师）
- Role 包含多个 Action（动作）
- Role 通过 `_watch` 订阅感兴趣的消息
- Role 通过 `_think` 决定执行哪个 Action
- Role 通过 `_act` 执行具体 Action

**核心代码结构：**

```python
class Role(BaseModel):
    """角色基类"""
    
    name: str = ""                    # 角色名称
    profile: str = ""                 # 角色类型
    goal: str = ""                    # 角色目标
    constraints: str = ""             # 约束条件
    actions: list[Action] = []        # 可执行的动作列表
    
    def _watch(self, actions: list[Type[Action]]):
        """订阅特定 Action 产生的消息"""
        self.rc.watch = {any_to_str(action) for action in actions}
    
    async def _think(self) -> bool:
        """思考：决定下一步做什么"""
        # 根据收到的消息，选择合适的 Action
        if self.rc.todo:
            return True
        return False
    
    async def _act(self) -> Message:
        """执行：运行选定的 Action"""
        response = await self.rc.todo.run(self.rc.memory.get())
        msg = Message(content=response, role=self.profile)
        return msg
    
    async def run(self, message: Message = None):
        """运行角色的完整流程：观察 → 思考 → 行动"""
        if message:
            self.put_message(message)
        
        await self._observe()  # 观察消息
        
        while await self._think():  # 思考
            msg = await self._act()  # 行动
            self.publish_message(msg)  # 发布消息
```

**设计模式：**
- **观察者模式**：通过 `_watch` 订阅消息
- **策略模式**：不同 Role 有不同的 Actions 组合
- **模板方法模式**：`run` 定义了执行框架，子类实现具体细节

### 4.3 具体角色实现

#### 4.3.1 ProductManager（产品经理）

**文件位置：** `metagpt/roles/product_manager.py`

```python
class ProductManager(Role):
    """产品经理：负责编写产品需求文档（PRD）"""
    
    name: str = "Alice"
    profile: str = "Product Manager"
    goal: str = "创建产品需求文档或市场研究"
    
    def __init__(self, **kwargs):
        super().__init__(**kwargs)
        # 设置能执行的动作
        self.set_actions([WritePRD])
        # 订阅用户需求消息
        self._watch([UserRequirement])
```

**职责：**
- 接收用户需求
- 进行竞品分析
- 编写详细的 PRD 文档

#### 4.3.2 Architect（架构师）

**文件位置：** `metagpt/roles/architect.py`

```python
class Architect(Role):
    """架构师：负责系统设计"""
    
    name: str = "Bob"
    profile: str = "Architect"
    goal: str = "设计简洁、可用、完整的软件系统"
    
    def __init__(self, **kwargs):
        super().__init__(**kwargs)
        # 设置动作
        self.set_actions([WriteDesign])
        # 订阅 PRD 消息
        self._watch([WritePRD])
```

**职责：**
- 基于 PRD 设计系统架构
- 定义数据结构和 API
- 选择技术栈和开源库

#### 4.3.3 Engineer（工程师）

**文件位置：** `metagpt/roles/engineer.py`

```python
class Engineer(Role):
    """工程师：负责编写代码"""
    
    name: str = "Charlie"
    profile: str = "Engineer"
    goal: str = "根据设计文档编写优雅、可读、可维护的代码"
    
    def __init__(self, **kwargs):
        super().__init__(**kwargs)
        # 多个动作：写代码、代码审查
        self.set_actions([WriteCode, WriteCodeReview])
        # 订阅设计文档
        self._watch([WriteDesign])
```

**职责：**
- 根据架构设计编写代码
- 进行代码审查
- 修复 Bug

### 4.4 Action（动作）

**文件位置：** `metagpt/actions/action.py`

**核心概念：**
- Action 是最小的执行单元
- 每个 Action 封装一个具体任务（如写 PRD、写代码）
- Action 调用 LLM 完成任务

**基类结构：**

```python
class Action(BaseModel):
    """动作基类"""
    
    name: str = ""                    # 动作名称
    llm: LLM = None                   # 使用的大模型
    context: str = ""                 # 上下文信息
    prefix: str = ""                  # Prompt 前缀
    desc: str = ""                    # 动作描述
    
    async def run(self, context) -> ActionOutput:
        """执行动作的核心方法"""
        # 1. 构造 Prompt
        prompt = self._make_prompt(context)
        
        # 2. 调用 LLM
        response = await self.llm.aask(prompt)
        
        # 3. 解析输出
        output = self._parse_response(response)
        
        return output
```

#### 4.4.1 WritePRD（编写 PRD）

**文件位置：** `metagpt/actions/write_prd.py`

```python
class WritePRD(Action):
    """编写产品需求文档"""
    
    async def run(self, requirements: str) -> Document:
        """
        输入：用户需求
        输出：结构化的 PRD 文档
        """
        # 1. 构造 PRD 模板
        prompt = f"""
        基于以下需求编写详细的 PRD：
        {requirements}
        
        请包含：
        - 项目名称
        - 功能需求
        - 非功能需求
        - 用户故事
        - 竞品分析
        """
        
        # 2. 调用 LLM 生成 PRD
        prd_content = await self.llm.aask(prompt)
        
        # 3. 保存为文档
        doc = Document(content=prd_content, filename="prd.md")
        return doc
```

#### 4.4.2 WriteDesign（编写设计文档）

**文件位置：** `metagpt/actions/design_api.py`

```python
class WriteDesign(Action):
    """编写系统设计文档"""
    
    async def run(self, prd: Document) -> Document:
        """
        输入：PRD 文档
        输出：系统设计文档（包括数据结构、API 定义）
        """
        prompt = f"""
        基于以下 PRD 设计系统架构：
        {prd.content}
        
        请包含：
        - 系统架构图
        - 数据结构设计
        - API 接口设计
        - 技术栈选择
        """
        
        design = await self.llm.aask(prompt)
        return Document(content=design, filename="design.md")
```

#### 4.4.3 WriteCode（编写代码）

**文件位置：** `metagpt/actions/write_code.py`

```python
class WriteCode(Action):
    """根据设计文档编写代码"""
    
    async def run(self, design: Document, task: str) -> str:
        """
        输入：设计文档 + 具体任务
        输出：代码
        """
        prompt = f"""
        根据以下设计实现功能：
        设计：{design.content}
        任务：{task}
        
        要求：
        - 代码清晰易读
        - 添加必要注释
        - 遵循 PEP 8 规范
        """
        
        code = await self.llm.aask(prompt)
        return code
```

### 4.5 Environment（环境）

**文件位置：** `metagpt/environment/base_env.py`

**职责：**
- 管理所有角色
- 处理消息传递
- 维护共享内存

**核心代码：**

```python
class Environment(BaseModel):
    """环境：角色协作的场所"""
    
    roles: dict[str, Role] = {}       # 所有角色
    memory: Memory = Memory()         # 共享内存
    history: str = ""                 # 历史记录
    
    def add_role(self, role: Role):
        """添加角色到环境"""
        self.roles[role.name] = role
        role.set_env(self)
    
    def publish_message(self, message: Message):
        """发布消息到环境"""
        self.memory.add(message)
        # 通知订阅了该消息的角色
        for role in self.roles.values():
            if role.is_subscribed(message):
                role.put_message(message)
    
    async def run(self):
        """运行一轮：让所有有任务的角色执行"""
        for role in self.roles.values():
            if role.has_todo():
                await role.run()
```

### 4.6 Message（消息）

**文件位置：** `metagpt/schema.py`

**消息系统设计：**

```python
class Message(BaseModel):
    """消息：角色间通信的载体"""
    
    id: str = ""                      # 消息 ID
    content: str                      # 消息内容
    role: str = ""                    # 发送者角色
    cause_by: str = ""                # 触发该消息的 Action
    send_to: Set[str] = set()         # 接收者
    
    def __init__(self, content: str, **kwargs):
        super().__init__(content=content, **kwargs)
        self.id = str(uuid.uuid4())
```

**消息路由机制：**
- `cause_by`：标识消息来源（哪个 Action 产生）
- `send_to`：指定接收者（支持广播和单播）
- Role 通过 `_watch` 订阅特定 `cause_by` 的消息

---

## 5. 代码结构分析

### 5.1 项目目录结构

```
MetaGPT/
├── metagpt/                    # 核心框架
│   ├── actions/               # 所有 Action 实现
│   │   ├── action.py         # Action 基类
│   │   ├── write_prd.py      # 编写 PRD
│   │   ├── design_api.py     # 系统设计
│   │   ├── write_code.py     # 编写代码
│   │   └── ...
│   ├── roles/                 # 所有 Role 实现
│   │   ├── role.py           # Role 基类
│   │   ├── product_manager.py
│   │   ├── architect.py
│   │   ├── engineer.py
│   │   └── ...
│   ├── provider/              # LLM 提供商接口
│   │   ├── openai_api.py
│   │   ├── azure_api.py
│   │   └── ...
│   ├── environment/           # 环境模块
│   │   └── base_env.py
│   ├── memory/                # 记忆模块
│   ├── tools/                 # 工具集合
│   ├── utils/                 # 工具函数
│   ├── team.py                # Team 核心类
│   ├── schema.py              # 数据结构定义
│   ├── llm.py                 # LLM 封装
│   ├── context.py             # 上下文管理
│   └── software_company.py    # CLI 入口
├── examples/                   # 示例代码
│   ├── hello_world.py         # 简单示例
│   ├── write_game_code.py     # 游戏开发示例
│   ├── debate.py              # 辩论示例
│   ├── di/                    # 数据分析器示例
│   └── ...
├── tests/                      # 测试代码
├── docs/                       # 文档
├── config/                     # 配置示例
│   └── config2.example.yaml
├── requirements.txt            # 依赖包
├── setup.py                    # 安装配置
└── README.md                   # 项目说明
```

### 5.2 核心依赖关系

```
外部依赖：
├── openai                      # OpenAI API
├── anthropic                   # Claude API
├── aiohttp                     # 异步 HTTP
├── pydantic                    # 数据验证
├── tenacity                    # 重试机制
├── tiktoken                    # Token 计数
└── ...

内部模块依赖：
software_company.py (CLI 入口)
    → team.py
        → environment/
        → roles/
            → actions/
                → llm.py
                    → provider/
```

### 5.3 配置系统

**配置文件：** `~/.metagpt/config2.yaml`

```yaml
# LLM 配置
llm:
  api_type: "openai"          # 或 azure/claude/ollama
  model: "gpt-4-turbo"        # 模型名称
  base_url: "https://api.openai.com/v1"
  api_key: "YOUR_API_KEY"
  max_tokens: 4096
  temperature: 0.7

# 项目配置
project:
  name: ""                    # 项目名称
  workspace: "./workspace"    # 工作目录

# 角色配置
roles:
  product_manager:
    enable: true
  architect:
    enable: true
  engineer:
    enable: true
```

**配置加载：**

```python
from metagpt.config2 import config

# 自动加载配置文件
config = Config.default()

# 访问配置
llm_config = config.llm
project_path = config.project_path
```

---

## 6. 实际应用场景

### 6.1 软件开发（Software Company）

**场景：** 从一句话生成完整项目

**示例：**

```python
from metagpt.software_company import generate_repo

# 输入需求
repo = generate_repo("创建一个 2048 游戏")

# 输出：
# workspace/2048_game/
#   ├── docs/
#   │   ├── prd.md          # 产品需求文档
#   │   ├── design.md       # 设计文档
#   │   └── api.md          # API 文档
#   ├── src/
#   │   ├── game.py         # 游戏逻辑
#   │   ├── ui.py           # 界面
#   │   └── utils.py        # 工具函数
#   ├── tests/
#   │   └── test_game.py    # 测试代码
#   └── README.md           # 项目说明
```

**生成的文档质量：**
- PRD：包含功能需求、用户故事、竞品分析
- Design：包含架构图、数据结构、API 定义
- Code：可运行的代码，带注释和错误处理

### 6.2 数据分析（Data Interpreter）

**场景：** 自动进行数据分析和可视化

**示例：**

```python
from metagpt.roles.di.data_interpreter import DataInterpreter

di = DataInterpreter()
await di.run("分析 sklearn Iris 数据集，生成可视化图表")

# 自动执行：
# 1. 加载数据
# 2. 数据探索（统计特征、分布等）
# 3. 生成可视化图表（散点图、热力图等）
# 4. 编写分析报告
```

**能力：**
- 数据清洗和预处理
- 统计分析
- 机器学习建模
- 可视化生成

### 6.3 研究助手（Researcher）

**场景：** 自动进行文献调研和总结

**示例：**

```python
from metagpt.roles.researcher import Researcher

researcher = Researcher()
await researcher.run("调研多智能体系统的最新进展")

# 输出：
# - 搜索相关论文
# - 总结关键发现
# - 生成调研报告
```

### 6.4 辩论系统（Debate）

**场景：** 多个 AI 角色进行辩论，从多角度分析问题

**示例：**

```python
from metagpt.team import Team
from metagpt.roles import Role

# 创建正反方角色
pro = Role(name="正方", goal="支持观点 A")
con = Role(name="反方", goal="反对观点 A")

team = Team()
team.hire([pro, con])
await team.run(idea="是否应该使用 AI 辅助编程？")

# 输出多轮辩论内容
```

### 6.5 自定义智能体

**场景：** 创建自己的智能体

**示例：**

```python
from metagpt.roles import Role
from metagpt.actions import Action

class WriteArticle(Action):
    """编写文章的动作"""
    async def run(self, topic: str) -> str:
        prompt = f"写一篇关于 {topic} 的文章"
        return await self.llm.aask(prompt)

class Writer(Role):
    """作家角色"""
    def __init__(self):
        super().__init__(
            name="Writer",
            profile="Professional Writer",
            goal="写出高质量文章"
        )
        self.set_actions([WriteArticle])

# 使用
writer = Writer()
article = await writer.run("人工智能的未来")
```

---

## 7. 关键技术要点

### 7.1 异步编程（Async/Await）

**为什么使用异步？**
- LLM API 调用耗时长（几秒到几十秒）
- 多个角色可以并发执行
- 提高整体效率

**示例：**

```python
# 同步调用（慢）
result1 = llm.ask("问题1")
result2 = llm.ask("问题2")

# 异步并发（快）
result1, result2 = await asyncio.gather(
    llm.aask("问题1"),
    llm.aask("问题2")
)
```

### 7.2 Prompt Engineering

**MetaGPT 的 Prompt 设计特点：**

1. **角色定义 Prompt**：

```python
PREFIX_TEMPLATE = """
你是一个{profile}，名字是{name}，你的目标是{goal}。
约束条件是{constraints}。
"""
```

2. **结构化输出 Prompt**：

```python
# 要求 LLM 输出 JSON 格式
prompt = """
请以 JSON 格式输出：
{
  "project_name": "项目名称",
  "features": ["功能1", "功能2"],
  "tech_stack": ["技术1", "技术2"]
}
"""
```

3. **上下文管理**：

```python
# 包含历史对话和当前任务
context = f"""
## 历史记录
{history}

## 当前任务
{current_task}

## 输出要求
{requirements}
"""
```

### 7.3 记忆管理（Memory）

**记忆类型：**

```python
class Memory:
    """记忆系统"""
    
    # 短期记忆：最近的消息
    short_term: list[Message] = []
    
    # 长期记忆：重要文档
    long_term: dict[str, Document] = {}
    
    def add(self, msg: Message):
        """添加到短期记忆"""
        self.short_term.append(msg)
        # 超过限制时自动压缩
        if len(self.short_term) > MAX_SHORT_TERM:
            self._compress()
    
    def _compress(self):
        """压缩记忆：提取关键信息"""
        summary = self._summarize(self.short_term)
        self.long_term["summary"] = summary
        self.short_term = []
```

### 7.4 Token 管理

**问题：** LLM API 按 Token 计费，需要控制成本

**解决方案：**

```python
class LLM:
    """LLM 包装器，带 Token 计数"""
    
    def __init__(self):
        self.total_tokens = 0
        self.max_tokens = 100000
    
    async def aask(self, prompt: str) -> str:
        # 1. 预估 Token 数
        estimated_tokens = self._count_tokens(prompt)
        
        # 2. 检查是否超过预算
        if self.total_tokens + estimated_tokens > self.max_tokens:
            raise NoMoneyException("Token 预算已用完")
        
        # 3. 调用 API
        response = await self._call_api(prompt)
        
        # 4. 记录实际使用的 Token
        self.total_tokens += response.usage.total_tokens
        
        return response.content
```

### 7.5 错误处理和重试

**重试机制：**

```python
from tenacity import retry, stop_after_attempt, wait_exponential

class LLM:
    
    @retry(
        stop=stop_after_attempt(3),          # 最多重试 3 次
        wait=wait_exponential(min=1, max=60) # 指数退避
    )
    async def aask(self, prompt: str) -> str:
        """带重试的 LLM 调用"""
        try:
            return await self._call_api(prompt)
        except RateLimitError:
            # 速率限制，等待后重试
            await asyncio.sleep(60)
            raise
        except APIError as e:
            # 记录错误日志
            logger.error(f"API 错误：{e}")
            raise
```

### 7.6 序列化和恢复

**场景：** 长任务可能中断，需要保存进度

**实现：**

```python
class Team:
    
    def serialize(self, path: Path):
        """序列化团队状态"""
        data = {
            "idea": self.idea,
            "investment": self.investment,
            "roles": [role.serialize() for role in self.env.roles],
            "messages": [msg.serialize() for msg in self.env.memory],
        }
        write_json_file(path / "team.json", data)
    
    @classmethod
    def deserialize(cls, path: Path) -> "Team":
        """从文件恢复团队"""
        data = read_json_file(path / "team.json")
        team = cls(idea=data["idea"])
        # 恢复角色和消息
        for role_data in data["roles"]:
            role = Role.deserialize(role_data)
            team.hire([role])
        return team

# 使用
team.serialize(Path("./storage"))
team = Team.deserialize(Path("./storage"))
await team.run()  # 继续执行
```

### 7.7 工具使用（Tool Use）

**MetaGPT 支持角色使用外部工具：**

```python
from metagpt.tools import SearchEngine, Calculator

class Researcher(Role):
    
    def __init__(self):
        super().__init__()
        # 配置可用工具
        self.tools = {
            "search": SearchEngine(),
            "calculate": Calculator(),
        }
    
    async def _act(self):
        # LLM 决定使用哪个工具
        response = await self.llm.aask("""
        你有以下工具可用：
        - search(query): 搜索信息
        - calculate(expression): 计算数学表达式
        
        任务：{task}
        请选择工具并调用。
        """)
        
        # 解析工具调用
        tool_name, args = self._parse_tool_call(response)
        
        # 执行工具
        result = await self.tools[tool_name](*args)
        
        return result
```

### 7.8 多模型支持

**MetaGPT 支持多种 LLM：**

```python
# provider/openai_api.py
class OpenAILLM(BaseLLM):
    async def aask(self, prompt):
        return await openai.ChatCompletion.acreate(...)

# provider/claude_api.py
class ClaudeLLM(BaseLLM):
    async def aask(self, prompt):
        return await anthropic.messages.create(...)

# provider/azure_api.py
class AzureLLM(BaseLLM):
    async def aask(self, prompt):
        return await azure_openai.chat.completions.create(...)

# 工厂模式选择
def create_llm(provider: str) -> BaseLLM:
    if provider == "openai":
        return OpenAILLM()
    elif provider == "claude":
        return ClaudeLLM()
    elif provider == "azure":
        return AzureLLM()
```

---

## 8. 面试准备建议

### 8.1 核心知识点

面试时需要重点掌握的知识点：

#### 8.1.1 项目核心价值

**必答题：** "介绍一下 MetaGPT 项目"

**参考回答：**

> MetaGPT 是一个多智能体协作框架，它的核心创新是将软件公司的 SOP（标准操作流程）应用到 LLM 团队中。
> 
> **核心理念**是 `Code = SOP(Team)`，即通过定义清晰的角色分工和工作流程，让多个 AI 智能体协作完成复杂任务。
> 
> **技术实现**上，我们定义了 ProductManager、Architect、Engineer 等角色，每个角色有特定的职责和可执行的 Action。角色之间通过消息传递进行通信，形成一个完整的软件开发流程。
> 
> **实际效果**是可以将一句话的需求转化为包含文档、代码、测试的完整项目。这个项目已在 ICLR 2024 发表，并在 GitHub 上获得广泛关注。

#### 8.1.2 架构设计

**常见问题：** "MetaGPT 的架构是怎么设计的？"

**关键点：**
1. **分层架构**：Team → Environment → Roles → Actions → LLM
2. **消息驱动**：发布-订阅模式，角色通过消息通信
3. **角色抽象**：每个 Role 包含多个 Action，通过 `_watch` 订阅消息
4. **异步执行**：大量使用 async/await，提高并发效率
5. **可扩展性**：易于添加新角色和新 Action

#### 8.1.3 技术难点

**常见问题：** "实现过程中遇到了哪些技术挑战？"

**参考答案：**

1. **Prompt 设计**
   - 挑战：如何让 LLM 理解角色定位并输出结构化结果
   - 解决：设计了角色模板 + 任务模板 + 输出格式要求的组合 Prompt

2. **消息路由**
   - 挑战：多个角色同时工作，如何正确传递消息
   - 解决：使用 `cause_by` 和 `send_to` 字段，实现精确的消息路由

3. **记忆管理**
   - 挑战：LLM 上下文长度有限，无法记住所有历史
   - 解决：区分短期记忆和长期记忆，自动压缩和总结

4. **成本控制**
   - 挑战：多次调用 LLM 成本高
   - 解决：Token 计数、预算控制、缓存机制

5. **错误恢复**
   - 挑战：长任务可能中断
   - 解决：序列化团队状态，支持断点续传

### 8.2 代码细节

**常见问题：** "说说你对某个关键类的理解"

#### 示例 1：Role 类

**回答要点：**

```python
# Role 是所有角色的基类，核心方法包括：

1. _watch()：订阅感兴趣的消息类型
   - 使用场景：Architect 订阅 WritePRD 的输出

2. _observe()：接收和过滤消息
   - 从环境中读取消息
   - 只处理订阅的消息类型

3. _think()：决策逻辑
   - 根据当前状态选择下一个 Action
   - 返回 True 表示有任务要执行

4. _act()：执行 Action
   - 调用选定的 Action.run()
   - 生成输出消息

5. run()：完整执行流程
   - 循环：_observe() → _think() → _act()
   - 直到没有更多任务
```

#### 示例 2：Message 类

**回答要点：**

```python
# Message 是角色间通信的载体，关键字段：

1. content：消息内容（字符串或结构化数据）

2. cause_by：触发消息的 Action 类名
   - 用于消息路由：角色通过 _watch 订阅特定 cause_by

3. send_to：接收者集合
   - 支持单播（发给特定角色）
   - 支持广播（发给所有角色）

4. role：发送者角色名称

5. id：唯一标识符（UUID）
```

### 8.3 实战问题

**场景题：** "如果要添加一个新角色 Tester（测试工程师），你会怎么做？"

**参考答案：**

```python
# 1. 定义 Action：编写测试代码
class WriteTest(Action):
    async def run(self, code: str) -> str:
        prompt = f"""
        为以下代码编写单元测试：
        {code}
        
        要求：
        - 使用 pytest 框架
        - 覆盖主要功能
        - 包含边界情况
        """
        test_code = await self.llm.aask(prompt)
        return test_code

# 2. 定义 Role：测试工程师
class Tester(Role):
    name: str = "Tester"
    profile: str = "QA Engineer"
    goal: str = "编写高质量测试代码"
    
    def __init__(self):
        super().__init__()
        # 设置 Action
        self.set_actions([WriteTest])
        # 订阅代码编写完成的消息
        self._watch([WriteCode])

# 3. 添加到团队
team = Team()
team.hire([
    ProductManager(),
    Architect(),
    Engineer(),
    Tester(),  # 新增
])
```

**场景题：** "如何优化 Token 使用，降低成本？"

**参考答案：**

1. **Prompt 优化**
   - 精简 Prompt，去除冗余描述
   - 使用更小的模型（如 gpt-3.5-turbo）处理简单任务

2. **缓存机制**
   ```python
   class LLM:
       cache: dict = {}
       
       async def aask(self, prompt):
           # 相似问题使用缓存
           if prompt in self.cache:
               return self.cache[prompt]
           
           response = await self._call_api(prompt)
           self.cache[prompt] = response
           return response
   ```

3. **增量更新**
   - 不要每次都生成完整文档
   - 只生成变更部分

4. **分批处理**
   - 将大任务拆分成小任务
   - 失败时只重试失败的部分

### 8.4 延伸问题

**问题：** "MetaGPT 与 AutoGPT、LangChain 的区别？"

**参考答案：**

| 维度 | MetaGPT | AutoGPT | LangChain |
|------|---------|---------|-----------|
| **定位** | 多智能体协作框架 | 单智能体自主任务执行 | LLM 应用开发工具链 |
| **核心概念** | Role + SOP | Agent + Task | Chain + Tool |
| **协作方式** | 多角色分工协作 | 单一 Agent 循环执行 | 链式调用 |
| **适用场景** | 复杂多步骤任务（如软件开发） | 自主探索式任务 | 快速构建 LLM 应用 |
| **输出质量** | 高（通过 SOP 保证） | 不稳定（依赖 Agent 自主决策） | 取决于 Chain 设计 |

**问题：** "如何评估 MetaGPT 生成代码的质量？"

**参考答案：**

1. **自动化评估**
   - 语法正确性：能否通过 linter
   - 功能正确性：单元测试通过率
   - 代码规范：符合 PEP 8 等标准

2. **人工评估**
   - 代码可读性
   - 架构合理性
   - 文档完整性

3. **对比评估**
   - 与人类程序员编写的代码对比
   - 与其他 AI 工具生成的代码对比

4. **实际应用**
   - 能否实际运行
   - 是否满足需求
   - 维护成本如何

### 8.5 项目经验包装

**面试话术建议：**

1. **参与深度**
   - "我深入研究了 MetaGPT 的源码，理解了其多智能体协作机制"
   - "我基于 MetaGPT 开发了 XXX 应用（如自动化测试工具、代码审查助手）"
   - "我为 MetaGPT 贡献了 XXX 功能/修复了 XXX Bug"（如果有）

2. **技术掌握**
   - "我熟悉异步编程，理解 MetaGPT 中如何使用 async/await 提高效率"
   - "我研究了 Prompt Engineering，知道如何设计有效的角色提示词"
   - "我了解消息驱动架构，理解发布-订阅模式在多智能体中的应用"

3. **实践应用**
   - "我用 MetaGPT 生成了 XXX 项目，分析了生成代码的质量"
   - "我对比了不同 LLM（GPT-4、Claude）在 MetaGPT 中的表现"
   - "我优化了 Token 使用，将成本降低了 XX%"

4. **深度思考**
   - "我思考了多智能体协作的挑战，如消息冲突、角色决策等"
   - "我研究了如何将 MetaGPT 应用到其他领域，如数据分析、内容创作"
   - "我关注了相关论文，了解了多智能体系统的前沿进展"

### 8.6 常见面试问题清单

#### 基础问题
1. MetaGPT 的核心思想是什么？
2. MetaGPT 包含哪些角色？每个角色的职责是什么？
3. 什么是 SOP？MetaGPT 如何实现 SOP？
4. MetaGPT 的消息传递机制是怎样的？
5. Role 和 Action 的关系是什么？

#### 进阶问题
1. MetaGPT 如何保证多个角色协作的一致性？
2. 如何处理 LLM 输出的不确定性？
3. MetaGPT 的记忆管理机制是怎样的？
4. 如何控制 Token 使用和成本？
5. 如何扩展 MetaGPT，添加新的角色或功能？

#### 实战问题
1. 用 MetaGPT 生成的代码质量如何？有哪些优缺点？
2. MetaGPT 在实际项目中的应用场景有哪些？
3. 如何评估多智能体系统的性能？
4. MetaGPT 与其他 AI 代码生成工具（如 Cursor、GitHub Copilot）的区别？
5. 多智能体系统的未来发展方向是什么？

#### 代码问题
1. 解释 Role 类的核心方法（_watch, _think, _act）
2. Message 类的关键字段及其作用
3. Team 类如何管理多个 Role？
4. 如何实现一个新的 Action？
5. 异步编程在 MetaGPT 中的应用

---

## 9. 学习路径建议

### 9.1 入门阶段（1-2 周）

**目标：** 理解基本概念，能运行示例

**学习内容：**
1. 阅读 README 和官方文档
2. 安装和配置 MetaGPT
3. 运行 `hello_world.py` 和 `write_game_code.py`
4. 理解 Role、Action、Message 的概念
5. 阅读 `team.py`、`role.py`、`action.py` 的核心代码

**实践任务：**
- 用 MetaGPT 生成一个简单项目（如计算器、井字棋）
- 修改配置，尝试不同的 LLM（GPT-4、Claude）
- 观察生成的文档和代码，分析质量

### 9.2 进阶阶段（2-3 周）

**目标：** 深入理解架构，能自定义角色

**学习内容：**
1. 阅读完整的 `roles/` 目录代码
2. 阅读完整的 `actions/` 目录代码
3. 理解消息路由机制
4. 理解记忆管理和上下文处理
5. 学习 Prompt Engineering 技巧

**实践任务：**
- 实现一个自定义 Role（如代码审查员、文档撰写者）
- 实现一个自定义 Action（如代码重构、性能优化）
- 分析生成代码的 Token 使用情况
- 对比不同 Prompt 的效果

### 9.3 高级阶段（3-4 周）

**目标：** 掌握核心技术，能贡献代码

**学习内容：**
1. 阅读相关论文（MetaGPT ICLR 2024）
2. 研究多智能体系统的理论
3. 学习 LLM 的高级用法（Function Calling、Tool Use）
4. 了解其他多智能体框架（AutoGen、CrewAI）
5. 参与社区讨论，阅读 Issues 和 PRs

**实践任务：**
- 在实际项目中应用 MetaGPT
- 优化 MetaGPT 的性能或成本
- 为 MetaGPT 贡献代码或文档
- 撰写技术博客，分享学习心得

---

## 10. 总结

### 10.1 MetaGPT 的优势

1. **结构化**：清晰的角色分工和工作流程
2. **可扩展**：易于添加新角色和新功能
3. **高质量**：通过 SOP 保证输出质量
4. **实用性**：可实际用于软件开发等场景
5. **社区活跃**：持续更新和维护

### 10.2 MetaGPT 的局限

1. **成本较高**：多次调用 LLM，Token 消耗大
2. **速度较慢**：串行执行多个步骤，耗时长
3. **质量波动**：依赖 LLM 输出，存在不确定性
4. **复杂度限制**：适合中小型项目（<2000 行代码）
5. **需要人工介入**：生成代码通常需要人工审查和修改

### 10.3 未来发展方向

1. **自我进化**：让 MetaGPT 能够自我学习和优化
2. **更多角色**：添加更多专业角色（如 DevOps、UI 设计师）
3. **更好的协作**：优化角色间的通信和协作机制
4. **更低成本**：通过缓存、增量更新等降低 Token 消耗
5. **更广应用**：扩展到更多领域（如数据分析、内容创作、科研）

### 10.4 面试金句

准备几句精炼的总结，面试时可以脱口而出：

1. **核心理念**
   > "MetaGPT 的核心是 `Code = SOP(Team)`，它将软件公司的标准流程应用到 AI 团队中，通过多角色协作生成高质量代码。"

2. **技术亮点**
   > "MetaGPT 使用消息驱动架构，角色通过发布-订阅模式通信，实现了松耦合、可扩展的多智能体系统。"

3. **实践价值**
   > "我用 MetaGPT 生成了多个项目，深刻理解了 Prompt 设计、记忆管理、成本控制等关键技术，这对我理解大模型应用开发很有帮助。"

4. **未来展望**
   > "多智能体系统是大模型应用的重要方向，MetaGPT 探索了让 AI 像团队一样协作的可能性，我相信这会在软件工程、数据分析等领域产生重大影响。"

---

## 附录

### A. 重要文件清单

**必读文件：**
1. `metagpt/team.py` - 团队管理
2. `metagpt/roles/role.py` - 角色基类
3. `metagpt/actions/action.py` - 动作基类
4. `metagpt/schema.py` - 数据结构定义
5. `metagpt/environment/base_env.py` - 环境管理
6. `metagpt/software_company.py` - CLI 入口

**角色实现：**
1. `metagpt/roles/product_manager.py`
2. `metagpt/roles/architect.py`
3. `metagpt/roles/engineer.py`

**动作实现：**
1. `metagpt/actions/write_prd.py`
2. `metagpt/actions/design_api.py`
3. `metagpt/actions/write_code.py`

**示例代码：**
1. `examples/hello_world.py`
2. `examples/write_game_code.py`
3. `examples/di/` 目录

### B. 学习资源

**官方资源：**
- 官网：https://www.deepwisdom.ai/
- 文档：https://docs.deepwisdom.ai/
- GitHub：https://github.com/geekan/MetaGPT
- 论文：https://arxiv.org/abs/2308.00352

**社区资源：**
- Discord：https://discord.gg/ZRHeExS6xv
- Twitter：@MetaGPT_

**相关论文：**
1. MetaGPT: Meta Programming for A Multi-Agent Collaborative Framework (ICLR 2024)
2. Data Interpreter: An LLM Agent For Data Science (arxiv 2024)
3. AFlow: Automating Agentic Workflow Generation (ICLR 2025 Oral)

**相关项目：**
- AutoGPT：https://github.com/Significant-Gravitas/AutoGPT
- LangChain：https://github.com/langchain-ai/langchain
- AutoGen：https://github.com/microsoft/autogen

### C. 术语表

- **LLM**：Large Language Model，大语言模型
- **Agent**：智能体，能自主决策和执行任务的 AI 实体
- **Multi-Agent**：多智能体，多个 Agent 协作完成任务
- **SOP**：Standard Operating Procedure，标准操作流程
- **Role**：角色，具有特定职责的 Agent
- **Action**：动作，Role 执行的具体任务
- **Message**：消息，Role 之间通信的载体
- **Environment**：环境，Role 协作的场所
- **Prompt**：提示词，给 LLM 的指令
- **Token**：词元，LLM 处理文本的最小单位
- **PRD**：Product Requirement Document，产品需求文档
- **API**：Application Programming Interface，应用程序接口
- **Async/Await**：异步编程关键字
- **Pub-Sub**：Publish-Subscribe，发布-订阅模式

---

**祝你面试顺利！加油！** 🚀
