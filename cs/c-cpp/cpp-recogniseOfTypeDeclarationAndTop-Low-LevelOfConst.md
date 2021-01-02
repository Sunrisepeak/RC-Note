# Recognise of Type And Top-Low-Level of Const for CPP

### 文章导读

>1.几个例子
>
>2.类型申明的识别方法
>
>3.顶层(Top-Level) const 和 底层(Low-Level) const 的分析
>
>4.强制类型转换的类型识别
>
>5.避免复杂类型申明的建议与方法
>
>6.最后总结
>
>7.文章的解读视频链接



### 1.几个例子(后面会解释)

```c++
const int (*a)[10];			

const int * const p;		

((void (*) (void))initKernel)()	

const int (&a)[10] = init;
```



### 2.变量申明的类型识别方法（两步走）

> 先看是什么(x)，再看是什么(y)的什么(x)。

这句话有点绕,下面面通过一个简单的例子解释一下。

```c++
const int *p;	// p是　指向int型常量　的指针
```

从这个例子中我们能够得到

​		**p是什么(x):** 　p是一个指针。

​		**p是什么(y)的指针:**　p是一个指向  int型常量　的指针。

所以这个例子里：　x = 指针，　y = int型常量



>核心思路与方法：就近原则与递归分析



#### 2.1 解决"先看"的问题

> 这里其实是优先级问题，以变量名为基准，先右后左，由里到外的分析。
>
> 并且通过"先看"，可以帮助我们快速判定一个变量的基本属性。

例如你现在想定义**一个指向**　**存储int型常量 大小为10的数组　的指针**。

```c++
const int *a[10];	// 假如我们随手一写
```

对于上面的声明，我们可以通过(上述方法)先右后左  a[10] 可以看出a是一个数组而不是一个指针。所以上面的定义不符合预期。对于下面这个例子：

```c++
const int (*a)[10];			// exmple3
```

我们可以用同样的方法　分析 ( *a ) 左边为空，右边为 * 可知a 是一个指针，符合最基本的属性要求是个指针。



#### 2.2 解决"再看"的问题

> 把分析完的部分看成一个整体在带入分析，类似于数学上的复函函数，来一层层推测出　是什么的什么。

```c++
如: 通过(* a)知道a是一个指针了，从而把(*a)看成一个整体b进行简化
    const int (* a)[10]   ---->  const int b[10]
    进而去分析b的属性
```



#### 2.3 一个完整的分析过程(递归分析)

```c++
const int (*a)[10];		//就从上面的这个例子入手，看a的类型
```

> 1.以ａ为基准分析可知(* a)表明a是一个指针.
>
> > 2.把(* a)看成一个整体b  -----> const int b[10], 可以分析得 b[10] 为一个大小为10的数组.
> >
> > > 3.把b[10]看成一个整体c -----> const int c, 可以分析得　c 是int型的常量
> >
> > 2_.可得b是存储 　int型常量 	大小为10　的数组
>
> 1_.可得a是一个指向　存储int型常量　且大小为10　的数组　的指针。



### 3.顶层(Top-Level) const 和 底层(Low-Level) const 的分析

> 在cpp中变量的初始化和赋值过程中会忽略变量的顶层const，如果要想保留顶层const的属性,需要显示声明，这也为变量类型的定义和分析增加了一点难度。

#### 3.1 顶层和底层const 是什么？

> 对于普通类型int char.....等，他们的顶层const = 底层const, 或者说他们不区分顶层和底层.而对于指针而言即包含本身与所指类型的const属性。



#### 3.2 普通类型

```c++
const int a = 0;
int b = a;			// a 和 b 是不同类型,但a却能给b初始化，这里是因为编译器忽略了a的const属性
const int c = a;	// 对b显式声明const,进行保留a的const属性
```

