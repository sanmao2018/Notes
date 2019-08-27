## python日志模块随记

[TOC]

### 利用 TimedRotatingFileHandler切割日志

> TimedRotatingFileHandler这个模块是满足文件名按时间自动更换的需求，这样就可以保证日志单个文件不会太大。
>
> **TimedRotatingFileHandler(filename [,when [,interval [,backupCount]]])**
>
> **filename** 是输出日志文件名的前缀
>
> **when** 是一个字符串的定义如下：
> “S”: Seconds
> “M”: Minutes
> “H”: Hours
> “D”: Days
> “W”: Week day (0=Monday)
> “midnight”: Roll over at midnight
>
> **interval** 是指等待多少个单位when的时间后，Logger会自动重建文件，当然，这个文件的创建
> 取决于filename+suffix，若这个文件跟之前的文件有重名，则会自动覆盖掉以前的文件，所以
> 有些情况suffix要定义的不能因为when而重复。
>
> **backupCount** 是保留日志个数。默认的0是不会自动删除掉日志。若设10，则在文件的创建过程中
> 库会判断是否有超过这个10，若超过，则会从最先创建的开始删除。
>
> 
>
> 注：之前想要按天切割日志，选择了when为D，但是比较坑的地方是这种方式是初始化Handler后追加86400秒作为切割的时间点。我每重启一下程序，这个时间都会往后推移。靠谱的还是midnight，它每次初始化的切割时间都是午夜。

注意:按天切割还是选择midnight

### sentry和ELK

