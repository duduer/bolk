
---
title: 博客搭建说明
tags:
 - about
archives: /archives
---

**博客使用基于node.js的[Hexo](https://hexo.io/)和zhwangart的[theme](https://zhwangart.github.io/)，以及[github.io](https://github.com/)和[阿里云](https://cn.aliyun.com/)域名管理搭建完成。**
<!-- more -->
## The First Step

- 拥有Github账号
- 本地下载Hexo
- Hexo配置关联Github(yourname.github.io)
	```
	deploy:
  	  type: git
      repository: git@github.com:duduer/duduer.github.io.git
      branch: master
	```

## The Second Step

- Hexo找个[主题](https://hexo.io/themes/)
- Hexo同步环境文件到Github分支
- Hexo插件安装

## The Last Step

- 阿里云免费域名(不知道现在还有没有)
- 配置域名解析到Github

## hexo相关操作记录

#### hexo基本操作

- hexo目录结构如下：
	*source：存放元数据，以及通过以下命令*
		hexo new page
	*创建的目录，以及需要在对应的 themes 主题下修改对应的_config.yml*
		menu:
  		#Home
  		  首页: /
  		# Archives
  		  读书: /archives
  *public: 发布文件夹，存放发布到Github的静态资源文件路径，使用如下命令操作*
  		hexo g #生成静态文件
  		hexo d #发布到github上
  		hexo s #启动本地服务器（http://localhost:4000/）
      hexo clean #清理生成的静态文件
  *categories: 分类文件夹，需要支持分类的话，需要在该目录下新建目录，且在_config.yml添加*
      category_map:
        代码: code
        读书: book
  *themes目录下的languages目录中包含zh-CN.yml，可以将英文title转换中文，且需要在全局配置中添加 language: zh-CN*
      categories: 分类
      code: 代码
      book: 读书
      search: 搜索
- hexo创建page和文章：
  *创建page，page有默认格式支持，但是一个标签只有一个页面*
      hexo new page xx
  *page是否支持分类，需要主题支持，如果不支持，则只能使用categories，默认支持分类，需要修改themes下config.yml*
      menu:
        读书: /categories/book
  *创建文章和草稿*
      hexo new xx
      hexo new draft xx #创建草稿
      hexo publish [layout] <filename> #发布成文章
  *文章参数*
        title: shell #标题
        date: 2019-01-21 23:35:09
        categories: 代码 #分类，会和category_map中的映射对应。
        tags: 
          - shell #标签，可以支持多个
          - bash
        archives: /archives #归档路径

### 如何在Github分支上保存hexo其他资源，进行备份

- Github创建分支
- hexo其他需要同步的静态资源
- 维护更新(注意事项)
