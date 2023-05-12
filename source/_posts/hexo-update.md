---
title: 记一次升级Hexo博客的过程
date: 2023-05-12
updated: 2023-05-12
---
## 记一次升级Hexo博客的过程

### 前言

> 毕业后这几年一直比较佛系，但是最近突然感觉自己不能再这样下去了，所以我就在notion上面通过database建立了一个属于自己的计划池，把之前一直想做但是都被自己拖延的计划一个一个扔进池子里(类似需求池那种感觉吧)，并且会给这个计划打上一些标签，预估周期之类的。然后呢，等自己有时间的时候，一个一个完成。
> 

类似这样：

![Untitled](Untitled.png)

### 开始

首先自然是拉项目了，我的博客是部署在Github Page上的，整体架构大概就是hexo+next主题，然后加上之前搭建的isso评论系统和集成了gitter聊天室，还有之前写了一个github 发布的workflow

目录树是这样的：

```jsx
├── _config.landscape.yml
├── _config.next.yml
├── _config.yml
├── db.json
├── package-lock.json
├── package.json
├── scaffolds
│   ├── draft.md
│   ├── page.md
│   └── post.md
├── source
│   └── _posts
│       └── hello-world.md
├── .github
│   └── workflows
│       └── pages.yml
└── themes
```

是的，你没有看错，自打博客搭起来后，只有一篇hell-world哈哈

### 升级过程

项目有了，首先看版本号，先去官网看一下最新的hexo版本号和next主题的最新版本号，hexo最新的是7.0.0-rc1，next主题最新的是8.16.0，看了下现在的版本，倒也没有差太多，索性就一不做二不休，直接把这些依赖通通升级到最新的版本号，然后再一个一个解决问题就行了，升级后是右图

![Untitled](Untitled1.png)

![Untitled](Untitled2.png)

然后运行`yarn` 安装依赖，果然报错了，报错如下：

![Untitled](Untitled3.png)

报错很明显了，切换个node版本就可以看，然后我用fnm包管理工具切换node版本到16.x ,然后再`yarn`安装，果然没啥问题了

![Untitled](Untitled4.png)

OK,那么万里长征已经走了一半了，接下来就是运行`yarn deploy` ，看能不能正常运行；

咦，报了两个错：

![Untitled](Untitled5.png)

![Untitled](Untitled6.png)

先看第一个，不能找到模块vue，报错文件是根目录下的scripts/index.js

我已经忘了当时为啥要加这个文件，看了一下文件内容，还是压缩后的代码，有点像elementUI的构建产物，既然想不起来为啥加了，就直接把他删了吧，然后再运行`yarn deploy` ，果然只剩下后面那个报错了；

这个报错是找不到一个文件，看了一下hexo的帮助文档，emm，没什么用；

然后我尝试将报错丢给gpt，也没有给我什么有用的信息，然后我就尝试全局搜，既然是找不到tomorrow-night.css，那肯定是哪个地方用到了，果然，在主题配置文件中找到了：

![Untitled](Untitled7.png)

看到这，我大概明白了，就是highlight库里没有这个主题，我们换一个现在有的就可以了，改成了`stackoverflow-dark` 

然后再运行`yarn deploy` ，大功告成：

![Untitled](Untitled8.png)

### 部署

因为之前我写了一个发布hexo博客的工作流，我需要把里面用到node版本升级到16，不然安装依赖时会报错，如下：

![Untitled](Untitled9.png)

然后提交代码，看看github actions是否运行成功

![Untitled](Untitled10.png)

OK，没问题，然后访问[https://stevensunny.com/](https://stevensunny.com/)，没有问题

### gitter问题

之前给博客配置过gitter，是一个聊天室，在博客右下角就可以发送消息，但是这次部署后我试了一下，现在处于不可用状态，就顺便排查了一下，首先看调试工具，发现一个sidecar的js没有请求成功，然后经过搜集资料，这个文件就是实现gitter集成在hexo的文件，但是不幸的是，这个库已经被官方存档了，也就是不维护了，之前的静态文件自然也请求不到了。

![Untitled](Untitled11.png)

然后我就换成了next支持的另外一个聊天室，chatra

![Untitled](Untitled12.png)

> 其实后来想了想，我是否可以自己把sidecar的库下载下来，然后build，把构建的产物放在自己的服务器上，然后修改next插件的源码，把文件地址替换为自己服务器上的地址，这样应该就能正常使用gitter了吧，毕竟gitter更符合对聊天室的定义(等有空了实操一下)
> 

### Isso评论

之前为什么选型用Isso搭建？在评论系统的选择中，我认为能够让读者方便留言是最重要的，因此需要科学上网的、需要注册账号的都被我排除在外，从而 Isso 成为了首要选择。试了一下，这次升级后Isso正常：

![Untitled](Untitled13.png)

### 小彩蛋

想起来之前写了一个脚本，用来自动生成hexo+next博客，并且自动push到github，自带workflow文件，需要的话可以自取，用法是：

```bash
my_scripts ${文件夹名称} ${github repo地址}
```

```bash
npm install -g hexo-cli
mkdir $1
cd $1
hexo init .
npm install
npm install hexo-theme-next
touch _config.next.yml
cp node_modules/hexo-theme-next/_config.yml _config.next.yml
nl /_config.yml | sed ‘100c theme: next’
mkdir .github/workflows
echo 'name: Pages
on:
  push:
    branches:
      - source  # default branch
jobs:
  pages:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Use Node.js 16.x
        uses: actions/setup-node@v1
        with:
          node-version: '16.x'
      - name: Cache NPM dependencies
        uses: actions/cache@v2
        with:
          path: node_modules
          key: ${{ runner.OS }}-npm-cache
          restore-keys: |
            ${{ runner.OS }}-npm-cache
      - name: Install Dependencies
        run: npm install
      - name: Build
        run: npm run build
      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
          publish_branch: master  # deploying branch' > .github/workflows/pages.yml
git init
git branch -M source
git remote add origin $2
echo -e '\nyarn.lock\npackage-lock.json' >> .gitignore
git add .
git commit -m “init”
git push -u origin source
```
