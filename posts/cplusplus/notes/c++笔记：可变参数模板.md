## 解包

### 递归

```c++
template<typename T>
T addplus(T t) {
    return t;
}

template<typename T, typename... Args
T  addplus(T a, Args ...args) {
    return a + addplus<T>(args...);
}
```

### 初始化列表



## 参考

[泛化之美--C++11可变模版参数的妙用 - qicosmos(江南) - 博客园 (cnblogs.com)](https://www.cnblogs.com/qicosmos/p/4325949.html)

[Variadic templates in C++ - Eli Bendersky's website (thegreenplace.net)](https://eli.thegreenplace.net/2014/variadic-templates-in-c/)

[Parameter pack(since C++11) - cppreference.com](https://en.cppreference.com/w/cpp/language/parameter_pack)

