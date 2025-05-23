左值/右值
- 左值指表达式结束后依然存在的对象，可以去地址，可以将右值赋给左值。右值指表达式结束后不存在的临时对象，不可取地址，不能复制给右值。
- 函数返回可以为左值也可以为右值。
- 左值引用的底层实现是指针，非常量左值引用只能绑定到非常量左值，常量左值引用可以绑定到非常量左值，也能绑定到常量左值和右值。
- 右值引用使用&&声明，可以消除两个对象交互时不必要的对象拷贝，能更简明定义泛型函数。
- std::move可将左值转化为右值。
- 多重引用可以折叠，在模板T &&t中，是未定引用类型，既可接受左值，也可接受右值

指针用法
- 指针是一种变量，其值为另一个变量的直接地址，64位计算机中指针占8个字节空间，通过*运算返回位于操作数地址中变量的值。
- 同类型的指针可以比较大小、相减；指针变量可以和整数或常量相加相减；指针变量可以自增自减。
- const修饰的指针为常量指针，指向的内容不能更改。
- 指向函数的指针为函数指针。
- this指针是指向类的当前对象的指针常量。

指针/引用
- 指针保存的是另一个变量的内存地址，引用是引用变量的别名。对编译器来说，指针和引用一样，实际可以将引用视为具有自动间接寻址的常量指针。
- 指针指向的内存空间在程序运行时可变，引用绑定的对象一旦初始化不能改变。
- 指针本身在内存中占空间，引用不占。
- 指针可以悬空，可以有多级；引用必须绑定对象，只能引用一级。

常量指针/指针常量
- 常量指针本质为指针，指向的是常量，不能通过指针修改常量的值，但可以对常量指针重新赋值。
- 指针常量本质是常量，指针本身不能变，但指向的内容是可变的。
- 指向常量的指针常量，指针的指向不可改，指向的内存也不可改。

函数指针
- 本质为指针，指向的是函数的起始地址。
- 对于一般函数fun，指针赋值为fun和&fun是一样的，但sizeof(fun)和siziof(&fun)不一样。

参数传递
- 值传递，形参是实参的拷贝，函数对形参的所有操作不影响实参。
- 指针传递，本质是值传递，拷贝的是指针的值，拷贝后实参与形参是不同的指针，但指向的地址相同，可以修改所指对象的值。
- 引用传递，实参为引用。

迭代器
- 输入迭代器，只能向前单步迭代元素，不允许修改由迭代器引用的元素。
- 输出迭代器，只能向前单步迭代元素，对由该迭代器所引用的元素只有写权限。
- 向前迭代器，可在一个区间读写，拥有输入迭代器所有特性和输出迭代器部分特性。
- 双向迭代器，在向前迭代器基础上增加单步向后迭代的功能。
- 随机访问迭代器，综合4种迭代器的所有功能，还可以像指针那样进行算术计算。
- 在STL中，vector、deque提供随机访问迭代器，list提供双向迭代器，set、map提供向前迭代器。

悬空指针/野指针
- 若指针指向一块内存，当这块内存被释放后，指针依然指向这块内存，这个指针就是悬空指针。对悬空指针再次释放将会出现不可预估的错误。
- 未初始化的指针为野指针，其初始值随机。

强制类型转换
- static_cast，静态转换也即编译期间转换。
- const_cast，强制去掉常量属性。
- reinterpret_cast，改变指针或引用的类型。
- dynamic_cast，程序运行时的转换，会进行类型检查。只能用于带有虚函数的基类或派生类的指针或者引用对象的转换。

类型萃取
- 指的是确定变量去除引用修饰以后的真正变量类型或CV属性。
- cpp类型萃取一般用于模板中，当定义一个模板函数后，需要知道模板类型形参并加以运用时就可以用类型萃取。

结构体相等
- 不可使用memcmp比较，因为其是逐字节对比，而结构体中存在内存对齐现象，填充字节是随机的。
- 需重载运算符==来判断两个结构体是否相等。

模板
- 模板定义以关键字template开始，后跟模板参数列表typename，不能为空。
- 函数模板中编译器会根据实参来推断模板实参，类模板不可，需指明类型，变量模板也需指明特定的类型。
- 函数模板实例化由编译程序在处理函数调用时完成，类模板实例化需在程序中显式指定。函数模板不允许有默认参数，只能全特化，类模板可以有默认参数，可以全特化也可以偏特化。特化指的是在某种特定类型下的具体实现，本质是接管了编译器的工作。
- cpp支持可变参数模板，将可变数目的参数成为参数包，用省略号...指出一个参数表示一个包，当需要知道包中元素数量时用sizeof...函数。可变参数函数通常是递归的。

switch/case
- switch后的{}表示作用域，而不是每一个case对应一个作用域。
- 使用时，每一个case之间互不影响，相对封闭，因此switch的case里不建议定义变量。














