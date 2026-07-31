# MOJOY LINK 官网项目 — AI 项目规则

> **本文件是 AI 在本项目中的最高行为准则。** 任何 AI 在执行任何操作前，必须首先阅读并遵守本文件。
> 本文件保持精简，只放规则条文；详细说明请参阅 `_ai-memory/AI运维交接规范_MOJOY_LINK.md`。

---

## R1. 编译安全

- **R1.1** 所有 AI 辅助文件必须存放在 `_ai-memory/` 目录下（以 `_` 开头，Hugo 自动忽略）
- **R1.2** 禁止修改 `themes/blowfish/` 目录下的任何文件
- **R1.3** 禁止修改 `.github/workflows/hugo.yaml`（除非用户明确指令修复部署问题）
- **R1.4** 禁止修改 `config/_default/hugo.toml` 中的 `baseURL`（除非用户明确指令）
- **R1.5** 每次修改后必须执行 `hugo server -D` 本地预览，确认无编译错误

## R2. Git 操作

- **R2.1** `git push` 前必须告知用户推送内容，等待用户明确同意后才执行
- **R2.2** 禁止使用 `git push --force` 或 `git push -f`
- **R2.3** `git add` 优先指定具体文件路径，禁止使用 `git add .` 或 `git add -A`
- **R2.4** 提交信息使用规范前缀：`feat:` / `update:` / `fix:` / `assets:` / `config:` / `content:`
- **R2.5** 禁止提交包含敏感信息的内容（密码、密钥、内部 IP）

## R3. 内容格式

- **R3.1** 所有 front matter 使用 TOML 格式（`+++` 包裹），禁止使用 YAML 或 JSON
- **R3.2** `date` 字段必须使用 `"YYYY-MM-DDTHH:MM:SS+08:00"` 格式，时区固定 `+08:00`
- **R3.3** 图片文件名禁止使用中文，必须使用英文+数字+短横线
- **R3.4** 产品特征图必须命名为 `feature.png` 或 `feature.jpg`
- **R3.5** 正文图片使用 Page Bundle 相对路径（与 index.md 同目录），禁止使用绝对路径

## R4. 标签管理

- **R4.1** `tags` 取值范围由用户独家维护，权威列表见 `_ai-memory/knowledge/tags-library.md`
- **R4.2** AI 新增页面时必须从标签库中选取，禁止擅自编造标签
- **R4.3** 如需新增标签，必须向用户申请并获得明确同意

## R5. 操作记录

- **R5.1** 每次修改操作完成后，必须立即在 `_ai-memory/AI操作日志_MOJOY_LINK.md` 中追加日志
- **R5.2** 日志必须包含：操作类型、文件清单、操作过程、用户确认记录、验证结果
- **R5.3** 日志按追加模式写入，禁止覆盖历史记录

## R6. 用户协作

- **R6.1** 用户提供原始资料后，AI 负责分析、生成元数据、优化 SEO、提出推广建议
- **R6.2** 每次新增内容必须输出 5 类交付物（元数据/正文/SEO说明/推广建议/待办清单）
- **R6.3** 用户提交的原始资料存放于 `_ai-memory/inbox/`，内容上线后可清除
- **R6.4** 禁止使用 Front Matter CMS 插件功能，AI 直接操作文件

## R7. 知识库维护

- **R7.1** 新增/删除产品后，必须同步更新 `_ai-memory/knowledge/product-catalog.md`
- **R7.2** 新增标签后，必须同步更新 `_ai-memory/knowledge/tags-library.md`
- **R7.3** 每次内容上线后，必须同步更新 `_ai-memory/knowledge/site-changelog.md`
- **R7.4** SEO 关键词积累记录在 `_ai-memory/knowledge/seo-keyword-bank.md`
