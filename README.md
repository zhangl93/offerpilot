# OfferPilot

OfferPilot 是一个面向求职者的 Codex Skill。用户只需提供公司名、职位链接、JD 或截图，即可获得公司风险、岗位性质、薪资福利和待核实问题的初步诊断；需要进一步判断个人匹配时，再补充脱敏简历或简短工作背景。

## 核心能力

- 调查公司主体、招聘主体、公开风险和近期负面事件
- 汇总员工、前员工与候选人的公开反馈，并标注可信度
- 识别外包、派遣、驻场、职责错位和可疑招聘信号
- 分析岗位门槛、薪资结构、福利完整度与个人匹配
- 提供简历修改优先级、面试核实问题和交互式面试训练
- 在证据不足时明确降级，不把匿名评论或推测写成事实

## 安装

安装前建议先阅读 `SKILL.md` 和 `references/`。OfferPilot 本身不包含可执行脚本，但运行时会搜索互联网并处理用户提供的招聘材料。

### 方式一：使用 Skills CLI（推荐）

[Skills CLI](https://www.skills.sh/docs/cli) 可以把同一个 Skill 安装到多个兼容的编码代理。需要 Node.js 18 或更高版本。

先查看仓库中可安装的 Skill：

```bash
npx skills add zhangl93/offerpilot --list
```

全局安装到 Codex 和 Claude Code：

```bash
npx skills add zhangl93/offerpilot \
  --skill offerpilot \
  --agent codex \
  --agent claude-code \
  --global \
  --yes
```

去掉 `--global` 可安装到当前项目。CLI 默认推荐使用符号链接维护单一副本，也可以增加 `--copy` 创建独立副本。

### 方式二：安装到 Codex

Codex 的用户级 Skill 存放在 `~/.agents/skills/`，适用于所有项目：

```bash
mkdir -p ~/.agents/skills
git clone https://github.com/zhangl93/offerpilot.git \
  ~/.agents/skills/offerpilot
```

只在当前仓库使用时，用复制模式安装到项目目录，避免产生嵌套 Git 仓库：

```bash
npx skills add zhangl93/offerpilot \
  --skill offerpilot \
  --agent codex \
  --copy \
  --yes
```

也可以在 Codex 中让内置安装器处理 GitHub 仓库：

```text
使用 $skill-installer 安装 https://github.com/zhangl93/offerpilot.git
```

Codex 通常会自动发现变化；如果没有出现，重启 Codex。使用 `/skills` 查看已发现的 Skills，或输入 `$offerpilot` 显式调用。参见 [Codex Skills 官方文档](https://developers.openai.com/codex/skills)。

### 方式三：安装到 Claude Code

Claude Code 的个人 Skill 存放在 `~/.claude/skills/`：

```bash
mkdir -p ~/.claude/skills
git clone https://github.com/zhangl93/offerpilot.git \
  ~/.claude/skills/offerpilot
```

只在当前项目使用时，用复制模式安装到 `.claude/skills/`：

```bash
npx skills add zhangl93/offerpilot \
  --skill offerpilot \
  --agent claude-code \
  --copy \
  --yes
```

如果团队希望独立跟踪上游版本，可以改用正式子模块，例如 `git submodule add https://github.com/zhangl93/offerpilot.git .claude/skills/offerpilot`，并同时提交 `.gitmodules`。

Claude Code 会监视已存在的 Skill 目录；如果是在会话中首次创建顶层目录，请重启 Claude Code。在 Claude Code 中输入 `/offerpilot` 显式调用，也可以让 Claude 根据描述自动启用。参见 [Claude Code Skills 官方文档](https://code.claude.com/docs/en/slash-commands)。

### 方式四：Git clone 加符号链接

希望 Codex 和 Claude Code 共用一份、并通过 `git pull` 更新时，可以采用业界常见的“集中克隆＋符号链接”方式：

```bash
git clone https://github.com/zhangl93/offerpilot.git \
  ~/.local/share/agent-skills/offerpilot

mkdir -p ~/.agents/skills ~/.claude/skills
ln -s ~/.local/share/agent-skills/offerpilot ~/.agents/skills/offerpilot
ln -s ~/.local/share/agent-skills/offerpilot ~/.claude/skills/offerpilot
```

更新 Skill：

```bash
git -C ~/.local/share/agent-skills/offerpilot pull --ff-only
```

Windows 用户可以使用 Skills CLI 的 `--copy` 模式，避免符号链接权限问题。

## 使用方式

在 Codex 中调用：

```text
使用 $offerpilot 帮我调查这个职位：https://example.com/job/123
```

也可以直接提供最少信息：

```text
帮我看看某某科技的 Java 后端岗位是否值得投。
```

进入个人匹配阶段时，无需先提交完整简历，可以只提供三行背景：

```text
Java 后端，8 年经验；主要做电商订单系统；期望上海 25K 以上。
```

## 输出内容

默认报告优先回答：

1. 公司是否存在需要注意的公开风险；
2. 岗位实际在招什么，招聘要求是否合理；
3. 薪资福利哪些已经确认，哪些仍需核实；
4. 是否值得继续沟通，以及下一步必须问什么。

OfferPilot 不保证公司绝对可靠，也不会仅凭单条匿名评价判定公司或岗位有问题。所有动态信息应联网核实，并区分已确认事实、公开报道、用户反馈、推断和未知信息。

## 项目结构

```text
offerpilot/
├── SKILL.md
├── AGENTS.md
├── README.md
├── agents/
│   └── openai.yaml
└── references/
    ├── report-templates.md
    └── source-evaluation.md
```

- `SKILL.md`：核心工作流、交互规则和安全边界
- `agents/openai.yaml`：Skill 的展示名称、简介和默认提示词
- `references/source-evaluation.md`：信息源分级与交叉验证规则
- `references/report-templates.md`：快速避坑、个人匹配和面试复盘模板

## 开发与校验

使用通用 Skills CLI 检查当前目录能否被识别：

```bash
npx skills add . --list
```

输出中应包含 `offerpilot`。如果本机安装的 Codex 包含内置 `skill-creator`，还可以额外运行其快速校验；该路径是 Codex 安装细节，不保证在 Claude Code 或所有 Codex 版本中存在：

```bash
VALIDATOR="${CODEX_HOME:-$HOME/.codex}/skills/.system/skill-creator/scripts/quick_validate.py"
test -f "$VALIDATOR" && python3 "$VALIDATOR" .
```

检查是否残留模板内容：

```bash
rg -n 'TODO|\[TODO|Structuring This Skill' SKILL.md agents references
```

详细贡献规范参见 [AGENTS.md](AGENTS.md)。

## 隐私与诚信

提交简历前，请隐藏姓名、电话、邮箱、身份证号、精确地址、客户机密和未公开业务指标。OfferPilot 不应协助伪造工作经历、项目数据、推荐信或面试证据。
