# Water Card

`wonyoungsen` `water card`  `NFC` `RFID`

## 说明

>这里说明下背景，在学校没事就研究一下RFID技术，于是先拿water-card试试。

## 工具

>这里使用acr122u，其他工具应该也可，主要是acr122u的PJ方便，价格相对较贵，购买时注意便宜的多为非真。

## 核心密钥及数据分析
>首先说下，m1卡总共64扇区，每扇区有四块，其中第四块是密钥部分。密钥又分A密钥和B密钥，每块总共32位（16进制），前12位为A密钥，中间8位为控制字段，后12位为B密钥，中间的控制字段标志着此块数据如何读写，具体详细的介绍请自行GG。下面给出此“卡”数据部分介绍，说明下，此“卡”的数据部分是存储在5、6扇区，5扇区为卡ID验证部分（似乎此卡没有什么验证，没有参透值是如何加密的，但是不涉及6扇区数据，所以扇区可以不用考虑），6扇区中是主要的数据部分，如图：

![](https://raw.githubusercontent.com/wonyoungsen/water-card/master/water2.png)




>其中，禁恶部分是实际十进制禁恶乘以100，然后转化为十六进制，本图中0.61，扩大之后为61，转换为16进制后为003D；
25块和26块分别和24块对应。
红框前面的数据可能是随机码，这里只是猜测，哈哈。

>第27块是密钥部分，此“卡”是：

![](https://raw.githubusercontent.com/wonyoungsen/water-card/master/water3.png)

>其中控制位是错误的，真实的是下面里面的，0780ff69 ，可能是因为PJ时是根据b密钥出的结果，这个分情况，有时可以出正确的，但数据不对，有时可出数据，但是控制位不对，不知道是软件的问题还是卡的问题。
控制字段含义：

![](https://raw.githubusercontent.com/wonyoungsen/water-card/master/water1.png)

>这里控制字段含义是只能通过B密钥进行读写0块数据，后面三块依次。方法：

* 1、克隆：直接PJ读取原卡数据，dump出来一个dump文件（acr122u），然后使用克隆工具，克隆到UID白卡，即复制一张完全一样的卡。

    * **优点**：不用考虑密钥，简单，容易操作。

    * **缺点**：需要有UID白卡。
    
        * 注意：如果克隆时出现错误，失败，则要考虑自己的uid卡是不是真的UID。
    
* 2、修改：通过工具（这里使用PCSC）,使用B密钥连入，主动式修改里面的数据，只需修改第六扇区里面的第24、25、26三块中的禁恶数值即可。编码规则见上面说明。（最大数值为101.00）

    * **优点**：无需UID白卡，可以随意更改想要禁恶。
    
    * **缺点**：需要明白密钥使用原理，修改工具的使用有点学习成本。
    
        * 注意：工具的使用建议在win7 32位下，其他系统可能无法正常使用。


## 后语：
>由于做这个研究有一年多的时间了，一直没有时间来总结，临近离校，抽空来总结一波，但也忘得差不多了，可能有不对的地方，如发现还请指出，大家可自行研究。

## 声明：
>本文只是感兴趣研究一下，请大家不要涉及“禁恶”，尊重技术，低调研究，更不要触犯别人的利益，做事之前要想到后果。重要的事说三遍：不要搞事情！不要搞事情！不要搞事情！


