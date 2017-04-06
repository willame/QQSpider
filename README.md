##**QQSpider1:**##
<br/>
详情请见博客： [《QQ空间爬虫分享（一天可抓取 400 万条数据） 》](http://blog.csdn.net/bone_ace/article/details/50771839)
如果出现报错：
```
Traceback (most recent call last):
  File ".\init.py", line 20, in <module>
    my_messages.backups() # 备份爬虫信息
NameError: name 'my_messages' is not defined
```

<br/>
多半的原因是 BitVector 模块用不了，可自行调试。
<br/>
如果确定是BitVector用不了的话可以用 "BitVector模块报错解决" 里面的两个文件替换掉原有文件，不使用BitVector判重，改用python的list判重（数据量不大的话效果是一样的）。

<br/>
<br/>
-------------------------------------------------------&nbsp;&nbsp;&nbsp;分界线&nbsp;&nbsp;&nbsp;-------------------------------------------------------
<br/>
<br/>
##**QQSpider2:**##
更新后的版本，详情请见博客： [《QQ空间爬虫分享（2016年11月18日更新）》](http://blog.csdn.net/Bone_ACE/article/details/53213779)

<br/>
有同学反映，爬QQ空间的很多都是学生想爬一些数据做统计研究的，本不是计算机专业，爬起来比较困难，希望有现成的数据出售。但是因为工作变动，其实今年3月份 程序开发完后我就没有跑过了，所以手上也没有数据。不过接下来我会开一两台机器跑这个爬虫，如果需要数据可以邮件联系我（bone_ace@163.com）。

遇到什么问题请尽量留言，方便后来遇到同样问题的同学查看。也可加一下QQ交流群：<a target="_blank" href="http://shang.qq.com/wpa/qunwpa?idkey=d3bd977692493ea2764aec73f6ead724e3b339c2e4f3999383331a0fab20e2a9"><img border="0" src="http://pub.idqqimg.com/wpa/images/group.png" alt="QSpider" title="QSpider"></a>。

<br/>
<br/>

##**结语：**##

爬虫是**偏后台型的任务**，以抓取效率为主，并没有很好的用户界面，并且需要不断地维护。所以对于完全没有编程基础的人来说，可能会遇到各种各样的问题。此项目最初的目的是为大家提供QQ空间爬虫的一种架构，并不保证程序一直能跑。只要腾讯服务器端稍有变动，例如某一个链接变了，可能程序就抓不到数据了，此时程序也要相应地将链接换成新的，如果网页结构变了，解析规则也要相应地修改。

<br/>

##**代码说明：**##

- mongodb用来存放数据，redis用来存放待爬QQ和Cookie。
- 爬虫之前使用的是BitVector去重，有一部分人反映经常会报错，所以现在使用基于Redis的位去重，内存占用不超过512M，能容纳45亿个QQ号瞬间去重，而且方便分布式扩展。
- 爬虫使用phantomJS模拟登陆QQ空间，有时候会出现验证码。我使用的是云打码（自行百度），准确率还是非常高的，QQ验证码是4位纯英文，5元可以识别1000个验证码。如果需要请自行去注册购买，将账号、密码、appkey填入 yundama.py，再将 public_methods.py 里的dama=False改成dama=True即可。
- 分布式。现在已经将种子队列和去重队列都放在了Redis上面，如果需要几台机器同时爬，只需要将代码复制一份到另外一台机子，将连Redis时的localhost改成同一台机器的IP即可。如果想要将爬下来的数据保存到同一台机，也只需要将连MongoDB时的localhost改成该机器的IP即可。
- 为了让程序不那么复杂难懂，此项目只用了多线程，即只用到了一个CPU。如果实际生产运行的话可以考虑将程序稍作修改，换成多进程+协程，或者异步。速度会快很多。
- 最后提醒一下，爬虫无非就是模仿人在浏览器上网的行为，你在浏览器上无法查看的信息爬虫一般也是无法抓取。所以，就不要再问我能不能破解别人相册的这种问题了，空间加了访问权限的也无法访问。程序输出的日志中2016-11-19 01:05:33.010000 failure:484237103 (None - http://user.qzone.qq.com/484237103)这种，一般就是无法访问的QQ。还有，我们是无法查看一个QQ的所有好友的，所以爬下来的好友信息也只是部分好友。

- 爬虫不是黑客，希望理解。
<br/>

##**启动程序：**##

进入 myQQ.txt 写入QQ账号和密码（不同QQ换行输入，账号密码空格隔开）。如果你只是测试一下，则放三两个QQ足矣；但如果你开多线程大规模抓取的话就要用多一点QQ号（thread_num_QQ的2~10倍），账号少容易被检测为异常行为。
进入 init_messages.py 进行爬虫参数的配置，例如线程数量的多少、设置爬哪个时间段的日志，哪个时间段的说说，爬多少个说说备份一次等等。
运行 launch.py 启动爬虫。

<br/>
##**使用说明：**##

启动前配置：

- 需要安装的软件：python、Redis、MongoDB（Redis和MongoDB都是NoSQL，服务启动后能连接上就行，不需要建表什么的）。
- 需要安装的Python模块：requests、BeautifulSoup、multiprocessing、selenium、itertools、redis、pymongo。
- 我们登陆QQ要使用到phantomJS[下载地址：](http://phantomjs.org/download.html），下载完将里面的 phantomjs.exe 解压到python目录下即可。

<br/>
[原文引自九茶的博客](http://blog.csdn.net/Bone_ACE/article/details/53213779)
