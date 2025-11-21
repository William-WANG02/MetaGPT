# MetaGPT 面试准备速查手册

> 快速准备面试的核心知识点整理

## 一句话介绍

**MetaGPT 是一个多智能体协作框架，通过模拟软件公司的 SOP（标准操作流程），让多个 AI 角色协作完成复杂的软件开发任务。**

核心公式：**`Code = SOP(Team)`**

---

## 核心概念速记

### 1. 五大核心组件

```
Team（团队）
  ↓ 管理
Environment（环境）
  ↓ 容纳
Role（角色）
  ↓ 执行
Action（动作）
  ↓ 调用
LLM（大模型）
```

### 2. 关键角色

| 角色 | 英文 | 职责 | 输入 | 输出 |
|------|------|------|------|------|
| 产品经理 | ProductManager | 编写 PRD | 用户需求 | 产品需求文档 |
| 架构师 | Architect | 系统设计 | PRD | 设计文档、API |
| 工程师 | Engineer | 编写代码 | 设计文档 | 源代码 |
| 测试工程师 | QA Engineer | 编写测试 | 代码 | 测试代码 |

### 3. 消息流转

```
用户需求 → ProductManager → PRD
         → Architect → 设计文档
         → Engineer → 代码
         → QA Engineer → 测试
```

### 4. 关键类说明

#### Role（角色基类）

```python
class Role:
    # 核心属性
    name: str           # 角色名称
    profile: str        # 角色类型
    goal: str           # 角色目标
    actions: list       # 可执行动作
    
    # 核心方法
    def _watch()        # 订阅消息
    def _observe()      # 接收消息
    def _think()        # 决策
    def _act()          # 执行
    def run()           # 完整流程
```

#### Action（动作基类）

```python
class Action:
    # 核心属性
    name: str           # 动作名称
    llm: LLM           # 大模型实例
    
    # 核心方法
    async def run()     # 执行动作
```

#### Message（消息）

```python
class Message:
    content: str        # 消息内容
    role: str          # 发送者
    cause_by: str      # 触发的 Action
    send_to: Set       # 接收者
```

---

## 面试高频问题

### Q1: MetaGPT 的核心思想是什么？

**答：**
MetaGPT 的核心思想是 **`Code = SOP(Team)`**，即通过标准操作流程（SOP）组织 AI 团队（Team）来生成代码（Code）。

具体来说：
1. **Team**：多个具有不同角色的 AI 智能体
2. **SOP**：软件公司的标准开发流程（需求 → 设计 → 编码 → 测试）
3. **Code**：高质量的软件产出

这种方式模拟了真实软件公司的工作方式，每个角色职责明确，通过协作完成复杂任务。

---

### Q2: MetaGPT 的架构设计？

**答：**
MetaGPT 采用**分层 + 消息驱动**的架构：

```
分层架构：
- Team 层：管理整体流程
- Environment 层：消息传递和角色管理
- Role 层：角色定义和决策
- Action 层：具体任务执行
- LLM 层：大模型调用

消息驱动：
- 发布-订阅模式
- Role 通过 _watch() 订阅感兴趣的消息
- Action 执行后发布新消息
- 实现松耦合、可扩展
```

---

### Q3: 如何实现多角色协作？

**答：**
通过**消息机制**实现：

1. **消息定义**：每个消息包含 `content`（内容）、`cause_by`（来源）、`send_to`（接收者）

2. **发布-订阅**：
   - Role 通过 `_watch([Action])` 订阅特定 Action 的输出
   - Action 执行完发布消息到 Environment
   - Environment 分发给订阅了该消息的 Role

3. **执行流程**：
   ```python
   # ProductManager 订阅用户需求
   pm._watch([UserRequirement])
   
   # Architect 订阅 PRD
   architect._watch([WritePRD])
   
   # 串行执行
   用户需求 → PM 写 PRD → Architect 写设计 → Engineer 写代码
   ```

---

### Q4: 如何控制成本和 Token 使用？

**答：**

1. **预算控制**：Team 通过 `investment` 参数限制总 Token 数

