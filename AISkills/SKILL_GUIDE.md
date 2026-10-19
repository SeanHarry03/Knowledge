# 本项目的 Agent Skills 选择指南

> 适用范围：当前 `evil-spirit` 项目。先看任务，再选 skill；不要一次启用整套技能。

## 1. 项目画像与选择依据



- `package.json` 指明项目使用 **Cocos Creator 3.5.2**、TypeScript 和 `protobufjs`。
- `assets/Game/Scripts/` 有战斗、敌人、玩家、塔、寻路等逻辑；`assets/LobbyBundle/Scripts/` 是大厅功能与界面。
- `assets/CommonFrameWork/Manager/NetMgr.ts` 管理 WebSocket 通信，`assets/Net/` 含协议文件。
- `build-configs/wechat.json` 与 `build-configs/web-mobile.json` 分别对应微信小游戏、移动网页构建；`extensions/` 还有编辑器和构建扩展。
- 当前 `package.json` 没有测试命令，项目内未发现常见单元测试框架配置。因此，使用测试类 skill 时，先明确可测试的纯逻辑及实际验证方式，不要默认已有完整测试基础设施。
- 构建配置中存在敏感的上传配置。处理构建、发布和仓库安全时，优先使用 `security-and-hardening` 核查；本指南不复制配置值。

以上是选 skill 的依据，不是对代码质量的全面审计。

## 2. 最简单的选择法

1. **先说任务**：是修 bug、加功能、改 UI、查卡顿、改网络协议，还是构建发布？
2. **选一个主 skill**：从下表找到最贴近当前任务的一个。
3. **只在确有需要时加专项 skill**：例如战斗卡顿加 Cocos 性能 skill；涉及网络输入或构建凭据加安全 skill。
4. **让验证跟任务匹配**：纯算法可做单元测试；场景、Prefab、动画和平台适配还要在 Cocos 预览或目标平台验证。没有可用的测试环境时，应如实记录未验证部分。

`using-agent-skills` 是选技能的入口，不代表每次都要执行它列出的完整生命周期。小修复通常只需 2～3 个相关 skill。

## 3. 你最常用的 8 个

| Skill                          | 何时使用                             | 在本项目中的典型场景                        |
| ------------------------------ | -------------------------------- | --------------------------------- |
| `debugging-and-error-recovery` | 行为异常、构建失败、线上问题                   | 敌人不移动、塔攻击错误、场景加载失败、小游戏报错          |
| `incremental-implementation`   | 功能修改跨多个文件或系统                     | 新道具、战斗机制、跨大厅与战斗的功能                |
| `test-driven-development`      | 改算法、规则或修可复现的逻辑 bug               | 寻路、伤害计算、冷却、资源 ID 转换；先找可隔离的逻辑再落测试  |
| `code-review-and-quality`      | 提交或合并前检查                         | 检查行为回归、生命周期、网络回调、资源释放             |
| `source-driven-development`    | 使用 Cocos 3.5 API、微信平台 API 或插件接口时 | 核对 3.5.2 对应文档，避免套用其他版本写法          |
| `git-workflow-and-versioning`  | 改代码、整理改动、提交或处理冲突                 | 保留已有未提交改动，按功能组织提交；提交信息按项目规则写中文    |
| `performance-optimization`     | 已有卡顿、掉帧、内存增长证据时                  | 战斗帧率、敌人批量生成、资源加载、长时间运行内存          |
| `security-and-hardening`       | 网络、外部输入、用户数据、构建上传配置              | 审查 WebSocket/HTTP 数据处理和构建配置中的敏感信息 |

**本项目专用补充：**`cocos-creator-3-5-performance-optimization`（本机目录名为 `Cocos`）适合已定位的 Cocos 运行时热点，如 `update`、批量敌人、子弹创建及对象复用。先测量，再应用建议；不要仅凭 skill 的通用清单把所有调用都内联或加入对象池。性能提升百分比必须来自实测，不能凭估计当作结果。

## 4. 按任务直接选

| 你现在要做什么             | 主 skill → 需要时追加                                                                    | 验证重点                               |
| ------------------- | ---------------------------------------------------------------------------------- | ---------------------------------- |
| 修战斗/大厅 bug          | `debugging-and-error-recovery` → `test-driven-development`                         | 复现条件、修复后的相同场景、相关回归                 |
| 加一个跨脚本的新功能          | `incremental-implementation` → `test-driven-development`、`code-review-and-quality` | 一小步一小步接入，最后进场景实测                   |
| 新功能需求还说不清           | `interview-me` 或 `idea-refine` → `spec-driven-development`                         | 明确玩家行为、边界和验收条件后再写代码                |
| 已有明确需求，改动范围大        | `planning-and-task-breakdown` → `incremental-implementation`                       | 每个任务有可观察的完成条件                      |
| 改大厅/战斗 UI           | `frontend-ui-engineering` → `browser-testing-with-devtools`（网页预览且工具可用时）            | 分辨率、触控、层级、字体和真实预览效果                |
| 修改 Cocos API 或编辑器扩展 | `source-driven-development` → `debugging-and-error-recovery`                       | 以 Cocos Creator 3.5.2 文档和实际编辑器行为为准 |
| 改协议、请求或模块边界         | `api-and-interface-design` → `security-and-hardening`                              | 消息格式、兼容性、超时/重连、异常输入                |
| 查卡顿、内存增长            | `performance-optimization` → `cocos-creator-3-5-performance-optimization`          | 记录优化前后同场景帧率、耗时或内存数据                |
| 改微信小游戏或网页构建         | `ci-cd-and-automation` 或 `shipping-and-launch` → `security-and-hardening`          | 构建产物、目标平台、上传配置和回退办法                |
| 提交前自查               | `code-review-and-quality` → `code-simplification`（确实难读时）                           | 只审本次改动及直接关联代码                      |

