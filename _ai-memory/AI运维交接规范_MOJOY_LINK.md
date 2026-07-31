+++
title = "MOJOY LINK 企业官网 AI 运维交接规范"
date = 2026-07-30
draft = false
+++

> 本文档面向 AI 助手，用于统一官网更新维护的标准操作流程。任何接手本项目的 AI 必须首先阅读本文档全部内容后再执行操作。

---

## 一、项目速览（30秒了解全局）

| 项目 | 说明 |
|------|------|
| 公司全称 | 沐玥智联科技有限公司 |
| 英文名称 | MOJOY LINK TECH |
| 官网域名 | `https://www.mojoylink.com` |
| 主营业务 | 水质传感器、一体化气象站、物联网云监控平台、海洋/水利/环保监测解决方案 |
| 技术栈 | Hugo 静态站点生成器 + Blowfish 主题 |
| 本地 Hugo 版本 | v0.163.2（Windows，已安装可用） |
| 部署方式 | GitHub Pages（GitHub Action 自动构建） |
| CDN / DNS | Cloudflare（代理 + 缓存 + SSL） |
| 项目根目录 | `C:\Users\10056\Documents\mojoylink` |
| 源码仓库 | GitHub（与本地完全同步，分支 `master`） |
| **项目知识库路径** | `C:\Users\10056\Documents\knowledgebase_local\01_Work\01_Project\mojoylink官网项目` |

**核心工作流：**
```
本地修改代码 → hugo server 本地预览 → 用户确认 → git commit → 请求用户确认推送
                                                                          ↓
                    GitHub Action 自动触发 hugo 构建 ← git push origin master
                                                                          ↓
                    自动部署到 GitHub Pages → Cloudflare CDN 分发（必要时手动清缓存）
```

---

## 二、AI 操作边界（能做什么 / 不能做什么）

### ✅ AI 可以独立完成
- 新增/修改/删除产品页面（Markdown 内容）
- 更新图片资源（static/img、页面捆绑包内的图片）
- 修改网站文字内容、菜单、配置参数
- 更新联系信息、公司介绍等常规内容
- 执行本地 `hugo server` 预览，并在浏览器中打开指定页面供用户验证
- 执行 `git add` 和 `git commit`
- **请求用户确认后**执行 `git push`

### ⚠️ AI 执行前必须请求用户确认
- **Git 推送操作**（`git push`）：每次推送前必须告知用户推送内容，等待用户明确同意后才执行
- 修改 `hugo.toml`、`params.toml` 等核心配置文件
- 新增或删除菜单项
- 替换首页轮播图、logo 等关键视觉资源
- 删除任何已有文件或目录

### ❌ AI 绝对禁止操作
- **禁止** 使用 `git push --force` 或 `git push -f`
- **禁止** 未经用户确认直接执行 `git push`
- **禁止** 修改 `.github/workflows/hugo.yaml` 文件（除非用户明确指令修复部署问题）
- **禁止** 修改 `themes/blowfish/` 主题目录下的任何文件（应通过本地 `layouts/` 或 `assets/` 覆盖）
- **禁止** 提交包含敏感信息的内容（密码、密钥、内部 IP 等）
- **禁止** 在未经用户确认时修改 `baseURL` 或域名相关配置
- **禁止** 使用 Front Matter CMS 插件相关功能（该插件仅供用户个人使用，AI 直接操作文件）

---

## 三、目录结构与关键文件地图

