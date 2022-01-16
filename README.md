# C++ Primer 5th 

<br>

```bash
参考资源：https://github.com/applenob/Cpp_Primer_Practice

```
<br>

### P253 ~ P276 类

```C++
1.一个 const 成员函数如果以引用的形式返回 *this ,那么它的返回类型将是常量引用。

const Screen& Screen::display() const; 
```
```C++
2.如果成员是 const ,引用，或者属于某种未提供默认构造函数的类类型，
我们必须通过构造函数初始值列表为这些成员提供初值。
```
```C++
3.在实际中，如果定义类其他构造函数，那么最好也提供一个默认构造函数。
```

<br>
<br>

### P493 ~ P487 拷贝控制

```c++
析构函数
```
```c++
1.隐式销毁一个内置普通指针类型的成员不会 delete 它所指向的对象。
```
```c++
2.当指向一个对象的引用或指针离开作用域时，析构函数不会执行。
```
```c++
4.如果一个类需要自定义析构函数，几乎可以肯定它也需要自定义拷贝赋值运算符
和拷贝构造函数。
```
<br>

```c++
阻止拷贝--删除函数
```
```c++
1.本质上，当不可能拷贝，赋值或销毁类的成员时，类的合成拷贝控制成员就被定义为删除的。
```


<br>

### p490 ~ p523 重载运算与类型转换
#### 基本概念
```C++
1.当一个重载的运算符是成员函数时，this 绑定到左侧运算对象。成员运算符函数的（显式）参数数量比运算对象的数量少一个
```