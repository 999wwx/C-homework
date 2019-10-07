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

```cpp
#include<bits/stdc++.h>
using namespace std;
int main() {
    string a, b;
    cin >> a >> b;
    if(a == b)
        cout << "a、b大小相等";
    else
        cout << max(a, b);
    return 0;
}
```

改写：

```cpp
#include<bits/stdc++.h>
using namespace std;
int main() {
    string a, b;
    cin >> a >> b;
    if(a.size() == b.size())
        cout << "a、b长度相等";
    else if(a.size() > b.size())
        cout << a;
    else
        cout << b;
    return 0;
}
```

## 3.5

```cpp
#include<bits/stdc++.h>
using namespace std;
int main() {
    string s, S;
    while(getline(cin, s))
    {
        S += s;
        cout << S;
    }

    return 0;
}
```

改写：

```cpp
#include<bits/stdc++.h>
using namespace std;
int main() {
    string s, S;
    while(getline(cin, s))
    {
        S += s + ' ';
        cout << S;
    }

    return 0;
}
```

## 3.20

```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    vector<int> a;
    int x,sum=0;
    cin >> x;
    a.push_back(x);
    while(cin >> x)
    {
        a.push_back(x);
        int i=a.size();
        sum=a[i-1]+a[i-2];
        cout << sum << endl;
    }
    return 0;
}
```

修改：

```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    vector<int> a;
    int x;
    while(cin >> x)
    {
        a.push_back(x);
    }
    for(int it=0;it<(int)a.size();it++)
    {
        int i=a.size()-1-it;
        cout << a[it]+a[i];
    }
    return 0;
}
```

## 3.23

```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    vector<int> a;
    int x;
    for(int i=0;i<10;i++)
    {
        cin >> x;
        a.push_back(x);
    }
    for(vector<int>::iterator it=a.begin();it!=a.end();it++)
    {
        *it=*it*2;
        cout << *it << endl;
    }
    return 0;
}
```

## 6.10

```cpp
#include<bits/stdc++.h>
using namespace std;

void my_swap(int *a, int *b)
{
    int t;
    t = *a;
    *a = *b;
    *b = t;
}

int main() {
    int a, b, *pa = &a, *pb = &b;
    cin >> a >> b;
    *pa = a;
    *pb = b;
    my_swap(pa, pb);
    printf("%d %d", *pa, *pb);
    return 0;
}
```

## 6.19

>(a)函数只能传入一个参数，不能传入两个参数。

>(b)合法。

>(c)合法，但精度可能不正确

>(d)合法，但要注意精度丢失的问题

##6.39

>(a)不合法。重复声名了 calc 函数。

>(b)不合法。get()函数参数一致，重复声名了get函数。

>(c)合法。

## 7.16

>访问说明符出现的位置和次数没有限定。
整个程序都可以访问的成员，应该定义在public之后。
可以被类的成员函数访问，但不能使用该类访问的代码访问，并且想隐藏实现细节的代码，应该定义在private之后。

## 7.27

```cpp
#include<iostream>
#include<string>
class Screen{
public:
    typedef std::string::size_type pos;
    Screen() = default;
    Screen(pos ht, pos wd, char c): height(ht), width(wd), contents(ht * wd, c){}
    char get() const
        { return contents[cursor];}
    char get(pos r, pos c) const
        {
            pos row = r * width;
            return contents[row + c];
        }
    Screen &move(pos r, pos c);
    Screen &set(char);
    Screen &set(pos r, pos col, char ch);
    Screen &display(std::ostream &os)
    {
        do_display(os);
        return *this;
    }
    
private:
    pos cursor = 0;
    pos height = 0, width = 0;
    std::string contents;
    void do_display(std::ostream& os) const {
		os << contents;}

};

Screen &Screen::move(pos r, pos c)
{
    pos row = r * width;
    cursor = row + c;
    return *this;
}

Screen &Screen::set(char c)
{
    contents[cursor] = c;
    return *this;
}

Screen &Screen::set(pos r, pos col, char ch)
{
    contents[r * width + col] = ch;
    return *this;
}

int main()
{
    Screen myScreen(5, 5, 'X');
	myScreen.move(4, 0).set('#').display(std::cout);
	std::cout << "\n";
	myScreen.display(std::cout);
	std::cout << "\n";
    return 0;
}
```

## 7.49

>(a)合法

>(b)不合法，Salesdata&类型与Salesdata类型之间不可转换

>(c)不合法，const不对，因为combine本身是需要改变传入参数的

## 7.58

>static double rate = 6.5;不合法 rate不是常量类型。

>static const int vexSize = 20;合法。

>static vector<double> vec(vecSize);不合法 vector是模板不是字面值常量类型。
