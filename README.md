# OfferPilot

> 发来一个职位链接，先查公司和岗位风险，再决定是否值得投。

[![GitHub stars](https://img.shields.io/github/stars/zhangl93/offerpilot?style=flat-square)](https://github.com/zhangl93/offerpilot)

OfferPilot 是一个面向求职者的 Agent Skill。它从公司名、职位链接、JD 或截图开始，调查公司主体、公开风险、员工反馈、岗位性质、薪资福利和个人匹配；不要求用户先填写长表单，也不会把匿名评论或推测写成事实。

## Why

招聘信息往往只告诉你“公司想要什么”，却没有回答求职者真正关心的问题：

- 招聘主体、品牌公司和合同主体是不是同一家？
- 岗位是正式研发，还是外包、派遣、驻场或交付？
- 薪资范围是否合理，固定工资、绩效和福利是否说清楚？
- 公开负面事件和员工反馈中，有哪些信号值得核实？
- 我的经历是否匹配，面试官最可能追问哪里？

OfferPilot 将公开事实、媒体报道、用户反馈、推断和未知信息分开呈现，帮助你减少无效投递和入职踩坑。

## Showcase

输入一条职位链接：

```text
帮我看看这个岗位是否值得投：https://example.com/job/123
```

OfferPilot 默认返回一份短报告：

```text
初步结论：核实关键问题后再决定

重点发现
- 公司：招聘主体与品牌主体不同，合同主体尚未确认。
- 岗位：名称是后端研发，但职责包含长期驻场和客户培训。
- 待遇：月薪范围明确，绩效比例、公积金基数和年终奖未说明。

下一步必须问
1. 劳动合同与哪家公司签？
2. 驻场时间占比是多少？
3. 固定工资与绩效工资分别是多少？

信息边界
- 以上为演示内容；真实报告会标注查询日期、证据类型和来源链接。
```

## 30 秒开始

安装后无需记住命令，直接用自然语言提问即可。需要强制调用时：

| 使用环境 | 调用方式 |
|---|---|
| Codex | `$offerpilot https://example.com/job/123` |
| Claude Code | `/offerpilot https://example.com/job/123` |
| 自动触发 | `帮我看看这个岗位是否值得投：https://example.com/job/123` |

Codex 也可以输入 `/skills` 后选择 OfferPilot。没有链接时，只提供“公司名＋岗位名”即可开始。

## Install

安装前建议阅读 [`SKILL.md`](SKILL.md) 和 [`references/`](references/)。OfferPilot 本身不包含可执行脚本，但运行时会搜索互联网并处理用户提供的招聘材料。

### Skills CLI（推荐）

当前 [Skills CLI](https://www.skills.sh/docs/cli) 实际需要 Node.js 20.12.0 或更高版本，推荐 Node.js 22 LTS。

```bash
npx skills add zhangl93/offerpilot \
  --skill offerpilot \
  --agent codex \
  --agent claude-code \
  --global \
  --yes
```

去掉 `--global` 可安装到当前项目；增加 `--copy` 可创建独立副本。

<details>
<summary>Codex 手动安装</summary>

用户级 Skill 目录为 `~/.agents/skills/`：

```bash
mkdir -p ~/.agents/skills
git clone https://github.com/zhangl93/offerpilot.git \
  ~/.agents/skills/offerpilot
```

也可以在 Codex 中输入：

```text
使用 $skill-installer 安装 https://github.com/zhangl93/offerpilot.git
```

如果没有自动出现，请重启 Codex。参见 [Codex Skills 文档](https://developers.openai.com/codex/skills)。

</details>

<details>
<summary>Claude Code 手动安装</summary>

个人 Skill 目录为 `~/.claude/skills/`：

```bash
mkdir -p ~/.claude/skills
git clone https://github.com/zhangl93/offerpilot.git \
  ~/.claude/skills/offerpilot
```

如果是在会话中首次创建顶层 Skill 目录，请重启 Claude Code。参见 [Claude Code Skills 文档](https://code.claude.com/docs/en/slash-commands)。

</details>

<details>
<summary>Codex 与 Claude Code 共用一份</summary>

```bash
git clone https://github.com/zhangl93/offerpilot.git \
  ~/.local/share/agent-skills/offerpilot

mkdir -p ~/.agents/skills ~/.claude/skills
ln -s ~/.local/share/agent-skills/offerpilot ~/.agents/skills/offerpilot
ln -s ~/.local/share/agent-skills/offerpilot ~/.claude/skills/offerpilot
```

Windows 用户可以使用 Skills CLI 的 `--copy` 模式，避免符号链接权限问题。

</details>

## Update

使用 Skills CLI 安装时，重新执行安装命令即可覆盖更新。使用 Git clone 或符号链接安装时：

```bash
git -C ~/.local/share/agent-skills/offerpilot pull --ff-only
```

如果直接克隆到了某个Agent目录，请将命令中的路径换成实际安装路径。

## Use

无需完整简历，也无需一次提供所有信息。

```text
帮我查一下某某公司的负面新闻和员工反馈
分析这份JD的外包、驻场和薪资风险
Java后端，8年经验，主要做订单系统；我适合这个岗位吗？
根据这份JD模拟面试，先从深挖模式开始
这是我的面试记录，帮我判断问题出在哪里
```

OfferPilot 会根据已有材料先给出可完成的分析，每轮最多追问两个会实质改变结论的问题。

## What it checks

- **公司**：法律主体、招聘主体、经营风险、近期事件和公开反馈
- **岗位**：硬门槛、实际职责、外包派遣、驻场交付和可疑招聘信号
- **待遇**：固定与绩效工资、几薪、试用期、五险一金、年假和补贴
- **个人匹配**：已有证据、简历未体现、真实缺口和投递优先级
- **面试**：高风险问题、项目深挖、回答可信度和复盘训练

## Evidence & privacy

- 不把单条匿名评论作为确定事实
- 不把搜索命中数冒充已审阅样本
- 同名公司主体未确认前，不归属负面信息
- 找不到负面信息不等于公司没有风险
- 不编造经历、项目数据、推荐信或面试证据
- 上传简历前，请隐藏电话、邮箱、身份证号、客户机密和未公开指标

完整规则参见 [`references/source-evaluation.md`](references/source-evaluation.md)。

## Troubleshooting

如果出现：

```text
SyntaxError: The requested module 'node:util' does not provide an export named 'styleText'
```

说明当前Node.js版本低于20.12.0。推荐升级到Node.js 22：

```bash
nvm install 22
nvm use 22
node --version
```

不希望升级Node.js时，请使用上面的Git clone安装方式；OfferPilot本身不依赖Node.js。

## Project

```text
offerpilot/
├── SKILL.md
├── agents/openai.yaml
└── references/
    ├── report-templates.md
    └── source-evaluation.md
```

- `SKILL.md`：核心工作流、交互规则和安全边界
- `agents/openai.yaml`：展示名称、简介和默认提示词
- `references/`：证据规范和输出模板

## Development

检查Skill能否被通用CLI识别：

```bash
npx skills add . --list
```

如果当前Codex安装包含内置 `skill-creator`，还可以运行：

```bash
VALIDATOR="${CODEX_HOME:-$HOME/.codex}/skills/.system/skill-creator/scripts/quick_validate.py"
test -f "$VALIDATOR" && python3 "$VALIDATOR" .
```

贡献规范参见 [`AGENTS.md`](AGENTS.md)。
