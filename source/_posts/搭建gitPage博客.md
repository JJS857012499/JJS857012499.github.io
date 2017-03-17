---
title: 搭建gitPage博客
date: 2017-03-08 10:26:31
categories: hexo #文章文类
tags: [hexo,github] #文章标签，多于一项时用这种格式
---
在这里我把我用hexo搭建gitPage博客记录下来，同时也给我一个同学参考参考哈~~~
## 环境准备
需要[git](https://git-scm.com/),[Node.js](https://nodejs.org/en/)环境
## 安装hexo
利用 npm 命令即可安装。（在任意位置点击鼠标右键，选择Git bash）
```shell
npm install -g hexo
```
## 创建hexo文件夹
选择存放hexo文件的位置,执行以下指令(Git bash终端下)，Hexo即会自动在目标文件夹建立网站所需要的所有文件。
```shell
hexo init
```
## 安装依赖包
```shell
npm install
```
## 本地查看
现在我们已经搭建起本地的hexo博客了，执行以下命令(在hexo文件下)，然后到浏览器输入localhost:4000看看。
```shell
hexo generate #此命令是生成静态页面，不执行该命令也可以
hexo server
```
到此，本地服务以及搭建好了。

## 打包上传到github
### 如果没有github账户，则需要注册
### 创建仓库，配置ssh秘钥
*注意*：Repository name命名规则：你的github账号.github.io (这个一定要这么命名，具体我也不清楚)
## hexo使用
目录结构
.
├── .deploy #需要部署的文件
├── node_modules #Hexo插件
├── public #生成的静态网页文件
├── scaffolds #模板
├── source #博客正文和其他源文件，404、favicon、CNAME 都应该放在这里
| ├── _drafts #草稿
| └── _posts #文章,可以用子文件来存放文章
├── themes #主题
├── _config.yml #全局配置文件
└── package.json

## 全局配置 _config.yml
```shell
# Hexo Configuration
## Docs: https://hexo.io/docs/configuration.html
## Source: https://github.com/hexojs/hexo/

# Site
title: 深水暗流
subtitle: 冰是睡着的水
description: 一个用gitpage来当笔记的家伙
author: JJS
email: 857012499@qq.com
language: zh-Hans
timezone:

# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: http://jjs857012499.github.io
root: /
permalink: :year/:month/:day/:title/
permalink_defaults:

# Directory
source_dir: source
public_dir: public
tag_dir: tags
archive_dir: archives
category_dir: categories
code_dir: downloads/code
i18n_dir: :lang
skip_render:

# Writing
new_post_name: :title.md # File name of new posts
default_layout: post
titlecase: false # Transform title into titlecase
external_link: true # Open external links in new tab
filename_case: 0
render_drafts: false
post_asset_folder: true
relative_link: false
future: true
highlight:
  enable: true
  line_number: true
  auto_detect: false
  tab_replace:

# Category & Tag
default_category: uncategorized
category_map:
tag_map:

# Date / Time format
## Hexo uses Moment.js to parse and display date
## You can customize the date format as defined in
## http://momentjs.com/docs/#/displaying/format/
date_format: YYYY-MM-DD
time_format: HH:mm:ss

# Pagination
## Set per_page to 0 to disable pagination
per_page: 10
pagination_dir: page

# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: hexo-theme-next #（默认只有一套主题），需要把下载的主题放在theme文件下

# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: git@github.com:JJS857012499/JJS857012499.github.io.git
  branch: master

#添加搜索
algolia:
  applicationID: 'OAK683V6NK'
  appId: 'OAK683V6NK'
  apiKey: '28f4a0e09972bf5b783013b7410faebe'
  adminApiKey: 'c713030255b892af9297f242c0b6c0f3'
  indexName: 'jjs857012499.github.io'
  chunkSize: 5000
  fields:
    - title
    - slug
    - path
    - content:strip
```
*注意*：
- 配置文件的冒号“:”后面有一个空格
- repo: 刚刚github创库地址.git
## hexo命令行使用
```shell
hexo help #查看帮助
hexo init #初始化一个目录
hexo new "postName" #新建文章
hexo new page "pageName" #新建页面
hexo generate #生成网页，可以在 public 目录查看整个网站的文件
hexo server #本地预览，'Ctrl+C'关闭
hexo deploy #部署.deploy目录
hexo clean #清除缓存，**强烈建议每次执行命令前先清理缓存，每次部署前先删除 .deploy 文件夹**
```
简写
```shell
hexo n == hexo new
hexo g == hexo generate
hexo s == hexo server
hexo d == hexo deploy
```
## 编辑文章
### 新建文章
```shell
hexo new "标题"
```
在 _posts 目录下会生成文件 标题.md
```shell
title: Hello World
date: 2015-07-30 07:56:29 #发表日期，一般不改动
categories: hexo #文章文类
tags: [hexo,github] #文章标签，多于一项时用这种格式
---
正文，使用Markdown语法书写
编辑完后保存，hexo server 预览
```
### hexo部署
执行下列指令即可完成部署。
```shell
hexo generate
hexo deploy
```
hexo deploy问题：Deployer not found: git
执行
```shell
npm install hexo-deployer-git --save
```
重新deploy即可
## 图片
我这里是使用本地的图片
### 安装
```shell
npm install hexo-asset-image --save
```
1. 安装该插件后，每次hexo new 新建博文后，会在该文件同级目录下生成一个和文件同名的文件夹，该文件夹就是用来存放图片的
2. 确保你的_config.yml 配置 post_asset_folder: true
3. 然后使用
```shell
![logo](logo.jpg)
```
在博文中插入logo.jpg.

来源：
[http://wuxiaolong.me/2015/07/31/build-blog-by-hexo/](http://wuxiaolong.me/2015/07/31/build-blog-by-hexo/)
[http://www.tuicool.com/articles/umEBVfI](http://www.tuicool.com/articles/umEBVfI)