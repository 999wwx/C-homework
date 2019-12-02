## 9.11
```cpp
#include<iostream>
#include<vector>
int main() {
    std::vector<int> ivec;                               // 默认构造函数
    std::vector<int> ivec1{1, 2, 3};                     // 拷贝列表元素
    std::vector<int> ivec2(ivec1);                       // 拷贝构造
    std::vector<int> ivec3 = ivec1;                      // 拷贝构造
    std::vector<int> ivec4(ivec1.begin(), ivec1.end());  // 指定范围元素的拷贝
    std::vector<int> ivec5(10, 1);                       // 顺序容器相关的构造函数
    return 0;
}
```

## 9.22
> 下面的程序的意思应该是容器中的值和某个值匹配，如果相等的话就在当前位置之前插入一个2倍的值,应该在if之后加一个else { iter++ }

## 9.25
> 如果elem1和elem2相等，将不删除任何元素；

> 如果elem2是尾后迭代器，将删除从elem1之后所有元素；

> 如果两者皆为尾后迭代器，将不删除任何元素；

## 9.29
> vec.resize(100)会在容器末尾增加75个新的初始化元素，和容器类型有关；
  
> vec.resize(10)会丢弃多出的容器后部的15个元素；

## 9.43
```cpp
#include<iostream>
#include<string>
void function (std::string &s, std::string &oldval, std::string &newval) {
    int size = oldval.size();
    for (auto beg = s.begin(); beg <= (s.end() - size); beg ++) {
        if (s.substr(beg - s.begin(), size) == oldval) {
            beg = s.erase(beg, beg + size);
            beg = s.insert(beg, newval.begin(), newval.end());
            beg += (newval.size() - 1);
        }
    }
}
int main() {
    std::string s, oldVal, newVal;
    std::cin >> s;
    std::cin >> oldVal;
    std::cin >> newVal;
    function(s, oldVal, newVal);
    std::cout << s << std::endl;
    return 0;
}
```

## 9.52
```cpp
#include<iostream>
#include<stack>
#include<string>
#include<typeinfo>
int main() {
    std::stack<char> instack;
    std::string s;
    std::cin >> s;
    for (auto s1: s) {
        instack.push(s1);
	if (instack.top() == ')') {
	    std::string s2;
	    while(!instack.empty()) {

	        if (instack.top() != '(' && instack.top() != ')') {
		    s2.insert(s2.begin(), instack.top());
		}
		if (instack.top() == '(') {
		    instack.pop();
		    break;
		}
		instack.pop();

	    }

	    auto d = s2.find_first_of("+-*/");
	    std::string s3 = s2.substr(0, d);
	    std::string s4 = s2.substr(d + 1, s2.size() - d);
	    int d1 = stoi(s3);
	    int d2 = stoi(s4);
	    int result = 0;
	    if (s2[d] == '+') {
	        result = d1 + d2;
	    }
	    if (s2[d] == '-') {
		result = d1 - d2;
	    }
	    if (s2[d] == '*') {
	        result = d1 * d2;
	    }
	    if (s2[d] == '/') {
		result = d1 / d2;
	    }

	    auto result1 = result + '0';
	    instack.push(result1);
	    s2.clear();
        }

    }
    while (!instack.empty()) {
        std::cout << instack.top() << std::endl;
        instack.pop();

    }

    return 0;
}
```

## 10.3
```cpp
#include<iostream>
#include<vector>
#include<numeric>
int main() {
    std::vector<int> ivec{1, 2, 3, 4, 5};
    int sum = accumulate(ivec.begin(), ivec.end(), 0);
    std::cout << sum << std::endl;
    return 0;
}
```

## 10.15
```cpp
[sz](int &a) {return (a + sz);}
```

## 10.34
```cpp
#include<iostream>
#include<vector>
#include<string>
int main() {
    std::vector<std::string> words{"learn", "C++", "every", "day"};
    for (auto it = words.crbegin(); it != words.crend(); it++) {
        std::cout << *it << " ";
    }
    return 0;
}
```

## 10.42
```cpp
#include<list>
#include<iostream>
#include<algorithm>
#include<string>
void elimdups(std::list<std::string> &words) {
    words.sort();
    words.unique();
}
int main() {
    std::list<std::string> sentence{"I", "am", "am", "I", "learn", "C++", "I"};
    elimdups(sentence);
    for_each(sentence.begin(), sentence.end(), [](const std::string &s) { std::cout << s << " "; });
    return 0;
}
```

## 11.12
```cpp
#include<iostream>
#include<utility>
#include<vector>
#include<string>
int main() {
    std::vector<std::pair<std::string, int>> ivec;
    for (int i = 0; i < 3; i++) {
        std::string str;
        int num;
        std::cin >> str >> num;
        std::pair<std::string, int> p(str, num);
        ivec.push_back(p);
    }
    for (auto &c : ivec) {
        std::cout << c.first << " " << c.second << std::endl;
    }
    return 0;
}
```

