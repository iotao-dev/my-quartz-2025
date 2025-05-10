---
permalink: obsidian-publish
tags:
  - "#Category/handbook"
  - "#Topic/obsidian"
draft:
---

# 01 方案说明

##  🗺 方案背景与适用人群 
捣鼓这个方案主要出于个人需要，作为长期all in one模式使用obsidian进行日常记录的非技术背景用户，一直试图寻找符合以下需求的对外发布方案

> [!info] 核心需求
> * 保持 Obsidian 的本地使用习惯，不需要迁移或者分割已有的内容结构
> * 兼容Obsdian的核心功能如markdown语法、双链、Graph View、tag、Mermaid图表、 Frontmatter属性、搜索等
> * 部署在自己控制的平台上，而不是受制于单一平台
> * 具备长期可维护性，但不希望技术复杂性过高，最好是启用之后只管写作与发布就行
> * 对于有助于正反馈的功能有较好支持（如统计与评论），自定义外观或其他高级功能具备可行性但不是必须的任务
> * 能够安全地隔离「私密笔记」与「公开内容」
> * 当然还有最重要的：完全免费

最终确定的方案如下，其中每一个环节都有可替代项，可根据需要进行选择：

> [!tldr] 方案概要
> * 本地使用Obsidian全量记录
> 	* 自定义一个子目录如Public放置公共发布内容 （只有该目录的内容被后续环节使用）
> 	* 使用官方sync或者其他同步服务实现多终端同步，即并不使用git对全量库进行托管
> * Quartz ：将markdown文件构建为静态页面
> * Github托管：使用`.gitignore`过滤私有数据
> * Cloudflare Pages自动发布：从托管到Github的源码自动完成站点发布

## 🧰 工具清单

| 工具               | 用途               | 选择理由                                         | 可替代项             | 免费  |
| ---------------- | ---------------- | -------------------------------------------- | ---------------- | --- |
| Obsidian         | 本地笔记写作与管理        | Markdown 支持好，插件生态活跃，可控性高                     | Logseq           | ✅   |
| Quartz 4         | 将 Markdown 构建为网站 | 高度兼容obsidian 、美观简洁、文档清晰上手门槛相对低、可自定义性高、开源社区活跃 | Hugo / Jekyll    | ✅   |
| GitHub           | 源码托管             | 与 Cloudflare Pages 原生集成                      | GitLab           | ✅   |
| Cloudflare Pages | 免费静态网站部署平台       | 快速、稳定、支持自定义域名与自动部署、自定义域名自带免费的基础统计            | Netlify / Vercel | ✅   |

##  📁 目录结构

```bash
quartz/                    # Quartz的安装目录
├── content/               # 所有 Obsidian 内容（可直接设置为 Vault）
│   ├── Public/            # ✅ 发布目录，仅此被 Git 跟踪和部署
│   ├── <其他目录与md文件>   # 私有笔记，保留本地
│   └── ...             
├── .gitignore             # 控制仅提交 Public/ 的内容到github
├── ...                    # Quartz安装后自动生成的其他文件与目录
```

