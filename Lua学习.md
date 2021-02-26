# Lua

## 基本类型

nil：只有一种值nil

boolean：true和false

number：双精度的浮点数

string：字节数组

table：关联数组，可以通过任何值来索引(nil除外)

function：Lua函数或Lua虚拟机接口函数的原型编写的C函数

userdata：指向用户内存块的指针 分两种：heavy，内存由Lua分派，并由垃圾回收机制处理；light，内存由用户分配并且释放

thread：协程

任何类型的值都是first-class，可以将其存入全局变量、局部变量或table中，或者将其作为实际参数传递给函数，或从函数中返回值

## Table 

散列表+数组实现

数组：数组下标1~n；散列表：非整数键和超过数组范围n的整数键举例：

```lua
Table1 = 
{
    10,						-- 相当于[1] = 10
    [100] = 40,				-- 相当于[100] = 40	
    John = 					-- 等价于["John"]
    {
        Age = 27,			-- 等价于["Age"]
        Gender = Male		-- 等价于["Gender"]
    },
    20						-- 相当于[2] = 20
}
```

1. 所有元素之间，总是用逗号"，"隔开
2. 所有索引值都需要用"["和"]"括起来；如果是字符串，还可以去掉引号和中括号
3. 如果不写索引，则索引就会被认为是数字，并按顺序自动从1往后编

表类型可以拥有任意类型的值，包括函数，因此Lua可以使用面向对象编程，如下

```lua
t = {
    Arg = 27,
    add = function(self, n) self.Age = self.Age + n end
}
print(t.Age)	-- 27
t.add(t, 10)	-- 相当于 t:add(10)
print(t.Age)	-- 37
```

## Function

```lua
function 函数名(参数列表)
    语句块 
end    
```

Lua函数可以接受可变参数个数，用"..."来定义，比如：function sum(a, b, ...)可以通过在函数中访问arg局部变量获得...所代表的参数，比如sum(1,2,3,4)，在函数中，a=1，b=2，arg={3,4}

Lua编译一个函数时，其中包含了函数体对应的虚拟机指令，函数用到的常量值和一些调试信息。在运行时，每当Lua执行一个形如function...end 这样的函数时，它就会创建一个新的数据对象，其中包含相应函数原型的引用、环境(用来查找全局变量的表)的引用以及一个由所有upvalue引用组成的数组，这个数据对象成为 **闭包**

```lua
function f1(n)
    local function f2()
        print(n)	-- 引用外包函数的局部变量
    end
    n = n + 1
  	return f2
end

g1 = f1(1)
g1() 	-- 输出2
    
```

**upvalue**定义：内嵌函数f2可以访问外包函数f1已经创建的局部变量，而这些局部变量则称为该内嵌函数的外部局部变量(即是upvalue)

流程：执行f1(1)的 n = n + 1时， 闭包已经创建，但是变量n并没有离开作用域，所以闭包仍然引用堆栈上的n，当return f2完成时，n即将结束生命，此时闭包便将变量n（ n=2）复制到自己管理的空间以便将来访问

[![yLl0AS.png](https://s3.ax1x.com/2021/02/23/yLl0AS.png)](https://imgchr.com/i/yLl0AS)

## 简单对象

```lua
function create(name, id)
    local obj = {name = name, id = id}
    function obj:SetName(name) -- obj.SetName = function(self, name)
    	self.name = name
    end
   	function obj:GetName() 
    	return self.name
    end
    return obj
end    
object1 = create("Sam", 001)
print(object1:GetName())		-- object.GetName(object)
```

## 元表

允许改变table行为，每个行为关联了对应的元方法_index，...

\_index键，可以直接对应一个table，\_index = {}；如果对应一个function，

\_index = function(Table, 参数)，例如tab.key，function(tab, key)

\_add键，对应一个function，\_add = function(table, 参数)，例如 tab1 + tab2，function(tab1, tab2)

类似C++重载，举例_index

```lua
mytable = {key1 = "value1"}                          -- 普通表 
mymetatable = {_index = {key2 = "metatablevalue"}}   -- 元表
mytable = setmetatable(mytable, mymetatable) -- 设置mymetatable为mytable元表
```

Lua查找一个元素时规则：

1. 在表中查找，如果找到，返回该元素，找不到则继续
2. 判断该表是否有元表，如果没有元表，返回 nil，有元表则继续
3. 判断元表有没有 **index 方法，如果** index 方法为 nil，则返回 nil；如果 **index 方法是一个表，则重复 1、2、3；如果** index 方法是一个函数，则返回该函数的返回值

## Thread（协程）

[![yz6cy4.md.png](https://s3.ax1x.com/2021/02/26/yz6cy4.md.png)](https://imgtu.com/i/yz6cy4)