## 11.17
> (1)第一个调用inserter，创建了一个有关c的迭代器，将v中元素拷贝到c.end()这个迭代器之前，合法；

> (2)第二个调用不合法，multiset不可以用push_back函数；

> (3)第三个调用inserter，创建了一个有关v的迭代器，将c中元素拷贝到v.end()这个迭代器之前，合法；

> (4)第四个调用back_inserter，创建了一个v的迭代器(可以使用push_back函数),合法;

## 11.38
```cpp
#include<iostream>
#include<unordered_map>
#include<string>
int main() {
    std::unordered_map<std::string, int> word_count;
    std::string word;
    while (std::cin >> word) {
        word_count[word]++;
    }
    for (const auto &w : word_count) {
        std::cout << w.first << ":" << w.second << std::endl;
    }
    return 0;
}
```
```cpp
#include<unordered_map>
#include<string>
#include<fstream>
#include<sstream>
#include<iostream>
std::unordered_map<std::string, std::string> buildmap(std::ifstream &map_file) {
    std::unordered_map<std::string, std::string> trans_map;
    std::string key;
    std::string value;
    while (map_file >> key && getline(map_file, value)) {
        if (value.size() > 1) {
            trans_map[key] = value.substr(1);
        }

    }
    return trans_map;
}
const std::string& transform(const std::string &s, const std::unordered_map<std::string, std::string> trans_map) {
    auto trans_s = trans_map.find(s);
    if (trans_s == trans_map.end()) {
        return s;
    }
    else {
        return trans_s->second;
    }
}
void word_transform(std::ifstream &map_file, std::ifstream &input) {
    auto trans_map = buildmap(map_file);
    std::string text;
    while (getline(input, text)) {
        std::istringstream stream(text);
        std::string word;
        bool firstword = true;
        while (stream >> word) {
            if (firstword) {
                firstword = false;
            }
            else {
                std::cout << " ";
            }
            std::cout << transform(word, trans_map);
        }
        std::cout << std::endl;
    }

}
```

## 13.12
> 析构函数调用三次(accum, item1, item2)

## 13.18
```cpp
#ifndef _HOME_DOJCHBEST____C___EMPLOYEE_EMPLOYEE_H_
#define _HOME_DOJCHBEST____C___EMPLOYEE_EMPLOYEE_H_
#include <iostream>
#include <string>
class Employee {
 public:
    explicit Employee(const std::string &name);
    ~Employee() = default;
    static int get_number();

 private:
    std::string Employee_name;
    int Employee_number;
    static int number_now;
};
int Employee::number_now = 1;
Employee::Employee(const std::string &name) {
    Employee_name = name;
    Employee_number = number_now++;
}
int Employee::get_number() {
    return number_now;
}
#endif                                          // _HOME_DOJCHBEST____C___EMPLOYEE_EMPLOYEE_H_
```

## 13.46
> f()的返回值是右值，用&&；

> vi[0]是左值，用&；

> r1是左值，用&；

> vi[0] * f()是右值，用&&；

## 13.49
```cpp
#ifndef STRVEC_H
#define STRVEC_H
#include<string>
#include<iostream>
#include<memory>
#include
class strvec {
public:
    strvec():elements(nullptr), first_free(nullptr), cap(nullptr) {}
    strvec(strvec &&s) noexcept:elements(s.elements), first_free(s.first_free), cap(s.cap) 
                               {s.elements = s.first_free = s.cap = nullptr;}
    strvec(const strvec &);
    strvec &operator=(const strvec&);
    ~strvec();
    void push_back(const std::string&);
    size_t size() const { return first_free - elements; }
    size_t capacity() const { return cap - elements; }
    std::string *begin() const { return elements; }
    std::string *end() const { return first_free; }
private:
    std::string *elements;
    std::string *first_free
    std::string *cap;
    static std::allocator<std::string> alloc;
    void chk_n_alloc() { if (size() == capacity()) reallocate(); }
    std::pair<std::string*, std::string*> alloc_n_copy(const std::string *b, const std::string *c) {
        auto data = alloc.allocate(c - b);
        return (data, uninitialized_copy(b, c, data));
    }
    void free();
    void reallocate();
}
void strvec::push_back(const std::string &s) {
    chk_n_alloc();
    alloc.construct(first_free++, s);
}
void strvec::free() {
    if (elements) {
        for (auto p = first_free; p != elements; )
            alloc.destroy(--p);
    }
}
strvec::strvec(const strvec &s) {
    auto newdata = alloc_n_copy(s.begin(), s.end());
    elements = newdata.first;
    first_free = cap = newdata.second;
}
strvec::~strvec() {
    free();
}
strvec &strvec::operator=(const strvec &rhs) {
    auto data = alloc_n_copy(rhs.begin(), rhs.end());
    free();
    elements = newdata.first;
    first_free = cap = newdata.second;
    return *this;
}
void strvec::reallocate() {
    auto newcapacity = size() ? 2 * size() : 1;
    auto newdata = alloc.allocate(newcapacity);
    auto dest = newdata;
    auto elem = elements;
    for (size_t i = 0; i != size(); i++) {
        alloc.construct(dest++, std::move(*elem++));
    }
    free();
    elements = newdata;
    first_free = dest;
    cap = elements + newcapacity;
}
```

