ulimit -u查看线程数限制

## 【动态】top

动态查看CPU、内存的占用量

右上角有个load average：系统1、5min、10min的平均负载值，如果3个值相加除以3乘以100%  >  60%，则系统的负载压力太大。

按1键，可以查看每个CPU的负载情况。

uptime：直接查看load average

![在这里插入图片描述](常用Linux命令.assets/950745d9625c4e59b1e474dcbde7909e.png)

## 【CPU】vmstat

查看CPU（包括但不限于）

![在这里插入图片描述](常用Linux命令.assets/9ffc5c05e47d42d4a07e4fc5c71aa3ce.png)

![在这里插入图片描述](常用Linux命令.assets/5483e93f85d54b5ebe9c5fb5715c6a89.png)

## 【内存】free

![在这里插入图片描述](常用Linux命令.assets/dc14f1f6fd48468780280d0441fe3535.png)

free：以字节byte为单位

free -g：以G为单位

free -m：以M为单位

![在这里插入图片描述](常用Linux命令.assets/f4ff5a3b9b8843adbba098a226e6d877.png)

![在这里插入图片描述](常用Linux命令.assets/bb38aac4c64c44bcbff3fa6fb5f0d9a8.png)

## 【内存】pidstat

查看指定进程使用的内存

![在这里插入图片描述](常用Linux命令.assets/45599ab0bc54433e95b2e43cd9c38b99.png)

## 【硬盘】df

df -h

h:human用人类看得懂的方式显示，K/M/G都会有

![在这里插入图片描述](常用Linux命令.assets/cb3844b30cad46048a18d68cdc90a855.png)

## 【磁盘IO】iostat

iostat -xdk 2 3 

Linux命令之磁盘IO查看iostst和pidstat

![在这里插入图片描述](常用Linux命令.assets/f87ff397e5c540e981ce839e7dbca8e9.png)

![在这里插入图片描述](常用Linux命令.assets/6dfd61811123463a96df604389ab5800.png)

![在这里插入图片描述](常用Linux命令.assets/8ea651cadc934564a765634cc9d72a66.png)

![在这里插入图片描述](常用Linux命令.assets/4b8540d8ebeb42b884c270396e9e724b.png)

![在这里插入图片描述](常用Linux命令.assets/aa80cc7c93364ccc9f4bfc60129a95d1.png)

## 【网络IO】ifstat

![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\46a6bec2e522472dbde013bc3dc57cbb\f575c1f95b3a4f3d8fae60f9d874604b.png)

## 【进程】ps

```
ps -ef|grep java|grep -v grep
```

ps -ef 和ps aux：两者没太大差别，讨论这个问题，要追溯到Unix系统中的两种风格，System Ｖ风格和BSD 风格，ps aux最初用到Unix Style中，而ps -ef被用在System V Style中，两者输出略有不同。

```
grep -v grep：去除包含grep的进程行
```

ps命令将某个进程显示出来

grep命令是查找

中间的|是管道命令 是指ps命令与grep同时执行

PS是LINUX下最常用的也是非常强大的进程查看命令

## 假如生产环境出现CPU占用过高，请谈谈你的分析思路和定位

### CPU占用过高的定位分析思路

![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\e295b8af937a4b0bb4dd9c6f9987c5c1\541e6f3281824e4a8f11d6cc93ac6a51.png)

![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\0f3bc65ec70545bd89921b1068fb4780\86c63cd1c914474091a83cf47059ddf2.png)

#### 1、top命令找出CPU占比最高的

![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\119a778cb66e40a4bc25dc18b12c25b1\1c73852f8ef848e3b574eeff10071bba.png)

#### 2、ps -ef与jps进一步定位进程类型

ps -ef根据PID查看进程

![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\4a12b5d2cfaa4f3bbffde3f24bc3e446\2cef958dd02a4c3988262ae6a0b1c264.png)

#### ![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\1d9764500a09436dadd166e36a524909\89e3cdcf39e842fa9792c9517a713e32.png)3、ps -mp定位到具体的线程

![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\476abee29685432285d6efad3942e05a\9bc942f5dc1747a1b760e887286a8d4a.png)

![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\645d30c4701b449fab509c32b621ead7\a119da2aa3514ed7b3899e14c8221fc5.png)

#### 4、使用计算器将线程ID转换成具体的16进制格式

【线程号应该是3929，不是5102】

![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\4dd47652b85549ffb45581964cf7c9c9\47d88228dc664c2c932ba99115c8f6a9.png)

![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\5c8f6ad2d66e4b28965ec6d191e7fe87\b1e13eb4e13e46df99623b8cbc5da0f7.png)

#### 5、jstack打印栈内存信息，找到出错的代码行

![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\d7254647d01a40fcafeb6b5a78eb050e\e8831d7be994442eb33e7cc8de8645e8.png)

![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\1b5e8ffd235b4ab28cddbca6fe237295\3924dea1eeff4742ac0ec9a343e3e4e9.png)

![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\58fb3dbe1ae04b498e5f906e7040542d\5b80027d160a42878429b271606eec9d.png)

## 对于JDK自带的JVM监控和性能分析工具用过哪些？

一般你是怎么用的？

