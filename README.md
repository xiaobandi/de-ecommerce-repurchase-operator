# 私域复购运营

> 私域运营专员·小复 ｜ 一人店用得起的私域，不碰有赞/微盟的重资产玩法
>
> **私域运营专员·小复**　·　电商商家　·　阶段 P2 后期（首发组合跑通后的第二增长曲线）　·　
> 复杂度 S-M　·　50-150 元/月（SCRM 市场已验证付费意愿，据R3调研）　·　定义卡 6.4-⑧

## 演示（真实执行录播）

![演示](workflows/wf-ecom-repurchase-package-card/docs/images/08-demo-run.gif)

*真实执行回放（GIF 自动循环，非摆拍）· [▶ 观看完整版 MP4](workflows/wf-ecom-repurchase-package-card/docs/images/08-demo-run.mp4) · 台账见 [`reports/real-run/wf-ecom-repurchase-package-card.md`](reports/real-run/wf-ecom-repurchase-package-card.md)*

![架构](docs/images/architecture.svg)

## 效果（真实执行）

![真实效果](workflows/wf-ecom-repurchase-package-card/docs/images/00-effect.png)

*上图为**真实效果对比**，不是示意图：左栏是输入材料与要求，右栏是该次执行的产出原文，
两栏取自同一次真实执行记录。*

- 完整台账：[`reports/real-run/wf-ecom-repurchase-package-card.md`](reports/real-run/wf-ecom-repurchase-package-card.md)
- 录屏与终端回放：[`workflows/wf-ecom-repurchase-package-card/docs/images/`](workflows/wf-ecom-repurchase-package-card/docs/images/)（`08-demo-run.gif` / `08-demo-run.mp4` / `04-terminal-run.png`）
- 使用示例：[`workflows/wf-ecom-repurchase-package-card/docs/使用示例.md`](workflows/wf-ecom-repurchase-package-card/docs/使用示例.md)
> 实测判定：**通过**　·　可用度 4/5　·　编造数字 0 处　·　红线命中 2 次 / 拦截 2 次


**一句话**：有老客盘（累计订单 1000+）的腰部卖家

---

## 这个数字员工解决什么问题

私域运营专员·小复 ｜ 一人店用得起的私域，不碰有赞/微盟的重资产玩法

| 项 | 内容 |
|---|---|
| 服务对象 | 有老客盘（累计订单 1000+）的腰部卖家 |
| 可量化目标 | 复购率环比提升；老客触达打开率 ≥20%；月报按时交付 |
| 投入产出 | 对标有赞/微盟数千到数万报价（据R3调研）；月省 10h≈¥300 |
| 需求依据 | D21、D07 |
| 分发渠道 | GitHub、小红书（私域话题流量池）、千问办公（办公场景触达小微经营者） |

## 30 秒上手

```bash
git clone <本仓库地址> && cd de-ecommerce-repurchase-operator
pip install pyyaml
python3 tests/run.py            # 先跑一遍自检，确认环境与契约都对得上
```

试用（把 `<输入>` 换成本人真实材料）：

```text
请以「私域复购运营」的身份，帮我跑「包裹卡/售后卡文案」。
材料如下：<输入>
要求：只使用材料里出现过的数字，缺的写 <待填>；对外文案停在待确认状态。
```

详细接入见 [`docs/01-安装与接入.md`](docs/01-安装与接入.md)。

## 仓库结构

```text
de-ecommerce-repurchase-operator/
├── employee.md            # 人读定义卡（12 主字段 + 2 扩展组，由 employee.yaml 生成）
├── employee.yaml          # 机器契约（能力清单 + 依赖锁）
├── dependencies.yaml      # 对 core 的外部依赖（含版本）
├── persona/               # 运行提示词 / 边界 / KPI
├── skills/                # 私有原子技能（4 个）
├── spec-bindings/         # 有意不公共化能力的本地绑定声明（0 个）
├── workflows/             # 本员工工作流（5 条）
├── kb/                    # 知识库 RAG 契约
├── connectors/            # 连接器绑定（只读为主，写操作人工确认）
├── docs/                  # 六类发布文档
└── tests/                 # 自动化多场景测试
```

## 私有原子技能（4）

| 技能 | slug | 复杂度 | 阶段 | 成熟度 |
|---|---|---|---|---|
| `活动方案生成` | `campaign-plan-generator` | S | P2 | 契约完备 |
| `营销文案` | `marketing-copy` | S | P1 | 契约完备 |
| `复购周期计算` | `repurchase-cycle-calc` | S | P2 | 契约完备 |
| `复购报表` | `repurchase-report` | S | P2 | 契约完备 |

## 平台公共技能依赖（2）

| 技能 | slug | 全平台复用员工数 |
|---|---|---|
| `平台合规词扫描` | `compliance-word-scan` | 6 |
| `RFM 分层` | `rfm-segmentation` | 2 |

> 这些技能来自 `agent-skills-core@1.0.0`，本仓库**只引用不复制**。

## Spec 绑定层（0）

（无）

> 这一族能力**有意不公共化**（与外部平台接口强绑定）。本仓不存正文，
> 只留本地绑定声明与连接器配置。

## 工作流（5）

| 工作流 | slug | 触发 | 阶段 | 调用技能数 |
|---|---|---|---|---|
| `包裹卡/售后卡文案` | `wf-ecom-repurchase-package-card` | 人工 | P1 | 2 |
| `订单老客分层（RFM）` | `wf-ecom-repurchase-segment` | 定时（每周） | P2 | 1 |
| `复购周期预测与触达文案` | `wf-ecom-repurchase-remind` | 定时（每周） | P2 | 2 |
| `会员日活动方案` | `wf-ecom-repurchase-member-day` | 人工（每月） | P2 | 1 |
| `复购率月报` | `wf-ecom-repurchase-monthly-report` | 定时（每月） | P2 | 1 |

## 知识库与连接器

- 知识库语料：['订单历史库（后台导出 CSV）+会员分层记录+活动复盘库']
- 连接器：LLM 推理 API、平台发布端点（Coze/Dify/WorkBuddy）、企业微信私域

## 质量与验收

| 项 | 值 |
|---|---|
| 技能文件数 | 166 |
| 多场景用例数 | 245 |
| 用例通过 | 245 |
| 验收结论 | ✅ 通过 |

完整报告：[`docs/05-验收与测试.md`](docs/05-验收与测试.md)

## 许可

专有许可，详见 [`LICENSE`](LICENSE)。
