# 来之不易的blog

标签（空格分隔）：firstone 

---
   第一次自己建立了一个blog，实属不易，写写自己创建blog的方法，免得以后想要创建blog的小白走弯路（此攻略仅针对小白，大神随意）
   



 **- 购买域名及域名解析**




  *一、去GoDaddy购买域名*
  我是在[GoDady][1]上购买的这个blog的域名，购买的时候有优惠码，可以Google一下，有不同的优惠政策，自己挑吧。![][2]
  *二、去GoDaddy修改域名*
  1、点击你的账户，管理我的域名。
  ![][3]
  2、点击管理DNS。
  ![][4]
  3、将 GoDaddy 的 Nameservers 更改成 f1g1ns1.dnspod.net 和 f1g1ns2.dnspod.net
  ![][5]
   4、然后去[DNSPod][6]上进行域名解析，注意域名解析时填表如下：
   
   
|主机记录|记录类型|线路类型|记录值|TTL|
| -------| -----  |--------|------|---|
|*       |A|默认|192.30.252.153|600|
|@       |A|默认|192.30.252.154|600|
|@       |NS|默认|f1g1ns1.dnspod.net.|86400|
|@       |NS|默认|f1g1ns2.dnspod.net.|86400|
|WWW     |CNAME|默认|username.github.io.|600|

这个*username*是你fork完我的库后要改动的，要注意。
**- 操作GitHub**



*一、注册一个GitHub账号*
  点击[Github][7]按照步骤注册账号。
*二、fork我的库*
  点击[kenramzes666.github.io][8]把我的库fork到自己的账号里去。
*三、修改*
  1、点击setting将名称改为"username.github.io"(username是你的Github的名字)
  2、将库中的CNAME文件的内容改成你刚刚购买的域名
  3、打开_config.yml和about.md修改成你的个人信息
  4、将刚刚表格
  
  |WWW     |CNAME|默认|username.github.io.|600|
  |-------|---|---|---|---|
 
 中的username.github.io.改成这个库的名字。
 
 
 
 **到这里一个简易的blog就搭建好了**
 我这里还有很多模板你可用[好的blog模板][9]
 文章修改在./test/_POST/内用MarkDown编辑器就能够完成博文的书写和上传了
 MarkDown编辑器我使用的是[Cmd MarkDown编辑阅读器][10]
 
 **参考资料**
1 | [一步步在GitHub上创建博客主页 全系列][11]| by pchou

2 | [搭建一个免费的，无限流量的Blog----github Pages和Jekyll入门][12] by 阮一峰

3 | [如何搭建个人博客][13] 知乎问题

4 | [折腾了个Pacman主题][14] by yangjian

5 | [hexo你的博客][15] by ibruce

6 | [git/github初级运用自如][16] by 虫师

7 | [Google][17] 自学成才
 



                                                                              2017.12.10  2：00
                                                                              作者    肖俊瑞
                                                                              by KenRamzes666

  [1]: https://sg.godaddy.com/zh/offers/domains?isc=gennbacn07&countryview=1&currencytype=CNY&mkwid=1jXK3KLS5_pcrid_18782177220_pdv_c_
  [2]: http://openmindclub.qiniudn.com/omt/BuildBlog02.jpg
  [3]: http://openmindclub.qiniudn.com/omt/BuildBlog016.jpg
  [4]: http://openmindclub.qiniudn.com/omt/BuildBlog017.jpg
  [5]: http://openmindclub.qiniudn.com/omt/BuildBlog018.jpg
  [6]: https://www.dnspod.cn/
  [7]: https://github.com/
  [8]: https://github.com/KenRamzes/KenRamzes.github.io
  [9]: https://github.com/cnfeat/GoodThingList/blob/master/GoodJekyllBlogList.md
  [10]: https://www.zybuluo.com/
  [11]: http://www.pchou.info/ssgithubPage/2013-01-03-build-github-blog-page-01.html
  [12]: http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html
  [13]: https://www.zhihu.com/question/20463581
  [14]: http://wuchong.me/blog/2014/01/24/change-to-pacman/
  [15]: http://ibruce.info/2013/11/22/hexo-your-blog/
  [16]: http://www.cnblogs.com/fnng/archive/2012/01/07/2315685.html
  [17]: https://www.google.com/