2. **Token 计数**：
   ```python
   class LLM:
       total_tokens: int = 0
       max_tokens: int = 100000
       
       async def aask(self, prompt):
           # 检查是否超预算
           if self.total_tokens >= self.max_tokens:
               raise NoMoneyException()
   ```

3. **优化策略**：
   - Prompt 精简
   - 缓存相似请求
   - 增量更新而非完全重新生成
   - 使用更便宜的模型处理简单任务

---

### Q5: 如何保证输出质量？

**答：**

1. **SOP 保证**：通过标准流程确保不遗漏关键步骤

2. **角色专业化**：每个角色专注自己的领域

3. **Prompt Engineering**：
   - 角色定位明确
   - 任务描述清晰
   - 输出格式结构化

4. **代码审查**：Engineer 可以配置 `use_code_review=True`

5. **测试验证**：QA Engineer 编写测试确保功能正确

---

### Q6: MetaGPT vs AutoGPT vs LangChain？

**答：**

| 特性 | MetaGPT | AutoGPT | LangChain |
|------|---------|---------|-----------|
| 类型 | 多智能体框架 | 自主 Agent | 工具链 |
| 协作方式 | 多角色分工 | 单 Agent 循环 | 链式调用 |
| 适用场景 | 软件开发等复杂任务 | 自主探索任务 | 快速开发应用 |
| 质量保证 | SOP + 多角色审查 | Agent 自主决策 | 开发者设计 |
| 可控性 | 高（预定义流程） | 低（自主探索） | 高（显式定义） |

---

### Q7: 如何添加新角色？

**答：**

```python
# 步骤 1：定义 Action
class MyAction(Action):
    async def run(self, input) -> str:
        prompt = f"执行任务：{input}"
        return await self.llm.aask(prompt)

# 步骤 2：定义 Role
class MyRole(Role):
    name: str = "MyRole"
    profile: str = "专家"
    goal: str = "完成特定任务"
    
    def __init__(self):
        super().__init__()
        self.set_actions([MyAction])
        self._watch([SomeOtherAction])  # 订阅消息

# 步骤 3：添加到 Team
team = Team()
team.hire([MyRole()])
```

---

### Q8: 异步编程在 MetaGPT 中的应用？

**答：**

MetaGPT 大量使用 `async/await`：

**原因：**
- LLM API 调用耗时（几秒到几十秒）
- 多个角色可以并发执行
- 提高整体效率

**示例：**
```python
# 串行（慢）
result1 = await llm.aask("问题1")
result2 = await llm.aask("问题2")

# 并发（快）
result1, result2 = await asyncio.gather(
    llm.aask("问题1"),
    llm.aask("问题2")
)
```

---

### Q9: 如何处理 LLM 的不确定性？

**答：**

1. **重试机制**：
   ```python
   @retry(stop=stop_after_attempt(3))
   async def aask(self, prompt):
       return await self._call_api(prompt)
   ```

2. **结构化输出**：
   - 要求 LLM 输出 JSON/YAML 格式
   - 使用正则表达式解析输出
   - 验证输出格式

3. **多次采样**：
   - 生成多个候选结果
   - 选择最佳结果

4. **人工审核**：
   - 关键决策需要人工确认
   - 支持中途介入修改

---

### Q10: MetaGPT 的局限性？

**答：**

**当前局限：**
1. **成本高**：多次调用 LLM，Token 消耗大
2. **速度慢**：串行执行多个步骤
3. **复杂度限制**：适合中小型项目（<2000 行）
4. **质量波动**：依赖 LLM 输出质量
5. **需人工介入**：生成代码通常需要审查修改

**改进方向：**
1. 并行执行部分步骤
2. 缓存和增量更新
3. 支持更大规模项目
4. 自我优化和学习
5. 更智能的决策机制

---

## 关键技术点

### 1. Prompt Engineering

```python
# 角色定位
PREFIX = """
你是一个{profile}，名字是{name}。
你的目标是{goal}。
约束条件：{constraints}
"""

# 任务描述
TASK = """
基于以下输入完成任务：
{input}

要求：
- {requirement_1}
- {requirement_2}
"""

# 输出格式
OUTPUT = """
请以 JSON 格式输出：
{
  "field1": "value1",
  "field2": "value2"
}
"""
```

