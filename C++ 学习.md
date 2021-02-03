# C&C++ 学习

标签（空格分隔）： c&c++

---
## 指针知识

概念：指针也是一种变量，内存存储的是一个标识其他位置的地址
int * p; //变量保存的地址所在内存单元的数据类型为整型

#### 引用、指针、值

 - 内置数据类型 || 小型结构，按值传递

 - 数据对象是数组，使用指针，并将指针声明为指向const的指针

 - 数据对象是较大的结构，使用const指针或const引用，节省复制结构的时间和空间

 - 数据对象是类对象，使用const引用，传递类对象参数的标准方式是按引用传递

 - 指针传递和引用传递
   为了进一步加深大家对指针和引用的区别，下面我从编译的角度来阐述它们之间的区别：

   > 程序在编译时分别将指针和引用添加到符号表上，符号表上记录的是变量名及变量所对应地址。指针变量在符号表上对应的地址值为指针变量的地址值，而引用在符号表上对应的地址值为引用对象的地址值。符号表生成后就不会再改，因此指针可以改变其指向的对象（指针变量中的值可以改），而引用对象则不能修改。

## 字符串

 - 字符数组和字符串

   ```c++
   char dog[3] = {'d', 'o', 'g'};
   char cat[4] = {'c', 'a', 't', '\0'};
   ```

    两者均为char数组，第二个为字符串

 - 字符串常量存放在静态内存区域，不会被修改，访问效率高

 - cin >> x 字符读取；cin.get() 和 cin.getline()按行读取

> getline()：每次读完一行，在存储字符串时，会把换行符替换为空字符
 > get()：读取到行尾巴，区别在于：get()并不在读取并丢弃换行符，而是将其留在输入队列中。如果两次连续调用get()，下次get()获取到的第一个字符是换行符，因此get()认为已到达行尾

 - char str[] 和 char* str 区别

  > char str[10] = "hello"， 可通过str[i]来改变值，编译器会把str数组从第一个元素把hello\0组逐个填入
  > char* str = "hello"，字符串常量，不可改变

 - char a[], char *, char * a[]的区别

    > char a[];&nbsp;&nbsp;&nbsp;&nbsp;//字符数组
    > char *;&nbsp;&nbsp;&nbsp;&nbsp;//字符串常量
    > char * a[];&nbsp;&nbsp;&nbsp;&nbsp;//a先和[]结合，是个数组，数组元素是字符串

## 模板

 - 对于给定的函数名，可以有 非模板函数、模板函数和显示具体化模板函数以及它们的重载版本(优先顺序：非模板 > 显示具体化 > 模板)

   ```c++
   void Swap(job &, job &);
   template <class Any>
   void Swap(Any &, Any &);
   template <> void Swap<int>(int&, int&)
   template <> void Swap(int&, int&)
   ```

## 头文件

 - 函数原型

 - 使用#define或const定义的符号常量

 - 结构声明

 - 类声明

 - 模板声明

 - 内联函数

 - 包含头文件，使用"coordin.h"，使用""会在当前目录寻找，使用头文件

   ```c++
   ifndef COORDIN_H_
   define COORDIN_H_
   ```

    这样的做法，不能防止编译器将文件包含两次，可以让它忽略第一次包含之外所有内容

## C++ 3种不同方案存储数据

主要区别：数据在内存保留时间

 - 自动存储持续性：在函数定义中声明的变量
 - 静态存储持续性：在函数定义外、使用关键字static定义的变量
 - 动态存储持续性：使用new操作符分配的内存将一直存在，直到delete操作符将其释放或程序结束为止
 - 在多文件程序中，可以在**一个文件有且只有一个文件**中定义一个外部变量，使用改变量的其他文件必须使用关键字extern声明它

## 类

- #### 名词含义

  argumnet object: 参数对象
  invoking object: 调用对象

 - #### 初始化对象

   ```c++
   Stock stock1 = Stock();
   Stock stock2();
   Stock *stock3 = new Stock();
   ```

 - #### 构造函数、析构函数

    如果是new Stock()初始化对象，需要执行delete()，默认执行~Stock()

