# crossborder-ai-learning

跨境电商 AI Agent 全链路项目 —— **学习计划 + 参考教程 + 实现代码**，放在同一个仓库里。

这个仓库装三样东西，各自解决一个问题：

| 装什么 | 解决什么 | 在哪 |
|---|---|---|
| **计划** | 做什么、做到什么算过、按什么顺序 | `docs/01`–`docs/04` |
| **教程** | 框架怎么用（LangChain / LangGraph / MCP / RAG） | `reference/ai-agents-from-zero/` |
| **代码** | 边学边落地的实现 | `cli/` `app/` `learning/` |

**融合的实质在 [`docs/00-学习主线.md`](docs/00-学习主线.md)**：把教程 35 节按 8 个里程碑重排，标明每节读到什么程度、什么时候读、读完产出什么。这篇是唯一入口，其余都是它的材料。

---

## 当前状态

**M0 地基**，卡在 `cli/chat.py` 四个函数壳。

| 项 | 值 |
|---|---|
| 里程碑 | M0（M0–M7 见 [`docs/02`](docs/02-里程碑与推进节奏.md)） |
| 当前任务 | CLI 聊天机器人 v0.1 |
| 未开始 | 数据层三实现、20 条评测集骨架、后台历史数据导出 |

真实状态以 [`docs/progress/state.md`](docs/progress/state.md) 为准，本段只是快照。

---

## 项目是什么

一个自用的跨境电商业工具，覆盖四个环节：

```
运营诊断 ──差评/搜索词──> 选品决策 ──> 内容生产（图文 + 视频）──> 上架
    ↑                                                          │
    └──────────────── 销售 / 广告数据回流 ─────────────────────┘
```

**架构上只有 1 个真 Agent，其余是确定性工作流。** 这是刻意的：

| 模块 | 形态 | 为什么 |
|---|---|---|
| 选品调研 | **Agent**（LangGraph） | 工具集不确定、调用顺序依赖中间结果 |
| 图文生成 | 工作流（Prompt Chaining） | 三段有固定依赖顺序 |
| 视频生成 | 工作流（流水线） | 分镜→片段→配音→字幕→合成，顺序固定 |
| 运营诊断 | 工作流（规则 + RAG） | 指标计算用代码，LLM 只组织语言 |

工作流可测、可回放、成本可预测；Agent 只用在真正需要决策的地方。

---

## 目录

```
.
├── docs/
│   ├── 00-学习主线.md              ★ 教程 × 里程碑 的融合索引（先看这篇）
│   ├── 01-学习路线与技能清单.md     技能表、够用线、砍掉清单
│   ├── 02-里程碑与推进节奏.md       M0–M7 验收标准（不绑日期）
│   ├── 03-模块设计-选品到内容.md    数据流、素材三档、词库分层
│   ├── 04-教程对照与补入清单.md     35 节逐节判定与补入项
│   └── progress/
│       ├── state.md                当前状态（唯一状态源）
│       ├── speed-log.md            估算 vs 实际，用于预测
│       ├── usage-log.md            真实使用记录
│       └── reference-tracking.md   上游版本基线与精读状态
│
├── reference/
│   └── ai-agents-from-zero/        教程全量原文（含 1664 张配图，MIT）
│
├── cli/                            CLI 聊天机器人（当前任务卡）
├── app/                            FastAPI 骨架
├── learning/                       Python / LLM 练习代码
├── prompts/                        Prompt 模板
├── docker/                         Dockerfile
└── requirements.txt
```

---

## 怎么开始

```bash
# 1. 环境
python -m venv .venv && source .venv/Scripts/activate   # Windows Git Bash
pip install -r requirements.txt
cp .env.example .env        # 填入 DEEPSEEK_API_KEY

# 2. 当前任务
python cli/chat.py

# 3. 开工前查两处
#    docs/progress/state.md     我现在在哪
#    docs/00-学习主线.md §3      这个里程碑该读哪几节
```

---

## 三条不能破的线

写在文档里，也写在代码断言里：

1. **素材真实性**：AI 生成的图不能当主图（亚马逊要求主图为实际商品照片）。产品像素只能来自供应商图或自拍图，竞品图只进提示词、不进像素。
2. **数据合规**：只用授权数据源（自有店铺后台导出 + 卖家精灵 MCP），不做爬虫；评论入库前去标识。
3. **成本可控**：单次任务、单轮评测、月度三层预算上限，超限拒绝执行而不是继续烧。

---

## 参考教程

[`didilili/ai-agents-from-zero`](https://github.com/didilili/ai-agents-from-zero) —— 33 章正文 + 2 个实战项目 + 5 套可跑源码。

- 上游基线：commit `fe39818`（2026-11-06）
- 许可：MIT，原文与 `LICENSE` 一并保留在 `reference/ai-agents-from-zero/`
- 用法：**不通读**。按 `docs/00-学习主线.md` 的时点读对应小节，12 段精读、其余查阅或跳过

教程讲框架怎么用，本项目讲项目怎么设计。两者不重叠，所以不存在「跟哪个学」的问题。

---

## 这个仓库不放什么

- **真实经营数据**（后台导出、广告报表、评论原文）—— 公开仓库，一律不入库，放本地 `data/snapshots/`
- **API Key** —— `.env` 已进 `.gitignore`
- **求职材料** —— 与项目无关，另一个仓库
