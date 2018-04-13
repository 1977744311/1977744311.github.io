---
layout: post
title: 从零搭建一个博客（jekyll和hexo任选）
categories: 博客
description: 建立一个属于自己的博客
keywords: 博客
---
# 写在开头

这篇文章主要讲如何拥有自己的域名，如何利用 jekyll 或者 hexo 轻松搭建一个属于自己的博客。
然后你可以在属于自己的网页上展示你的 github、微信、微博、QQ、Twitter 、Email 等信息，但主要是展示自己的作品。
>树立个人品牌，让名企hr主动找你
 
## 第一步：注册域名
 
注册域名你有两个选择
1. 向国内服务商购买，不过你需要备案（备案就是把你的照片、姓名、住址和手机号告诉管理机构）
2. 向国外服务商购买，不需要备案，不过需要你懂一点英文。
 
如果你选择第一种，那么可以去阿里旗下的万网注册，根据你的资金水平选择合适你的域名。
如果你选择第二种，那么推荐你去[namesilo.com](https://www.namesilo.com)注册，具体步骤如下：
 
1. 进入官网搜索一个你喜欢的域名，比如我搜索的是 stevenself，然后你就会知道哪些域名是可以买的
![serch](https://1977744311.github.io/images/posts/blog/blog1.png)
2. 接下来就是选择一个你喜欢的域名，最便宜的有一刀多美元，折合下来不到12快一年，很划算，当然国内服务器的域名可能有更划算的。
3. 选中你想要的域名，点击 REGISTER CHECKED DOMAINS 蓝色按钮。
4. 别急着结账，有一个地方可以输入优惠码（如图）去谷歌或者百度搜索「namesilo 优惠码」，填到里面，就可以优惠一美元！我搜到的优惠码是 onesaving。但是你要注意，一个用户只能使用一次优惠码，下次你再购买域名就没有优惠啦。
![](https://1977744311.github.io/images/posts/blog/blog2.png)
尽量按照图片中的Privacy Setting:选择whois privacy，可以保护你的域名不被人查到你的信息。
 
5. 用支付宝结账（一定要先输入邮箱，再选中支付宝，最后点击 go）支付成功后域名就是你的了，你可以给这个域名设置 A 记录和 CNAME。（ps：在支付前会先让你注册一个账号，除了邮箱，其他都可以瞎填）
 
至此，第一步就算完成了。
 
## 第二步：开通 GitHub Pages
* 什么是 GitHub Pages ？
GitHub Pages 是一个免费的服务器，它能给你提供一个G的空间存储放东西，然后也能请求到，所以用它来做你的博客的服务器最好不过了，而且还不用备案，在国内还拥有
不错的访问速度。
* 申请 GitHub Pages 
那么一个这么好的神器，我们怎么才能申请呢？首先你要拥有一个 GitHub 账号（没有怎么办？那就去注册一个啊=-=），在拥有账号后，点击你账号下的 New repository 
新建一个仓库，然后就会来到如下页面：
![](https://1977744311.github.io/images/posts/blog/blog3.png)
按照图示创建，然后点击绿色的创建按钮，然后你的 GitHub Pages 就自动开通了。
接下来需要将你的 GitHub Pages 目录克隆到本地，在终端打开一个安全的目录输入以下命令：

```
echo "# ithub.io" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin git@github.com:yourname/yourname.github.io.git
git push -u origin master
```
就是你建完仓库页面给你的代码复制粘贴就好了~。（如果你是第一次使用 GitHub 那么可能需要配置一下ssh，不会的可以百度关键字github ssh）

## 第三步：创建博客
### 使用jekyll搭建博客
1.fork本博客代码至你的 GitHub Pages 仓库里，地址为[代码](https://github.com/1977744311/1977744311.github.io)。
fork 方法是，点击页面上的绿色按钮 clone or download ,会出现一行代码，点击右边那个小按钮复制，然后在终端输入 git clone + 刚刚复制的地址就OK了。

2.设置域名,如果你需要绑定自己的域名，那么修改 CNAME 文件的内容；如果不需要绑定自己的域名，那么删掉 CNAME 文件。

3.修改配置，在 _config.yml 文件中把有关于个人信息的内容全部更改为自己的，例如网站的 url、title、subtitle 和第三方评论模块的配置等。

4.删除我的文章与图片。_posts、_drafts、_wiki、images 这些文件夹里除了 template.md 文件，其他都可以删除。

5.修改关于页面，将 pages/about.md 里的文件内容有关个人的信息替换成自己的。

6.至此你的博客就搭建成功了，如果需要发表博客那就在 _posts 文件下导入你的.md文件，文件格式头参考 template.md 文件（如果没有设置域名，则你的博客为yourname.github.io） 

### 使用hexo搭建博客
1.从终端进入一个安全的目录，然后敲下如下命令：
```
npm install -g hexo-cli //安装hexo
hexo init myBlog  //初始化一个hexo文件夹
cd myBlog    //打开这个文件夹
npm i        //安装依赖
hexo new 开博大吉     //新建一篇名为开博大吉的博客
```
现在你的博客就搭建好了，并且目录里有一篇名为开博大吉的博客。

2.配置 _config.yml 文件,把第 6 行的 title 改成你想要的名字,把第 9 行的 author 改成你的大名,把最后一行的 type 改成 type: git ,在最后一行后面新增一行，
左边与 type 平齐，加上一行 repo: 你的 GitHub Pages 仓库地址。

3.安装 git 部署插件,在终端输入以下命令：
`npm install hexo-deployer-git --save`

4.开启博客，在终端输入以下命令：
`hexo deploy`

5.在地址栏输入你的 GitHub Pages 仓库的预览地址，就可以看到你的博客了。

## 常见问题
1.如果你是windows操作系统，那么建议你用jekyll搭建博客，因为在windows下，hexo很容易安装不成功。
2.如果你不喜欢我的博客的主题，那么可以去 [Jekyll Themes](http://jekyllthemes.org) 上找寻自己喜欢的主题，然后用相同的方法fork就可以了。
3.如果你不喜欢hexo初始的主题，也可以去 [Hexo Themes](https://hexo.io/themes/) 上上找寻自己喜欢的主题。
4.安装的过程中需要会一点点命令行，如果中间有哪点不懂可以百度或谷歌，也可以联系我的邮箱stevenself.top@gmail.com.
5.如果你觉得本篇博客不错，欢迎转载，注明来源就可以了。


 
 
 
 