# 项目生态：从安装到组合使用

> 四个项目相辅相成，覆盖 AI 编程的完整链路。本文教你从零安装每个项目，并演示它们如何组合使用。

```
注入方法论 → 加载专家角色 → 多角色编排 → 安全防护
(superpowers)  (agents)       (orchestrator) (shellward)
```

---

## 1. superpowers-zh — 给 AI 注入工作方法论

**解决什么问题**：AI 编程工具默认"上来就写代码"，不会先想清楚再动手。装了 superpowers-zh 后，AI 会先分析需求、拆解任务、确认方案，再开始实现。

### 安装

```bash
cd /your/project
npx superpowers-zh
```

自动检测你项目中使用的工具（Claude Code / Cursor / Copilot / Windsurf 等 14 种），将 20 个 skills 安装到正确位置。

### 效果对比

**没装 superpowers-zh：**
```
你：给用户模块加个批量导出功能
AI：好的，我来实现...（直接开始写代码）
    export async function exportUsers() { ... }
你：等等，格式不对，没分页，大数据量会 OOM...
```

**装了 superpowers-zh：**
```
你：给用户模块加个批量导出功能
AI：在开始实现之前，我需要了解几个关键问题：
    1. 导出格式是 CSV 还是 Excel？
    2. 预计数据量多大？需要异步处理吗？
    3. 有权限要求吗？
    → 给出 2-3 个方案，确认后再动手
```

### 常用 Skills

| Skill | 用途 | 触发方式 |
|-------|------|---------|
| brainstorming | 需求分析、方案讨论 | 描述一个新功能需求 |
| debugging | 系统化调试 | 报一个 bug |
| tdd | 测试驱动开发 | 要求写测试 |
| code-review | 代码审查 | 要求审查代码 |
| planning | 任务规划 | 描述一个大任务 |

---

## 2. agency-agents-zh — 187 个 AI 专家角色

**解决什么问题**：AI 默认是"通用助手"，什么都能聊但不够专业。装了专家角色后，AI 会按特定领域的专业流程和标准来工作。

### 安装

```bash
git clone https://github.com/jnMetaCode/agency-agents-zh.git
cd agency-agents-zh

# 自动检测已安装的工具，一键安装
./scripts/install.sh

# 或指定安装到特定工具
./scripts/install.sh --tool claude-code
./scripts/install.sh --tool cursor
./scripts/install.sh --tool copilot
```

### 使用示例

**安全审查：**
```
你：用安全专家审查 src/api/auth.py
AI：（激活 security-engineer 角色）
    按 OWASP Top 10 逐项检查：
    🔴 严重：SQL 注入风险 — 第 42 行直接拼接了用户输入
    🟡 中等：session token 明文存储在 cookie 中
    🟢 建议：添加 rate limiting 防暴力破解
```

**数据库优化：**
```
你：用 DBA 角色优化这个慢查询
AI：（激活 database-optimizer 角色）
    1. 当前执行计划：全表扫描，耗时 3.2s
    2. 问题：缺少 user_id + created_at 联合索引
    3. 建议：ALTER TABLE orders ADD INDEX idx_user_created (user_id, created_at)
    4. 预估优化后：0.05s
```

### 角色分类（18 个部门，187 个角色）

| 部门 | 角色数 | 示例 |
|------|:---:|------|
| 工程 | 27 | 后端架构师、前端开发、安全工程师、DevOps |
| 设计 | 8 | UI 设计师、UX 研究员、无障碍专家 |
| 营销 | 29 | 小红书运营、抖音策略师、SEO 专家 |
| 产品 | 5 | 产品经理、Sprint 排序师 |
| 测试 | 8 | API 测试员、性能基准师 |
| 更多 | 110 | 金融、法务、供应链、游戏开发等 |

---

## 3. agency-orchestrator — 多角色 YAML 编排

**解决什么问题**：单个角色很强，但复杂任务需要多个角色协作。orchestrator 用 YAML 定义工作流，自动调度角色协作，支持并行执行。

### 安装

```bash
npm install agency-orchestrator
npx ao init  # 下载 187 个角色定义
```

### 快速体验（无需 API Key）

```bash
npx agency-orchestrator demo
```

