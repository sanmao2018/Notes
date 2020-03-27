### QT相关

[toc]

> l目前感觉 不得不说坑多，不适合做底层的事情，特性比较奇怪，目前不太了解它的好处

#### explicit

> 首先, C++中的explicit关键字只能用于修饰只有一个参数的类构造函数, 它的作用是表明该构造函数是显示的, 而非隐式的, 跟它相对应的另一个关键字是implicit, 意思是隐藏的,类构造函数默认情况下即声明为implicit(隐式).
>
> 那么显示声明的构造函数和隐式声明的有什么区别呢? 我们来看下面的例子:
> 
>
> ```c++
>  class CxString  // 没有使用explicit关键字的类声明, 即默认为隐式声明  
> {  
> public:  
>     char *_pstr;  
>     int _size;  
>     CxString(int size)  
>     {  
>         _size = size;                // string的预设大小  
>         _pstr = malloc(size + 1);    // 分配string的内存  
>         memset(_pstr, 0, size + 1);  
>     }  
>     CxString(const char *p)  
>     {  
>         int size = strlen(p);  
>         _pstr = malloc(size + 1);    // 分配string的内存  
>         strcpy(_pstr, p);            // 复制字符串  
>         _size = strlen(_pstr);  
>     }  
>     // 析构函数这里不讨论, 省略...  
> };  
> 
> 
> // 下面是调用:  
>   
> CxString string1(24);     // 这样是OK的, 为CxString预分配24字节的大小的内存  
> CxString string2 = 10;    // 这样是OK的, 为CxString预分配10字节的大小的内存  
> CxString string3;         // 这样是不行的, 因为没有默认构造函数, 错误为: “CxString”: 没有合适的默认构造函数可用  
> CxString string4("aaaa"); // 这样是OK的  
> CxString string5 = "bbb"; // 这样也是OK的, 调用的是CxString(const char *p)  
> CxString string6 = 'c';   // 这样也是OK的, 其实调用的是CxString(int size), 且size等于'c'的ascii码  
> string1 = 2;              // 这样也是OK的, 为CxString预分配2字节的大小的内存  
> string2 = 3;              // 这样也是OK的, 为CxString预分配3字节的大小的内存  
> string3 = string1;        // 这样也是OK的, 至少编译是没问题的, 但是如果析构函数里用free释放_pstr内存指针的时候可能会报错, 完整的代码必须重载运算符"=", 并在其中处理内存释放 
> CxString string2(10);  
> 或  
> CxString temp(10);  
> CxString string2 = temp;
> ```
>   上面的代码中, "CxString string2 = 10;" 这句为什么是可以的呢? 在C++中, 如果的构造函数只有一个参数时, 那么在编译的时候就会有一个缺省的转换操作:将该构造函数对应数据类型的数据转换为该类对象. 也就是说 "CxString string2 = 10;" 这段代码, 编译器自动将整型转换为CxString类对象, 实际上等同于下面的操作:
>
> ```c++
> 
> CxString string2(10);  
> 或  
> CxString temp(10);  
> CxString string2 = temp;
> ```
>
> 但是, 上面的代码中的_size代表的是字符串内存分配的大小, 那么调用的第二句 "CxString string2 = 10;" 和第六句 "CxString string6 = 'c';" 就显得不伦不类, 而且容易让人疑惑. 有什么办法阻止这种用法呢? 答案就是使用explicit关键字. 我们把上面的代码修改一下, 如下:
>
> ```c++
> class CxString  // 使用关键字explicit的类声明, 显示转换  
> {  
> public:  
>     char *_pstr;  
>     int _size;  
>     explicit CxString(int size)  
>     {  
>         _size = size;  
>         // 代码同上, 省略...  
>     }  
>     CxString(const char *p)  
>     {  
>         // 代码同上, 省略...  
>     }  
> };  
> // 下面是调用:  
>   
> CxString string1(24);     // 这样是OK的  
> CxString string2 = 10;    // 这样是不行的, 因为explicit关键字取消了隐式转换  
> CxString string3;         // 这样是不行的, 因为没有默认构造函数  
> CxString string4("aaaa"); // 这样是OK的  
> CxString string5 = "bbb"; // 这样也是OK的, 调用的是CxString(const char *p)  
> CxString string6 = 'c';   // 这样是不行的, 其实调用的是CxString(int size), 且size等于'c'的ascii码, 但explicit关键字取消了隐式转换  
> string1 = 2;              // 这样也是不行的, 因为取消了隐式转换  
> string2 = 3;              // 这样也是不行的, 因为取消了隐式转换  
> string3 = string1;        // 这样也是不行的, 因为取消了隐式转换, 除非类实现操作符"="的重载  
> ```
>  
>
>  **explicit关键字的作用就是防止类构造函数的隐式自动转换.
>
> 上面也已经说过了, explicit关键字只对有一个参数的类构造函数有效, 如果类构造函数参数大于或等于两个时, 是不会产生隐式转换的, 所以explicit关键字也就无效了.** 例如:  
>
> ```
> 
> class CxString  // explicit关键字在类构造函数参数大于或等于两个时无效  
> {  
> public:  
>     char *_pstr;  
>     int _age;  
>     int _size;  
>     explicit CxString(int age, int size)  
>     {  
>         _age = age;  
>         _size = size;  
>         // 代码同上, 省略...  
>     }  
>     CxString(const char *p)  
>     {  
>         // 代码同上, 省略...  
>     }  
> };  
> ```
>
> **但是, 也有一个例外, 就是当除了第一个参数以外的其他参数都有默认值的时候, explicit关键字依然有效, 此时, 当调用构造函数时只传入一个参数, 等效于只有一个参数的类构造函数**, 例子如下:
>
> ```
> class CxString  // 使用关键字explicit声明  
> {  
> public:  
>     int _age;  
>     int _size;  
>     explicit CxString(int age, int size = 0)  
>     {  
>         _age = age;  
>         _size = size;  
>         // 代码同上, 省略...  
>     }  
>     CxString(const char *p)  
>     {  
>         // 代码同上, 省略...  
>     }  
> };  
>   
>     // 下面是调用:  
>   
>     CxString string1(24);     // 这样是OK的  
>     CxString string2 = 10;    // 这样是不行的, 因为explicit关键字取消了隐式转换  
>     CxString string3;         // 这样是不行的, 因为没有默认构造函数  
>     string1 = 2;              // 这样也是不行的, 因为取消了隐式转换  
>     string2 = 3;              // 这样也是不行的, 因为取消了隐式转换  
>     string3 = string1;        // 这样也是不行的, 因为取消了隐式转换, 除非类实现操作符"="的重载 
> ```