通过上面的例子，我相信大家肯定感觉，平常都是这样用，没有什么特别的。下面将会讨论指针的Top And Low const.(这个是我在初学c++期间，会感到模糊的地方，不知道大家的情况是什么样的)



#### 3.3 对于指针而言

> 指针，可以看成两部分。一是，他本身为指针类型和引用类型。二是，他们指向的和绑定的类型。下面用画图的方式来简单描述下他们的顶层和底层const.

![](RC-Note/cs/c-cpp/picture/cpp_type_const.png)

**上面图片里描述了：**

​		１．当const作用于指针变量本身时，则其为顶层const

​		２．当const作用于指针变量所指向的类型时，则为底层const

**Test:**

```c++
int main() {
    
    const int a = 3;
    const int b = 4;

    int a1 = a;					// ok,忽略了const

    
    //下面这个声明的类型,可以用上面的分析知,p 是一个指向int型常量　的常量指针
    const int * const p = &b;	// ok, p 自己和所指向的都是const,即有顶层const和底层const
    const int *p1;
    p1 = p;						// ok, p1 是变量，并且p的顶层const被忽略
    
    int * const p2 = &a1;		// ok, p2的底层是变量,顶层是const常量,而a1是个int型常量
    const int * p3 = &a;		// ok, p3 是一个指向int型常量　的变量指针
    p2 = p3;		// error, p3有底层const,而p2底层是变量，而且p2顶层是const常量，不能修改值
   
    return 0;
}

```

**编译信息:编译器报了底层const的错误，还有一个错误是p2是常量**

![](/RC-Note/cs/c-cpp/picture/const_1.png)



#### 3.4 一个特殊的类型－－－引用

> 引用本身就是自带顶层const,而且引用还能把底层数组的维度信息给保留

```c++
/*用指针去理解引用*/
int main() {
    int a = 2;
    int b = 3;
	//*ref1_1 与 ref1_2 等效
    int * const ref1_1 = &a;
    int &ref1_2 = b;
	
    //*ref2_1 与ref2_2　等效
    const int * const ref2_1 = &a;
    const int &ref2_2 = b;

    return 0;
}
```

**当函数调用传递数组时，通过引用来传递数组的维度信息:**

```c++
int a[10] = {......};

//通常传递数组大小的方法
void f(int a[10], int size);

void f(int a[], int size);

void f(int *a, int size);　// 上面两个都等同于这个(直接传递数组会被转换成指针,大小需要通过size来传递)

void f(int (&a)[10]) {	　// 通过引用，携带了数组大小的信息
 	for (auto v : a) {
        //.....
    }   
}

void f(int &a[10]);		 // note: 这个a 是(int引用的)数组　而不是(int数组的)引用
void f(int & *a);		 // 等同与上面

```

**Test:**

```c++
void f1(int a[10]) {

}

void f2(int (&a)[10]) {
    for (auto v : a) {
	
    }
}

int main() {
    int b[8], c[10];
    f1(b);	// ok, 因为int a[10] --> int *a, 不校验数组大小
    f2(b);	// error, 数组大小不匹配
    f2(c);	// ok
    return 0;
}
```

编译信息:

![](/RC-Note/cs/c-cpp/picture/referenve.png)



> Note:  这里只是提一个引用的特性，至于这种方法来传递数组的使用，要看个人和团队。毕竟程序的可读性也是很重要的。(毕竟连编译器都给了一个note)。



### 4.强制类型转换的类型识别

> 强制类型转换　类型的识别和变量的声明的类型识别有很多相同之处，所以有了上面的基础就能很好的识别一些较为复杂的强制类型转换。
>
> 由于强制类型转换在一些内核代码中，出现的较多。所以能够准确且较快的识别一个强制类型转换，在一些同学学习内核时也能有助于一些内核代码的阅读。

**一个出现在内核代码的例子：**

```c++
((void (*) (void))initKernel)();
```

这个代码一眼看上去，就给人很复杂的感觉(我在第一次看到这代码时也有种懵懵的感觉)，下面用一个图来简单分析下他的结构。

