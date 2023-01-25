# string 流初步认识与运用

## 1 引言
在 C++ 的使用中我们经常用到 IO 操作，让我们回顾一下 IO 库类型和头文件。

- iostream 定义了用于读写流的基本类型。

- fstream 定义了读写命名文件的类型。

- sstream 定义了读写内存 string 对象的类型。


<center>

|头文件 | 类型 |
| :---: | :--- |
|iostream	| istream，wistream 从流读取数据<br> ostream，wostream 向流写入数据<br> iostream，wiostream 读写流|
|fstream	| ifstream，wifstream 从文件读取数据<br> ofstream，wofstream 向文件写入数据<br> fstream，wfstream 读写文件|
|sstream	| istringstream，wistringistream 从 string 读取数据<br> ostringstream，wostringstream 向string写入数据 <br> stringstream，wstringstream 读写string|

</center>

本文主要对 istringstream、ostringstream 和 stringstream 进行初步介绍。

## 2 istringstream 类

istringstream 类从 string 中提取数据，支持 >> 操作，默认分割符为空格。

构造函数原型：`istringstream::istringstream(string str)`

常用成员函数：`str()：使 istringstream 对象返回一个 string 字符串`

示例程序：
```cpp
#include <iostream>
#include <string>
#include <sstream>
using namespace std;
int main() {
    string sentence = "Study Hard and Day Day Up";
    istringstream iss(sentence);
    string temp;
    // 可用 while(iss >> t) 代替，默认空格分割
    while(getline(iss, temp, ' ')) {//此处手动指定用空格分隔
        cout << iss.str() << "#";
        cout << temp << endl;
    }
    return 0;
}
```

输出结果：
```
Study Hard and Day Day Up#Study
Study Hard and Day Day Up#Hard 
Study Hard and Day Day Up#and  
Study Hard and Day Day Up#Day  
Study Hard and Day Day Up#Day  
Study Hard and Day Day Up#Up 
```

## 3 ostringstream 类

ostringstream 类将其他类型数据往 string 中写入，支持 << 操作，默认分割符为空格。

构造函数原型：`ostringstream::ostringstream(string str)`

常用成员函数：`str()：使 ostringstream 对象返回一个 string 字符串，或者使用其进行初始化`

示例程序：
```cpp
#include <iostream>
#include <string>
#include <sstream>
using namespace std;
int main() {
    float num = 114.514;
    ostringstream oss;
    oss << num;
    cout << oss.str() << endl;
    oss.str("1919810");
    cout << oss.str() << endl;
    return 0;
}
```

输出结果：
```
114.514
1919810
```

## 4 stringstream 类

stringstream 类是 istringstream 和 ostringstream 的综合，支持 <<，>> 操作，通常用来数据转换。

构造函数原型：`stringstream::stringstream(string str)`

常用成员函数：
```
str()：使 stringstream 对象返回一个 string 字符串，或者使用其进行初始化。
clear()：多次使用 stringstream 需要对其进行清空，否则会输出错误结果。
```
示例程序：
```cpp
#include <iostream>
#include <string>
#include <sstream>
using namespace std;
int main() {
    float num = 114.514;
    stringstream ss;
    string s;
    ss << num;
    ss >> s;
    //不能使用ss.str("")进行清空；否则结果不变
    ss.clear();
    ss.str("1919810");
    cout << s << endl;
    cout << ss.str() << endl;
    return 0;
}
```

输出结果：
```
114.514
1919810
```


