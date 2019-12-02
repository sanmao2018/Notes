## boost 相关

[toc]

### thread

>

### bind

> bind 是一组重载的函数模板.
> 用来向一个函数(或函数对象)绑定某些参数. 
> bind的返回值是一个函数对象.
>
> 意思是要固定某些函数的参数吗

### lexical_cast

> boost::lexical_cast为数值之间的转换（conversion）提供了一揽子方案，比如：将一个字符串"123"转换成整数123，代码如下：
>
> 1. string s = "123";  
> 2. **int** a = lexical_cast<**int**>(s); 
> 3. 如果转换发生了意外，lexical_cast会抛出一个bad_lexical_cast异常，因此程序中需要对其进行捕捉。

### boost::metux::scoped_lock

> scoped_lock具有可在构造时加锁，析构时解锁的特性，我们经常会使用局部变量来实现对mutex的独占访问
>
> eg:
>
> ```c++
> #include <boost/thread/thread.hpp>
> #include <boost/thread/mutex.hpp>
> #include  <iostream>
> boost::mutex io_mutex;
> void count()    // worker function
> {    
>     for (int i = 0; i < 10; ++i)   
>     {      
>         boost::mutex::scoped_lock lock(io_mutex);      
>         std::cout << i << std::endl;     
>     } 
> }  
> int main(int argc, char* argv[]) 
> {   
>     boost::thread thrd1(&count);   
>     boost::thread thrd2(&count);   
>     thrd1.join();   
>     thrd2.join();   
>     return 0;
> }
> ```