```
mojoylink/                          # 项目根目录
│
├── .github/workflows/hugo.yaml     # GitHub Action 构建脚本（只读，勿改）
│
├── config/_default/                # 核心配置目录
│   ├── hugo.toml                   # Hugo 全局配置（baseURL、语言、输出等）
│   ├── languages.zh-cn.toml        # 中文语言配置（日期格式、logo、作者）
│   ├── menus.zh-cn.toml            # 顶部导航菜单 + 底部菜单
│   ├── params.toml                 # Blowfish 主题参数（搜索、页脚、首页布局等）
│   └── markup.toml                 # Markdown 渲染配置
│
├── content/                        # 网站内容目录（核心工作区）
│   ├── _index.md                   # 首页内容（实际布局由 custom.html 控制）
│   ├── about/
│   │   └── index.md                # 关于我们页面
│   ├── contact/
│   │   └── index.md                # 联系我们页面
│   ├── products/                   # 产品中心
│   │   ├── _index.md               # 产品列表页（分支页面）
│   │   ├── h/                      # 水文仪器产品
│   │   │   ├── _index.md
│   │   │   └── tide-s05/
│   │   │       ├── index.md        # 产品详情页（叶子页面）
│   │   │       ├── feature.png     # 产品特征图
│   │   │       └── gallery/        # 产品图库
│   │   ├── met/                    # 气象观测产品
│   │   └── wq/                     # 水质监测产品
│   └── solutions/                  # 解决方案
│       └── iot/
│           └── index.md
│
├── static/                         # 静态资源（直接复制到 public）
│   ├── img/                        # 全局图片（banner、logo 等）
│   └── pdf/                        # PDF 彩页下载文件
│
├── assets/                         # 资源管道文件（Hugo 会处理）
│   ├── img/                        # 主题资源图片（带哈希缓存）
│   └── js/                         # 自定义 JS
│
├── layouts/                        # 自定义模板（覆盖主题）
│   ├── partials/home/custom.html   # 首页自定义布局（核心）
│   ├── partials/extend-footer.html # 页脚扩展
│   └── shortcodes/                 # 自定义短代码
│
├── i18n/
│   ├── zh-cn.yaml                  # 中文翻译覆盖（如"相关产品"→"同类产品推荐"）
│   └── en.yaml                     # 英文翻译覆盖
│
├── themes/blowfish/                 # Blowfish 主题（Git 子模块，禁止直接修改）
│
├── public/                         # 构建输出目录（自动生成，勿提交）
│
└── _ai-memory/                     # ★ AI 辅助文件根目录（Hugo 自动忽略，不影响编译）
    ├── rules/                      #   AI 项目规则（最高行为准则）
    │   └── project-rules.md        #     精简规则条文（R1-R7）
    ├── knowledge/                  #   项目专属知识库（长期维护）
    │   ├── README.md               #     知识库索引
    │   ├── product-catalog.md      #     产品目录索引
    │   ├── tags-library.md         #     标签库（用户独家维护）
    │   ├── seo-keyword-bank.md     #     SEO 关键词积累库
    │   └── site-changelog.md       #     站点变更记录
    ├── inbox/                      #   临时资料存放（用户提交的原始资料，可清除）
    │   └── README.md
    ├── manuals/                    #   操作手册
    │   └── 用户操作手册.md          #     面向用户的操作指南
    ├── AI运维交接规范_MOJOY_LINK.md  #   本文件（完整规范文档）
    └── AI操作日志_MOJOY_LINK.md     #   操作记录存档
```

### 关键文件速查

| 文件 | 作用 | 修改频率 |
|------|------|----------|
| `content/**/index.md` | 页面内容 | 高频 |
| `config/_default/menus.zh-cn.toml` | 导航菜单 | 中频 |
| `config/_default/params.toml` | 主题显示参数 | 低频 |
| `layouts/partials/home/custom.html` | 首页布局 | 低频 |
| `static/img/` | 全局静态图片 | 中频 |
| `i18n/zh-cn.yaml` | 文案翻译覆盖 | 低频 |
| `_ai-memory/rules/project-rules.md` | AI 项目规则（最高准则） | 低频 |
| `_ai-memory/knowledge/tags-library.md` | 标签库（用户维护） | 低频 |
| `_ai-memory/knowledge/product-catalog.md` | 产品目录索引 | 中频 |
| `_ai-memory/AI操作日志_MOJOY_LINK.md` | 操作日志 | 每次操作 |

---

## 四、标准工作流（SOP）

### 4.0 通用操作流程（每次修改必须遵循）

```
步骤 1：确认需求 → 明确要修改哪些文件
步骤 2：备份现状 → git status 查看当前状态，记录修改前情况
步骤 3：执行修改 → 编辑/新增文件
步骤 4：本地预览 → 启动 hugo server，浏览器打开目标页面供用户验证
步骤 5：用户确认 → 用户预览后同意或提出修改意见
步骤 6：Git 提交 → git add + git commit（不推送）
步骤 7：请求推送 → 向用户说明推送内容，等待确认
步骤 8：执行推送 → 用户确认后 git push origin master
步骤 9：观察构建 → 提示用户关注 GitHub Actions 构建状态
步骤 10：线上验证 → 构建成功后提示用户访问官网验证
步骤 11：记录日志 → 将本次操作完整记录到操作日志（见第八节）
```

### 4.1 新增一个产品页面

