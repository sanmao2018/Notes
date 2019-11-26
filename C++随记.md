## C, C++随记

### #pragma region 用法

> pragma region 是一个Visio Studio Code Editor中的命令，来定义可以扩展和收缩的代码区域的开头和结尾，可以用来收缩或者展开一段代码。
>
> pragma region name
> pragma endregion comment
> 1
> 2
> 参数
> comment(optional)： 本段代码的注释，可选
> name(optional)： 代码段的名称，可选
> 注解
> pragma endregion 标记代码段的结束，一个#region必须以#pragma endregion结束。
>
> 示例
> #pragma region Region_1
> void Test() {}
> void Test2() {}
> void Test3() {}
> #pragma endregion Region_1
>
> int main() {}
> ————————————————
> 版权声明：本文为CSDN博主「man_of_sillyy」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
> 原文链接：https://blog.csdn.net/zkybeck_ck/article/details/46572249