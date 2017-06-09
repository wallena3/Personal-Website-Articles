在C和C++的函参列表中void的应用



C and C++ have two other ways to declare an argument list. If you have an empty argument list, you can declare it as func( ) in C++,which tells the compiler there are exactly zero arguments. You should be aware that this only means an empty argument list in C++. In C it means “an indeterminate number of arguments (which is a “hole” in C since it disables type checking in that case). In both C and C++, the declaration func(void); means an empty argument list. The void keyword means “nothing” in this case (It can also mean “no type” in the case of pointers).The other option for argument lists occurs when you don’t know how many arguments or what type of arguments you will have,this is called a variable argument list. Defining a function with a variable argument list is significantly more complicated than defining are regular function. You can use a variable argument list for a function that has a fixed set of arguments if (for some reason) you want to disable the error checks of function prototyping. Because of this,you should restrict your use of variable argument lists to C and a void them in C++ (in which, as you’ll learn, there are much better alternatives).

C语言和C++提供两种方法来声明函参列表。如果有一个函参为空的函数，在C++中你可以如此声明：func()，这会准确地告诉编译器这个函数拥有零个函参。然而在C语言中这意味着“一个参数不确定个数的函参列表”（这是C语言的一个缺陷因为如果这样生命，它将禁止类型的检查）。在C语言和C++中，func(void);这种声明都意味着空的函参列表。在这种情况下，void关键字意味着“什么都没有”（在指针的情况下也以为着没有类型）。这样声明的另一种情况是你不知道将要用到多少函数参数，或者不知道将要用到何种类型的参数，这被成为一个可变的参数列表。定义一个可变参数列表的函数明显要比定义一个普通函数复杂。如果处于某些原因，你想要禁止函数原型的错误检查，你可以将可变参数列表应用在一个拥有一系列固定参数的函数中。因此你应当限制可变函数列表在C中以及void在C++中的使用（没错，你将学到更好的代替品）。

通俗的总结，C++不用写void编译器明白是空参数。C不写void的话，编译器会认为参数可以是任意个,这个是C的缺陷。

Reference:
1:C语言编程思想