**步骤清单：**
1. 确认产品所属分类（`h` 水文 / `met` 气象 / `wq` 水质）
2. 在对应分类下创建页面捆绑包目录，目录名使用**英文+数字**（如 `wq-mp-s7`）
3. 目录内创建 `index.md`，按规范填写 front matter（见第五节）
4. 放置产品图片：`feature.png`（必填，作为缩略图和头图）、其他展示图、`gallery/` 图库
5. 正文使用 Markdown 编写产品介绍、参数表格、应用场景
6. 如需 PDF 彩页下载，将 PDF 放入 `static/pdf/`，页面内添加下载按钮
7. **启动本地预览**（见 4.4 节），打开新增产品页面供用户验证
8. 用户确认后 → `git add` → `git commit` → **请求用户确认推送**
9. 用户确认推送后 → `git push origin master`
10. 提示用户关注 GitHub Actions 构建状态
11. 构建成功后提示用户访问 `https://www.mojoylink.com` 线上验证
12. **记录操作日志**（见第八节）

**目录结构示例：**
```
content/products/wq/
└── wq-mp-s7/                       # 产品目录（英文，短横线连接）
    ├── index.md                    # 产品页面内容
    ├── feature.png                 # 特征图（主题自动识别）
    ├── 01.png                      # 其他展示图
    ├── 02.png
    └── gallery/                    # 图库（可选）
        ├── 01.jpg
        └── 02.jpg
```

### 4.2 修改现有页面内容

1. 定位到对应 `content/.../index.md`
2. 修改内容，保持 front matter 格式不变
3. 如需更新图片，替换同名文件或采用新文件名（新文件名需同步修改 md 中的引用）
4. **启动本地预览**，打开修改页面供用户验证
5. 用户确认后 → Git 提交 → **请求确认推送** → 推送
6. **记录操作日志**

### 4.3 更新首页轮播图 / 全局图片

1. 新图片放入 `static/img/`（如 `banner-new.jpg`）
2. 修改 `layouts/partials/home/custom.html` 中的图片引用路径
3. **重要**：如使用同名替换，必须先清理缓存：
   ```powershell
   Remove-Item -Recurse -Force resources, public -ErrorAction SilentlyContinue
   hugo --gc --ignoreCache
   ```
4. 启动本地预览，打开首页供用户验证
5. 用户确认后 → Git 提交 → **请求确认推送** → 推送
6. 推送后告知用户：可能需要 Cloudflare 清缓存（见 6.3 节）
7. **记录操作日志**

### 4.4 本地预览操作规范

**启动 Hugo 开发服务器：**

```powershell
# 在项目根目录执行（Windows PowerShell）
cd C:\Users\10056\Documents\mojoylink
hugo server -D
```

- `-D` 参数：包含草稿页面预览（如果修改的页面 draft=true）
- 如草稿已设为 false，直接 `hugo server` 即可
- 服务器启动后默认地址：`http://localhost:1313/`

**自动打开浏览器指定页面：**

AI 启动 hugo server 后，应根据修改内容告知用户具体的预览地址：

| 修改场景 | 预览地址 |
|----------|----------|
| 首页 | `http://localhost:1313/` |
| 产品列表页 | `http://localhost:1313/products/` |
| 具体产品页 | `http://localhost:1313/products/wq/wq-mp-s7/` |
| 关于我们 | `http://localhost:1313/about/` |
| 联系我们 | `http://localhost:1313/contact/` |
| 解决方案 | `http://localhost:1313/solutions/` |

**预览注意事项：**
- 预览期间 hugo server 会自动热更新，修改保存后浏览器自动刷新
- 预览完成后，按 `Ctrl + C` 停止服务器
- 停止服务器后再执行 Git 操作，避免文件锁冲突

---

## 五、内容编写规范

### 5.1 Front Matter 元数据配置规范

> **本节为 front matter 元数据的唯一权威参考。** 日后如需调整字段规则，只需修改本节内容即可，全文其他位置不再重复定义。

#### 5.1.1 格式要求

- 所有内容文件（`content/**/*.md`）顶部必须使用 `+++` 包裹的 **TOML 格式**
- 禁止使用 YAML（`---`）或 JSON 格式
- 字段顺序建议保持与模板一致，便于阅读和维护

#### 5.1.2 标准模板（以实际产品页为例）

```toml
+++
title = "H-TIDE-S05 高精度压力温度传感器"
description = "H-TIDE-S05 水位温度传感器支持电池 / 外接双供电，精度 ±0.05% FS，IP68 钛合金 / 不锈钢外壳，内置大容量存储，Modbus RS485 输出，用于海洋潮位、河湖地下水长期水位水温监测。"
tags = [ "水文仪器设备" ]
summary = "H-TIDE-S05 一体式潮位记录仪集成水深压力与水温检测，超低功耗锂电池续航可达 10 年，可存储 3 万条以上数据，耐腐蚀外壳适配海水淡水，适配浮标、岸站、地下水井定点观测。"
date = "2026-06-30T22:49:59+08:00"
keywords = [
  "H-TIDE-S05",
  "压力温度记录仪",
  "地下水水位监测仪",
  "海水液位探头",
  "潮位传感器",
  "高精度水位计",
  "钛合金水下液位传感器"
]
+++
```