## 14.3
> (a)使用了C++内置版本的==

> (b)使用了string重载的==

> (c)使用了vector重载的==

> (d)使用了strinng重载的==

## 14.20
```cpp
#ifndef SALES_DATA_H
#define SALES_DATA_H
#include<iostream>
#include<string>
class sales_data {
public:
    sales_data() = default;
    sales_data(const std::string &s):bookno(s) { }
    sales_data(const std::string &s, unsigned n, double p):
	       bookno(s), solds(n),revenue(p * n) { }
    std::string isbn() const {return bookno;}
    sales_data& combine(const sales_data &);
    sales_data& operator+=(const sales_data &);
    sales_data& operator+(const sales_data &);
    double avgprice() const { 
        return (solds)? revenue / solds : 0; 
    }
    friend std::istream& operator>>(std::istream &, sales_data &);
    friend std::ostream& operator<<(std::ostream &, const sales_data &);
private:
    std::string bookno;
    unsigned solds = 0;
    double revenue = 0;
            
};
inline std::istream& operator>>(std::istream &in, sales_data &s){
    double price;
    in >> s.bookno >> s.solds >> price;
    if (in)
        s.revenue = s.solds * price;
    else
        s = sales_data();
    return in;
}

inline std::ostream& operator<<(std::ostream &out, const sales_data &s){
    out << s.bookno << " " <<s.solds << " " << s.revenue << std::endl;
    return out;
}
sales_data& sales_data::combine(const sales_data &rhs) {
    solds += rhs.solds;
    revenue += rhs.revenue;
    return *this;
}
sales_data& sales_data::operator+=(const sales_data &rhs) {
    if (bookno == rhs.bookno) {
        solds += rhs.solds;
        revenue += rhs.revenue;
    }
    else {
        std::cout << "不是同一书名不可以进行该操作哦！" << std::endl;
    }
    return *this;
}
sales_data& sales_data::operator+(const sales_data &rhs) {
    *this += rhs;
    return *this;
}
#endif
```

## 14.38
```cpp
#ifndef STR_CMP_H
#define STR_CMP_H
#include<string>
class str_cmp {
public:
    str_cmp(size_t n):size(n) {};
    bool operator()(const std::string &s) {
        return s.size() == size; 
    }
private:
    size_t size;
};
#endif
#include "str_cmp.h"
#include<vector>
#include<iostream>
int main() {
    std::string s;
    std::vector<std::string> words;
    std::vector<size_t> num;
    while (std::cin >> s) {
        if (s[s.size() - 1] == '.' || s[s.size() - 1] == ',' || s[s.size() - 1] == '?' || s[s.size() - 1] == '!') {
            s = s.substr(0, s.size() - 1);
        }
            words.push_back(s);
    }
    for (size_t i = 1; i <= 10; i++) {
        str_cmp cmp(i);
        int sum = 0;
        for (auto it : words) {
            if (cmp(it)) {
                sum++;
            }

        }
        num.push_back(sum);
    }
    for (int i = 0; i < 10; i++) {
        std::cout << "长度为" << i + 1 << "的单词有" << num[i] << "个" << std::endl;
    }
    return 0;
}
```

## 14.52
> 第一个存在二义性；

> 第二个是合理的，使用的是Longdouble operator+(Longdouble&, double);

## 15.12
> 有必要，因为override是表面覆盖了父类的某个函数，而final是为了防止后续的其他类覆盖该函数。

## 15.16
```cpp
class new_quote : public Disc_quote {
public:
    new_quote();
    new_quote(const std::string &book, double price, std::size_t qty, double disc) : Disc_quote(book, price, qty, disc) {}
    double net_price(std::size_t) const override {
        if (n > quantity) {
	    return quantity * (1 - discount) * price + (n - quantity) * price; 
	}
        else {
	    return n * price;
	}
    
    }

};
```

## 15.30
```cpp
class basket {
public:
    void add_item(const std::shared_ptr<Quote> &sale) {
        items.insert(sale);
    }
    void add_item(const Quote& sale) {
        items.insert(std::shared_ptr<Quote>(sale.clone()));
    }
    void add_item(Quote&& sale) {
        items.insert(std::shared_ptr<Quote>(std::move(sale).clone()));
    }
    double total_receipt(std::ostream &os) const {
        double sum = 0.0;
	for (auto iter = items.cbegin(); iter != items.cend(); iter = items.upper_bound(*iter)) {
	    sum += print_total(os, **iter, item.count(*iter));
	}
	os << sum << std::endl;
	return sum;
    }
private:
    static bool cmp(const std::shared_ptr<Quote> &lhs, const std::shared_ptr<Quote> &rhs) {
        return lhs->isbn() < rhs.isbn();
    }
    std::multiset<std::shared_ptr<Quote>, decltype(cmp)*> items { cmp };
};
```
