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
        cout << temp << "#";
    }
    return 0;
}
```

输出结果：`Study#Hard#and#Day#Day#Up#`