## 用一个对象构造/赋值另一个对象

**构造 是要创建一个新的对象，而赋值是更新一个已经存在的对象**

```cpp
class A{
  public:
  A(const A& a); // copy constructor
  A& operaotr= (const A& a){return *this;}; // copy assignment operator
  A(A&& a); // move constructor;
  A& operator= (A&& a); // move assignment operator
}
```

```cpp
A a;// 构造函数
A a1 = A() // ==> A a1() 构造函数
A a2 = A(a1) // ==> A a2(a1) 拷贝构造
a2 = a; // 拷贝赋值

A a3(std::move(a)); //移动构造
A a4 = std::move(a);; //移动构造
a4 = std::move(a3); // 移动赋值
```

## 左值和右值

==我的理解：**类型** 和 **左右值**是两个不同维度的东西，调用函数时的函数签名既包含**类型**也包含**左右值**， 两个都对应才能调用==



左值：能取出地址，内存中实际存在，有一个标识符（名称）的值

右值：不能取出地址，通常是临时对象

左值和右值只是语法层面的东西，只在编译期发挥作用，指挥编译器选取对应函数



右值引用是一种**类型**，把右值引用（类型的变量）绑定到一个右值上。

**类型和左右值无关**

函数签名包括参数的类型和左右值

```cpp
A a;
A&& a1 = std::move(a);
// a1和std::move(a)类型都是A&&, 但a1是左值， std::move(a)是右值
```



在传参时，若参数类型为右值引用，表示把这个对象的所有权转移给了调用者。该对象原先的所有者应当不再对该对象操作。

和const &相比， 调用者可以对传入的&&进行修改操作

```cpp
class MyVector{
public:
    explicit MyVector(int n):m_length(n){m_data = new int[n];}
    ~MyVector(){delete[] m_data;}

    MyVector(const MyVector& other){
        m_length = other.m_length;
        m_data = new int[m_length];
        std::copy(other.m_data, other.m_data + other.m_length, m_data);
    }

    MyVector& operator=(const MyVector& other){
        if(this == &other) return *this;
        delete[] this->m_data;
        m_length = other.m_length;
        m_data = new int[m_length];
        std::copy(other.m_data, other.m_data + other.m_length, m_data);
        return *this;
    }

    MyVector(MyVector&& other){
        this->m_data = other.m_data;
        this->m_length = other.m_length;
        other.m_data = nullptr;
        other.m_length = 0;
    }
private:
    int* m_data;
    int m_length;
};
```



## 通用引用（universal reference）和右值引用

```cpp
// 右值引用
A&& a1 = A();


// 通用引用
auto&& a2 = A();

template<typename T>
void f(T&& param);
  

```

通用引用其实是effective c++的作者提出， 用来描述**引用折叠**效果的概念，cpp官方没有这个概念

## std::move和std::forward

### std::move处理右值引用

```cpp
Matrix operator+(const Matrix& lhs, const Matrix& rhs){
  Matrix ret = ...
  return ret;
}

// 调用方式
// Matrix a, b;
// Matrix c = std::move(a) + b;
// a是左值， std::move(a)是右值
// 拷贝给参数 Matrix&& lhs = std::move(a)  移动构造
Matrix operator+(Matrix&& lhs, const Matrix& rhs){
  // lhs是个左值
  lhs = ...
  return std::move(lhs); // std::move(lhs)是个Matrix&&类型的右值
}
// c = 【返回的右值】  移动构造
// 如果是 return lhs; 虽然返回的类型也是Matrix&&，但他是左值，会触发拷贝构造

```

上述 对operator+ 有两个重载，具体调用哪个取决于传入的参数类型



```cpp
void f(std::vector<int>&& v);
std::vector<int> a;
f(a);
f(std::move(a));
```

```cpp
void f(A&&);

std::unique_ptr<A> a = std::make_shared<A>();

```



### std::forward处理通用引用（TODO 系统学了模版元再来补充）

实现输入类T的构造参数，返回T的智能指针

```cpp
template<typename T, typename... Args>
std::shared_ptr<T> make_shared(Args&&... args){
  // args是Args&&...类型的左值
  // 所以下面调用的是T的拷贝构造
  return std::shared_ptr<T>(new T(args...));
}
```

改为

```cpp
template<typename T, typename... Args>
std::shared_ptr<T> make_shared(Args&&... args){
  // 完美转发 args的左/右值属性
  // 传入的是左值就转发左值
  // 传入的是右值就转发右值
  return std::shared_ptr<T>(new T(std::forward<Args>(args)...);
}
```



