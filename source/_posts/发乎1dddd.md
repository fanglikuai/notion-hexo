---
categories: 博客折腾手册
tags:
  - elog
  - notion
  - hexo
  - blog
description: ''
permalink: notion-hexo/
title: 发乎1dddd
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/dc3ab2f7-66cb-4a5c-91bd-0c775b8e2aa5/%E5%B9%BD%E7%81%B5%E5%85%AC%E4%B8%BB.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UH5PXP33%2F20250914%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250914T062337Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD8cyf9wIBUg3ovkLZgw7T7wYjVIYh98B5VmpgD5BZNugIgYQ9fH9EJKqcAaK4hRmgST2LcODS4Oq1oWCpYO0kidFsq%2FwMIVxAAGgw2Mzc0MjMxODM4MDUiDB33Rw%2F3JGugPUy6HircA2awh12KBMTTCaWQfgWKF8mzNxJ5BE4pN8PEtAUEd1PZJtBQEqSvfSwgyjk1GZSsSBl5LutVFA%2BQVf7bjBc055afV6E%2BpF7hWLAbD3yw6QEm%2BdloZbUmEd0nlyvDP%2Fo5VRSsmCFagqwLa7qc8Djo%2FkbAsU76BMLfejzfDNXknPaokBUu3qze36oN6HlgtsXSoGaaW9q8JQ3a7fTqyRZTcr6m2VrcXd%2F8G2oqLzZl33S0T001QU4MOplIqRP51GpBWwR%2B1CCTmIs0JvvKwr%2BjHP%2FltoQXjWxYtse41f1GVBJnQafuSUx8EoYSIphngbs17zyvVuLr1HVTlHCP6mktM4DwcJsLDdKPqwm3OyWJVOB7H%2F2psh%2B1fWjILwd4cT%2BciOAKtCe9oDnsH1MZA3IPhbm0sF4raHKX%2F1BZG7ReKoy%2B7ZfiRUUw5SBY6SWFAQDAA4A8Or%2FxgxfQOju4T%2BWguHVofdh2SBs%2B50lqcBPyOG%2BPRW9%2B5ZA2Ed4jcUx1lQIpUHwnD9FgQOgeFXY%2BqT5J3GaA8WsrkA166PotjZTvXCXUBnSfoqlHdT4ATXQ8QlsnbTnk4q8OFI5XBptK4sZEzuZh26MFyBGIJSqy9wW2gIlEnYiUmqgiNuL%2FWBeUMPqxmcYGOqUB%2FvC7E0POpZvWTTmSUzC4qoveefVftyoXXs8Mq5Lhvni25sVDdYpNCp0rQOD9vgra575KW6uLzSi1pcuXdxLyr5WGAJZ0ggrIJpj1faWk0JEObMWXZxXwNYGsgd3Uf6c5cddRusNIkPCjkKYNgFhz2rHgVNF8W4krBUhaJz6Mqq0RhKWpCKnOutrsk3%2Fb1S%2B6o%2Bin6ua03uyhrT4R3MwXaWmn40iF&X-Amz-Signature=cb6c9a3ab3679aa48007300fc3d24760c8d75f1bc732120caf17e9216985d664&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
date: '2025-09-14 13:19:00'
updated: '2025-09-14 14:07:00'
index_img: /images/1012f18b825b2259966e64985bba973d.jpg
---

我要发布了


# 博客工具

