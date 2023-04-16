## Namespace
>当我们在编写 C++ 代码时，可能会使用许多不同的库和函数，这些库和函数可能会使用相同的名称。为了避免这种名称冲突，C++ 提供了命名空间（namespace）的概念。
>命名空间是一种将标识符（如变量、函数、类等）分组并隔离的机制，以避免名称冲突。一个命名空间中包含的标识符在全局范围内是唯一的，不会与其他命名空间或全局标识符产生冲突。
>例如，如果你在自己的程序中使用了一个名为 "foo" 的函数，但是你也使用了一个库，这个库中也定义了一个名为 "foo" 的函数，那么这两个函数的名称就会冲突。但是，如果你将你的 "foo" 函数放入命名空间 "myNamespace" 中，那么你就可以使用 "myNamespace:: foo" 来访问你的函数，而使用 "库名:: foo" 来访问库中的函数。
>使用命名空间可以帮助我们在大型代码库中避免名称冲突，并提供更好的代码可读性和可维护性。

在 C++中有很多东西都放在 `std::` 命名空间里面
- 比如 `std::cout`, `std::cin`, `std::lower_bound`
- 建议不要使用 `using namespace std;`，这不是一个好习惯
一些关于 STL 的小 Tips：
- STL = Standard Template Library（标准模板库）
	- STL 包含了大量的功能（包括常用算法、容器、函数、迭代器等等），其中一些非常常用。
- STL 的 `namespace` 叫做 `std` 而不是 `stl`
	- `std` 是 standard 的缩写
	- 其实我也不知道为什么 STL 的 `namespace` 叫做 `std` 而不是 `stl`
- 使用 `std::` 来访问 STL 中的东西（不要不要不要使用 `using namespace std;`）

## Types
C++有一些基本类型，其实跟 C 差不多的，所以这里我不会介绍得特别详细。
```c++
#include <string>
int val = 5; //32 bits  
char ch = 'F'; //8 bits (usually)  
float decimalVal1 = 5.0; //32 bits (usually)  
double decimalVal2 = 5.0; //64 bits (usually)  
bool bVal = true; //1 bit
std::string str = "Sarah";
```
C++是一个静态类型语言——任何有名字的东西（比如变量或者函数）都需要在运行时前指定好类型。动态类型——任何有名字的东西将会在运行时基于它当前的值来确定类型。
编译型语言与解释型语言最大的不同就是——源代码在什么时候被翻译成机器可以理解的代码。如果是在运行之前被翻译，那么就是编译型语言；如果是在运行时被翻译，那么就是解释型语言。
静态类型可以帮助我们在运行之前就找出一些很明显的 bug，所以我个人还是更喜欢静态类型语言。当然其实动态类型语言也有他适合的用途，这两种类型的语言没有什么高下之分。
## Overloading 重载
如果我们想拥有一个函数的两个版本，分别用来处理不同的输入。也就是说根据函数输入的类型来选择不同的函数版本来执行。就可以使用函数重载的特性。
```c++
int half(int x) {  
	std::cout << “1” << endl; // (1)  
	return x / 2;  
}  
double half(double x) {  
	cout << “2” << endl; // (2)  
	return x / 2;  
}  
half(3) // uses version (1), returns 1  
half(3.0) // uses version (2), returns 1.5
```
我们可以定义两个函数，他们有相同的名字，但是有不同的类型：
```c++
int half(int x) {  
	std::cout << “1” << endl; // (1)  
	return x / 2;  
}  
double half(double x) {  
	cout << “2” << endl; // (2)  
	return x / 2;  
}  
half(3) // uses version (1), returns 1  
half(3.0) // uses version (2), returns 1.5
```
```c++
int half(int x, int divisor = 2) { // (1)  
	return x / divisor;  
}  
double half(double x) { // (2)  
	return x / 2;  
}  
half(4)// uses version (1), returns 2  
half(3, 3)// uses version (1), returns 1  
half(3.0) // uses version (2), returns 1.5
```

## struct
接下来就是来介绍 C 中赫赫有名的结构体了，结构体就是一组变量，他们各自有自己的类型，结构体将它们包装到了一起。
```c++
struct Student {  
	string name; // these are called fields  
	string state; // separate these by semicolons  
	int age;  
};  
Student s;  
s.name = "Sarah";  
s.state = "CA";  
s.age = 21; // use . to access fields
```
有一种更简洁的初始化结构体的方式 `Student s = {"Sarah", "CA", 21};`。

## std:: pair 是一个 STL 内置结构，包含两个任意类型的字段。
-  `std::pair` 是一个 `template` ，你必须为你创建的每个 `pair` 对象指定它的字段类型。
-  `std::pair` 的字段名字分别叫做 `first` 和 `second`。
```c++
struct Pair {  
	fill_in_type first;  
	fill_in_type second;  
};
```
```c++
std::pair<int, string> numSuffix = {1,"st"};  
cout << numSuffix.first << numSuffix.second;  
//prints 1st
```
我们可以使用 `std::pair` 来同时返回是否成功+结果
```c++
std::pair<bool, Student> lookupStudent(string name) {  
	Student blank;  
	if (notFound(name)) return std::make_pair(false, blank);  
	Student result = getStudentWithName(name);  
	return std::make_pair(true, result);  
}  
std::pair<bool, Student> output = lookupStudent(“Julie”);
```
使用 `std::make_pair(field1, field2)` 可以避免自己指定 `pair` 的类型，就像上面代码写到的。
## 使用 auto 进行类型推断
`auto`：用于代替声明变量时的类型，告诉编译器推断该类型。
下面是一个简单的示例
```c++
auto a = 3; // int  
auto b = 4.3; // double  
auto c = ‘X’; // char  
auto d = “Hello”; // char* (a C string)  
auto e = std::make_pair(3, “Hello”);  
// std::pair<int, char*>
```
**`auto` 并不意味着变量没有类型**，它只是说变量的类型由编译器来自行推断。

##总结回顾
- 在代码中，任何有名字的东西都有自己的类型。
- 强类型约束可以帮助我们在程序运行之前就找到部分错误。
- 结构体是可以让我们将很多不同类型的变量绑到一起的一个方式。
- `std::pair` 是 STL 中定义好的一个结构体类型。
- 我们可以通过指定 `std::` namespace 来使用 `std::pair`。
- `auto` 告诉编译器让编译器来自己推导这个变量的类型是什么，这个应该在变量的类型很显然或者类型写起来很复杂的时候用。