## python相关

### 内嵌函数

> 即函数内部定义的函数
>
> 只能在函数内部调用

```python
 def fun1():
    print('fun1()正在被调用')
    def fun2():
        print('fun2()正在被调用')
    fun2()
```

### 闭包

> 简单的讲，就是内部函数引用了外部函数的自由变量，外部函数返回内部函数的引用。
>
> 这样在使用的时候自由变量可以同内部函数一同存在，自由变量的生命周期和内部函数同步。

```python
def fun1(a,b):
	def fun2(c):
        return a + b + c
 	return fun2

fun1(1,2)(3)
```

 结果为6

	>这里要注意内嵌函数不能直接使用外部变量（如果是原生类型的话）
	>
	>python3可以使用nonlocal修饰外部变量



###lambda

lambda表达式，通常是在**需要一个函数，但是又不想费神去命名一个函数**的场合下使用，也就是指**匿名函数**。

lambda所表示的匿名函数的内容应该是很简单的，如果复杂的话，干脆就重新定义一个函数了，使用lambda就有点过于执拗了。

lambda就是用来定义一个匿名函数的，如果还要给他绑定一个名字的话，就会显得有点画蛇添足，通常是直接使用lambda函数。如下所示：

```
add = lambda x, y : x+y
add(1,2)  # 结果为3

```

 

那么到底要如何使用lambda表达式呢？

 

**1、应用在函数式编程中**

Python提供了很多函数式编程的特性，如：map、reduce、filter、sorted等这些函数都支持函数作为参数，lambda函数就可以应用在函数式编程中。如下：

```
# 需求：将列表中的元素按照绝对值大小进行升序排列
list1 = [3,5,-4,-1,0,-2,-6]
sorted(list1, key=lambda x: abs(x))

```

当然，也可以如下：

```
list1 = [3,5,-4,-1,0,-2,-6]
def get_abs(x):
    return abs(x)
sorted(list1,key=get_abs)

```

只不过这种方式的代码看起来不够Pythonic

 

**2、应用在闭包中**

```
def get_y(a,b):
     return lambda x:ax+b
y1 = get_y(1,1)
y1(1) # 结果为2

```

当然，也可以用常规函数实现闭包，如下：

```
def get_y(a,b):
    def func(x):
        return ax+b
    return func
y1 = get_y(1,1)
y1(1) # 结果为2

```

只不过这种方式显得有点啰嗦。

 

那么是不是任何情况下lambda函数都要比常规函数更清晰明了呢？

肯定不是。

Python之禅中有这么一句话：Explicit is better than implicit（明了胜于晦涩），就是说那种方式更清晰就用哪一种方式，不要盲目的都使用lambda表达式。

### map()函数

**map()** 会根据提供的函数对指定序列做映射。

第一个参数 function 以参数序列中的每一个元素调用 function 函数，返回包含每次 function 函数返回值的新列表。

### reduce函数

**reduce()** 函数会对参数序列中元素进行累积。

函数将一个数据集合（链表，元组等）中的所有数据进行下列操作：用传给 reduce 中的函数 function（有两个参数）先对集合中的第 1、2 个元素进行操作，得到的结果再与第三个数据用 function 函数运算，最后得到一个结果。