- #### 类初始化流程图

    [![yMRPyj.md.png](https://s3.ax1x.com/2021/02/03/yMRPyj.md.png)](https://imgchr.com/i/yMRPyj)

    

 - #### const含义

    ```c++
    const Stock & topval(const Stock & s) const;
     // 1const: 返回其中一个对象的引用 
     // 2const: 不会修改被显示访问的对象
     // 3const: 不会修改被隐式访问的对象
    ```

 - #### 使用类

     友元：友元函数、友元类、友元成员函数
     友元函数：**不是类成员**，类声明一个friend 成员函数，函数定义无需加上加上限定符

 - #### 重载

     operator op()

 - #### 复制构造函数

    最好使用深度复制，因为如果在构造函数有使用new初始化，如str = new char[len]，对象 B = A，这样只会浅复制，也就是对于成员str还说，A和B的值是一样的，都是指向同一个字符串，如果在A对象调用析构函数释放str内存，这样B对象在析构函数还调用就会报错

 - #### 静态

    类内定义静态成员函数、静态数据成员，两者是所有类对象共有的，不与特定类对象关联；静态成员函数只能访问静态成员

 - #### 布局new操作符

   ```c++
   new(str) class
   ```

     如果使用布局new操作符为类对象分配内存，则必须负责显示地为该对象调用析构函数 Class->~Class()

 - #### 派生类(is-a)

    派生类构造函数：

    > 基类对象首先被创建 
    >
    > 派生类构造函数应通过成员初始化列表将基类信息传递给基类构造函数
    >
    > 派生类构造函数应初始化派生类新增的数据成员

    派生类 && 基类：

    > 基类指针和引用可以指向派生类对象，但是只能调用基类的方法，不能调用派生类新增的方法
    >
    > 派生类可以访问基类public protected成员，但是无法访问private成员

    虚拟函数virtual：

    ```c++
    class A{virtual void func()}
    class APlus : class A{virtual void func()}
    //初始化类
    A a();
    APlus aplus();
    A & a1_ref = a;
    A & a2_ref = aplus;
    假若不定义虚拟函数：
    a1_ref.func() //use A::func()
    a2_ref.func() //use A::func()
    而定义虚拟函数：
    a1_ref.func() //use A::func()
    a2_ref.func() //use APlus::func()
    ```

    总结：如果在派生类中重新定义基类的方法，通常应将基类的方法声明为虚拟的。

    虚函数内存示意图：
    [![r6X9fS.png](https://s3.ax1x.com/2020/12/23/r6X9fS.png)](https://imgchr.com/i/r6X9fS)

 - #### 抽象基类

    虚函数定义：virtual double Area() const = 0;

    > 抽象类定义：至少包含一个纯虚函数
    >
    > 虚函数定义：virtual double Area() const = 0；
    >
    > 类声明包含虚函数，则不能创建该类对象

 - #### 继承和动态内存分配

    ```c++
    Brass blips;
    BrassPlus snips("Rafe", 9119);
    blips = snips;
    ```

    上述可转化：blips.operator=(snips)，基类Brass需要定义重载=(Brass :: operator=(const Brass &))函数，由于Brass和BrassPlus是is_a关系，允许Brass引用指向派生类对象

 - #### has-a关系

    包含对象成员的类
    构造函数初始化
    构造函数初始化顺序和它们被声明的顺序一致
    is-a 和 has-a构造函数的初始化区别，
    is-a是继承，在初始化基类是需要使用类名，has-a是组合，在初始化只需要类成员名

    ```c++
    class Student{
    	private：
        	string name;
       	valarray<double> scorces;
    };
    // 构造函数初始化
    Student(const std::string & s, int n) :name(s), scores(n){}
    ```

    对比is-a：

    ```c++
    class Student:private string, private valarray<double>
    {
    public:
     ...
    };
    // 构造函数初始化
    // ArrayDb是std::valarray<double>别名
    Student(const char * str,  const double * pd, int n) : std::string(s), ArrayDb(pd, n){}   
    ```

 - #### 多重继承

   ```c++
   Worker -> Singer
          -> Waiter
   Worker派生出Singer、Waiter
   新定义SingerWaiter类 继承Singer、Waiter类
   
   SingerWaiter ed;
   Worker * pw = &ed //错误，二义性
   Worker * pw1 = (Waiter *) &ed; //正确
   Worker * pw2 = (Singer *) &ed; //正确
   ```

   

   ```c++
/*
   使用单继承，对应构造函数的改变
   单继承：C类的构造函数只能调用B类的构造函数，B类只能调用A类的构造函数；
C类构造函数将值m和n传递给B类的构造函数，而B类的构造函数将n值传递给A类构造函数
   */
   class A{int a; A(int n = 0) {a = n;};
   claas B:public A{int b; B(int m=0, int n=0):A(n){b = m};
   class C:public B(int c; C(int q=0, int m=0, int n=0):B(m, n){c = q})
   ```
   
   解决多重继承产生二义性——虚基类
   
   ```c++
   class Worker{
       public:
       	int data;
   };
   class Singer:virtual public Worker {};
   class Waiter:virtual public Worker {};
   class SingerWaiter:public Singer, public Waiter {};
   ```
   
    在基类是虚拟的时，禁止信息通过中间类自动传递给基类
   
 - #### 总结

     1. explicit关键字：使用explicit防止单参数构造函数的隐式转换

        ```c++
        class Student{
        private:
        	typedef std :: valarray<double> ArrayDb;
        	std :: string name;
        	ArrayDb scores;
        public:
        	explicit Student(n) : name("Nully"), scores(){}
        
        }
        -------------------------------
        Student doh("Homer", 10);
        doh = 5;
        
        ```

        如果缺少explicit关键字，这将使用构造函数调用Student(5)将5转换为一个临时替换Student的对象，并使用赋值给doh，替换原来的doh值，使用explicit后，编译器会认为上述赋值操作符是错误的

     2. 使用const限制方法修改数据

     3. 通常，应使用包含建立has-a关系，如果新类需要访问原有类的保护成员，或需要重新定义虚函数，则应使用私有继

- #### 友元

  类友元：Remote对象所有方法都可影响Tv成员

  ```c++
  class TV
  {
      friend class Remote;
      ...
  };
  class Remote{...};
  ```

  类成员友元：只有Remote::function()可以影响Tv成员 

  ```c++
  class Tv;
  class Remote{...};
  class Tv
  {
      friend void Remote::function()
  }
  ```

  
