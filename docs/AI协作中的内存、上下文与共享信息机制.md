# AI协作中的内存、上下文与共享信息机制

## 1. 文档定位

这份文档是：

- 给你学习和理解这套协作模板用的解释型文档
- 用来说明 AI 协作中的 memory、context、shared files、recovery 和 RAG 取舍

这份文档不是：

- `active.md` 的运行规则文件
- Agent 启动即执行的核心约束文件
- 代替 `.claude/docs/context-management.md` 或 `.claude/docs/session-state-rules.md` 的规范文件

如果你要找真正的运行规则，请看：

- `.claude/docs/context-management.md`
- `.claude/docs/session-state-rules.md`
- `production/session-state/active.md`

这三者的关系是：

- `.claude/docs/context-management.md`
  - 会话管理总策略
- `.claude/docs/session-state-rules.md`
  - `active.md` 的精确规则
- `production/session-state/active.md`
  - 当前实际运行中的状态文件

---

## 2. 这份文档解决什么问题

当你开始真正用这个协作模板时，很快会遇到几个关键问题：

1. Agent 的“内存”到底是什么
2. 上下文信息保存在哪里
3. 多个 Agent 协作时，共享信息怎么传递
4. 关闭窗口、网络中断、新开会话后，哪些信息还能无缝恢复
5. 为什么这个模板没有 RAG，是否会影响软件项目协作

这份文档专门用来解释这些机制，而且只基于当前仓库已经实现的真实结构来讲，不讲抽象空话。

它的目标是帮助你正确理解系统，而不是直接作为运行期规则被执行。

---

## 3. 先把“内存”和“上下文”分开理解

很多人会把这两个概念混在一起，但在这个模板里，它们不是一回事。

## 3.1 内存信息

这里更接近指：

- 当前对话窗口里，模型暂时记住的内容
- 还没有写入文件、仍停留在聊天里的推理和讨论
- 当前轮次仍可直接利用的短期记忆

这类信息的特点是：

- 使用起来很方便
- 但不稳定
- 容易因为压缩、清空、新会话、断线而丢失或弱化

所以它不是一个可靠的长期状态载体。

---

## 3.2 上下文信息

这里更准确地说，是：

- 当前任务真正依赖的可恢复事实
- 当前工作阶段
- 已做出的关键决策
- 正在改哪些文件
- 还有什么没完成

在这个模板里，真正可靠的上下文不是聊天记录，而是写进文件系统的信息。

这也是 `.claude/docs/context-management.md` 里最重要的一句话：

> The file is the memory, not the conversation.

---

## 4. 这个模板的真实“长期记忆”存在哪里

从当前仓库实现来看，长期记忆主要存放在 4 类地方。

## 4.1 当前会话状态文件

最核心的是：

- `production/session-state/active.md`

它是整个模板里最重要的“可恢复工作记忆”文件。

按照 `.claude/docs/context-management.md` 的要求，它应该持续记录：

- 当前任务是什么
- 当前进行到哪一步
- 已完成哪些关键里程碑
- 做过哪些关键决策
- 正在操作哪些文件
- 还有哪些问题待确认

你可以把它理解成：

> 当前会话的工作现场记录。

---

## 4.2 会话归档和审计日志

这些文件主要放在：

- `production/session-logs/session-log.md`
- `production/session-logs/compaction-log.txt`
- `production/session-logs/agent-audit.log`

作用分别是：

### `production/session-logs/session-log.md`

- 会话结束时归档当前状态
- 记录近期提交和未提交修改

### `production/session-logs/compaction-log.txt`

- 记录发生过上下文压缩

### `production/session-logs/agent-audit.log`

- 记录子 Agent 被调用的情况

这些文件更像“工作室审计和追踪记录”，不是主状态文件，但对恢复和复盘很有价值。

---

## 4.3 真正的项目文件本身

这类文件包括：

- `src/`
- `assets/`
- `design/`
- `docs/`
- `tests/`
- `tools/`
- `production/`

这些文件里保存的是最可靠的项目知识。

例如：

- 设计写进 `design/`
- 技术决策写进 `docs/`
- 实现写进 `src/`
- 测试写进 `tests/`
- 冲刺和里程碑写进 `production/`

只要这些信息已经写入文件，它们就不再依赖聊天记忆存在。

---

## 4.4 工作室治理文件

这类文件主要在：

- `CLAUDE.md`
- `.claude/agents/`
- `.claude/rules/`
- `.claude/skills/`
- `.claude/docs/`

它们保存的是：

- 岗位职责
- 路径规则
- 标准流程
- 技术偏好
- 项目总章程

这部分不是“当前任务状态”，但它是所有 Agent 共享的工作基础。

---

## 5. 新会话、压缩、崩溃后，怎么无缝恢复

这个模板主要靠 hook 和文件化状态来做恢复。

## 5.1 新会话时怎么恢复

恢复入口主要来自：

- `.claude/hooks/session-start.sh`

