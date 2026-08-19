# AGENTS.md

awesome-dsh-plugin 的项目宪法。动手前先读。

## What this is

DeepSeek Harness（`dsh`）插件的**精选列表**（awesome list）+ 配套静态站点（awesome-dsh-plugin.com）。
数据是 YAML（`data/plugins/*.yml`），站点与 README 由脚本**生成**，不是手写的。本仓库的价值在数据的
准确性与审核纪律——被列出的插件会以用户权限运行第三方代码，列表本身不是安全背书，但审核流程要严谨。

## Start

- 数据源：`data/plugins/`（每插件一个 YAML，文件名 `owner__repo.yml`）。
- 生成 README：`npm run generate:readme`（`scripts/generate-readme.mjs`）。
- 构建站点：`npm run build:site`（`scripts/build-site.mjs`，模板在 `site/`）。
- 提交前检查：`npm run check:submission`（`scripts/check-submission.mjs`）。
- 新插件提交：读 CONTRIBUTING（PR 流程），把 YAML 放进 `data/plugins/` 并重新生成 README + 站点。

## Architecture map

- **数据** `data/plugins/*.yml`：插件元数据（作者/仓库/类别/描述/安装方式）。`data/added-dates.json`、
  `data/screenshots.json`：派生数据。
- **生成脚本** `scripts/`：
  - `generate-readme.mjs`：YAML → README.md（分类聚合、TOC）。
  - `build-site.mjs`：YAML → `site/` 静态页（`site/template.html` / `detail-template.html` 模板，
    `site/locales.mjs` 多语言）。
  - `check-submission.mjs`：PR 提交合规检查。
  - `probe-npm.mjs` / `probe-readmes.mjs` / `probe-stars.mjs`：数据保鲜探测（版本/文档/star 变化）。
  - `scan-decay.mjs`：停滞插件扫描。
  - `migrate-to-yaml.mjs` / `reorder-categories.py`：历史迁移工具，改数据格式才用。
- **站点** `site/`：模板 + 静态资源；`design-mocks/`：设计稿。
- **技能** `.claude/skills/` 与 `.agents/skills/`（互为镜像）：`ui-ux-pro-max`（设计系统目录数据 +
  Python 校验脚本）、`web-design-guidelines`（Web 设计准则）——站点视觉遵循这些技能。

## Hard rules

- **YAML 是唯一事实源**：README 与站点都是生成的，禁止手改生成物（README.md 的插件列表区、
  `site/` 输出、`data/added-dates.json`）。改数据 → 改 YAML → 重新生成。
- **数据格式有 schema 约定**：YAML 字段按既有文件对齐，新增字段要同步脚本与模板，并保证旧数据兼容。
- **审核纪律**：收录标准按 CONTRIBUTING 执行；被拒原因在 PR 里说清。安全警告区（README 顶部）
  不允许删减弱化。
- **探测脚本防滥用**：probe-* 脚本有频率与数量上限，别把 npm/github API 打到限流。
- **提交纪律**：提交前 `npm run check:submission` 必须过；PR 里数据变更与生成物变更要成对出现。

## Before changing X, read Y

- 加/改插件数据 → 读 3 个同类 YAML 看字段对齐 + `scripts/check-submission.mjs` 的校验规则。
- 改站点视觉 → 读 `site/template.html` + `.agents/skills/web-design-guidelines/SKILL.md`。
- 改生成逻辑 → 读 `scripts/generate-readme.mjs` 与 `build-site.mjs`（README 与站点是两套独立生成器）。

## Verification

- `npm run generate:readme && npm run build:site` 无报错。
- `npm run check:submission` 通过。
- git diff 中数据变更与生成物变更一一对应（README 段落、site 输出）。