————————————————
版权声明：本文为CSDN博主「tiankong19999」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/guoyunfei123/article/details/89003369

#### QTimer 类

> QTimer类提供了重复和单次触发信号的定时器。
>
> QTimer类为定时器提供了一个高级别的编程接口。很容易使用：首先，创建一个QTimer，连接timeout()信号到适当的槽函数，并调用start()，然后在恒定的时间间隔会发射timeout()信号。
>
> 注意：当QTimer的父对象被销毁时，它也会被自动销毁。

#### QCoreApplication

> 1 .QCoreApplication用于non-gui的应用程序。QApplication用于gui的应用程序。（是否需要包含QtGui库）。image scaled
>
> 2。继承关系 父----->子  QObject----QCoreApplication---QGuiApplication--QApplication
>
> 从所处模块来看，QCoreApplication定义在core模块中，为应用程序提供了一个非gui的事件循环；QGuiApplication定义在gui模块中，提供了额外的gui相关的设置，比如桌面设置，风格，字体，调色板，剪切板，光标；QApplication定义在widgets模块中，是QWidget相关的，能设置双击间隔，按键间隔，拖拽距离和时间，滚轮滚动行数等，能获取桌面，激活的窗口，模式控件，弹跳控件等。
>
> 具体使用哪个取决于你的应用程序，如果你的应用程序是无界面的，直接使用QCoreApplication即可，如果是gui相关，但没有使用widgets模块的就使用QGuiApplication，否则使用QApplication。l

### QT 国际化

>  

### QT智能指针

> Qt 的智能指针包括：
>
> - QSharedPointer
> - QScopedPointer
> - QScopedArrayPointer
> - QWeakPointer
> - QPointer
> - QSharedDataPointer

### QPixmap

> QPixmap类用于绘图设备的图像显示，它可以作为一个QPaintDevice对象，也可以加载到一个控件中，通常是标签或按钮，用于在标签或按钮上显示图像。
> QPixmap可以读取的图像文件类型有BMP、GIF、JPG、JPEG、PNG、PBM、PGM、PPM、XBM、XPM等。
>
> ## 方法
>
> | 方法         | 描述                             |
> | ------------ | -------------------------------- |
> | copy()       | 从QRect对象复制到QPixmap对象     |
> | fromImage()  | 将QImage对象转换为QPixmap对象    |
> | grabWIdget() | 从给定的窗口小控件创建一个像素图 |
> | grabWindow() | 在窗口中创建数据的像素图         |
> | load()       | 加载图像文件作为QPixmap对象      |
> | save()       | 将QPixmap对象保存为文件          |
> | toImage()    | 将QPixmap对象转换为QImage对象    |

### QElapsedTimer

