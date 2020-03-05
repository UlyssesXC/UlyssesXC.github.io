---
tags: ["服务器","笔记","瞎折腾"]
---

# 简述

室友买了一台WiFi6的路由器，玩了一下，配合我的三星S10e也带的WiFi6，瞬间被惊到了，steam link玩猛汉王居然延时只有10ms，手机连上Xbox手柄，躺在床上就可以快乐操虫棍了！！！

羡慕之余，想起来了漏油了的、之前刷过LEDE、被我肢解的**某训K3**，想起来K3的内网速度也是挺无敌的上一代博通旗舰方案，内网速度应该不差吧？于是乎，这个可怜的东西时隔2年又被我拿出来了，经过了好几天的散热改装、重刷新版固件，K3焕然一新，又省下了一笔钱，我真是小天才。

然后事情就变得有趣起来了...因为最近接触Linux也比较多，所以玩了一天的**OpenWrt**，虽然折腾了半天L2TP，失败了，但是发现这个系统太帅了！！！怎么以前没接触过！！！百度网盘、aria2、FTP、qBittorrent...功能也太多了吧！！！算上软件中心的拓展，四舍五入就是超强家庭娱乐中心啊！附上界面给大伙看看：

![](https://img-1253341704.cos.ap-shanghai.myqcloud.com/PIC20200301035252.png)

**我以后买路由器，一定要是能刷OpenWrt的**

这里才有了这篇记录，主要留给自己以后看，要是认真搞一下FTP，说不定这个博客的图床问题也可以完美解决了，折腾无止境，也算是另类的娱乐方式了。

# 资源

附上一部分过程用到的链接和说明，保留给自己迁移使用：

内网穿透最经典的思路就是把内网的端口号，直接映射到公网服务器ip地址相同的端口号，比如说内网是5299（百度网盘那个），外网访问就还是123.123.123.123：5299

恩山自编译固件：[K3 OpenWrt Lede](https://www.right.com.cn/forum/forum.php?mod=viewthread&tid=2223080&ordertype=1)

另一个没用上的版本：[K3 OPENWRT R20.2](https://www.right.com.cn/forum/forum.php?mod=viewthread&tid=306836&pid=2462826&page=1)

> LEDE或者OpenWrt固件之间互刷，直接在系统后台进行即可

反正服务器都没用了，所以在后台换了系统：[服务器后台](https://master-stack01.pacificrack.com/login.php)

服务器的frp设置教程：[少数派-使用frp进行内网穿透](https://sspai.com/post/52523)

> 教程中frp端口是7000 面板端口是7500  反正二级域名怎么样都无所谓
>
> 教程中的http和https不用 但是Openwrt是需要使用的 把http改到了80端口 如果屏蔽后面再改就是了

因为教程里的服务器frp没有办法保证开机自启，于是：[CSDN-ubuntu16开机自动执行代码](https://blog.csdn.net/weixin_42451919/article/details/88971503)

> 网上大部分教程是使用/ect/rc.d/rc.local文件自动运行无效，因为环境还没有完全启动

服务器设置好后，使用[阿里云](https://dc.console.aliyun.com/next/index?spm=5176.12818093.recommends.ddomain.488716d03AX9LN#/domain/list/all-domain)绑定了二级域名，frp面板就是frp.ulss.tech，路由器就是route.ulss.tech

附上frp的设置（虽然都打码了）

![](https://img-1253341704.cos.ap-shanghai.myqcloud.com/PIC8ecd4b2d42ee0b590d60d7d380b3133.png)

使用FTP需要TCP接好FTP的端口转发：[一楼下面有详细用法](https://www.right.com.cn/forum/thread-405878-1-1.html)

FTP是TCP协议的，所以服务器需要打开TCP端口监听：[CSDN-iptables](https://blog.csdn.net/fancancan/article/details/81286689)

使用方法：[CSDN-iptables命令使用](https://blog.csdn.net/Henry_Wu001/article/details/19151213?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task)

**百度网盘、aria2等功能用http穿不了，必须直接把内网端口用frp的TCP转到公网ip，再用公网ip访问：route.ulss.tech:各个插件功能的穿透上来的端口号**

设置好的端口转发规则可以在 http://frp.ulss.tech:7500 中查看

smartDNS设置：[下面有人分享出来了设置内容](https://www.right.com.cn/forum/thread-2096941-1-1.html)

然而，设置完上述内容之后，路由器开始每开机三分钟准时断网，微信发的出去，但是网站打不开，又是dns遇到了问题，但不管怎么关闭进程都无效。

最后还是换成了另一个固件解决，这个固件没有frp，需要自己上传，已经下载好了，因为没办法排除是不是frp的问题（虽然不太可能），还是打算等到需要的时候再上传，设置步骤无需变动。