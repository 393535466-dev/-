# 学习与会议知识整理助手

`knowledge-organizer` 是一个项目级 Codex Skill，把课堂笔记、课程材料和会议记录整理成可理解、可复习、可执行的知识包。

## 能做什么

| 能力 | 交付内容 |
| --- | --- |
| 总结与划重点 | 一句话摘要、30 秒重点速览、主题框架 |
| 纠错 | 保留原记录、说明修正依据；不确定内容单列待确认 |
| 重要性分级 | 重点 / 次要 / 了解即可，附理由和掌握或行动标准 |
| 表格与图表 | 对照表、概念图、流程图；有真实数值时生成 SVG 柱状图 |
| 记忆辅助 | 主动回忆卡、自测题、可调整的复习建议 |
| 必要知识发散 | 补足前置知识、易混概念、相关应用，默认最多 3 条 |
| 会议跟进 | 区分决策、建议、未决问题，整理负责人、截止日与验收条件 |

学习模式关注理解与应用；会议模式关注决策与跟进；混合材料分别整理。缺失信息不会被补成事实，会议中的建议不会被写成已通过的决议。

## 项目级安装

下载本仓库或克隆到本地。在目标项目中，将以下四项放入 `.agents/skills/knowledge-organizer/`：

```text
.agents/skills/knowledge-organizer/
├── SKILL.md
├── agents/openai.yaml
├── references/
└── scripts/
```

例如，本仓库在项目外时，从目标项目目录执行（将路径换成实际位置）：

```bash
mkdir -p .agents/skills/knowledge-organizer
cp -R /path/to/knowledge-organizer/SKILL.md /path/to/knowledge-organizer/agents /path/to/knowledge-organizer/references /path/to/knowledge-organizer/scripts .agents/skills/knowledge-organizer/
```

重新打开项目或开始新对话，在 Skill 列表中检查是否加载。运行环境需支持 Codex Skills；生成 SVG 的脚本需 Python 3，其余核心整理功能不需要安装 Python 包。

## 触发方式

显式调用最清楚：

```text
$knowledge-organizer 整理以下课堂笔记，指出错误，划分重点、次要与了解即可，生成概念图和记忆卡：
【粘贴笔记】
```

```text
$knowledge-organizer 整理以下会议记录，提取已确认决策、待办和未决问题，缺失负责人或日期请标为待确认：
【粘贴会议记录】
```

也支持自然语言，如“帮我整理学习笔记并划重点”“把会议记录变成行动清单”“生成复习表格、记忆卡和必要知识拓展”。自动匹配由运行环境决定；只说“帮我整理”而没有材料时，会先请你提供材料。

可附加要求：考试主题、学习阶段、会议日期、输出长度、保留原话、只做表格、生成可导出的图表。不会自动录音、参会、发送消息或设置提醒。

## 作业展示

- [学习示例](examples/study.md)：纠正带条件的概念错误，生成分级表、概念图和自测卡。
- [会议示例](examples/meeting.md)：区分决策与建议，保留冲突数据和缺失日期。
- [验收场景](tests/scenarios.md)：人工检查 Skill 是否保留证据、正确处理不确定性。
- [示例数值](examples/chart-data.json)与[SVG 图](examples/chart.svg)：数据均为虚构演示，不是用户真实笔记。

![虚构示例柱状图](examples/chart.png)

## 图表脚本与验证

```bash
python3 scripts/render_bar_chart.py examples/chart-data.json examples/chart.svg
python3 -m unittest discover -s tests -v
```

输入 JSON 必须包含 `title`、`unit`、`source` 和 `items`，每项提供唯一的 `label` 与有限数值 `value`。不接受未知值、重复标签、NaN 或无限值。支持正数、负数和全零数据；不能将概念的重要性凭空变成数值。

Skill 指令是模型行为约定，不是独立运行的应用。结构验证和脚本测试不能保证每次模型输出完全正确；遇到专业事实或冲突信息仍需依据来源核实。公开仓库仅收录 Skill 与虚构示例，不含真实课堂或会议记录。