这个 hook 会在新会话开始时：

- 显示当前分支
- 显示最近提交
- 显示当前 sprint 和 milestone
- 检查 `production/session-state/active.md` 是否存在
- 如果存在，自动提示“这里有上次会话状态，先读这个文件”

也就是说，新会话恢复的第一入口不是旧聊天，而是：

- `production/session-state/active.md`

---

## 5.2 上下文压缩前怎么保底

保底入口主要来自：

- `.claude/hooks/pre-compact.sh`

这个 hook 会在压缩前输出：

- `active.md` 的内容摘要
- 当前工作树里修改了哪些文件
- 哪些设计文档仍带有 WIP / TODO 标记
- 恢复提示

所以压缩不是“完全失忆”，而是先把关键状态吐出来，再进入压缩。

---

## 5.3 会话结束时怎么归档

归档入口主要来自：

- `.claude/hooks/session-stop.sh`

它会：

- 把 `active.md` 归档到 `production/session-logs/session-log.md`
- 记录近期提交
- 记录当前未提交修改

也就是说，哪怕正常退出，这个模板也尽量把工作痕迹留在仓库里，而不是只留在聊天窗口。

---

## 5.4 子 Agent 的协作信息怎么追踪

子 Agent 调用会通过：

- `.claude/hooks/log-agent.sh`

记录到：

- `production/session-logs/agent-audit.log`

这不是完整的“子 Agent 记忆复制”，但至少会留下：

- 某个时间点调用过哪类 Agent

如果你要追更完整的协作结果，仍然要靠：

- `active.md`
- 实际写入的文件
- handoff 摘要

---

## 6. 多个 Agent 协作时，共享信息是怎么传递的

这个模板里的共享信息机制，不是一个神秘的中央记忆池，而是三层组合。

## 6.1 第一层：共享治理层

所有 Agent 默认共享：

- `CLAUDE.md`
- `.claude/agents/`
- `.claude/rules/`
- `.claude/skills/`
- `.claude/docs/`

这部分告诉所有 Agent：

- 项目是什么
- 规则是什么
- 岗位如何协作
- 标准流程是什么

这是“共同工作语言”。

---

## 6.2 第二层：共享项目文件层

所有 Agent 都可以基于仓库中文件继续工作，例如：

- `design/gdd/*.md`
- `docs/architecture/*.md`
- `src/**/*`
- `tests/**/*`
- `production/**/*`

这部分是真正的项目知识载体。

当任务转给下一个 Agent 时，最可靠的交接方式就是：

- 上一个 Agent 已经把关键结论写进这些文件

---

## 6.3 第三层：共享会话状态层

这部分主要是：

- `production/session-state/active.md`

它承接的是：

- 当前任务进度
- 当前会话关注点
- 当前已确认决定
- 当前下一步动作

这类信息最适合作为“跨 Agent、跨会话、跨窗口”的短中期桥梁。

---

## 7. 任务转交给下一个 Agent 时，信息到底去哪了

任务转交并不是把“一个脑子里的想法”直接复制到另一个脑子里。

更准确地说，它靠 4 种方式完成交接：

1. 当前会话中的 handoff 摘要
2. `production/session-state/active.md`
3. 已写入的项目文件
4. 当前工作树中的修改文件

所以如果你问：

> “转交给下一个 Agent，这些信息在哪里？”

答案是：

- 一部分在当前会话摘要里
- 一部分在 `active.md`
- 最重要的一部分在真正的文件系统里

这也是为什么这个模板一直强调：

- 关键决策尽快落盘
- 不要把重要事实只留在聊天里

---

## 8. 新开会话、转交、断线、关窗后，哪些东西还在

最清晰的理解方式是下面这张表。

| 信息类型 | 平时在哪里 | 新会话后 | 断线或关窗后 |
|---|---|---|---|
| 当前聊天短期记忆 | 当前对话上下文 | 可能只剩摘要 | 不可靠 |
| 当前任务状态 | `production/session-state/active.md` | 可恢复 | 在 |
| 会话归档 | `production/session-logs/session-log.md` | 可读 | 在 |
| 子 Agent 调用痕迹 | `production/session-logs/agent-audit.log` | 可读 | 在 |
| 项目知识 | `src/`、`design/`、`docs/`、`tests/` 等 | 可读 | 在 |
| 工作室规则 | `CLAUDE.md`、`.claude/*` | 可读 | 在 |

最重要的结论就是：

> 最不可靠的是聊天里的脑内记忆，最可靠的是已经写入仓库的文件。

---

## 9. 怎样让 AI 在信息越来越多时，依然保持“聪明”

这是最关键的问题之一。

AI 变笨，通常不是因为事情变多，而是因为：

- 重要信息没有结构化
- 聊天里堆了太多低价值文字
- 关键决策没有及时沉淀
- 一个会话扛了太多不同话题

这个模板的解决思路是：

