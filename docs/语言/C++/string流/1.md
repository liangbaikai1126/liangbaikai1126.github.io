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

### 4.1 常用介绍

stringstream 类是 istringstream 和 ostringstream 的综合，支持 <<，>> 操作，通常用来数据转换。

构造函数原型：`stringstream::stringstream(string str)`

常用成员函数：
```
str()：使 stringstream 对象返回一个 string 字符串，或者使用其进行初始化。
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
    cout << s << endl;
    return 0;
}
```

输出结果：
```
114.514
```

### 4.2 clear() 的用法介绍

先来看一段代码：
```cpp
#include <iostream>
#include <string>
#include <sstream>
using namespace std;
int main() {
    float num = 114.514;
    stringstream ss("1919810");
    string s;

    ss >> s;
    cout << ss.str() << " ";
    
    //ss.clear();

    ss << num;
    cout << ss.str() << endl;
    return 0;
}
```
编写如上代码时，我们预期的输出结果为：`1919810 114.514 `

实际输出结果为：`1919810 1919810`

我们发现，在后续步骤的`ss << num`并未正确执行。 

原因：在第一次调用完`>>`和`<<`后，来到了 end-of-file 的位置，此时 stringstream 会为其设置一个 eofbit 的标记位，标记其为已经到达 eof。 当 stringstream 设置了 eofbit，任何读取 eof 的操作都会失败。同时，会设置 failbit 的标记位，标记为失败状态。所以后面的操作都失败了。

解决方法：clear 函数的作用就是清除掉所有的 error state 以及流状态，所以在代码前面加一个`ss.clear()`即可达到预期结果。

!!! warning "注意"
    clear 函数的作用并非是清空缓冲区，而是重置流状态。stringstream.str("") 可清空缓冲区，两个函数经常组合使用。

通过以下两个示例程序的对比，可以直观地体现出 clear 函数 和 str("") 的作用。

只调用 clear 函数而不调用 str("")：
```cpp
#include <iostream>
#include <string>
#include <sstream>
using namespace std;
int main() {
    stringstream ss;
    string s1 = "114";
    string s2 = "514";
    
    ss << s1;
    cout << ss.str() << " ";
    
    ss.clear();
    //ss.str("");

    ss << s2;
    cout << ss.str() << " ";
    return 0;
}
```
输出结果：`114 114514` 

同时调用 clear() 和 str("")：
```cpp
#include <iostream>
#include <string>
#include <sstream>
using namespace std;
int main() {
    stringstream ss;
    string s1 = "114";
    string s2 = "514";
    
    ss << s1;
    cout << ss.str() << " ";
    
    ss.clear();
    ss.str("");

    ss << s2;
    cout << ss.str() << " ";
    return 0;
}
```
输出结果：`114 514 `

以上是对 string 流的初步介绍。