![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\db866d2213504d3d8f9ca15a3311458d\cf0e7d08f61849f4ae9ab4a1f2ab028f.png)

![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\1f193abd42ff45e49df3c52f163cae44\d764dc7179ba4c76a88d4341f006102b.png)

![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\59bd47dc67de468e97915a7eee132114\e5e7a1d876124aa0992798397fc100ea.png)

![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\bb9edbeb04024c809ccf39803c403bff\760be49df4494cfa9d1d88ec7fa0d6e3.png)

![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\42aec549bdb442c38a10e888a47d69e9\0b3be3daaf114844aced6498c3e5e5e8.png)

![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\011b4328788746089b1a40fb5fe9e281\92e48f339adc4eeb96776956c8d26b84.png)

**GitHub操作之开启**

![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\68d9d3b18379487d9f7ab51cdd9535c8\77bc0c5304bc45a9a1a4ec544b871a57.png)

![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\85b266a82fe345a296c1b10a586d03af\d7bb58484af94293aac8e8ef6338eacb.png)

![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\23ff954e4b634cabaf6e176fa81d3f2d\39946f0967c74623bc0178c4a19a4080.png)

## GitHub操作之常用词

![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\c4f5dc2ed1394ea4a2360c65995a4f68\864225d433f146ad8efec875a839861d.png)

![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\bff23cb89cee4dee9b99fc2ae978fb8e\43b4905b2b1e49d78b47848a83b6ac78.png)

### GitHub操作之in搜索

![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\4ed78f9184144ef08c686705a16f4516\5af0f013a3b240438b8619fc11c37d17.png)

![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\548f35faea24433c8cb4007b10347946\7e1da5ad5a274ec3a9d86ac3d494fc40.png)

![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\ba8207adc9f64f31bce04e2fa7cb699c\dbced21b14be440b9a39eec7d16cd5e0.png)

![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\472cea3f57434692962104130c86d979\cbbef465a6e84dc093454cf23ef9e38a.png)

![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\12a0f111718f41c393360fd94968d0dc\5ae4eb504b114e09b124328d620b5387.png)

![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\720a57ebe7e74b12b7ac78b020281cf6\b2d46dab9e4144d89446c4b7fdc0b86f.png)

### GitHub操作之star和fork范围搜索

![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\67d789cd81aa4040ac44cd538ca24be1\3f9da93bd3114098a328f1e3fb34e9e4.png)

![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\89b1ad80489a49b1a14847163aa10763\ca020d0f133449ae889d6cb3a5864a2a.png)

![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\d942a938d7374539ac15cc69b905a0b1\7dca6449575b4ff6a4863a7668c6eaa8.png)

![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\75cd4b75b4ca44cdb30c229dc4a3da28\2824555c95134aada37accbdafbcdeba.png)

![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\b0dd58e18b6c4ad5b6a546276fbbc633\1960b3d3c694485fb0250531e8b4b21e.png)

![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\f5d7743be8674b7b8ef6ba9855445e72\6fbe6d812d9741cd89579aff509e05e2.png)

![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\7f51c967b35e499b988dd863d41f6b97\69d61ea16c334cff8749ef587e9c7007.png)

![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\60fcf5a9519447eea3850859b9e5aa7f\193d385357db4cdcad57d88e1a4276ff.png)

### GitHub操作之awesome搜索

![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\aebad1dc98fe458586fd38280ead0b3d\d262235f166d4cd7a0aceea179ff3405.png)

![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\5100ffdda483461ca6da4d38d9b550d5\15de7efa1ad9446b853ea0908b5380ee.png)

![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\c237078f950b42bc8cfb5c3965eb0140\bfc6b63030fb43219b46033bfc61b8bc.png)

### GitHub操作之#L数字

![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\032ff8fd2ec2440f91604655833913e5\dae36b894ece48db8a30e5c1e67277ef.png)

![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\10c6bbbeda9d49bbba974fca201d8803\9223e58d03ae436a8b371fa36fcbdcce.png)

![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\003c6d76a4d84e659448ff818c3e956f\5759c5e59ce84c6f88cc95b10e6a3e88.png)

![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\0ee98521c8e347599cc3bfbdd153ca00\e5d682010a4e446aba8f33bb8a4aaf0f.png)

![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\36bbc28c98564fb3bc97598867e685e9\c923d018635f46c8b67a8710f48c507d.png)

### GitHub操作之T搜索

![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\b3d84e323d9e492ab24187dda2719b33\0bce7890d0ce4e00b7545ddf2d7b612f.png)

![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\4ebef0b4de1642fa889a508cece04db2\516cecc9cf89494ea2bf11d09364e9cb.png)

### GitHub操作之搜索区域活跃用户

![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\6348778adfc6439d9a57f3129edae587\7b7f005b2732483185235af5dacfa4a3.png)

![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\0278513c14444ebc9e64f39d7243fca2\5fca9eff45ad47e89f72c931f662259a.png)

![在这里插入图片描述](D:\Program\Youdao\YNoteData\weixinobU7VjttUaQpmMx2t56gFqBfm2gQ\336225210aae422a8ae588aea8c6e250\2e84253cd236436b85db14fb0fcfe2d3.png)