> 不依赖越来越长的聊天记录，而依赖越来越清晰的文件化结构。

## 9.1 把关键决策尽快写入文件

例如：

- 设计写进 `design/`
- 架构决策写进 `docs/`
- 当前任务写进 `production/session-state/active.md`

这样就能把聊天中的重要事实抽离出来，变成稳定知识。

---

## 9.2 `active.md` 只保留高价值摘要

不要把 `production/session-state/active.md` 写成一份新的长聊天记录。

它应该只保留：

- 当前任务
- 当前步骤
- 已完成事项
- 关键决策
- 影响文件
- 未决问题

这样它才能成为高密度、高恢复力的状态文件。

---

## 9.3 增量写文档，而不是长时间只聊天

`.claude/docs/context-management.md` 已经给出很明确的建议：

1. 先创建文件 skeleton
2. 一节一节讨论
3. 用户批准后立即写入
4. 写完一节就更新 `active.md`

这种方式最大的好处是：

- 已完成内容不再占据聊天上下文
- 聊天只保留当前正在处理的局部问题

---

## 9.4 主动压缩，不要等爆窗

这个模板建议在大约 60%-70% 上下文使用率时就主动 compact。

最佳压缩点通常是：

- 一节文档刚写完
- 一个任务刚完成
- 一个话题即将切换
- 一个 Agent 阶段刚结束

---

## 9.5 善用新会话交接

如果：

- 主任务完成了
- 话题明显漂移了
- 角色需要切换了
- 回合数已经太长了

就应该主动新开会话，并附带 handoff prompt。

这样做不是打断思路，而是在保护 Agent 的清晰度和准确度。

---

## 9.6 用子 Agent 做消耗型探索

如果某项任务需要：

- 读大量文件
- 搜大量线索
- 做大量横向对比

更适合让子 Agent 去探索，再把高价值摘要返回主线程。

这样主会话就不会被大量低价值探索内容污染。

---

## 10. 这个模板为什么没有 RAG

当前仓库里并没有一个正式的 RAG 模块。

也就是说，它没有：

- 向量数据库
- 嵌入召回
- 语义检索管线
- 独立知识库检索服务

它当前采用的是另一种思路：

- 文件化状态管理
- 清晰目录结构
- 路径规则约束
- 主动 handoff
- 搜索工具和直接读文件

---

## 11. 软件项目中是不是不需要 RAG

不是“不需要”，而是：

> 对很多中小型、单仓库、结构清楚的软件项目，先不做 RAG 也完全可以很好地工作。

尤其当你已经具备下面这些条件时：

- 目录结构清晰
- 文档纪律良好
- 关键状态会写入文件
- GDD / ADR / 技术文档较规范
- Agent 能通过搜索快速找到需要的文件

在这种情况下，RAG 的收益未必马上高于成本。

---

## 12. 什么情况下 RAG 才会明显更有价值

当项目发展到这些情况时，RAG 会开始更有意义：

- 仓库非常大
- 多仓库并行
- 文档来源很多
- 历史文档极多且分散
- 还有 Notion、Jira、Wiki、接口平台、会议纪要等外部知识源
- 单靠路径搜索和关键词搜索已经不够高效

换句话说：

- 小到中型项目：可以先不用 RAG
- 大型、多知识源、长周期项目：RAG 会越来越重要

---

## 13. 对这个模板的实际建议

如果你现在想先把这套协作模板用稳，我更建议先把下面 5 件事做好，而不是急着上 RAG。

1. 按 `.claude/docs/session-state-rules.md` 规范维护 `production/session-state/active.md`
2. 关键设计和技术决策及时写入正式文件
3. 保持目录结构清晰稳定
4. 保持 handoff prompt 质量高
5. 让 `CLAUDE.md`、Rules、技术偏好文件始终简洁而准确

当这些基础都成熟后，如果你发现：

- 搜索成本越来越高
- 历史知识越来越难召回
- 仓库外知识越来越多

再考虑在这个模板上叠加一层 RAG，会更合适。

---

## 14. 最后的核心结论

这份文档的正确定位是：

- 它属于 `docs/` 下的解释型、学习型文档
- 它帮助你理解系统为什么这样设计
- 它不直接替代运行规则

真正执行层面的规则仍然应该以：

- `.claude/docs/context-management.md`
- `.claude/docs/session-state-rules.md`
- `production/session-state/active.md`

为准。

---

## 15. 最后的核心结论

这份文档最重要的结论有 5 条：

1. 当前对话里的“内存”不可靠，文件化状态才可靠
2. `production/session-state/active.md` 是当前模板最核心的会话恢复桥梁
3. 真正的共享信息主要存在于项目文件、治理文件和状态文件中
4. 要让 AI 持续保持清晰，不是让它记住更多聊天，而是让它依赖更高质量的结构化沉淀
5. 当前模板没有 RAG 不等于落后，而是采用了更轻量、更适合中小型软件项目的文件化协作机制
