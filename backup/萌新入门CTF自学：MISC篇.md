### **前言**
此贴用于记录本人在入门CTF（MISC）的历程。
因为个人感觉MISC对于纯萌新（对相关知识完全没有了解）来说最好入门，其相关知识也相对容易通过自己摸索来掌握，所以目前会以MISC为切入点对CTF相关知识进行学习。
会随着学习进度逐步更新。
题目来源于CTFSHOW:   https://ctf.show/

**纯萌新的第一题**
_**题目：**_
> 小明想给心爱的妹子表白很久，可是不知道怎么开口，你能帮帮小明吗？
 已知 md5(表白的话+ctf)=ed400fbcff269bd9c65292a97488168a
提交flag{表白的话}

说实话刚看到的时候很崩溃，大脑中全是疑惑，MD5什么东西？后面一大串字符又是什么鬼？
但静下心一想，已经大学了，要学会利用搜索引擎，又不是高中不给带电子产品。一搜果然茅舍顿开。原来MD5是一种加密算法，去找解密工具就能得到flag。
一不做二不休，搜索md5解密工具，找到网站https://www.somd5.com/ ，把密文丢进去，得到“ _helloctf_ ”
所以flag就是flag{hello}。解决！

**第二题：压缩包假密码**
**_题目：_**

> 小明的压缩包又忘记密码了？他去电脑维修店去修，人家扔出来说这个根本就没有密码，是个假密码。小明懵了，明明有密码的啊，你能帮帮小明吗？

带一个附件压缩包

![Image](https://github.com/user-attachments/assets/61c323f7-dd19-42af-8649-6870fdd33dc8)
点开看看，里面有flag！

![Image](https://github.com/user-attachments/assets/f8c19209-e467-49cb-9f37-1b8c3bc40bd2)
奶奶的有密码，哎小明怎么这么坏

![Image](https://github.com/user-attachments/assets/fc01bbc3-386e-4e4b-9e68-73c2feb55613)
怎么办？爆破吗？一点线索也没有怕不是要试到猴年马月，只能另寻他路。回到题干，“_根本就没有密码_”
在搜索引擎搜索“压缩包假密码”试试看吧。

![Image](https://github.com/user-attachments/assets/bd6dc027-0e6e-4d6f-a731-7a179f6d2355)
阅读完一遍已经有头绪了，就是更改了压缩包的数据让压缩包从无加密变为有加密（实际上没密码）

> 具体如下：识别真假加密
> 无加密
> 数据区 的全局加密应当为 00 00
> 目录区 的全局方式位标记应当为 00 00
> 
> 假加密
> 数据区 的全局加密应当为 00 00
> 目录区 的全局方式位标记应当为 09 00
> 
> 真加密
> 数据区 的全局加密应当为 09 00
> 目录区 的全局方式位标记应当为 09 00
> ————————————————

> 原文链接：https://blog.csdn.net/qq_44105778/article/details/94961293

此时只要将压缩包的16进制编码进行小更改就好了。
此时问题又来了，用什么来更改呢，再搜！用winhex。

![Image](https://github.com/user-attachments/assets/9d0a8121-781d-4963-b8ec-034006d858b1)
用winhex打开后找到目标，将09 00改为00 00，再保存后打开压缩包，果然没有密码了！
打开里面的txt文件，得到flag：“_flag{c_t_f_s_h_o_w}_” ，解决！

**第三题：图片编码**
**_题目1:_**

> 小明小心翼翼的打开压缩包，竟然是个图片，什么鬼？
> 要是图片能继续往长一点该多好啊，小明暗暗的想。
> 你能帮小明完成这个朴素的梦想吗？

![Image](https://github.com/user-attachments/assets/c46fb04a-5682-431a-9d19-c58a5e12da41)

我的天

![Image](https://github.com/user-attachments/assets/29e478f2-8c13-461a-8644-5973930a49da)
既然给了一张图片，那么flag肯定就藏在图片里，结合题目小明想让图片更长一点。
这是什么意思，图片变长？那普通得拉伸操作肯定不行。不管了，谷歌启动！
经过一番吉列地搜索，了解到可以通过修改png图片的编码来修改图片的长宽，这下思路便清晰了，只要知道了png的编码格式，找到其中与图片宽和高有关的参数，修改一下就行。
用winhex打开图片，通过询问AI了解到具体改修改哪些数据

![Image](https://github.com/user-attachments/assets/f8abcc61-09c9-4753-b987-1e88517ddb18)
修改后保存，此时再看图片

![Image](https://github.com/user-attachments/assets/42b8d87c-6656-48d8-945e-be6c966d673c)
得到flag为flag{beautiful}，解决！

**_题目2:_**

> 小明看完图片老脸一红，心想，我女朋友能有这么瘦就好了。

![Image](https://github.com/user-attachments/assets/571e09d6-3d5b-4a5c-85c1-ae5ebcb7fa67)
？
结合题目，“瘦”，那应该就是把图片的宽度变小咯。
用winhex打开图片，找到宽度对应的数据

![Image](https://github.com/user-attachments/assets/368714b5-0096-49cf-87e3-f0836a7fe825)
图片的宽度是780px，780对应的16进制数为30C，那就把00 00 03 0C改成00 00 02 0C看看。
保存后得到图片

![Image](https://github.com/user-attachments/assets/56b56827-591d-4618-bf0b-6c840b3ee911)
解决！


