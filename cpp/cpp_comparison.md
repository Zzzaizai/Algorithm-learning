cpp11新特性，相较于cpp98
- auto自动类型推导，auto定义的变量必须有初始值。auto关键字声明的变量，不能自动推导出顶层的const或volatile，也不能自动导出引用类型。对于数组，auto自动推导为指针
- decltype类型推导，返回的是变量类型，auto根据等号右侧值推导出变量类型赋给左侧，而decltype根据括号内的变量推导出变量类型，并不赋值，因此不要求初始化。
- lambda表达式，又称lambda匿名函数，格式为：```[capture list](parameter list)->return type{function body};```。如果在lambda函数定义时加上mutable关键字，则lambda函数按值捕获的变量可以被修改。
- 范围for语句，格式为：```for (declaration : expression){statement}```，expression必须是一个序列，拥有返回迭代器begin和end成员，declaration一般用auto声明。
- 右值引用，左右值的根本区别在于是否允许取地址运算以获得内存地址。左右值的引用都是对象存储地址的引用，必须初始化。常量的左值引用可以绑定右值，即：```const int &ref = 20;```；常量或非常量的右值引用只能绑定右值，即：```const int &&ref = 20;```。
- 标准库move函数，可获得绑定到左值上的右值引用。
- 智能指针，取代auto_ptr。
- 使用（default）或禁用（delete）对象的默认函数，如```~A() = default;```表示使用默认的析构函数，```A(const A &) = delete```表示类的对象禁止拷贝构造。
- constexpr，允许用户保证函数或对象的构造函数时编译器常量，修饰函数时其返回类型不能为void，函数体不能声明变量或定义新类型，只能包含声明、null或一段return语句。
- 初始化列表std::initializer_list，初始化列表为常量，一般的函数可以使用初始化列表作为形参。
- nullptr表示空指针，从而将整数0与空指针的概念拆开，避免了函数重调用错误。nullptr的类型为nullptr_t，能隐式转化为任意指针。但如果函数对多个指针类型进行重载，则需要显式声明一个函数来消除歧义。
- 可扩展随机数，linear_congruential可产生整数，速度慢质量差；subtract_with_carry可产生整数和随机数，速度快质量中；mersenne_twister可产生整数，速度快质量好。

cpp14新特性
- 函数返回类型也可使用auto自动推导。返回值为初始化列表、函数为虚函数以及某些递归时不能使用。
- 允许lambda在捕获列表中赋值，并支持定义新变量与初始化。
- constexpr中除了static与thread_local变量外可以声明新变量且必须初始化，可包含条件分支语句、循环语句，可改变一个对象的值，但该对象的生命周期需在函数内部开始。
- deprecated属性允许标记不推荐使用的实体，在编译期间输出警告。
- std::make_unique，cpp11中只有std::make_shared。
- 共享的互斥体和锁，使用std::shared_timed_mutex和std::shared_lock来进行线程同步。

cpp17新特性
- 结构化绑定，可把pair、tuple、array、struct的成员赋值给多个变量。
- if-switch语句可初始化。
- constexpr lambda表达式，用于在编译前计算。
- namespace嵌套。
- std::any，可以存储任何类型，可以转化为任意类型。
- std::basic_string_view，对外部字符串或字符串片段的引用，可访问但不可修改字符串吗，本质是与字符串共享一段空间。
- std::filesystem，便于对文件操作。

c/cpp
- cpp继承了c强大的底层操作特性，又被赋予面向对象机制。现代cpp编译器完全兼容c语言语法。
- cpp支持继承、封装、多态，增加了泛型编程机制（Template），异常处理，引用，运算符重载，标准模板库（STL），命名空间等。
- cpp的类型转换更严格，对类或结构体的成员变量有访问权限控制。
- c常用于直接控制硬件，特别是嵌入式领域。cpp可用于应用层开发，用户界面开发，图形图像编程等领域。

java/cpp
- java移除了指针，以引用代替。移除了运算符重载和多重继承，用接口代替。它不同于一般编译语言或解释性语言，首先将原始码编译为字节码，再依赖各平台的虚拟机解释执行字节码，从而一次编写到处运行。
- java是完全面向对象的语言，所有函数和变量必须是类的一部分。而cpp允许将函数和变量定义为全局的。
- java语言具有垃圾回收机制，无需考虑内存管理问题。
- java不支持多重继承，但允许一个类继承多个接口。不支持运算符重载。
- java中非static方法均可被覆盖，而cpp中用virtual修饰的方法才能被覆盖。
- java标准SDK提供thread类，不支持结构体与联合体。
- java 目前主要用来开发web应用，运行效率不如cpp。

python/cpp
- python为脚本语言，解释执行，不经过编译。
- python在循环外可访问循环内的变量，没有严格限定函数的返回类型。
- cpp运行效率更高，但python代码和cpp代码最终都会编程cpu指令。python开发效率高，语法灵活。
- cpp可直接操作硬件，适合系统编程，python小巧精湛，适合做工具开发与运维。

go/cpp
- go类似c，主要面向过程，不支持继承多态等，可通过结构体含有方法支持面向对象。
- go自带垃圾回收，不支持指针运算，不支持SIMD指令生成，需遵循强类型语言规则，不支持隐式类型转换。
- go不支持函数重载与操作符重载，可使用goroutines与channel进行并发与多线程。

rust/cpp
- rust不支持空指针与悬空指针，只支持函数式编程，没有头文件。
- rust自带内存管理，编译时静态分析保证代码使用内存的安全。
- rust在编译时会分析代码由于并发引起的数据竞争。