#### 5.1.3 字段说明（统一参考表）

| 字段 | 类型 | 是否必填 | 说明 | 示例值 |
|------|------|----------|------|--------|
| `title` | string | ✅ 必填 | 页面标题，显示在浏览器标签页和页面顶部，包含产品型号和中文名称 | `"H-TIDE-S05 高精度压力温度传感器"` |
| `description` | string | ✅ 必填 | 页面描述（SEO meta），一段话概括产品核心卖点和应用场景，控制在一到两句话 | `"H-TIDE-S05 水位温度传感器支持电池 / 外接双供电，精度 ±0.05% FS..."` |
| `date` | string | ✅ 必填 | 页面发布日期，ISO 8601 格式，**必须携带时区 `+08:00`（北京时间）** | `"2026-06-30T22:49:59+08:00"` |
| `draft` | bool | ✅ 必填 | 是否为草稿。发布时必须设为 `false` | `false` |
| `tags` | array | ✅ 必填 | 页面标签（Hugo 分类法），一个页面可配多个标签，影响搜索和关联推荐 | `[ "水文仪器设备" ]` |
| `summary` | string | ✅ 必填 | 页面摘要，显示在产品列表卡片中，概括产品核心特点和适用场景 | `"H-TIDE-S05 一体式潮位记录仪集成水深压力与水温检测..."` |
| `keywords` | array | ✅ 必填 | SEO 关键词数组，影响搜索引擎收录，5-8 个为宜，包含产品型号、别名、应用场景词 | `[ "H-TIDE-S05", "压力温度记录仪", ... ]` |
| `weight` | int | ⬜ 选填 | 排序权重，越小越靠前。未设置时默认按日期倒序 | `10` |
| `icon` | string | ⬜ 选填 | 菜单或卡片中显示的图标名称（Blowfish 内置图标） | `"water"` |
| `showReadingTime` | bool | ⬜ 选填 | 是否显示阅读时间，默认 `false` | `false` |
| `showHero` | bool | ⬜ 选填 | 是否显示 Hero 大图，默认 `true` | `true` |
| `heroStyle` | string | ⬜ 选填 | Hero 大图样式，可选 `basic` / `big` / `background` / `thumbAndBackground` | `"big"` |
| `featureimage` | string | ⬜ 选填 | 全局特征图路径（仅当不使用页面捆绑包 `feature.png` 时才需要设置） | `"/img/iceland.jpg"` |

#### 5.1.4 编写要点

1. **`date` 格式**：必须使用 `"YYYY-MM-DDTHH:MM:SS+08:00"` 格式，带引号，时区固定 `+08:00`
2. **`keywords` 多行写法**：当关键词较多时，可换行书写，每行一个关键词，末尾不加逗号，最后一个不加逗号
3. **`description` 与 `summary` 区别**：
   - `description`：偏向 SEO，浓缩核心参数和卖点，给搜索引擎看
   - `summary`：偏向用户浏览，概括产品特点和场景，显示在列表卡片中
4. **新建页面时**：参考同分类下已有产品页的 front matter 作为模板，确保格式统一

#### 5.1.5 Tags 标签管理规范

> **重要**：`tags` 字段的取值范围由 **用户（您）** 独家维护。AI 必须严格遵守以下规则，不得擅自新增或修改标签库。
> **权威标签库文件**：`_ai-memory/knowledge/tags-library.md`

**当前可用标签库**：
(详见 `_ai-memory/knowledge/tags-library.md`，以下为快速参考)
- `水文仪器设备`
- `气象观测设备`
- `水质与生态观测`
- `物联网平台`
- `企业信息`
- `联系我们`

**AI 操作规则**：
1. **优先选择**：新增页面时，必须从上述「当前可用标签库」中选择最匹配的标签。
2. **申请新增**：如现有标签均不适用于新页面，AI 必须向用户说明情况，并**请求新增特定标签**。只有在用户明确同意后，才能在 front matter 中使用该新标签。
3. **同步更新**：用户同意新增标签后，AI 有责任提醒用户更新本节的「当前可用标签库」列表，以保持规范的一致性。

### 5.2 图片规范

