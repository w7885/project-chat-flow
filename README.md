# project-chat-flow

一个用于在同一项目的不同聊天窗口之间交接工作的 Codex Skill。它把已经完成的工作、当前状态、关键决定、卡点和下一步集中记录在项目里的一个 `project-status.md` 中，帮助新聊天快速核对并接续任务。

## 适用场景

- 在不同聊天窗口或不同模型之间切换，需要传递项目进度和下一步。
- 长会话告一段落，需要整理已完成事项、决定和遗留问题。
- 希望多个聊天围绕同一份项目状态文档协作。

## 使用方式

本 Skill 仅在显式调用时运行。在 Codex 中使用 `$project-chat-flow`，并说明要执行的动作，例如：

```text
$project-chat-flow 总结本次完成情况，更新项目状态，并记录下一步计划。
```

在新的聊天窗口中读取状态并继续：

```text
$project-chat-flow 读取项目状态，核对相关文件和当前 Git 状态，然后继续下一步。
```

也可以明确要求它只调整 `project-status.md` 中的下一步计划。

## 状态文件规则

- 文件名必须是 `project-status.md`，可以位于项目文件夹内任意位置，不要求放在 `docs/`。
- 首次显式调用时，如果项目内没有该文件，Skill 会依据可核实的项目资料创建项目根目录下的初始状态文件。
- 如果项目中已有一份，就继续使用它；如果找到多份，会先列出路径并请用户指定权威版本。
- 状态内容按证据记录项目目标、已完成工作、当前状态、已确认决定、卡点和下一步；不确定的信息会标为待确认。

## 安装

将仓库中的 `project-chat-flow/` 文件夹复制到以下任一位置：

**仅当前项目可用：**

```text
<项目根目录>/.agents/skills/project-chat-flow/
```

**个人所有项目可用：**

```text
$HOME/.agents/skills/project-chat-flow/
```

在 Windows 上，个人级目录通常展开为：

```text
C:\Users\<用户名>\.agents\skills\project-chat-flow\
```

安装后，若 Skill 尚未出现在可用列表中，重新启动 Codex 再试。

## 边界

- 只创建或更新项目中的 `project-status.md`，不创建、移动或修改 `AGENTS.md`、`current-plan.md`、代码或其他文件。
- 不会自动创建或切换聊天，也不会选择模型或推理强度；这些由用户在应用中安排。
- 读取交接状态时会核对相关文件和 Git 状态；发现记录过期或与当前工作区冲突时会指出，不会默默当作最新事实。
- 不会仅因读取或恢复任务而改写状态文件；只有用户要求记录进展或调整计划时才更新。

## 文件结构

```text
project-chat-flow/
├── SKILL.md
├── README.md
└── agents/
    └── openai.yaml
```

更多关于 Skill 结构和 Codex 安装位置的信息，请参阅[官方文档：Build skills](https://learn.chatgpt.com/docs/build-skills)。

