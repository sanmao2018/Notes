## udp相关

[toc]

### 什么是udp

> UDP 是User Datagram Protocol的简称， 中文名是用户数据报协议，是[OSI](https://baike.baidu.com/item/OSI)（Open System Interconnection，[开放式系统互联](https://baike.baidu.com/item/开放式系统互联/562749)） 参考模型中一种无连接的[传输层](https://baike.baidu.com/item/传输层)协议，提供面向事务的简单不可靠信息传送服务，IETF RFC 768 [1] 是UDP的正式规范。UDP在IP报文的协议号是17。
>
> UDP协议与[TCP](https://baike.baidu.com/item/TCP)协议一样用于处理数据包，在[OSI](https://baike.baidu.com/item/OSI)模型中，两者都位于[传输层](https://baike.baidu.com/item/传输层/4329536)，处于IP协议的上一层。UDP有不提供数据包分组、组装和不能对数据包进行排序的缺点，也就是说，当报文发送之后，是无法得知其是否安全完整到达的。UDP用来支持那些需要在[计算机](https://baike.baidu.com/item/计算机/140338)之间传输数据的网络应用。包括[网络视频会议](https://baike.baidu.com/item/网络视频会议)系统在内的众多的客户/服务器模式的网络应用都需要使用UDP协议。UDP协议从问世至今已经被使用了很多年，虽然其最初的光彩已经被一些类似协议所掩盖，但即使在今天UDP仍然不失为一项非常实用和可行的网络传输层协议。
>
> 许多应用只支持UDP，如：多媒体数据流，不产生任何额外的数据，即使知道有破坏的包也不进行重发。当强调传输性能而不是传输的完整性时，如：音频和多媒体应用，UDP是最好的选择。在数据传输时间很短，以至于此前的连接过程成为整个流量主体的情况下，UDP也是一个好的选择。