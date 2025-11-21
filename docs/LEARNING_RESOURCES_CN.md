# MetaGPT 学习资源索引

> 本文档汇总了所有 MetaGPT 学习资源，帮助你快速找到需要的材料

## 📚 新增学习文档

### 1. [项目详细讲解指南](PROJECT_GUIDE_CN.md)
**适合对象：** 希望深入理解 MetaGPT 项目的学习者

**内容概览：**
- 📖 10 个章节，约 30,000 字
- 🎯 全面覆盖项目概述、核心理念、框架架构
- 💻 详细的代码结构分析和核心组件讲解
- 🎓 面试准备建议和学习路径规划
- 📊 包含架构图、代码示例、表格等

**推荐阅读时间：** 2-3 周系统学习

### 2. [面试准备速查手册](INTERVIEW_PREP_CN.md)
**适合对象：** 准备面试需要快速复习的学习者

**内容概览：**
- ⚡ 快速参考手册，约 8,000 字
- 🎯 10 个高频面试问题及参考答案
- 📝 核心概念速记表和关键技术点
- 💡 面试金句和实用话术
- 🚀 1 周快速入门计划

**推荐阅读时间：** 面试前 1-2 天快速复习

## 📖 官方文档

### 在线文档
- [官方文档（中文）](https://docs.deepwisdom.ai/main/zh/)
- [官方文档（英文）](https://docs.deepwisdom.ai/main/en/)

### 快速开始
- [安装指南](https://docs.deepwisdom.ai/main/zh/guide/get_started/installation.html)
- [快速开始](https://docs.deepwisdom.ai/main/zh/guide/get_started/quickstart.html)
- [配置说明](https://docs.deepwisdom.ai/main/zh/guide/get_started/configuration.html)

### 教程
- [智能体入门（Agent 101）](https://docs.deepwisdom.ai/main/zh/guide/tutorials/agent_101.html)
- [多智能体入门（MultiAgent 101）](https://docs.deepwisdom.ai/main/zh/guide/tutorials/multi_agent_101.html)

### 应用场景
- [数据分析器（Data Interpreter）](https://docs.deepwisdom.ai/main/zh/guide/use_cases/agent/interpreter/intro.html)
- [辩论（Debate）](https://docs.deepwisdom.ai/main/zh/guide/use_cases/multi_agent/debate.html)
- [研究员（Researcher）](https://docs.deepwisdom.ai/main/zh/guide/use_cases/agent/researcher.html)
- [票据助手（Receipt Assistant）](https://docs.deepwisdom.ai/main/zh/guide/use_cases/agent/receipt_assistant.html)

## 📝 代码示例

### 基础示例
```python
# 位置：examples/hello_world.py
# 简单的 LLM 调用示例

from metagpt.llm import LLM

llm = LLM()
response = await llm.aask("你好，请介绍一下 MetaGPT")
```

### 软件开发示例
```python
# 位置：examples/write_game_code.py
# 使用 MetaGPT 生成游戏代码

from metagpt.software_company import generate_repo

repo = generate_repo("创建一个 2048 游戏")
print(repo)  # 打印生成的项目结构
```

### 数据分析示例
```python
# 位置：examples/di/data_visualization.py
# 使用数据分析器进行数据可视化

from metagpt.roles.di.data_interpreter import DataInterpreter

di = DataInterpreter()
await di.run("分析 Iris 数据集并生成可视化图表")
```

### 自定义角色示例
```python
# 位置：examples/build_customized_agent.py
# 创建自定义智能体

from metagpt.roles import Role
from metagpt.actions import Action

class MyAction(Action):
    async def run(self, context):
        return await self.llm.aask(f"处理任务：{context}")

class MyRole(Role):
    def __init__(self):
        super().__init__(name="MyRole", goal="完成自定义任务")
        self.set_actions([MyAction])
```

## 🔬 学术资源

### 核心论文
1. **MetaGPT: Meta Programming for A Multi-Agent Collaborative Framework**
   - 会议：ICLR 2024
   - 链接：https://arxiv.org/abs/2308.00352
   - 介绍：MetaGPT 的核心论文，介绍 Code = SOP(Team) 的理念

2. **Data Interpreter: An LLM Agent For Data Science**
   - 链接：https://arxiv.org/abs/2402.18679
   - 介绍：数据分析器的论文，展示 MetaGPT 在数据科学中的应用

3. **AFlow: Automating Agentic Workflow Generation**
   - 会议：ICLR 2025 Oral (top 1.8%)
   - 链接：https://openreview.net/forum?id=z5uVAKwmjf
   - 介绍：自动化智能体工作流生成

### 更多学术工作
- [学术工作列表](ACADEMIC_WORK.md)

## 🎥 视频教程

### 官方演示
- [MetaGPT 官方演示视频](https://github.com/geekan/MetaGPT/assets/2707039/5e8c1062-8c35-440f-bb20-2b0320f8d27d)

### 社区教程
- [Matthew Berman: How To Install MetaGPT](https://youtu.be/uT75J_KG_aY)

### 在线演示
- [Hugging Face Space 在线体验](https://huggingface.co/spaces/deepwisdom/MetaGPT-SoftwareCompany)

## 🗺️ 学习路径建议

### 路径 1：快速入门（1 周）
**目标：** 了解基本概念，能运行示例

**学习计划：**
1. **Day 1-2**：阅读 [README](../README.md) 和 [快速开始指南](https://docs.deepwisdom.ai/main/zh/guide/get_started/quickstart.html)
2. **Day 3-4**：安装 MetaGPT，运行 `examples/hello_world.py` 和 `examples/write_game_code.py`
3. **Day 5-6**：阅读 [面试准备速查手册](INTERVIEW_PREP_CN.md)
4. **Day 7**：用 MetaGPT 生成一个简单项目，总结学习心得

### 路径 2：深度学习（3-4 周）
**目标：** 深入理解架构，能自定义角色

**学习计划：**
1. **Week 1**：
   - 阅读 [项目详细讲解指南](PROJECT_GUIDE_CN.md) 前 5 章
   - 理解核心概念：Team, Role, Action, Message
   - 阅读核心代码：`team.py`, `role.py`, `action.py`

2. **Week 2**：
   - 阅读 [项目详细讲解指南](PROJECT_GUIDE_CN.md) 后 5 章
   - 学习 Prompt Engineering 和异步编程
   - 实践：实现一个自定义 Role

3. **Week 3**：
   - 深入研究各个 Role 的实现
   - 分析生成代码的质量
   - 实践：实现一个自定义 Action

4. **Week 4**：
   - 阅读相关论文
   - 对比不同的多智能体框架
   - 准备面试话术和案例

### 路径 3：专家级（2-3 个月）
**目标：** 掌握核心技术，能贡献代码

**学习计划：**
1. **Month 1**：完成路径 2 的所有内容
2. **Month 2**：
   - 在实际项目中应用 MetaGPT
   - 优化性能和成本
   - 参与社区讨论
3. **Month 3**：
   - 为 MetaGPT 贡献代码或文档
   - 撰写技术博客
   - 深入研究多智能体理论

## 💡 面试准备清单

### 必须掌握的知识点
- [ ] MetaGPT 的核心理念：Code = SOP(Team)
- [ ] 五大核心组件：Team, Environment, Role, Action, LLM
- [ ] 主要角色：ProductManager, Architect, Engineer
- [ ] 消息流转机制：发布-订阅模式
- [ ] 关键技术：异步编程、Prompt Engineering、记忆管理

### 必须能回答的问题
- [ ] 介绍 MetaGPT 项目（30 秒 + 3 分钟版本）
- [ ] MetaGPT 的架构设计
- [ ] 如何实现多角色协作
- [ ] 如何添加新角色
- [ ] 如何控制成本和 Token 使用
- [ ] MetaGPT vs AutoGPT vs LangChain

### 必须准备的项目经验
- [ ] 用 MetaGPT 生成过至少 3 个项目
- [ ] 分析过生成代码的质量
- [ ] 实现过至少 1 个自定义角色或 Action
- [ ] 了解 Token 使用情况和成本

### 推荐准备的加分项
- [ ] 阅读过 MetaGPT 的核心论文
- [ ] 为 MetaGPT 贡献过代码或文档
- [ ] 撰写过技术博客或文章
- [ ] 对比过不同的多智能体框架

## 🌐 社区资源

### 官方渠道
- **GitHub**: https://github.com/geekan/MetaGPT
- **Discord**: https://discord.gg/ZRHeExS6xv
- **Twitter**: [@MetaGPT_](https://twitter.com/MetaGPT_)
- **邮箱**: alexanderwu@deepwisdom.ai

### 相关项目
- [AutoGPT](https://github.com/Significant-Gravitas/AutoGPT) - 自主 AI Agent
- [LangChain](https://github.com/langchain-ai/langchain) - LLM 应用框架
- [AutoGen](https://github.com/microsoft/autogen) - 微软的多智能体框架
- [CrewAI](https://github.com/joaomdmoura/crewAI) - 角色扮演型多智能体框架

## 📊 项目统计

- **GitHub Stars**: 44,000+
- **代码规模**: 50,000+ 行 Python 代码
- **发布时间**: 2023 年
- **最新版本**: v1.0.0
- **支持 Python 版本**: 3.9-3.11
- **开源协议**: MIT
- **顶会论文**: ICLR 2024

## 🚀 下一步行动

根据你的目标选择：

### 如果你想快速了解项目（1-2 天）
👉 阅读 [面试准备速查手册](INTERVIEW_PREP_CN.md)

### 如果你想深入学习（2-4 周）
👉 阅读 [项目详细讲解指南](PROJECT_GUIDE_CN.md)

### 如果你想实际应用
👉 查看 [官方教程](https://docs.deepwisdom.ai/main/zh/) 和 `examples/` 目录

### 如果你想参与开发
👉 查看 [开发路线图](ROADMAP.md) 和 [贡献指南](../README.md#contribution)

## ❓ 常见问题

### Q: 学习 MetaGPT 需要什么基础？
**A:** 
- 必需：Python 编程基础、对 LLM 的基本了解
- 推荐：异步编程、Prompt Engineering、软件工程基础

### Q: 学习 MetaGPT 需要多长时间？
**A:**
- 快速了解：1-2 天
- 基本掌握：1-2 周
- 深入理解：3-4 周
- 专家级别：2-3 个月

### Q: 是否需要付费 LLM API？
**A:** 是的，MetaGPT 需要调用 LLM API（如 OpenAI、Claude）。建议：
- 学习阶段：使用 GPT-3.5-turbo（成本较低）
- 实际应用：使用 GPT-4（质量更高）
- 也可以使用本地模型（如 Ollama）

### Q: 如何在面试中介绍这个项目？
**A:** 参考 [面试准备速查手册](INTERVIEW_PREP_CN.md) 中的"面试金句"部分，准备好：
- 30 秒电梯演讲
- 3 分钟详细介绍
- 技术深度问题的回答
- 项目实践经验

### Q: 学完这个项目能应聘什么岗位？
**A:**
- 大模型应用工程师
- AI Agent 开发工程师
- 多智能体系统研究员
- 软件工程自动化方向
- AI 产品经理（技术向）

## 📧 反馈与建议

如果你在学习过程中有任何问题或建议，欢迎：
- 在 GitHub 上提 [Issue](https://github.com/geekan/MetaGPT/issues)
- 加入 [Discord 社区](https://discord.gg/ZRHeExS6xv) 讨论
- 发送邮件到 alexanderwu@deepwisdom.ai

---

**祝你学习顺利，面试成功！** 🎉🚀