### 编写工作流

```yaml
# workflows/product-review.yaml
name: "产品需求评审"
steps:
  - id: analyze
    role: "product/product-manager"
    task: "分析这份 PRD，提取核心需求和风险点：{{prd_content}}"
    output: requirements

  - id: tech_review
    role: "engineering/engineering-software-architect"
    task: "评估技术可行性，给出架构建议：{{requirements}}"
    depends_on: [analyze]
    output: tech_assessment

  - id: security_review
    role: "engineering/engineering-security-engineer"
    task: "审查安全风险：{{requirements}}"
    depends_on: [analyze]
    output: security_assessment

  - id: summary
    role: "product/product-manager"
    task: "综合技术评估和安全评估，输出最终评审结论：\n技术：{{tech_assessment}}\n安全：{{security_assessment}}"
    depends_on: [tech_review, security_review]
```

### 执行

```bash
# 设置 API Key（选一个）
export DEEPSEEK_API_KEY=your-key    # DeepSeek（便宜）
export ANTHROPIC_API_KEY=your-key   # Claude
export OPENAI_API_KEY=your-key      # GPT

# 运行工作流
npx ao run workflows/product-review.yaml --input prd_content='你的 PRD 内容'
```

执行过程：
```
产品经理 ──→ 架构师（技术评估）──→ 产品经理（汇总）
           └→ 安全专家（安全评估）─┘
            （自动并行执行）
```

结果输出到 `ao-output/product-review-<timestamp>/`。

### 迭代某一步

不满意某一步的输出？不用全部重跑：

```bash
npx ao run workflows/product-review.yaml --resume last --from tech_review
```

---

## 4. shellward — AI Agent 安全防护

**解决什么问题**：AI Agent 有执行命令的能力，但可能被提示词注入攻击，或者意外执行危险操作（`rm -rf /`、泄露 `.env` 等）。shellward 在 Agent 和系统之间加了一层安全中间件。

### 安装

```bash
npm install shellward
```

### 核心能力

| 防护 | 说明 |
|------|------|
| 命令安全 | 拦截危险命令（`rm -rf`、`chmod 777`、`curl \| bash` 等） |
| 注入检测 | 检测提示词注入攻击 |
| 数据防泄露 | 防止 `.env`、密钥、凭证等敏感数据外泄 |
| 零依赖 | 不依赖外部服务，本地运行 |
| MCP Server | 可作为 MCP Server 集成到 Claude Code 等工具 |

---

## 组合实战：从零跑通一个完整流程

假设你要给项目加一个"用户批量导出"功能，四个项目配合使用：

### 第 1 步：装方法论（superpowers-zh）

```bash
cd /your/project
npx superpowers-zh
```

现在 AI 不会直接写代码了，会先问你需求细节。

### 第 2 步：装专家角色（agency-agents-zh）

```bash
git clone https://github.com/jnMetaCode/agency-agents-zh.git
cd agency-agents-zh && ./scripts/install.sh --tool claude-code
```

现在你可以说"用安全专家审查"、"用 DBA 优化查询"。

### 第 3 步：用编排跑评审（agency-orchestrator）

```yaml
# review-export.yaml
name: "导出功能评审"
steps:
  - id: design
    role: "engineering/engineering-software-architect"
    task: "设计用户批量导出方案，要求：支持 CSV/Excel、分页、大数据量不 OOM"
    output: design_doc

  - id: security
    role: "engineering/engineering-security-engineer"
    task: "审查导出方案的安全风险（权限、数据脱敏、频率限制）：{{design_doc}}"
    depends_on: [design]

  - id: review
    role: "engineering/engineering-code-reviewer"
    task: "审查方案的代码质量和可维护性：{{design_doc}}"
    depends_on: [design]
```

```bash
npx ao run review-export.yaml
```

三个角色自动协作：架构师设计 → 安全专家 + 代码审查员并行评审。

### 第 4 步：安全防护（shellward）

在实际编码阶段，shellward 确保 AI 不会执行危险操作：

```bash
npm install shellward
```

整个流程：**方法论确保 AI 先想后做 → 专家角色提供专业能力 → 编排引擎协调多角色 → 安全中间件兜底**。
