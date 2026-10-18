# aut-level-checker

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Domain Profile](https://img.shields.io/badge/domain-aut%20%7C%20domain%2F1.0-blue.svg)](domain.manifest.json)
[![Status: RESERVED](https://img.shields.io/badge/status-RESERVED%20%7C%20open%20claim-orange.svg)](domain.manifest.json)
[![Standard: GB/T 40429](https://img.shields.io/badge/standard-GB%2FT%2040429-2ea44f.svg)](domain.manifest.json)
[![Privacy: default-off](https://img.shields.io/badge/privacy-default--off-brightgreen.svg)](privacy.json)

> **状态：RESERVED（占位 · 开放认领）** — 本仓已按平台协议规范建好行业接入四件套骨架，
> 等待具备本域资质的运营方认领并填充真实规则。

把「这套系统到底算几级、责任在谁」拆成可核验的属性，让 AI 只做分级提示，不做等级认定。

**关键词 / Keywords**：驾驶自动化分级 · L0-L5 · ODD · 接管策略 · 责任边界 · 自动驾驶宣传口径 · GB/T 40429 · domain profile · compliance checker · regulatory verification · autonomous driving · ADAS · attribute verification · MCP tool · gate map · privacy by default-off

---

## 这个域管什么 / What this domain covers

驾驶自动化等级 0-5 判定与责任边界

驾驶自动化等级直接决定责任归属与宣传口径。现实中普遍存在把 L2 说成「自动驾驶」的误导。AI 能做的是按标准把等级要素与设计运行条件摆清楚，认定权在标准与监管。

This domain is the **driving · automation level (L0–L5)** profile of a multi-domain verification platform. It turns "which level is this system, and where does responsibility fall" into verifiable attributes; the AI only surfaces the level elements — grade determination stays with the standard and the regulator.

---

## 快速开始 / Quick Start

> 本仓是**域声明骨架**（行业接入四件套 JSON），不是可运行的应用程序。克隆后用标准 JSON 工具即可校验结构。

```bash
# 1. 克隆本仓 / clone this repo
git clone https://github.com/SynomosAI/aut-level-checker.git
cd aut-level-checker

# 2. 校验四件套结构是否为合法 JSON（任意一个报错即骨架不完整）
python -m json.tool domain.manifest.json > /dev/null
python -m json.tool gate-map.json        > /dev/null
python -m json.tool privacy.json         > /dev/null
python -m json.tool checker.io.json     > /dev/null

# 3. 查看本域名称与红线 / print domain name and red lines
python -c "import json;m=json.load(open('domain.manifest.json',encoding='utf-8'));print(m['domain_name']);[print('-',r) for r in m['red_lines']]"
```

核验器（MCP 工具）的输入输出契约见 `checker.io.json`。一个最小调用示例：

```json
{
  "subject_ref": "mpp:cn:2a7b9c4d",
  "attribute_keys": ["aut.automation_level", "aut.responsibility_boundary"],
  "evidence": ["tsa:2026-09-12#sha256:9f2a..."],
  "request_id": "req-20260912-0006"
}
```

> 核验器只有「出示与核验」一个动作，**没有「判定」选项**——`自动驾驶`、`无人驾驶`、`责任在系统`、`L4 已合规`等面向客户的结论性输出均在 `checker.io.json` 的 `_forbidden_outputs` 黑名单中。

---

## 域标识 / Domain identity

| 项 | 值 |
|---|---|
| 域 ID | `aut`（全局唯一，一经分配不复用） |
| 域名称 | 驾驶 · 自动化等级 |
| Profile 版本 | `domain/1.0` |
| 当前状态 | `RESERVED` |
| 占位时间 | 2026-09-12 |

## 属性清单 / Attributes

| 属性键 | 类型 | 说明 |
|---|---|---|
| `aut.automation_level` | enum | 按 GB/T 40429 驾驶自动化分级 · 取值 L0/L1/L2/L3/L4/L5 |
| `aut.odd_scope` | string | 设计运行条件（ODD）：道路类型、速度区间、天气、地理围栏 |
| `aut.takeover_capability` | enum | 接管与最小风险策略能力 · 取值 not_applicable/driver_supervised/fallback_ready/no_driver_needed |
| `aut.responsibility_boundary` | enum | 责任边界，须人工核验后填注 · 取值 driver/system/shared/undetermined |

## 本域红线（不可逾越，机器可读）/ Red lines

1. 不得输出高于实测或未经确认的自动化等级
2. 不得将 L0-L2 描述为「自动驾驶」「无人驾驶」等易致误认的表述
3. 责任边界判定须人工复核，AI 不得单独给出责任归属结论

> 红线在 `gate-map.json` 中均有对应阻断规则。平台校验器会检查「每条红线都有规则覆盖」，
> 缺失即校验失败——**制度与系统不允许不同步**。

## 行业接入四件套 / The four-piece access set

| 文件 | 作用 |
|---|---|
| `domain.manifest.json` | 本域声明：属性清单、签发方要求、有效期、红线 |
| `gate-map.json` | 本域「什么动作要多少摩擦」：silent / warn / confirm / block / require-owner |
| `privacy.json` | 本域隐私声明：默认关闭、最小必要、可撤回、可删除 |
| `checker.io.json` | 本域核验器（MCP 工具，**只出示核验，不下判定**） |

## 仓库结构 / Repository layout

```text
aut-level-checker/
├── README.md              # 本文件（中文为主，英文对照）
├── README.en.md           # English README
├── LICENSE                # MIT License（© 2026 SynomosAI）
├── CODEOWNERS             # @SynomosAI/synomosai-gov
├── domain.manifest.json  # 本域声明：属性清单 / 签发方 / 有效期 / 红线
├── gate-map.json          # 本域摩擦分级：silent/warn/confirm/block/require-owner
├── privacy.json           # 本域隐私声明：默认关闭 / 最小必要 / 可撤回 / 可删除
├── checker.io.json         # 本域核验器（MCP 工具）IO 契约
├── llms.txt               # 给 AI 的最小索引（llms.txt 标准）
├── llms-full.txt          # 给 AI 的完整文本
├── AGENTS.md              # 给 AI Agent 的工作指引
└── SECURITY.md            # 安全策略与报告途径
```

## 核心原则 / Core principles

**平台只当擂台，不当货架。** 本域的核验器只回答「这条声明是否可核验、缺什么要件」，
不回答「这件事是否合规、该不该做」。判定权在本域的资质方、监管方与人。

**隐私是准入条件，不是整改事项。** 缺失 `privacy.json` 或任一必填字段不符，
符合性校验直接失败——不是警告，是拒绝接入。

## 参考依据 / References

- GB/T 40429 汽车驾驶自动化分级
- 智能网联汽车道路测试与示范应用管理规范

> 上列依据仅用于说明本域属性的来源与口径，不构成法律意见。具体适用以现行有效文本与主管部门解释为准。

## 认领方式 / How to claim

本域面向具备相应资质的机构开放。认领后请：

1. Fork 本仓，填注 `operator` 与 `checker_endpoint`
2. 按本域现行有效规则校准属性取值与红线表述
3. 跑平台侧校验器自测（五项判据全过方可提交）
4. 提 PR，附资质证明与规则依据

---

## 许可与署名

代码与配置按 MIT 许可使用。文档的知识版权归 SynomosAI 所有。

© 2026 SynomosAI. All rights reserved.
