# OfferPilot

OfferPilot 是一个面向求职者的 Codex Skill。用户只需提供公司名、职位链接、JD 或截图，即可获得公司风险、岗位性质、薪资福利和待核实问题的初步诊断；需要进一步判断个人匹配时，再补充脱敏简历或简短工作背景。

## 核心能力

- 调查公司主体、招聘主体、公开风险和近期负面事件
- 汇总员工、前员工与候选人的公开反馈，并标注可信度
- 识别外包、派遣、驻场、职责错位和可疑招聘信号
- 分析岗位门槛、薪资结构、福利完整度与个人匹配
- 提供简历修改优先级、面试核实问题和交互式面试训练
- 在证据不足时明确降级，不把匿名评论或推测写成事实

## 使用方式

将本目录放入 Codex 的 Skills 目录，例如：

```text
~/.codex/skills/offerpilot/
```

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

修改后运行：

```bash
python3 /Users/a1-6/.codex/skills/.system/skill-creator/scripts/quick_validate.py .
```

检查是否残留模板内容：

```bash
rg -n 'TODO|\[TODO|Structuring This Skill' SKILL.md agents references
```

详细贡献规范参见 [AGENTS.md](AGENTS.md)。

## 隐私与诚信

提交简历前，请隐藏姓名、电话、邮箱、身份证号、精确地址、客户机密和未公开业务指标。OfferPilot 不应协助伪造工作经历、项目数据、推荐信或面试证据。