# 02  Quartz安装与基本设置 
## 📦 安装Quartz 4
- 需要先安装git和Nodejs
	* Node.js: [https://nodejs.org](https://nodejs.org)
	* Git: [https://git-scm.com](https://git-scm.com)
- 整个Quartz 4的安装过程基本参考官方的最新文档（[Welcome to Quartz 4](https://quartz.jzhao.xyz/)）的默认设置进行即可
```bash
# windows系统建议使用cmd运行命令行，减少出错几率
git clone https://github.com/jackyzha0/quartz.git
cd quartz
npm i
npx quartz create #注意该流程可能需要一些时间完成，不要重复运行

# 安装过程中均使用默认项即可，以下提示说明已经安装成功
—  You're all set! Not sure what to do next? Try:
  • Customizing Quartz a bit more by editing `quartz.config.ts`
  • Running `npx quartz build --serve` to preview your Quartz locally
  • Hosting your Quartz online (see: https://quartz.jzhao.xyz/hosting)
```
## ⚙️ 设置站点
- 编辑quartz安装目录下的`quartz.config.ts`，修改所需的配置，例如将`pageTitle`改为自己的站点名称
- 全部可配置项参考 [Configuration](https://quartz.jzhao.xyz/configuration)
## 📝 编辑笔记
- 安装完成后Quartz将会自动创建一系列目录与文件，其中`content/`默认作为需要发布的笔记目录，在不进行任何特殊设置的前提下，该目录下所有（包括子目录中）的markdown文件都会作为页面进行发布， `content/index.md`的内容将作为首页
- 如果将 content 目录直接设置为obsidian的Vault，则可创建专门用于发布内容子目录如`Public`，后续将其设置为部署目录，则该目录下的 `index.md`将作为首页
- Quartz对obsidian的兼容度很高，基本上保持正常的使用习惯即可，Quartz 原生支持的一些obsidian常见 frontmatter 字段可用于自定义设置

> [!example]
> - `title`：页面的标题。如果未提供，Quartz 将使用文件名作为页面标题
> - `description`：用于链接预览的页面描述
> - `permalink`：页面的自定义 URL，该属性不需要填写完整的URL，而是一个字符串，例如本页面设置为 `obsidian-publish`，则实际URL为 https://iotao.iwheel.lol/obsidian-publish ， 无论后续文件名是否更改， 该URL都会自动指向正确的页面文件。但在网站的内部链接不会自动使用该URL，适合用于分享
> - `aliases`：页面的别名
> - `tags`：页面的标签
> - `draft`：是否发布页面，设置为`true`则不会不发布为页面
> - `date`：表示注释发布日期的字符串，通常使用 format `YYYY-MM-DD`

- 详情参考 [Authoring Content](https://quartz.jzhao.xyz/authoring-content)
## 🚫 过滤发布内容
Quartz提供了三种方式来过滤发布内容，需要注意的是这些功能仅控制是否将内容部署静态网站的页面以及附件，==并不能控制是否同步到github以及Cloudflare Pages==，因此当使用公开的github时还需要设置`.gitignore`（见后续步骤03的相关说明）
- **黑名单模式**：默认可使用笔记的 `draft` 属性，见【编辑笔记】
- **白名单模式**：可以使用显式发布（[ExplicitPublish](https://quartz.jzhao.xyz/plugins/ExplicitPublish)），这将过滤掉所有没有在 frontmatter 前添加 `publish: true` 的笔记，方法为将``quartz.config.ts`中默认的`filters: [Plugin.RemoveDrafts()]`更改为`filters: [Plugin.ExplicitPublish()]`
- **指定规则批量过滤**：编辑`quartz.config.ts`的'ignorePatterns'可批量忽略目录与文件，可参考[Private Pages](https://quartz.jzhao.xyz/features/private-pages#ignorepatterns)
## 🌐 本地预览
- 安装完成后可运行本地部署，用浏览器查看页面效果： http://localhost:8080/
- 详细参考[Building your Quartz](https://quartz.jzhao.xyz/build)
```bash
# 默认部署content目录的全部内容
npx quartz build --serve

# 部署特定目录的内容如Public子目录
npx quartz build --directory=content/Public --serve
```
# 03 Github连接与推送
## ⚙️ 设置过滤规则
- 编辑 quartz安装目录下的`.gitignore`，加入需要过滤的文件规则，如果content目录中没有任何私有内容需要过滤可以略过此步骤
- 以过滤除 `content/Public`目录以外所有文件为例
```bash
# 忽略 content 目录下的所有内容
content/**

# 不忽略 content/Public 目录
!content/Public/

# 不忽略 content/Public 目录下的所有内容
!content/Public/**
```
## 🔗连接到个人Github
- 登录[Github](https://github.com/new)并创建一个空的仓库，注意保持默认选项，不要初始化`README`, license, 或者 `gitignor`
  ![[Pasted image 20250510112513.png]]
- 复制该仓库的url `REMOTE-URL`
- 参考文档： [Setting up your GitHub repository](https://quartz.jzhao.xyz/setting-up-your-GitHub-repository) 
```bash
# 在本地的quartz目录运行命令行将其设置为remote仓库的地址
git remote set-url origin REMOTE-URL
```

## ⤴ 推送本地源码到Github
- 每次有新内容需要发布时仅需要进行常规推送即可
```bash
# 首次推送
npx quartz sync --no-pull

# 常规推送
npx quartz sync
```

# 04 Cloudflare Pages自动部署
## ➡ 连接Github
- 注册登录 Cloudflare Pages 后在 **Workers & Pages** > **Create application** > **Pages** 导入上一步创建的 Github仓库
![[Pasted image 20250506170207.png]]
![[Pasted image 20250506170328.png]]
## 🌏 设置自动部署
- Project name 会影响最终获取的免费域名
- Build command 需要根据自己的需要填写，
	- 发布所有 content目录中的内容：`npx quartz build` 
	- 仅发布 `content/Public`：npx quartz build --directory=content/Public

| Configuration option   | Value                                         |
| ---------------------- | --------------------------------------------- |
| Production branch      | `v4`                                          |
| Framework preset       | `None`                                        |
| Build command          | `npx quartz build --directory=content/Public` |
| Build output directory | `public`                                      |
![[Pasted image 20250510114837.png]]
- 一分钟左右部署成功 ，此时访问Cloudflare Pages提供的网址即可，如果提示部署成功但是网址访问报错稍微等一会就OK了 
- 设置完成后，每次从本地推送源码到githbu都会自动部署实现页面的更新
- 参考文档 [Hosting](https://quartz.jzhao.xyz/hosting)

---
# Todo
- 进阶使用
	- 自定义域名
	- 统计
	- 评论
- 考虑做一个简单的插件来管理设置、一键发布