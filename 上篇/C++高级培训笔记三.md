## C++中的explicit ##
C++中， 一个参数的构造函数(或者除了第一个参数外其余参数都有默认值的多参构造函数)， 承担了两个角色。 1 是个构造器 ，2 是个默认且隐含的类型转换操作符。

所以， 有时候在我们写下如 AAA = XXX， 这样的代码， 且恰好XXX的类型正好是AAA单参数构造器的参数类型， 这时候编译器就自动调用这个构造器， 创建一个AAA的对象。

这样看起来好象很酷， 很方便。 但在某些情况下（见下面权威的例子）， 却违背了我们（程序员）的本意。 这时候就要在这个构造器前面加上explicit修饰， 指定这个构造器只能被明确的调用/使用， 不能作为类型转换操作符被隐含的使用。

explicit构造函数是用来防止隐式转换的。请看下面的代码：

	class Test1
	{
	public:
	    Test1(int n)
	    {
	        num=n;
	    }//普通构造函数
	private:
	    int num;
	};
	class Test2
	{
	public:
	    explicit Test2(int n)
	    {
	        num=n;
	    }//explicit(显式)构造函数
	private:
	    int num;
	};
	int main()
	{
	    Test1 t1=12;//隐式调用其构造函数,成功
	    Test2 t2=12;//编译错误,不能隐式调用其构造函数
	    Test2 t2(12);//显式调用成功
	    return 0;
	}

Test1的构造函数带一个int型的参数，代码23行会隐式转换成调用Test1的这个构造函数。而Test2的构造函数被声明为explicit（显式），这表示不能通过隐式转换来调用这个构造函数，因此代码24行会出现编译错误。

普通构造函数能够被隐式调用。而explicit构造函数只能被显式调用。

## 2.虚函数 ##
![](https://i.imgur.com/rLgBzuh.png)

## 3.静态static ##
![image](https://github.com/RainbowLLL/Cpp_houjie/blob/master/slides_imgs/static.PNG)
在类中，数据或者函数前面加了static，它就成为静态成员。
通过c1的地址来调用成员函数，complex::real(&c1)

成员函数在内存中是只有一份的，为了不同的对象中去调用，会传入this指针。

如果成员加了static关键字，它就脱离了对象。静态函数依然只有一份，静态成员变量也只有一份。例如，银行很多人账户的利率，都是相同的。

静态成员函数和一般成员函数的区别在于，静态函数没有this指针，它只能处理静态数据。

![image](https://github.com/RainbowLLL/Cpp_houjie/blob/master/slides_imgs/static_account.PNG)
静态成员变量必须在类外进行定义，黄色的行。

通过类名来调用静态函数，或者通过对象调用（此时，不会传入this指针）

## 4.单例 ##
(单键，单体，单例)
一种设计模式Singleton，一个类只希望创建一个对象

![imgae](https://github.com/RainbowLLL/Cpp_houjie/blob/master/slides_imgs/singleton2.PNG)

不想让外界创建A，把它的构造函数放在private

外界可能通过静态函数得到唯一的A

![imgae](https://github.com/RainbowLLL/Cpp_houjie/blob/master/slides_imgs/singleton1.PNG)
更好的写法，上一写法，即使无人使用A,单例也会存在，用下面这种写法，如果没有人使用，A就不存在，区别是把静态变量a放到了getInstance中
