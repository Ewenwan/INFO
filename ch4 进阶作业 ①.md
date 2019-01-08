# 培训经理

作者：周平



## 目录

1. 摘要

2. 背景

3. 分析

   3.1 思路

   3.2 步骤

4. 结论

5. 讨论

   5.1 对结论的讨论

   5.2 可优化的环节

   5.3 收获

   5.4 感想

6. 参考文献



## 1. 摘要









## 2. 背景

> 请挑选心仪的 1~2 种职业，如算法工程师、产品经理等，使用至少 2 个实践策略，形成对该职业的全局认识（需包含薪酬数据）。
>
> Ps. 可回顾借鉴前述公司分析、产品分析的技巧。

选择一个不了解的职业：培训经理，作为练习对象。了解职业概况、任职资格以及薪酬。重点练习爬虫工具的使用。



## 3. 分析

### 3.1 思路

- 通过WebScraper抓取薪酬数据。
  - 按照视频教程，抓取拉勾网数据。
  - 抓取猎聘网数据，刻意练习工具使用，同时做到薪酬数据的交叉验证。
- 收集职业概况及任职资格
  - 利用ONet了解。
  - 仿照ONet模型，通过WebScraper抓取招聘网站内容验证。



### 3.2 步骤

#### 薪酬分析

首先按照视频教程，顺利从拉勾网抓取到数据，并进行了表格整理，以待分析之用。

接着模仿教程，对猎聘网进行数据抓取。

在网站首页检索 `培训经理` ，转到网址：https://www.liepin.com/zhaopin/?sfrom=click-pc_homepage-centre_searchbox-search_new&d_sfrom=search_fp&key=%E5%9F%B9%E8%AE%AD%E7%BB%8F%E7%90%86 设置URL及点击下一页。

![image.png](https://upload-images.jianshu.io/upload_images/15321531-55e9993f4a0e6d84.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](https://upload-images.jianshu.io/upload_images/15321531-bfa77739c7a153ad.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这里遇到问题，测试发现，不会像拉勾网那样一页页往下翻。每点一下测试，只翻一页。尝试调整区块的选择，点击类型，点击元素唯一性等各项设置，均无法解决。

那么会不会是起始页面的URL有问题呢？

我发现，从第二页开始，链接变为：https://www.liepin.com/zhaopin/?init=-1&headckid=ad0a59911872177b&fromSearchBtn=2&ckid=ad0a59911872177b&degradeFlag=0&sfrom=click-pc_homepage-centre_searchbox-search_new&key=%E5%9F%B9%E8%AE%AD%E7%BB%8F%E7%90%86&siTag=8-J4ClLmqPYpm8lWX7cAGA~fA9rXquZc5IkJpXC-Ycixw&d_sfrom=search_fp&d_ckId=aed3b74f16eab44b13f5ffb792ce2e4a&d_curPage=0&d_pageSize=40&d_headId=aed3b74f16eab44b13f5ffb792ce2e4a&curPage=1 这与刚才的起始URL完全不同。

点击回到第一页，链接变为：https://www.liepin.com/zhaopin/?init=-1&headckid=ad0a59911872177b&fromSearchBtn=2&ckid=ad0a59911872177b&degradeFlag=0&sfrom=click-pc_homepage-centre_searchbox-search_new&key=%E5%9F%B9%E8%AE%AD%E7%BB%8F%E7%90%86&siTag=8-J4ClLmqPYpm8lWX7cAGA~fA9rXquZc5IkJpXC-Ycixw&d_sfrom=search_fp&d_ckId=aed3b74f16eab44b13f5ffb792ce2e4a&d_curPage=1&d_pageSize=40&d_headId=aed3b74f16eab44b13f5ffb792ce2e4a&curPage=0 

通过几次测试，发现只要在首页搜索，就会出现最初那种URL。通过点击页数1回到第一页，内容是一样的，但是URL会变成上述更有规律的形式 `……Page=n(n=0为第1页，n=99为第100页)` 。修改sitemap设置：

![image.png](https://upload-images.jianshu.io/upload_images/15321531-c625cce766d572bb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

问题没有解决，每次只翻一页。

与教程中的拉勾网URL：https://www.lagou.com/jobs/list_%E5%9F%B9%E8%AE%AD%E7%BB%8F%E7%90%86?city=%E5%85%A8%E5%9B%BD&cl=false&fromSearch=true&labelWords=&suginput= 进行对比。拉勾网点击页数，URL不会改变，而猎聘的URL呈现0-99的动态变化。翻看官网文档，在创建站点中发现如下说明：

![image.png](https://upload-images.jianshu.io/upload_images/15321531-708a659a6bfdda2a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

因此将URL改为：https://www.liepin.com/zhaopin/?init=-1&headckid=ad0a59911872177b&fromSearchBtn=2&ckid=ad0a59911872177b&degradeFlag=0&sfrom=click-pc_homepage-centre_searchbox-search_new&key=%E5%9F%B9%E8%AE%AD%E7%BB%8F%E7%90%86&siTag=8-J4ClLmqPYpm8lWX7cAGA~fA9rXquZc5IkJpXC-Ycixw&d_sfrom=search_fp&d_ckId=aed3b74f16eab44b13f5ffb792ce2e4a&d_curPage=1&d_pageSize=40&d_headId=aed3b74f16eab44b13f5ffb792ce2e4a&curPage=**[0-99]** ，继续测试，问题仍旧没有解决。

百度搜索 `WebScraper` ，通过简书的一篇文章，找到 [webscraper中文网](http://www.iwebscraper.com/) 网站。在模板中搜索 `猎聘` 得到[抓取猎聘网TOP500强企业招聘岗位信息](http://www.iwebscraper.com/top500-lp/) ，复制Sitemap，通过选项 `Import Sitemap` 导入到WebScraper。

![image.png](https://upload-images.jianshu.io/upload_images/15321531-55a3de81e09c300c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

通过分析发现：

- 起始URL的处理是正确的，类似这种每页不同编号的网站，需要用[0-99]（具体视网站而定）来表示多个页面范围。
- 第二层级直接是页面 `Element` 而不是 `Element click` ，这就是我之前一直出错的原因。我的理解是，网址URL不随页数变化的，需要通过点击来翻页。网址URL跟随页数变化的，这一步就不需要了，因为本身就设置了页面范围。

正确设置后，成功抓取了100页的数据。





#### 职业概况及任职资格

通过在ONet检索 `training manager` ，得到类似职业 [11-3131.00 - Training and Development Managers](https://www.onetonline.org/link/summary/11-3131.00) 。












## 4. 结论





## 5. 讨论

### 5.1 对结论的讨论





### 5.2 可优化的环节





### 5.3 收获

- 使用 `WebScraper` 有问题的，可以参考网站 [webscraper中文网](http://www.iwebscraper.com/) ，通过分析，模仿里面的模板，快速找到设置中的问题。





### 5.4 感想





## 6. 参考文献

- 



## ChangeLog

- 190105    完成初稿，通过爬虫工具抓取拉勾和猎聘的薪酬数据