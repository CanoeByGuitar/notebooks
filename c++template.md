



# Before Reading

描述modern c++新特性
(1) C++11
* varadic templates  变参模板
* Alias templates   别名模板
* move semantics, rvalue reference and perfect forwarding
* standard type traits
(2)C++14 and C++17
* TODO

why templates?
e.g: std::sort has to support different data type
solution:
(1) duplicate implement
(2) common base type(like Object and void*) and inherit
(3) preprocessor

# Baics
## Function Templates
### intro
```cpp
template<typename T>
T max(T a, T b){
    return a > b ? a : b;
}


int main(){
    int i = 42;
    // :: to ensure max is found in global namespace instead of std::max()
    std::cout << ::max(7, i);

    double f1 = 3.0, f2 = -7.4;
    std::cout << ::max(f1, f2);

    ::max("math", "mathematics");


    std::complex<float> c1, c2; // doesn't have operator <
    ::max(c1, c2); // error

}
```

模板的两次编译（two-phares translate）
```CPP
template<typename T>
void foo(T t){
    undeclared(); // check at first-phase
    undeclared(t); // check at second-phase
    static_assert(sizeof(int) > 10, "int too small");// check at first phase
    static_assert(sizeof(T) > 10,
    "T too small"); // check at second phase
}
```
### template argument deduction
```cpp
template<typename T>
T max(T const& a, T const& b){
    return b < a ? a : b;
}

max(1, 2); // T deduced as int

```
automatic type conversion is limited during type deduction

(1) declare params by reference: must match exactly
T max(T const& a, T const& b);

(2)declare params by value: *decay* type must match.
T max(T  a, T  b);
const/volatile ignored and then
* reference convert to reference
* raw array/function convert to corresponding pointer
```cpp
template<typename T>
T max(T a, T b);
int i = 1;
int const c = 42;
max(i, c); //  T is deduced as int

int& ir = i;
max(i, ir);  // T is deduced as int

int arr[4];
foo(&i, arr); // T is deduced as int*



// error case
max(4, 7.2);
// resolution:
// (1) max(static_cast<double>(4), 7.2)
// (2) max<double>(4, 7.2)
```

### Multiple Template Parameters
```cpp
template<typename T1, typename T2>
T1 max(T1 a, T2 b){
    return b  < a ? a : b;
}// return type is always T1
auto m = ::max(4, 7.2); // OK



// deducing return type

// c++14
template<typename T1, typename T2>
auto max(T1 a, T2 b){
    return b < a ? a  : b;
}

// c++11
template<typename T1, typename T2>
// decltype find return type at compile time
auto max(T1 a, T2 b)->decltype(b < a ? a : b){
    return b < a ? a : b;
}


```



