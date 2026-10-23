# response-protocol · 话术规范

A DeepSeek Harness skill that decides **when to answer and when to ask first**.

一个 DSH 技能：回答前先判断问题类型，决定「直接回答」还是「先澄清」，把询问成本压到最低。

## 它解决什么问题

大多数助手有两个相反的毛病：对事实性问题反问（「你要用在哪？」），或者对需求模糊的任务硬猜。这个技能用一套可执行的流程同时压住两头：

1. **分类**——A 信息询问型（有客观答案）/ B 决策型（取决于提问者偏好）/ C 任务需求型（交付物，质量依赖细节）；
2. **判充分度**——现有信息够不够给出高质量结果；
3. **分流**——A 类充分就直接答；A 类缺关键信息最多问 1 个问题；B/C 类不充分才进澄清；
4. **澄清预算**——最多 1 轮、最多 3 个问题，用户说「你决定」立刻停止追问；
5. **收尾自检**——需求是否覆盖、假设是否标注、风险是否说明、与确认过的理解有偏差是否指出。

## 什么时候触发

- 用户给出模糊的任务需求（「帮我写个 PPT」「做个排序算法」）；
- 用户提出需要个性化判断的问题（「选哪个专业」「今晚吃什么」）；
- 需要判断「这题该不该反问」的场合。

事实性、有标准答案的问题不会触发反问——这是技能里写死的禁止项。

## 安装

这是一个纯文本技能，没有依赖、没有脚本。

```bash
# 1. 复制到 DSH 技能目录
# macOS / Linux
cp -r response-protocol ~/.dsh/skills/

# Windows (PowerShell)
Copy-Item -Recurse response-protocol "$env:USERPROFILE\.dsh\skills\response-protocol"
```

2. 不需要重启：DSH 会自动扫描技能目录，下一次会话即可看到 `response-protocol`。

> 技能目录默认是 `$DSH_HOME/skills`，`DSH_HOME` 未设置时为 `~/.dsh`。

## 目录结构

```text
response-protocol/
├── SKILL.md      # 技能本体：判定流程、澄清流程、自检、禁止项、示例
├── README.md
├── LICENSE
└── .gitignore
```

## 设计取舍

- **只写规则，不写输出模板**：自检只列有问题的项，避免每次回复都套四行八股；
- **澄清必须带选项**：禁止开放式提问，问题本身就是选择题；
- **一轮封顶**：一轮之后仍不明确，给保守默认方案并标注假设，而不是继续盘问。

## License

MIT
