# agent-workflow-router

[English](README.md) | **简体中文**

像 Claude Code 和 OpenAI Codex CLI 这样的 AI 编程助手,写代码的能力都不差,
但在真正动手之前该"怎么做"——先写清楚一份需求规范、先写一个会失败的测试、
先搭一个能跑通的雏形,还是干脆直接开始写——它们往往没有明确的判断。这个
仓库把这个决策打包成一个可移植的 [Agent Skill](https://agentskills.io/specification)
——一种 Claude Code、Codex,以及大约 40 个其它智能体工具都原生支持的能力
格式,不需要任何转换步骤。

把它放进你的智能体的 skills 目录,在它开始动手写一个不算简单的编码任务
之前,它会先停下来,看一眼这个任务的规模、模糊程度和风险,然后告诉你该用
哪种工程方法——TDD、规范驱动开发(Spec-Driven Development)、Compound
Engineering、BMAD、GSD、Superpowers,或者压根不需要正式方法——以及应该
配上哪些辅助实践(调研、原型验证、代码审查、安全审查……)。**它只给建议,
从不自己安装或运行任何东西。**

## 实际效果长什么样

一个不算简单的请求:

```
你: 给我的 SaaS 应用加一个权限系统

推荐方法: Compound Engineering —— 这是个会长期维护的产品,把权限模型
一次做对,以后每次加功能都能省事;而且现在有时间把这次的决策写下来。

涉及的阶段:
  Frame(界定问题):  梳理现有角色和访问模式              [调研]
  Decide(做决定):   选定权限模型(RBAC 还是 ABAC)       [原型验证]
  Specify(写清楚):  写出访问控制的规范文档               [写规范]
  Plan(拆计划):    拆成几个里程碑
  Build(动手写):   实现鉴权逻辑                          [TDD]
  Verify(校验):    合并前做审查                          [代码审查, 安全审查]
  Compound(沉淀):  把这次的模型思路写下来,留给以后的功能用  [文档沉淀]

明确排除:
  BMAD —— 用不上;这是在现有产品里加一个功能,不是从零定义一个产品。
  Superpowers —— 除非你确实想要那种强约束的执行风格,否则不需要。

只给建议——没有安装或运行任何东西。如果上面哪个判断信号错了,请指出来。
```

一个很简单的请求:

```
你: 把按钮颜色改成蓝色

bypass —— 简单、低风险、单文件的 UI 调整。
直接进入 Build(动手写) → Verify(校验)。
```

## 安装 —— Claude Code

```bash
# 个人级(所有项目都能用):
mkdir -p ~/.claude/skills
cp -r skills/agent-workflow-router ~/.claude/skills/agent-workflow-router
# —— 或者用软链接,方便跟着仓库一起更新 ——
ln -s "$(pwd)/skills/agent-workflow-router" ~/.claude/skills/agent-workflow-router

# 项目级(只在这一个项目里生效):
mkdir -p /path/to/your-project/.claude/skills
cp -r skills/agent-workflow-router /path/to/your-project/.claude/skills/agent-workflow-router
```

重启 Claude Code,然后试着问一句:"我该怎么给我的应用加一个权限系统?"

## 安装 —— Codex CLI

```bash
# 个人级:
mkdir -p ~/.agents/skills
cp -r skills/agent-workflow-router ~/.agents/skills/agent-workflow-router

# 项目级:
mkdir -p /path/to/your-project/.agents/skills
cp -r skills/agent-workflow-router /path/to/your-project/.agents/skills/agent-workflow-router
```

部分较老版本的 Codex 及其文档用的是 `.codex/skills/` 而不是
`.agents/skills/`。如果这个 skill 没被识别到,两个路径都可以试一下——
截至目前,`.agents/skills/` 是官方文档记载的路径。

## 如何扩展或更新这个 router

这个项目本来就打算长期由不止一个人维护:

- **想加一个方法学:** 编辑
  [`skills/agent-workflow-router/references/methods.md`](skills/agent-workflow-router/references/methods.md),
  照着已有条目的模板写(是什么 / 适用信号 / 侧重哪些阶段 / 权威出处——
  尽量给真实链接,没有的话写"link TBD",不要编一个)。同时在
  [`decision-guide.md`](skills/agent-workflow-router/references/decision-guide.md)
  的矩阵里加一行。发 PR。
- **想加一个辅助能力:** 在
  [`capability-catalog.md`](skills/agent-workflow-router/references/capability-catalog.md)
  里加一行——名字、一句话描述、对应的阶段。发 PR。
- 这个仓库**故意做成人工维护**,不从 GitHub 或任何 marketplace 自动
  同步。这里没有 bot,也没有定时任务——这是有意为之:不能让某个突然
  火起来的仓库悄悄改写了"标准方法"是什么。详见 `SKILL.md` 里的
  "核心原则(Core principles)"部分。

## 仓库信息

- **许可证:** MIT——见 [`LICENSE`](LICENSE)。
- **这个 skill 不会做的事:** 不会自动安装或运行任何第三方 skill、工具
  或包;不会执行任何 shell 命令;本身不带任何脚本——只读它自己的三个
  引用文件然后推理;不会跨会话保存偏好或状态;不会联网扫描或"打电话
  回家"——能力目录就是一份人工维护的静态文件。

---

*skill 本体(`SKILL.md` 和 `references/` 里的内容)是纯英文写的,这是
为了让 Claude Code、Codex 和其它支持 Agent Skills 格式的工具都能无障碍
读取。这份中文说明只是给人看的介绍,不影响 skill 实际的运行方式。*