![](/RC-Note/cs/c-cpp/picture/convertType.png)



**Test:**

```c++
void f1(void) {

}

int main() {
    void (*f2)(void);
    void *f3(void);
    f2 = f1;	// ok, note: 函数名是函数入口的地址
    f2();
    f3 = f1;	// error, 通过上面介绍的原则，可知f3为　返回值为void* 参数为空的　函数变量
    			// 而不是,返回值为 void 参数为空 的函数或函数的指针
    return 0;
}

```


![](/RC-Note/cs/c-cpp/picture/funcPointer.png)



> Note: 由于函数名, 数组名，地址　和　函数指针，数组指针还有地址、汇编标号与指针的关系等知识点有点复杂，这里就不过多讨论了。等有时间专门写一片文章联合汇编和内存角度去探讨他们之间的联系，先在这留一个　转送门　。



### 5.避免复杂类型申明的建议与方法

> 在c++中可以通过给复杂类型设置别名的方法来降低，编程人员识别复杂类型的难度。



#### 5.1 typedef关键字

> typedef  +  正常变量声明方式 ；

```c++
const int b[10];
const int (&a)[10] = b;		//一个指向　存储int型常量 大小为10的数组　的指针

// 给a的这种类型定义一个别名
typedef const int (&ref_of_10arr_of_constint_by_tydef)[10];

ref_of_10arr_of_constint_by_tydef c = b;	// c的类型就和a的类型一样

// 当设置别一个类型的名时：
	// 第一步：写出这个类型的声明
	// 第二步：变量名换成一个见名知意的的别名(这个例子把a 换成了ref_of_10arr_constint)。
	// 第三步：在前面加上typedef关键字
```



#### 5.2 using 关键字

> using  别名 = 类型;

```c++
const int b[10];
const int (&a)[10] = b;		//一个指向　存储int型常量 大小为10的数组　的指针

using ref_of_10arr_of_constint_by_using = const int (&)[10];

// 通过using 设置一个别名时:
	// 第一步：写出这个类型的声明
	// 第二步：把去掉变量名的声明放在等号的左边。
	// 第三步：在前面加上using关键字和别名
```



**Test:**

```c++
#include <iostream>

using namespace std;

// 给a的这种类型定义一个别名
typedef const int (&ref_of_10arr_of_constint_by_tydef)[10];
using ref_of_10arr_of_constint_by_using = const int (&)[10];

int main() {
    const int b[10] = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
    const int (&a)[10] = b;			//一个指向　存储int型常量 大小为10的数组　的指针
    ref_of_10arr_of_constint_by_tydef c = b;	// c的类型就和a的类型一样
    ref_of_10arr_of_constint_by_using d = b;	// d的类型和c和a类型一样

    cout << "a: " << sizeof(a) << endl;
    cout << "b: " << sizeof(b) << endl;
    cout << "c: " << sizeof(c) << endl;
    cout << "d: " << sizeof(d) << endl;

    return 0;
}

```



**编译运行信息:**

![](/RC-Note/cs/c-cpp/picture/usingAndTypedef.png)



### 6.最后总结

> 关于特别复杂的变量名，其实在很多应用程序里是不多见。只有在一些驱动程序,嵌入式程序和内核代码中会出现一些较为复杂的类型和各种复杂的强制转换。本文主要是想通过对复杂类型的分析，并给出一些分析方法来增强对一些　多种类型复合在一起的复杂类型　的熟悉度，并且降低惧怕感。
>
> 而特别对于顶层const 和 底层const 的理解,　是能有效减少程序的隐藏BUG。也会减少我们平时写程序时对一些变量声明类型理解模棱两可的情况。
>
> 最后，大家要有对复杂类型分析好的方法，可以在评论区进行讨论交流　相互学习和提高。



### 7.文章的解读视频链接

> 有时间录

