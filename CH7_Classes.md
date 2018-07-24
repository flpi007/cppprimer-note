C++ 中最重要的特性之一就是类，通过类可以有效表达代解决的问题中的概念。类讲求数据抽象（data abstraction），即是将类的实现与类所能表达的功能分离开来。类背后的哲学就是数据抽象和封装（encapsulation）。

数据抽象意在将不必要的细节剥离出来以期更加接近问题的本质特性。数据抽象往往是设计系统的第一步，原因在于完整的系统非常复杂，几乎不能在没有一个简单抽象的情况下一步完成。数据抽象使得可以从最本质的特性开始然后一步步丰富到最终的系统。有一些别的术语与此相近，比如：建模（modeling）、泛化（generalization）。抽象是设计一些软件的开端，亦是最重要的武器。

封装则是两个相关但不同概念的组合：将对象状态隐藏使得外部无法直接访问；将数据和方法打包到一起；在一些语言中隐藏组件不是自动发生的，甚至不可能隐藏，因而，在这些语言中只有第二种含义。而第一种含义有另外一个术语表示：信息隐藏（information hiding）。

类使用数据抽象和封装来定义抽象数据类型（abstract data type，ADT），抽象数据类型使得可以以抽象的方式来思考，即仅需要知道类可以做什么，而不必深究类是怎么做到的，这些实现的细节留给设计者去考虑。

## 7.1 定义抽象数据类型

只有具有行为或者说接口的类型才能称为数据类型，如果只有数据而没有行为，所有行为都需要用户自行编写，那便不能称为抽象的数据类型。“行为”的含义了抽象机器的公理语义（axiomatic semantics）和操作语义（operational semantics），有些语境下还会包含计算复杂度（computational complexity）包括时间复杂度和空间复杂度。现实中的很多数据类型不是完美的抽象数据类型，原因在于机器现实，如：算数溢出等。抽象数据类型是计算机科学中的理论概念，不对应于任何特定的语言特性。这个概念被运用于设计和分析算法，数据结构和软件系统。许多概念类似于抽象数据类型：抽象类型（abstract types）、透传数据类型（opaque data types）、协议（protocols）和按约定设计（design by contract）。

[https://en.wikipedia.org/wiki/Abstract_data_type](https://en.wikipedia.org/wiki/Abstract_data_type)

### 7.1.1 设计类

设计类时需要分清楚两种不同的编程角色：设计者和用户。类的设计者负责设计接口和实现。用户则负责使用这些接口完成别的任务。不论在任何时候都需要清楚分离设计者和用户角色，这样能够很好的解耦从而使得程序演变更加容易。在一些简单的应用中，常常类的用户和设计者时同一个人，作为设计者应该努力思考让类更加容易使用，从而使得用户必须了解类的实现就能够很好使用类。设计良好的类通常接口时直观且易用的，并且实现地足够高效。

### 7.1.2 定义类

类中有一些函数是实现的一部分，这些函数不会作为接口的一部分，这些函数需要被定义为私有的。C++ 中会包含一些作为接口的非成员函数，这些函数将定义在类的外部。定义和声明成员函数（member functions）于普通函数类型，成员函数必须在类中声明，定义在类内或者类外。定义在类内的函数隐式称为内联函数。

示例代码：[use_sales_data.cc](https://github.com/chuenlungwang/cppprimer-note/blob/master/code/use_sales_data.cc)

当调用一个对象的成员函数时，所有操作都作用在此对象上。如：
````cpp
string isbn() const { return bookNo; }
````
当调用 `total.isbn()` 时，返回的是 `total.bookNo`，成员函数通过一个额外的隐式参数 this 来访问调用对象。this 被默认初始化为函数调用对象的地址。此处 this 是 total 对象的地址。就如以下代码：`isbn(&total)`

在成员函数内可以直接使用成员名字而不需要使用成员访问符去访问，这种直接使用成员名字会被编译器隐式转换为 `this->bookNo` 形式。

this 参数由编译器隐式定义，并且不允许程序员定义参数或者变量名为 this ，this 总是指向当前调用的对象，所以，this 是一个 const 指针，我们不能改变 this 使其指向别的对象。

参数列表后的 const 用来说明 this 指针指向 const 对象。通常情况下 this 是指向非 const 对象的 const 指针，尽管 this 是隐式定义的，它也遵循初始化原则，即不能将非 const 的 this 指向 const 对象，从而不能在 const 对象上调用非 const 的函数。为了在 const 对象上调用成员函数，必须使得 this 指向 const 对象，如果 this 指针可以在参数列表中，那么它将形如：`const Sales_data *const` ，然而 this 是隐式定义的并且不可能出现在参数列表中，所以语言通过在参数列表后放置 const 来表明 this 指针指向 const 对象，从而使得此成员函数称为 const 成员函数。isbn 函数可以描述为：
````cpp
string Sales_data::isbn(const Sales_data *const this)
{ return this->isbn; }
````
this 指针指向 const 对象，说明 const 成员函数不能改变调用对象的状态。const 对象、指向 const 对象的指针和绑定 const 对象的引用只能调用 const 成员函数。

### 7.1.3 定义类相关的非成员函数
### 7.1.4 构造函数
### 7.1.5 拷贝、赋值和析构
## 7.2 访问控制和封装
### 7.2.1 友元
## 7.3 其它类特性
### 7.3.1 类成员
### 7.3.2 返回 `*this` 的函数
### 7.3.3 类类型
### 7.3.4 友元再探
## 7.4 类作用域
### 7.4.1 名称查找和类作用域
## 7.5 构造函数再探
### 7.5.1 构造函数初始值列表
### 7.5.2 代理构造函数
### 7.5.3 默认构造函数的角色
### 7.5.4 隐式类类型转换
### 7.5.5 聚合类
### 7.5.6 字面类
## 7.6 static 成员
## 关键概念