`browser-testing-with-devtools` 依赖已配置的 Chrome DevTools MCP。它能检查网页版本的控制台、网络和运行时表现，但不能替代微信开发者工具中的小游戏验证。

## 5. 其余 skill：有触发条件再用

| Skill                               | 触发条件 / 当前建议                           |
| ----------------------------------- | ------------------------------------- |
| `constraint-driven-development`     | 想正式规定帧率、包体、测试、代码规模等质量门槛时使用；不必为单次修复启用。 |
| `context-engineering`               | 任务横跨多个资源包或 AI 经常弄错项目边界时，用来整理项目上下文。    |
| `documentation-and-adrs`            | 做了重要架构选择、协议变更或需要交接时记录原因。              |
| `doubt-driven-development`          | 高风险或陌生代码中的关键决策需要额外审查时使用；普通小改动不必启用。    |
| `observability-and-instrumentation` | 线上问题缺少日志和可定位信息时，先设计恰当的诊断信息。           |
| `deprecation-and-migration`         | 要淘汰旧协议、资源格式、存档字段或构建流程时使用。             |
| `shipping-and-launch`               | 真正要发微信小游戏或网页版本时使用。                    |
| `ci-cd-and-automation`              | 要建立或修改自动构建、自动校验、发布流水线时使用。             |
| `code-simplification`               | 功能已正确但实现太绕时使用，目标是保持行为不变。              |
| `spec-driven-development`           | 新功能较大且缺少清晰需求时使用；已有明确的小 bug 不必先写规格。    |
| `idea-refine`                       | 玩法想法还在比较方案时使用。                        |
| `interview-me`                      | 连目标玩家、问题或预期行为都不清楚时使用。                 |
| `find-skills`                       | 现有技能确实覆盖不了新任务，想找额外 skill 时才用。         |
| `using-agent-skills`                | 不确定该选哪个时使用；本指南就是它在当前项目的落地版。           |

## 6. 可直接复制的提问方式

明确写出任务、指定 skill、限定文件或场景，以及如何验证，比一次列出十几个 skill 更有效。

```text
使用 debugging-and-error-recovery，排查敌人进入房间后停止移动的问题。
先复现并定位原因；只修改相关逻辑，说明如何在 Cocos Creator 3.5.2 中验证。
```

```text
使用 incremental-implementation 和 test-driven-development，给 AStar 寻路增加障碍变化后的重新寻路。
先识别可独立测试的规则，再分步改动，并在实际场景中验证。
```

```text
使用 performance-optimization 和 cocos-creator-3-5-performance-optimization，分析战斗中大量敌人出现时的掉帧。
先给出可重复的测量方法和基线，再根据热点改代码，报告前后数据。
```

```text
使用 security-and-hardening，审查构建上传配置和网络入口中的敏感信息处理。
先列出可确认的问题与影响，不在回复中输出凭据原文。
```

在 Codex 中可以直接写 skill 名称或像本次一样引用对应 `SKILL.md`。GitHub 仓库 README 中的 `/spec`、`/build` 等是该仓库提供的命令方案；是否可用取决于实际安装方式，不必依赖这些命令才能使用 skill。

## 7. 本地安装注意事项

本机 `C:\Users\25330\.agents\skills\` 当前有 **27 个目录**：仓库的 25 个工程流程 skill，加上 `find-skills` 和 `Cocos`。这不表示每次任务都要加载 27 个。

当前安装目录未找到 `C:\Users\25330\.agents\references\`。部分 skill 的 `SKILL.md` 引用了 `../../references/` 下的共享检查表，因此这些补充链接在本机副本里可能失效。若要完整使用那些检查表，应从下载的原仓库保留 `references/`，并按原相对目录结构放置；不要把失效链接误当成已完成的检查。

## 资料

- [Agent Skills 仓库与使用说明](https://github.com/addyosmani/agent-skills)
- 本机技能入口：`C:\Users\25330\.agents\skills\using-agent-skills\SKILL.md`
- 本项目依据：`package.json`、`assets/Game/Scripts/`、`assets/CommonFrameWork/Manager/NetMgr.ts`、`build-configs/`

## 8.使用skill实例

### context 上下文问题

```
@context-engineering

当前会话已经很长。

整理当前上下文：

- 保留最终确定的方案
- 保留当前任务目标
- 保留仍然有效的约束
- 保留当前修改文件
- 保留当前错误
- 总结已经排除的方案
- 删除/忽略已经失效的尝试和旧错误
- 给出适合开启新 session 的 handoff summary
```

它属于​**基础设施型 Skill**​，不是那种“执行一个具体编码任务”的 Skill。

对你这种经常让 Codex 直接分析 Cocos 项目、改引擎源码、做性能优化的使用方式，我认为它比较值得装。尤其是 `AGENTS.md + context-engineering + performance-optimization + debugging-and-error-recovery` 这一套组合会比较实用。仓库自己的分类里也把 `context-engineering` 放在 Build 阶段，用于“Right context at the right time”。

如果只是记一个最简单的使用方式，就是：

```
@context-engineering

先为这个任务建立正确的项目上下文，
只读取与当前任务有关的代码，
找到项目中已有的类似实现后，
再开始修改。
```

## Codex修改内容重新Review指令

假设你刚才写的代码存在严重缺陷。
不要解释为什么它是正确的，
而是想办法证明它是错的。
主动设计极端输入、异常操作和边界测试，
尝试让程序崩溃或产生错误结果。
记录实际发现的问题，再给出修复方案