| 场景 | 存放位置 | 引用方式 |
|------|----------|----------|
| 页面捆绑包内图片 | `content/.../page-bundle/xxx.png` | `![alt](xxx.png)` 或 `./xxx.png` |
| 全局静态图片 | `static/img/xxx.jpg` | `![alt](/img/xxx.jpg)` |
| 主题资源图片 | `assets/img/xxx.jpg` | 通过 Hugo 资源管道引用 |

**要求：**
- 产品特征图必须命名为 `feature.png` 或 `feature.jpg`（主题自动识别）
- 图片文件名使用英文、数字、短横线，**禁止中文**
- 图片建议尺寸：特征图 1200×630px，图库图片不超过 1920px 宽
- 优先使用 `.jpg` 或 `.png` 格式

### 5.3 PDF 彩页规范

1. PDF 文件统一放入 `static/pdf/`
2. 线上访问路径为 `/pdf/xxx.pdf`
3. 页面内添加下载按钮（主题风格）：

```markdown
<a href="/pdf/wq-mp-s7.pdf" download class="btn btn-secondary inline-flex items-center gap-2">
  {{< icon "file-pdf" >}}
  产品完整技术彩页（含版权水印）
</a>
[在线预览彩页](/pdf/wq-mp-s7.pdf){target="_blank" class="ml-4"}
```

4. PDF 必须自带满铺半透明水印（15%-20% 透明度），关闭复制/打印权限，**不设置打开密码**

### 5.4 Markdown 正文规范

- 使用标准 Markdown 语法
- 技术参数使用表格展示
- 多图展示使用 `gallery` 短代码（见原始运维手册）
- 产品优势分点列出，中英双语按需使用
- 页面末尾可添加返回产品列表的链接

---

## 六、用户新增内容协作流程与资料提供模板

### 6.0 AI 在新增内容中的角色定位

**用户提供资料 → AI 主动加工 → 生成交付物 → 用户确认 → 上线**

AI 不只是模板填空工具，而是承担以下核心职责：

| AI 职责 | 说明 |
|---------|------|
| **资料理解分析** | 阅读用户提供的任意格式资料（PDF、Word、文案、图片、数据表、零散文字等），提取关键信息，理解产品/方案的核心卖点和目标受众 |
| **元数据生成** | 根据分析结果，AI 自动生成完整的 front matter 元数据（title、description、summary、keywords、tags 等），保证字段齐全、格式规范 |
| **SEO 优化** | 结合行业关键词和产品特点，优化 title 标题标签、description 描述、keywords 关键词密度，提升搜索引擎收录排名潜力 |
| **正文内容生成** | 基于原始资料，AI 按照官网已有页面的结构规范（产品页三章节/方案页多章节），生成排版规范、可读性强的 Markdown 正文 |
| **宣传推广建议** | 每次新增内容后，AI 会附带输出一份推广建议，包括：核心宣传语、推荐投放关键词、适合的行业/渠道、建议搭配的内链策略（关联到哪些已有产品/方案） |

---

### 6.1 用户资料提供方式

**您不需要严格按模板格式准备，可以直接发送任何形式的资料。** AI 的任务就是从原始资料中提取和整理信息。

可接受的资料形式：
- 产品彩页 PDF / 技术规格书
- Word / PPT 文档
- 已有的文案草稿、新闻稿
- 产品参数表格（Excel 截图或文字）
- 图片（产品图、场景图、截图）
- 零散文字描述 / 语音转文字稿
- 以上任意组合

**建议至少提供的最小资料集**（资料越丰富内容越完整）：
- 产品型号和名称
- 核心功能特点（哪怕 3-5 句话描述也行）
- 主要技术参数（或参数表截图）
- 应用场景（或目标行业）
- 产品图片（或说明稍后补）

---

### 6.2 AI 标准交付物清单

每次处理完您提供的资料后，AI 会按以下清单交付成果，请逐一确认：

