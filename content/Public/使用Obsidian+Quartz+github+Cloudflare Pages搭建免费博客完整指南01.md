---
permalink: obsidian-publish01
tags:
  - Projects/O记实录
  - "#Category/handbook"
  - "#Topic/obsidian"
draft:
---

# 01 方案说明
##  🗺 方案背景与适用人群 
捣鼓这个方案主要出于个人需要，作为长期all in one模式使用obsidian进行日常记录的非技术背景用户，一直试图寻找符合以下需求的对外发布方案
* 保持 Obsidian 的本地使用习惯，不需要迁移或者分割已有的内容结构
* 兼容Obsdian的核心功能如markdown语法、双链、Graph View、tag、Mermaid图表、 Frontmatter属性、搜索等
* 部署在自己控制的平台上，而不是受制于单一平台
* 具备长期可维护性，但不希望技术复杂性过高，最好是启用之后只管写作与发布就行
* 对于有助于正反馈的功能有较好支持（如统计与评论），自定义外观或其他高级功能具备可行性但不是必须的任务
* 能够安全地隔离「私密笔记」与「公开内容」
* 当然还有最重要的：完全免费

最终确定的方案如下，其中每一个环节都有可替代项，可根据需要进行选择：
* 本地使用Obsidian全量记录
	* 自定义一个子目录如Public放置公共发布内容 （只有该目录的内容被后续环节使用）
	* 使用官方sync或者其他同步服务实现多终端同步，即并不使用git对全量库进行托管
* Quartz 构建静态页面
* Github托管
* Cloudflare Pages自动发布

## 🧰 工具清单

| 工具               | 用途               | 选择理由                                         | 可替代项             | 免费  |
| ---------------- | ---------------- | -------------------------------------------- | ---------------- | --- |
| Obsidian         | 本地笔记写作与管理        | Markdown 支持好，插件生态活跃，可控性高                     | Logseq           | ✅   |
| Quartz 4         | 将 Markdown 构建为网站 | 高度兼容obsidian 、美观简洁、文档清晰上手门槛相对低、可自定义性高、开源社区活跃 | Hugo / Jekyll    | ✅   |
| Git + GitHub     | 版本控制 + 源码托管      | 与 Cloudflare Pages 原生集成                      | GitLab           | ✅   |
| Cloudflare Pages | 免费静态网站部署平台       | 快速、稳定、支持自定义域名与自动部署、自定义域名自带免费的基础统计            | Netlify / Vercel | ✅   |

## 📦 目录结构

```
quartz/                    # Quartz的安装目录
├── content/               # 所有 Obsidian 内容（可直接设置为 Vault）
│   ├── Public/            # ✅ 发布目录，仅此被 Git 跟踪和部署
│   ├── <其他目录与md文件>   # 私有笔记，保留本地
│   └── ...             
├── .gitignore             # 控制仅提交 Public/ 的内容到github
├── ...                    # Quartz安装后自动生成的其他文件与目录
```

<未完待续 : 02 Quartz安装与基本设置>
