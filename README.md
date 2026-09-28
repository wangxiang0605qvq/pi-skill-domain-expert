# pi-skill-domain-expert

pi 技能：领域专家模式。提问涉及专业判断（技术、工程、医疗、法律、金融、产品、学术等）时启用：先判定领域与问题类型，再按该领域标准方法分析，给出结论、依据、风险与可执行的下一步；需要外部依据时用 `web_search` 检索并给出专业来源。

## 结构（两层，省 token）

```text
domain-expert/
├── SKILL.md            # 摘要：全部硬规则，每次只读这一份
└── references/
    └── full.md         # 完整版：领域细则、检索边界、反例
```

pi 启动时只把 skill 的 `name` / `description` / 路径注入 system prompt，全文由模型按需读取。所以：

- 日常执行：模型读 `SKILL.md`（约 2.7 KB）即可，不再读别的文件。
- 需要判断边界、查反例：读一次 `references/full.md`；同一会话内不重复读。

## 规则要点

1. **判定**：一行给出领域、问题类型（诊断/决策/设计/估算/排错）、关键约束。
2. **分析**：用该领域真实的度量与术语、公认阈值与规范、常见失效模式、可证伪的验证方式。
3. **给意见**：结论、依据、风险与不确定性、下一步动作（最多 3 条），顺序固定。
4. **检索**：只在结论依赖易变或高风险事实时搜；用 `web_search`（`count` 默认 3、最多 8），默认 1 次、最多 2 次；来源优先官方文档、标准与法规原文；查不到写「未查证」。
5. **边界**：不虚构条款、数据与版本行为；现场测量、体检、出庭这类事只给判断框架并说明该由什么角色接手。

## 安装

### 方式一：作为 pi 包安装（推荐）

```bash
pi install git:github.com/wangxiang0605qvq/pi-skill-domain-expert
```

### 方式二：手动复制

```bash
mkdir -p ~/.pi/agent/skills/domain-expert/references
cp domain-expert/SKILL.md ~/.pi/agent/skills/domain-expert/SKILL.md
cp domain-expert/references/full.md ~/.pi/agent/skills/domain-expert/references/full.md
```

然后 `/reload`。

## 版权

著作权归作者所有，保留一切权利。详见 [LICENSE](LICENSE)。