```
【AI 交付物 1】元数据提案（请确认）
    - front matter 完整字段（title/description/summary/keywords/tags/date 等）
    - 标签 tags 从标签库选取，如需新增会明确告知您并申请确认

【AI 交付物 2】页面正文（请审阅）
    - 按官网现有结构生成 Markdown 正文
    - 产品页：产品简介 + 规格参数 + 适用场景 三章节
    - 方案页：根据资料丰富度生成相应章节
    - 图片引用位置标注清楚（待您放入图片文件）

【AI 交付物 3】SEO 优化说明
    - 本次选用的核心关键词及理由
    - 标题/描述的 SEO 优化策略
    - 可能遗漏的建议补充关键词

【AI 交付物 4】官网宣传推广建议
    - 一句话核心宣传语（可用于 Banner / 邮件签名 / 展会资料）
    - 三句话卖点提炼（可用于朋友圈/公众号摘要）
    - 推荐行业投放关键词（百度/搜狗/360 付费推广）
    - 站内关联建议（推荐关联哪些已有产品/方案页面，构建内链）
    - 建议搭配的营销渠道（如公众号推文、行业展会、B2B 平台上架等）

【AI 交付物 5】待办清单
    - 需要您补充的图片/文件列表
    - 需要您确认的决策点
```

---

### 6.3 新增产品页面 — 参考模板（供 AI 对齐格式）

> 此模板供 AI 内部参考以对齐输出格式，**用户无需按此模板提交资料**，直接发原始资料即可。

```markdown
+++
title = "产品型号 产品中文名称"
description = "AI生成的SEO描述（含核心参数、协议、应用场景）"
summary = "AI生成的卡片摘要（一句话卖点）"
date = "YYYY-MM-DDTHH:MM:SS+08:00"
draft = false
tags = [ "从标签库选取" ]
keywords = [
  "AI生成的关键词1",
  "关键词2",
  ...
]
+++

## 产品简介
（AI 基于原始资料生成的 1-3 段正文）

![](01.png)

## 规格参数
（AI 基于参数表生成图片引用；如参数为文字则生成表格）

## 适用场景
1. 场景1
2. 场景2
...
```

---

### 6.4 新增解决方案页面 — 参考模板（供 AI 对齐格式）

> 同 6.3，供 AI 内部参考以对齐输出格式。

```markdown
+++
title = "方案标题"
description = "AI生成的SEO描述"
summary = "AI生成的卡片摘要"
date = "YYYY-MM-DDTHH:MM:SS+08:00"
draft = false
tags = [ "从标签库选取" ]
weight = 1
keywords = [
  "AI生成的关键词1",
  ...
]
+++

## 一、平台总览
（AI 基于资料生成定位、核心价值、交付形态）
...
```

---

### 6.5 模板使用注意事项

1. **用户只需发原始资料**：不需要刻意整理格式，越原始越好，AI 会提取
2. **图片处理**：图片可以稍后单独提供，先发文字让 AI 创建页面框架
3. **规格参数**：产品页推荐用截图方式，保持与现有产品页风格一致；无图时 AI 可生成 Markdown 表格
4. **不完整的资料**：如果某些项缺失，AI 会在交付物中标注"待补"并给出建议
5. **多个产品**：如需同时新增多个产品，可一起发资料，AI 会逐个处理并在每次交付后确认

---

## 七、Git 提交与部署规范

### 7.1 提交信息格式

使用简洁明确的英文或中文提交信息，遵循以下前缀：

| 前缀 | 用途 | 示例 |
|------|------|------|
| `feat:` | 新增功能/页面/产品 | `feat: 新增 WQ-MP-S7 多参数水质传感器产品页` |
| `update:` | 更新现有内容 | `update: 更新联系页面商务邮箱` |
| `fix:` | 修复错误 | `fix: 修复气象产品页图片路径错误` |
| `assets:` | 更新图片/PDF 等资源 | `assets: 替换首页 banner 图为夏季活动版` |
| `config:` | 修改配置文件 | `config: 调整产品列表页排序权重` |
| `content:` | 修改文案内容 | `content: 更新关于我们公司简介文案` |

### 7.2 部署流程（含用户确认节点）

```
① 修改完成，保存文件
② 执行 hugo server -D，打开浏览器预览
   ↓ 【用户确认节点 1：预览验证】
③ 用户确认预览效果无误
④ git status                     # 查看变更文件
⑤ git add <具体文件路径>          # 添加文件（优先指定文件，不用 git add .）
⑥ git commit -m "feat: xxx"     # 提交到本地仓库
   ↓ 【用户确认节点 2：推送确认】
⑦ 向用户说明：本次推送包含 X 个文件，内容是 xxx
⑧ 用户确认推送
⑨ git push origin master         # 推送到 GitHub
⑩ 提示用户：GitHub Actions 正在构建，可在仓库 Actions 页面查看进度
⑪ 构建成功后，提示用户访问官网验证
⑫ 记录操作日志（见第八节）
```