- 写作平台：Notion
- 博客平台：[Hexo](https://hexo.io/)
- 博客主题：[Butterfly@4.10.0](https://github.com/jerryc127/hexo-theme-butterfly)
- 博客文档同步：[Elog](https://github.com/LetTTGACO/elog)
- 部署平台：Vercel
- 博客仓库：[https://github.com/LetTTGACO/notion-hexo](https://github.com/LetTTGACO/notion-hexo)

# 博客搭建指南


## Fork模板仓库


[点击 Fork](https://github.com/elog-x/notion-hexo/fork) 该模板仓库到个人 Github 账号仓库下并 clone 到本地


## 安装依赖


在项目根目录下运行命令安装依赖


```shell
npm install
```


## 新建 Elog 本地调试文件


在项目根目录中复制`.elog.example.env`文件并改名为`.elog.env`，此文件将用于本地同步Notion 文档


## 配置 Notion 关键信息


按照[文档提示](https://elog.1874.cool/notion/gvnxobqogetukays#notion)配置 Notion 并获取 `token` 和 `databaseId`，在本地`.elog.env`中写入


```plain text
NOTION_TOKEN=获取的token
NOTION_DATABASE_ID=获取的databaseId
```


## 本地调试


在项目根目录运行同步命令


```shell
npm run sync:local
```


## 启动 Hexo


在项目根目录运行hexo启动命令，会自动打开本地博客


```shell
npm run server
```


## 配置 Hexo 博客


根据 [Hexo](https://hexo.io/) 文档和 [Butterfly](https://github.com/jerryc127/hexo-theme-butterfly) 主题配置文档，配置你的博客直到你满意为主，你也可以换别的主题，这里不做演示


## 提交代码到 github


本地访问没问题直接提交所有文件到 Github 仓库即可


## 部署到 Vercel


注册 Vercel 账号并绑定 Github，在 Vercel 导入 该项目，Vercel 会自动识别出该 Hexo 项目，不需要改动，直接选择 Deploy 部署。部署完成会有一个 Vercel 临时域名，你也可以绑定自己的域名。


![Untitled.png](/images/dfc25e6748972e167c29fa32e8ddee12.png)


![Untitled.png](/images/48eaba26bc1b9ce5d86e43ee453281dc.png)


## 配置 Github Actions 权限


在 Github 仓库的设置中找到 `Actions-General`，打开流水线写入权限`Workflow permissions`


![Untitled.png](/images/acb4690046201ea61666691cfdaec570.png)


## 配置环境变量


在本地运行时，用的是`.elog.env`文件中定义的 Notion 账号信息，而在 Github Actions 时，需要提前配置环境变量。


在 Github 仓库的设置中找到 `Secrets  and variables`，新增仓库的环境变量`NOTION_DATABASE_ID`和`NOTION_TOKEN`和`.elog.env`保持一致即可


![Untitled.png](/images/ed7cbd2f6bd6d65c59c20d865236386d.png)


## 自动化部署


当在 Notion 中改动文档后，手动/自动触发 Github Actions流水线，会重新从 Notion 增量拉取文档，自动提交代码到 Github 仓库。


Vercel 会实时监测仓库代码，当有新的提交时都会重新部署博客。如此就实现了自动化部署博客。


整个流程的关键点就在于：如何手动/自动触发 Github Actions


在项目.`github/workflows/sync.yaml`中已经配置了外部 API 触发 Github Actions 事件，所以只需要调用 API 触发流水线即可。


### 手动触发


为了方便，这里提供一个部署在 Vercel 的免费公用的[**ServerlessAPI**](https://github.com/elog-x/serverless-api)，只需要配置好 URL 参数并浏览器访问即可触发流水线


```shell
https://serverless-api-elog.vercel.app/api/github?user=xxx&repo=xxx&event_type=deploy&token=xxx
```


### 自动触发


可在 Notion 中结合 Slack 触发，[参考教程](https://elog.1874.cool/notion/vy55q9xwlqlsfrvk)，这里就不做进一步演示了


# 自定义 Elog 配置


如果想自定义 Elog 配置，可访问 [Elog 文档](https://elog.1874.cool/)


# 博客示例


示例仓库：[https://github.com/LetTTGACO/notion-hexo](https://github.com/LetTTGACO/notion-hexo)


博客示例地址：[https://notion-hexo.vercel.app](https://notion-hexo.vercel.app/)