### 2. 消息路由

```python
# 发布消息
msg = Message(
    content="需求文档",
    cause_by="WritePRD",
    send_to={"Architect"}
)
env.publish_message(msg)

# 订阅消息
class Architect(Role):
    def __init__(self):
        self._watch([WritePRD])  # 订阅 WritePRD 的输出
```

### 3. 记忆管理

```python
class Memory:
    short_term: list[Message]  # 短期记忆（最近消息）
    long_term: dict            # 长期记忆（重要文档）
    
    def add(self, msg):
        self.short_term.append(msg)
        if len(self.short_term) > MAX:
            self._compress()  # 压缩为长期记忆
```

### 4. 序列化恢复

```python
# 保存状态
team.serialize(Path("./storage"))

# 恢复状态
team = Team.deserialize(Path("./storage"))
await team.run()  # 继续执行
```

---

## 代码示例

### 最简示例

```python
from metagpt.software_company import generate_repo

# 一行代码生成项目
repo = generate_repo("创建一个计算器")
```

### 自定义角色示例

```python
from metagpt.roles import Role
from metagpt.actions import Action

class MyAction(Action):
    async def run(self, task: str) -> str:
        prompt = f"完成任务：{task}"
        return await self.llm.aask(prompt)

class MyRole(Role):
    def __init__(self):
        super().__init__(
            name="MyRole",
            profile="专家",
            goal="完成任务"
        )
        self.set_actions([MyAction])
        self._watch([SomeAction])

# 使用
from metagpt.team import Team
team = Team()
team.hire([MyRole()])
await team.run(idea="任务描述")
```

---

## 项目数据

- **Star 数**：44k+ (GitHub)
- **发布时间**：2023 年
- **顶会论文**：ICLR 2024
- **代码规模**：50,000+ 行
- **编程语言**：Python
- **支持 Python 版本**：3.9-3.11
- **核心依赖**：OpenAI, Anthropic, Pydantic

---

## 面试金句

准备几句精炼的话术：

### 项目介绍
> "MetaGPT 是一个发表在 ICLR 2024 的多智能体协作框架，核心思想是 Code = SOP(Team)，通过模拟软件公司的标准流程，让多个 AI 角色协作完成软件开发任务。"

### 技术亮点
> "MetaGPT 使用消息驱动架构和发布-订阅模式，实现了松耦合、可扩展的多智能体系统，每个角色职责明确，通过 SOP 保证输出质量。"

### 个人收获
> "通过深入研究 MetaGPT，我掌握了多智能体协作、Prompt Engineering、异步编程等关键技术，理解了如何将大模型应用到实际场景中。"

### 项目价值
> "MetaGPT 探索了让 AI 像团队一样协作的可能性，这对软件工程自动化、AI Agent 开发等方向具有重要意义，代表了大模型应用的一个重要方向。"

---

## 学习建议

### 1 周快速入门
- 第 1-2 天：阅读 README，运行示例
- 第 3-4 天：阅读核心代码（team.py, role.py, action.py）
- 第 5-6 天：实现一个简单的自定义角色
- 第 7 天：总结核心概念，准备面试话术

### 重点文件
1. `metagpt/team.py` - 团队管理
2. `metagpt/roles/role.py` - 角色基类
3. `metagpt/actions/action.py` - 动作基类
4. `metagpt/roles/product_manager.py` - PM 实现
5. `metagpt/roles/engineer.py` - 工程师实现

### 实践建议
- 用 MetaGPT 生成几个项目，观察输出质量
- 阅读生成的代码，思考改进方向
- 尝试添加自定义角色或 Action
- 对比不同 LLM（GPT-4 vs Claude）的效果
- 分析 Token 使用和成本

---

## 相关资源

- **GitHub**：https://github.com/geekan/MetaGPT
- **文档**：https://docs.deepwisdom.ai/
- **论文**：https://arxiv.org/abs/2308.00352
- **Discord**：https://discord.gg/ZRHeExS6xv

---

**祝面试成功！** 🎉
