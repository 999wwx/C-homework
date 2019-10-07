# Frist_homework
## 2.23

>不能判断指针p是否指向了一个合法的对象。因为不知道p是否已经被初始化。

## 2.24

>void *是特殊的指针，可以存放任意对象的地址。指针lp和i两者类型不一致，所以非法。

## 2.25

>1: ip是一个int类指针型的，i是一个int型的变量，r是一个int型的引用。

>2: i是一个int型的变量，ip是一个空指针。

>3: ip是一个int类型的指针，ip2是一个int类型的变量。

## 2.35

>第一个auto  j  类型为int

>第二个auto  &k 类型为const int &

>第三个auto *p类型为const int *

>第四个auto j2类型为const int

>第五个auto &k2 类型为const int&

程序：
```cpp
#include<bits/stdc++.h>
using namespace std;
int main() {
    const int i = 42;
    auto j = i;
    const auto &k = i;
    auto *p = &i;
    const auto j2 = i, &k2 = i;
    cout << typeid(i).name() << endl;
    cout << typeid(j).name() << endl;
    cout << typeid(k).name() << endl;
    cout << typeid(p).name() << endl;
    cout << typeid(j2).name() << endl;
    cout << typeid(k2).name() << endl;
    return 0;
}
```

## 3.4