**Git 命令示例（Windows PowerShell）：**
```powershell
cd C:\Users\10056\Documents\mojoylink
git status
git add content/products/wq/wq-mp-s7/index.md content/products/wq/wq-mp-s7/feature.png
git commit -m "feat: 新增 WQ-MP-S7 多参数水质传感器产品页"
# ---- 此时暂停，向用户说明推送内容，等待确认 ----
# 用户确认后：
git push origin master
```

### 7.3 Cloudflare 缓存清理

当遇到以下情况时，需要告知用户手动清理 CDN 缓存：
- 替换了同名图片但线上仍显示旧图
- 修改了 CSS/JS 但样式未更新
- 页面内容已更新但访问时仍是旧版本

**操作步骤（告知用户执行）：**
1. 登录 Cloudflare 后台
2. 选择域名 `mojoylink.com`
3. 左侧菜单：Caching → Configuration
4. 点击 **Purge Everything**（清除所有内容）
5. 浏览器使用 `Ctrl + F5` 硬刷新验证

---

## 八、常见问题速查（FAQ）

### Q1：本地预览时图片已更新，但线上仍显示旧图
**原因**：Cloudflare CDN 缓存或 Hugo 资源管道缓存。
**解决**：
1. 清理 Cloudflare 缓存（Purge Everything）
2. 如图片在 `assets/` 目录下，本地执行：
   ```powershell
   Remove-Item -Recurse -Force resources, public -ErrorAction SilentlyContinue
   hugo --gc --ignoreCache
   hugo server -D
   ```
3. 终极方案：新图片重命名（如 `banner-v2.jpg`），同步修改引用路径

### Q2：网站搜索功能失效（输入关键词无结果）
**原因**：`index.json` 索引文件生成异常或 baseURL 协议错误。
**排查**：
1. 确认 `hugo.toml` 中有 `[outputs] home = ["HTML", "RSS", "JSON"]`
2. 确认 `params.toml` 中有 `enableSearch = true`
3. 确认 GitHub Action 构建日志中正常生成 `index.json`
4. 浏览器 F12 → Network → 查看 `index.json` 请求是否为 `https://`

### Q3：新增的产品页面在列表中不显示
**原因**：
- `draft = true`（草稿状态不会发布）
- 页面放在错误目录下，未被 Hugo 识别为内容页
- `weight` 排序问题

**解决**：
1. 确认 `draft = false`
2. 确认页面目录结构为页面捆绑包（`目录名/index.md`）
3. 检查所属分类的 `_index.md` 是否存在

### Q4：GitHub Action 构建失败
**排查步骤**：
1. GitHub → 仓库 → Actions → 点击失败的 workflow
2. 查看 `Build the site` 步骤的日志
3. 常见原因：
   - Markdown front matter 语法错误（TOML/YAML 混用）
   - 图片路径引用错误
   - Hugo 版本兼容问题

---

## 九、AI 操作日志规范

### 9.1 日志要求

**每次对项目执行任何修改操作，AI 必须在操作日志中留档记录。** 日志用于事后复查，确保所有变更有据可查。

### 9.2 日志文件位置

```
C:\Users\10056\Documents\mojoylink\_ai-memory\AI操作日志_MOJOY_LINK.md
```

### 9.3 日志格式

每次操作追加一条记录，格式如下：

```markdown
---

## [日期时间] 操作标题

**操作类型**：feat / update / fix / assets / config
**操作原因**：用户要求 xxx（引用用户原始需求）

### 修改文件清单
| 文件路径 | 操作类型 | 说明 |
|----------|----------|------|
| content/products/wq/wq-mp-s7/index.md | 新增 | 新建产品页面 |
| content/products/wq/wq-mp-s7/feature.png | 新增 | 产品特征图 |
| static/pdf/wq-mp-s7.pdf | 新增 | 产品彩页 PDF |

### 操作过程
1. 创建目录 content/products/wq/wq-mp-s7/
2. 创建 index.md，填写 front matter 和正文内容
3. 放置 feature.png 和展示图片
4. 启动 hugo server -D 本地预览
5. 用户预览确认无误
6. git add + git commit
7. 用户确认推送，git push origin master
8. GitHub Action 构建成功

### Git 提交信息
feat: 新增 WQ-MP-S7 多参数水质传感器产品页

### 用户确认记录
- 预览确认：用户确认页面效果无误（或用户提出了 xxx 修改意见，已修改）
- 推送确认：用户于 [时间] 确认推送

### 验证结果
- 本地预览：通过
- GitHub Action 构建：成功
- 线上访问：已验证 / 待用户验证

### 备注
（如有特殊情况、遗留问题、后续待办等，在此说明）
```

### 9.4 日志编写规则

