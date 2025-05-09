---
permalink: Complete Guide to Building a Free Blog with Obsidian Quartz GitHub and Cloudflare Pages 02
tags:
  - Projects/O记实录
  - "#Category/handbook"
  - "#Topic/obsidian"
draft: true
---

上一篇： [[使用Obsidian+Quartz+github+Cloudflare Pages搭建免费博客完整指南]]
# ✅ 02 安装启用
### 安装Quartz 4
- 整个安装过程基本参考官方的最新文档（[Welcome to Quartz 4](https://quartz.jzhao.xyz/)）的默认设置进行即可
```bash
# win10建议使用cmd运行命令行，减少出错几率
git clone https://github.com/jackyzha0/quartz.git
cd quartz
npm i
npx quartz create #注意该流程可能需要一些时间完成，不要重复运行

# 使用默认设置即可，以下提示说明已经安装成功
—  You're all set! Not sure what to do next? Try:
  • Customizing Quartz a bit more by editing `quartz.config.ts`
  • Running `npx quartz build --serve` to preview your Quartz locally
  • Hosting your Quartz online (see: https://quartz.jzhao.xyz/hosting)
```
- 安装完成后Quartz将会自动创建一系列目录与文件，其中`content/`默认作为需要发布的笔记目录，在不进行任何特殊设置的前提下，该目录下所有的文件和目录都会作为页面进行发布， `content/index.md`的内容将作为首页
### 测试本地部署
- 安装完成后可运行本地部署，用浏览器查看页面效果： http://localhost:8080/
- 详细参考[Building your Quartz](https://quartz.jzhao.xyz/build)
```bash
npx quartz build --serve
```
### 自定义设置
#### 站点
- 参考 [Configuration](https://quartz.jzhao.xyz/configuration)
- `quartz.config.ts`
```ts
pageTitle: "Quartz 4", 
baseUrl: "quartz.jzhao.xyz",
```

#### 笔记
- 修改笔记属性，可参考 [Authoring Content](https://quartz.jzhao.xyz/authoring-content)
- 可以使用 `permalink`固定url，即使文件路径更改，该 URL 也将保持不变。
	- permalink 填写字符而非完整url，并不会改变内部链接的模式，例如`https://iotao.iwheel.lol/obsidian-publish01` 会实际跳转到最新对应的文件名url `https://iotao.iwheel.lol/%E4%BD%BF%E7%94%A8Obsidian+Quartz+github+Cloudflare-Pages%E6%90%AD%E5%BB%BA%E5%85%8D%E8%B4%B9%E5%8D%9A%E5%AE%A2%E5%AE%8C%E6%95%B4%E6%8C%87%E5%8D%9701`
![[Pasted image 20250506172429.png]]
- 隐私控制
	- [Private Pages](https://quartz.jzhao.xyz/features/private-pages#ignorepatterns
	- 使用`quartz.config.ts`的'ignorePatterns'批量忽略目录与文件
	- 使用文件属性 `draft` 控制单个文件，或者可以使用显式发布（ExplicitPublish），这将过滤掉所有没有在 frontmatter 前添加 `publish: true` 的笔记。
## 托管到Github

- 按照 [Setting up your GitHub repository](https://quartz.jzhao.xyz/setting-up-your-GitHub-repository) 创建一个空的仓库
- 复制该仓库的url `https://github.com/iotao-dev/my-quartz-2025.git`
```bash
# 在本地的quartz目录运行命令行将其设置为remote仓库的地址
git remote set-url origin https://github.com/iotao-dev/my-quartz-2025.git

# 首次推送
npx quartz sync --no-pull

# 后续推送
npx quartz sync
```

## 托管到 Cloudflare Pages
### 基础设置
- [Hosting](https://quartz.jzhao.xyz/hosting)
- 设置 workers & Pages
![[Pasted image 20250506170207.png]]

![[Pasted image 20250506170328.png]]
![[Pasted image 20250506170517.png]]

![[Pasted image 20250506170736.png]]

- 一分钟左右部署成功 ，此时访问网站 [Welcome to Quartz](https://iotao2025.pages.dev/) 如果报错稍微等一会就OK了
![[Pasted image 20250506170924.png]]
- 修改部署配置
![[Pasted image 20250507111049.png]]

### 自定义域名
- 启用自定义域名后可以直接查看到cf内置的统计，无需添加其他统计代码
- 如果域名已经托管到cf，直接填写即可，cf将自动完成所需的域名配置，等待状态为 Active即可生效
![[Pasted image 20250506180337.png]]