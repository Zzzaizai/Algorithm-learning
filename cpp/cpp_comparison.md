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
- 