1. **时间戳**：使用北京时间（Asia/Shanghai），格式 `YYYY-MM-DD HH:MM`
2. **完整性**：必须列出所有修改的文件，包括新增、修改、删除
3. **可追溯性**：必须记录用户的原始需求和确认过程
4. **及时性**：操作完成后立即写入日志，不要事后补记
5. **追加模式**：新日志追加到文件末尾，不覆盖历史记录

---

## 十、危险操作清单（执行前必须二次确认）

| 操作 | 风险 | 确认方式 |
|------|------|----------|
| `git push --force` | 会覆盖远程历史，可能导致数据丢失 | ❌ 绝对禁止 |
| `git push`（任何推送） | 推送后立即触发自动部署 | 必须用户明确确认 |
| 修改 `hugo.yaml` 工作流 | 可能导致自动部署完全失效 | 必须用户明确指令 |
| 修改 `baseURL` | 会导致全站链接错误、搜索失效 | 必须用户明确指令 |
| 删除 `content/` 整级目录 | 会删除该分类下所有产品页面 | 向用户确认删除范围 |
| 修改主题目录 `themes/blowfish/` | 子模块修改会导致更新冲突 | 通过 `layouts/` 覆盖替代 |
| 清理 `public/` 目录 | 只是构建输出，可安全删除 | 确认不是手动放入的文件 |

---

## 十一、参考文档索引

### 项目内文档（`_ai-memory/` 目录下）

| 文档 | 路径 | 用途 |
|------|------|------|
| AI 项目规则 | `_ai-memory/rules/project-rules.md` | AI 最高行为准则（精简规则条文） |
| 本规范文档 | `_ai-memory/AI运维交接规范_MOJOY_LINK.md` | AI 完整操作规范与流程 |
| AI 操作日志 | `_ai-memory/AI操作日志_MOJOY_LINK.md` | 历次操作记录存档 |
| 用户操作手册 | `_ai-memory/manuals/用户操作手册.md` | 面向用户的操作指南 |
| 产品目录索引 | `_ai-memory/knowledge/product-catalog.md` | 全部产品/方案页面索引 |
| 标签库 | `_ai-memory/knowledge/tags-library.md` | tags 权威取值范围 |
| SEO 关键词库 | `_ai-memory/knowledge/seo-keyword-bank.md` | 按产品积累的关键词 |
| 站点变更记录 | `_ai-memory/knowledge/site-changelog.md` | 每次内容上线的记录 |

### 知识库文档（本地知识库）

| 文档 | 路径 | 用途 |
|------|------|------|
| 完整运维手册 | `knowledgebase_local/01_Work/02_Technical/个人网站/个人网站-独立站/20260730_*.md` | 详细技术方案、故障排查 |
| 网站框架说明 | `knowledgebase_local/01_Work/02_Technical/个人网站/个人网站-独立站/20260729_*.md` | Hugo 框架基础教程 |

**外部参考：**
- Blowfish 主题文档：https://blowfish.page/zh-cn/docs/
- Hugo 官方文档：https://gohugo.io/documentation/

---

## 十二、快速上手检查表

新 AI 接手任务时，按以下顺序执行：

- [ ] 阅读 `_ai-memory/rules/project-rules.md`（最高行为准则）
- [ ] 阅读本规范文档全文
- [ ] 查看知识库：`_ai-memory/knowledge/` 下的产品目录、标签库、SEO 关键词库
- [ ] 确认当前工作目录为 `C:\Users\10056\Documents\mojoylink`
- [ ] 查看 `git status` 确认本地与远程同步状态
- [ ] 查看操作日志 `_ai-memory/AI操作日志_MOJOY_LINK.md` 了解近期变更历史
- [ ] 明确用户本次修改需求（新增产品 / 修改内容 / 更新图片 / 修复问题）
- [ ] 如涉及新增页面，参考已有同类产品页面作为模板
- [ ] 修改完成后，启动 `hugo server -D` 本地预览
- [ ] 告知用户预览地址，等待用户确认
- [ ] 用户确认后，按规范格式 git add + git commit
- [ ] 向用户说明推送内容，等待用户确认推送
- [ ] 用户确认后 git push origin master
- [ ] 提示用户关注 GitHub Actions 构建状态
- [ ] 构建成功后提示用户访问官网验证
- [ ] **记录操作日志**到 `_ai-memory/AI操作日志_MOJOY_LINK.md`
- [ ] **同步更新知识库**（product-catalog / seo-keyword-bank / site-changelog）

---

> **文档维护**：每次遇到新的典型问题或规范调整，应更新本文档，保持其时效性。