> QElapsedTimer提供了一种快捷的计算流逝时间的方法。它通常被用来计算两个事件或操作之间过去了多久。并且，该类的方法非常类似于我们之前讲过的QTime类的三个计时函数，所以，我们可以很快速的在使用这两个类的代码之间进行移植。但是，不像QTime，QElapsedTimer会尽可能的使用某种单调时钟。这也就意味着，没办法将QElapsedTimer对象转换成人类可读的时间格式。
>
> 这个类的典型使用方法就是测量一个耗时操作花了多少时间。最简单的例子如下：
>
>       QElapsedTimer timer;
>       timer.start();
>    
>       slowOperation1();
>    
>       qDebug() << "The slow operation took" << timer.elapsed() << "milliseconds";
> 在这个例子中，我们下定义了一个QElapsedTimer的对象，然后调用它的start() 方法开始计时，完成一个耗时操作后，再调用它的elapsed() 方法，得到耗时操作所花费的具体时间，以毫秒计算。
> 我们也可以在一个耗时操作完成后，通过elapsed() 函数的返回值来决定下一个耗时操作可以运行的时间。这对于需要在一定的时间周期内完成几个耗时操作来说是至关重要的。比如下面代码所展现的案例：
>
> 
>
> ```C++
>   void executeSlowOperations(int timeout)
>   {
>       QElapsedTimer timer;
>       timer.start();
>       slowOperation1();
>       int remainingTime = timeout - timer.elapsed();
>       if (remainingTime > 0)
>       slowOperation2(remainingTime);
>   }
> ```
> 
>
> 另一个QElapsedTimer的使用案例是，一个具体的操作需要运行一定的时间长度。对于这种需求，QElapsedTimer提供了hasExpired() 函数，这个函数可以判断自从上次调用start() 或restart() 之后，是否过去了多少毫秒。如下代码所示：
> 
>
> ```C++
>   void executeOperationsForTime(int ms)
>   {
>       QElapsedTimer timer;
>       timer.start();
> 	  while (!timer.hasExpired(ms))
>       slowOperation1();
>    }
> ```
> 上面我们说，QElapsedTimer会使用运行平台所支持的某种单调参考时钟。这具有额外的好处，它可以使QElapsedTimer不受系统时间调整的影响，也不受系统时区设置的影响。当然，相对的，这也意味着我们只能把QElapsedTimer的值和那些使用了同样参考时钟的值相比较。特别是，当我们把QElapsedTimer的自从参考点以来的值序列化到某个变量中后，更应该注意这点。并且，不应该把这些值通过网络进行交换或存储到磁盘，因为无法保证接收端的计算机和产生该值的计算机是否使用同一种参考时钟和参考点，也无法保证其自从参考点以来是否发生过重启。但是，我们可以将这些值在运行在同一太机器上的不同进程间进行交换，条件是他们都使用相同的参考时钟。QElapsedTimer在同一台机器上总是使用相同的参考时钟，所以，在一个进程中比较来自另一个进程的QElapsedTimer值是安全的。
> 注意，QElapsedTimer所使用的时钟类型，有一些是有范围限制的，通常是32-bit，当达到上限时会发生溢出。QElapsedTimer会处理这种情况并提供一致时间。但是，当从QElapsedTimer中提取时间时，同一机器上的两个不同的进程会对实际逝去的时间有不同的理解。
> ————————————————
> 版权声明：本文为CSDN博主「求道玉」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
> 原文链接：https://blog.csdn.net/Amnes1a/article/details/64904690

### QImge

> ### [uchar](https://doc.qt.io/qt-5/qtglobal.html#uchar-typedef) *QImage::bits()
>
> Returns a pointer to the first pixel data. This is equivalent to [scanLine](https://doc.qt.io/qt-5/qimage.html#scanLine)(0).
>
> Note that [QImage](https://doc.qt.io/qt-5/qimage.html) uses [implicit data sharing](https://doc.qt.io/qt-5/implicit-sharing.html). This function performs a deep copy of the shared pixel data, thus ensuring that this [QImage](https://doc.qt.io/qt-5/qimage.html) is the only one using the current return value.
>
> **See also** [scanLine](https://doc.qt.io/qt-5/qimage.html#scanLine)(), [sizeInBytes](https://doc.qt.io/qt-5/qimage.html#sizeInBytes)(), and [constBits](https://doc.qt.io/qt-5/qimage.html#constBits)().
>
> ### const [uchar](https://doc.qt.io/qt-5/qtglobal.html#uchar-typedef) *QImage::bits() const
>
> This is an overloaded function.
>
> Note that [QImage](https://doc.qt.io/qt-5/qimage.html) uses [implicit data sharing](https://doc.qt.io/qt-5/implicit-sharing.html), but this function does *not* perform a deep copy of the shared pixel data, because the returned data is const.
>
> 获取图像原数据的方法

#### QML Settings

> The Settings type provides persistent platform-independent application settings.
>
> **Note:** This type is made available by importing the **Qt.labs.settings** module. *Types in the Qt.labs module are not guaranteed to remain compatible in future versions.*
>
> Users normally expect an application to remember its settings (window sizes and positions, options, etc.) across sessions. The Settings type enables you to save and restore such application settings with the minimum of effort.