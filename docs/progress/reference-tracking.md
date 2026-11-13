# 参考教程追踪

> 对象：`didilili/ai-agents-from-zero` —— 33 章正文 + 2 个实战项目 + 5 套可跑源码
> 用途：这是**参考书**，不是主线。只按 `docs/00-学习主线.md` 的时点取用。
> 主线永远是 `docs/02-里程碑与推进节奏.md` 的 M0–M7。

---

## 一、基线

| 项 | 值 |
|---|---|
| 仓库 | https://github.com/didilili/ai-agents-from-zero |
| **仓库内路径** | `reference/ai-agents-from-zero/`（全量快照，含图片） |
| 上游克隆（比对用） | `E:\ai-project\ai-agents-from-zero` |
| 当前 commit | `fe3981812b959f4b0d17b04accff140ba39ed261` |
| 上游最后提交 | 2026-11-06（作者：迪迪粒粒） |
| 上次核对 | 2026-11-13 |
| 仓库内体积 | 612 MB / 2357 个文件（含 1664 张 png、106 jpg、13 mp4） |
| 许可 | MIT，`LICENSE` 与署名随原文保留 |

### 为什么有两个副本

| 副本 | 作用 | 能不能改 |
|---|---|---|
| `reference/ai-agents-from-zero/` | 仓库内的**快照**，换设备克隆即可读 | 不改内容，只接收同步 |
| `E:\ai-project\ai-agents-from-zero` | 带 `.git` 的**上游克隆**，用于查更新 | 随意，只用于比对 |

仓库内的副本已经去掉 `.git`（否则会变成嵌套仓库），所以**不能在里面 `git pull`**。查更新在上游克隆里做。

---

## 二、更新与核对

```bash
# 1. 在上游克隆里拉最新
git -C "E:/ai-project/ai-agents-from-zero" pull --ff-only

# 2. 看这次动了哪些章节
git -C "E:/ai-project/ai-agents-from-zero" log --oneline -5 --name-only

# 3. 有实质改动才同步到仓库（同步后 commit）
```

同步命令（只在确认需要更新时执行）：

```bash
SRC="E:/ai-project/ai-agents-from-zero"
DST="E:/ai-project/crossborder-ai-learning/reference/ai-agents-from-zero"
tar -cf - -C "$SRC" --exclude=.git . | tar -xf - -C "$DST"
cd "E:/ai-project/crossborder-ai-learning" && git add reference && git commit -m "同步上游教程 <新commit>"
```

**核对规则**：

1. `pull` 后，如果改动落在下面「精读清单」里还没读的章节 → 读最新版
2. 已读过的章节被改写 → 只重读改动那段（`git show <sha> -- <文件>`）
3. 出现完全新主题 → 先判断"对应我哪个模块的哪一步"，答不上来就不读
4. **不因为教程更新改计划**。计划由项目依赖决定

每月一次，更新下表状态，并在 §四 记一行。

---

## 三、精读清单与状态

只读这些，其余不点开。完整判定见 `docs/00-学习主线.md` §2。

| # | 章节 | 取什么 | 用在 | 状态 |
|---|---|---|---|---|
| 1 | 1-3 §1 | 问题诊断四问 | 全局判断 | ☐ |
| 2 | 1-1 §1.3 / §3.1 | token 计量、训练 vs 推理 | 成本核算 | ☐ |
| 3 | 3 §1.6 | 三级形态分类 | README 边界表 | ☐ |
| 4 | 20 §3 / §5 | MCP 三角色、`tools/list` / `tools/call` | 数据层 | ☐ |
| 5 | 23 §2–3 | State / Reducer 选型 | M1 | ☐ |
| 6 | 24 §3 | Send / Command / Runtime | M1 | ☐ |
| 7 | 25 §1、§2.7–2.8 | stream_mode 五种、Store vs Checkpointer | M1 + SSE | ☐ |
| 8 | 25 §3–5 | interrupt / time-travel / 子图 | M1 | ☐ |
| 9 | 21 §4.4–4.8 | checkpointer / stream / middleware / 人类审核 | M1 | ☐ |
| 10 | 14 §8.2 + §9 | 输出方案选型表 + 五个误区 | M1 | ☐ |
| 11 | 18 §2.4–2.5 / §6.4 | 稠密/稀疏/标量、混合检索、rerank | M2 | ☐ |
| 12 | 19 §2.3 | 切分器与参数 | M2 | ☐ |
| 13 | 电商问数 §10–14 | 多路召回 → 合并 → 过滤 → 生成 → 校验 | M5 | ☐ |
| 14 | 8 §4 | volume / 数据边界 / 危险操作 | M6 | ☐ |
| 15 | 26 §3.3 | 何时才需要多智能体 | 随时 | ☐ |

状态用：☐ 未读 / ◐ 在读 / ☑ 已应用（不只是读完，要在代码里用过）

---

## 四、变更记录

| 日期 | 上游 commit | 变动 | 是否影响计划 |
|---|---|---|---|
| 2026-11-13 | `fe39818` | 建立追踪基线；完成全量章节对照，输出 10 项补入 + 6 天增量 | 是，M0/M1/M2/M6 估算上调 |
| 2026-11-13 | `fe39818` | 全量入库到 `reference/`（含图片，612MB），融合索引落 `docs/00-学习主线.md` | 否，只是取用方式变了 |

---

## 五、与教程相反的三处（别被带回去）

| 教程做法 | 我的做法 | 原因 |
|---|---|---|
| MCP 交给 Agent 自己选工具 | 写死工具名，直接 `tools/call` | 数据获取是确定性工作流，不需要模型决策 |
| 多智能体 + 子智能体做研搜 | 单 Agent + 三个工作流 | 难测、成本不可预测、错误归因变难 |
| 先搭平台再写代码 | 先写代码，Dify 只留 2 个工作流对照 | 平台会掩盖工程细节 |
