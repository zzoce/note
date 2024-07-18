

lambda表达式\=\=闭包\=\=匿名函数
直接在参数列表写入函数，给别的函数调用，简洁直观

![[Pasted image 20240611185227.png]]

C++ 的 lambda 表达式是一个简洁的方式来定义匿名函数（没有名字的函数），使得函数可以在需要的地方内联定义。lambda 表达式在 C++11 引入，并在 C++14 和 C++17 中进一步增强。它们广泛用于简化代码，特别是在处理 STL 容器和算法时。

### 基本语法

一个 lambda 表达式的基本语法如下：

```cpp
[捕获列表] (参数列表) -> 返回类型 { 函数体 }

```
- **捕获列表（capture list）**：用于指定 lambda 表达式可以捕获哪些外部变量，以及如何捕获这些变量（按值或按引用）。
- **参数列表（parameter list）**：与普通函数的参数列表相同，可以为空。
- **返回类型（return type）**：可选，如果省略，编译器会根据函数体自动推导返回类型。
- **函数体（function body）**：函数的实现部分。

### 示例

以下是一些使用 lambda 表达式的示例：

#### 示例 1：基本 lambda 表达式

```cpp
#include <iostream>

int main() {
    auto greet = []() {
        std::cout << "Hello, World!" << std::endl;
    };
    greet(); // 调用 lambda 表达式
    return 0;
}

```

#### 示例 2：带参数的 lambda 表达式

```cpp
#include <iostream>

int main() {
    auto add = [](int a, int b) -> int {
        return a + b;
    };
    std::cout << add(3, 4) << std::endl; // 输出 7
    return 0;
}

```

#### 示例 3：捕获外部变量

```cpp
#include <iostream>

int main() {
    int x = 10;
    auto printX = [x]() {
        std::cout << "x = " << x << std::endl;
    };
    printX(); // 输出 x = 10
    return 0;
}

```

#### 示例 4：按引用捕获外部变量

```cpp
#include <iostream>

int main() {
    int x = 10;
    auto incrementX = [&x]() {
        x++;
    };
    incrementX();
    std::cout << "x = " << x << std::endl; // 输出 x = 11
    return 0;
}

```

#### 示例 5：捕获所有外部变量

```cpp
#include <iostream>

int main() {
    int x = 10;
    int y = 20;
    
    // 按值捕获所有外部变量
    auto captureAllByValue = [=]() {
        std::cout << "x = " << x << ", y = " << y << std::endl;
    };
    
    // 按引用捕获所有外部变量
    auto captureAllByRef = [&]() {
        x++;
        y++;
        std::cout << "x = " << x << ", y = " << y << std::endl;
    };

    captureAllByValue(); // 输出 x = 10, y = 20
    captureAllByRef();   // 输出 x = 11, y = 21

    return 0;
}

```

####  使用 `this` 捕获

当在类的成员函数中使用 lambda 表达式时，如果需要访问该类的 **==成员变量或成员函数==**，可以使用捕获列表中的 `this` 指针。以下是一个示例，展示了如何使用 `this` 捕获来访问类的成员：
```cpp
#include <iostream>
#include <vector>
#include <algorithm>

class MyClass {
public:
    MyClass(int value) : value(value) {}

    void modifyAndPrintVector(std::vector<int>& vec) {
        // 使用 lambda 表达式捕获 this 指针
        std::for_each(vec.begin(), vec.end(), [this](int& elem) {
            elem += this->value;
        });

        // 打印修改后的向量
        std::for_each(vec.begin(), vec.end(), [](const int& elem) {
            std::cout << elem << " ";
        });
        std::cout << std::endl;
    }

private:
    int value;
};

int main() {
    MyClass myObject(10);
    std::vector<int> vec = {1, 2, 3, 4, 5};

    // 调用类的成员函数
    myObject.modifyAndPrintVector(vec);

    return 0;
}
```
###### 示例解释

1. **定义类 `MyClass` 和构造函数**：
    ```cpp
    class MyClass {
	public:
	    MyClass(int value) : value(value) {}
```
    
2. **成员函数 `modifyAndPrintVector`**：
    
    ```cpp
    void modifyAndPrintVector(std::vector<int>& vec) {
    // 使用 lambda 表达式捕获 this 指针
    std::for_each(vec.begin(), vec.end(), [this](int& elem) {
        elem += this->value;
    });
```
    
    - 在 `modifyAndPrintVector` 函数中，使用 `std::for_each` 遍历和修改向量 `vec` 中的每个元素。
    - 使用捕获列表 `[this]` 来捕获当前对象的指针，使得 lambda 表达式可以访问 `value` 成员。
3. **打印修改后的向量**：
    
    ```cpp
    std::for_each(vec.begin(), vec.end(), [](const int& elem) {
        std::cout << elem << " ";
    });
    std::cout << std::endl;
}
```
    
    - 使用另一个 `std::for_each` 和 lambda 表达式来打印修改后的向量。
4. **在 `main` 函数中创建对象并调用成员函数**：
    
    ```cpp
    int main() {
    MyClass myObject(10);
    std::vector<int> vec = {1, 2, 3, 4, 5};

    // 调用类的成员函数
    myObject.modifyAndPrintVector(vec);
    return 0;
}
```
    
    - 创建 `MyClass` 的对象 `myObject`，并传递初始值 `10`。
    - 调用 `modifyAndPrintVector` 函数，该函数使用 `this` 捕获来访问和修改成员变量 `value`。

######  捕获方式

在捕获列表中，`this` 捕获可以单独使用，也可以与其他捕获方式结合使用。例如：

- 捕获 `this` 指针和其他变量：
    
    ```cpp
    auto lambda = [this, otherVar](int x) { return x + this->value + otherVar; };
```
    
- 捕获所有外部变量按值捕获，并捕获 `this` 指针：
    
    ```cpp
    auto lambda = [=, this](int x) { return x + this->value; };
```
    
- 捕获所有外部变量按引用捕获，并捕获 `this` 指针：
    
    ```cpp
    auto lambda = [=, this](int x) { return x + this->value; };
```
    

通过这种方式，lambda 表达式能够灵活地访问类的成员变量和成员函数，并处理复杂的逻辑。
#### 示例 6：在 STL 算法中使用 lambda 表达式

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

int main() {
    std::vector<int> vec = {3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5};

    // 使用 lambda 表达式进行从大到小排序
    std::sort(vec.begin(), vec.end(), [](int a, int b) {
        return a > b;
    });

    // 输出排序后的结果
    for (const auto& elem : vec) {
        std::cout << elem << " ";
    }
    std::cout << std::endl;

    return 0;
}

```
<mark style="background: #FF5582A6;">注意：</mark>
在 C++ 中，lambda 表达式的语法没有 `auto` 关键字作为标识符。lambda 表达式本身已经是一种自动推导类型的机制，不需要使用 `auto` 关键字。直接使用 lambda 表达式作为 `std::sort` 的第三个参数即可。
### 返回类型推导

在 C++14 中，可以省略 lambda 表达式的返回类型，编译器会自动推导：

```cpp
#include <iostream>

int main() {
    auto add = [](int a, int b) {
        return a + b; // 返回类型自动推导为 int
    };
    std::cout << add(3, 4) << std::endl; // 输出 7
    return 0;
}

```

### 总结

C++ 的 lambda 表达式使得代码更加简洁和易读，特别是在使用标准库算法和回调函数时。通过捕获列表、参数列表和返回类型的灵活定义，lambda 表达式可以处理各种复杂的编程需求。


使用lambda表达式使sort函数按从大到小排序
```cpp
#include <iostream>
#include <vector>
#include <algorithm> // std::sort

int main() {
    std::vector<int> vec = {3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5};

    // 使用 lambda 表达式进行从大到小排序
    std::sort(vec.begin(), vec.end(), [](int a, int b) {
        return a > b;
    });

    // 输出排序后的结果
    for (const auto& elem : vec) {
        std::cout << elem << " ";
    }
    std::cout << std::endl;

    return 0;
}

```

<mark style="background: #FF5582A6;">注意：</mark>
在 C++ 中，lambda 表达式的语法没有 `auto` 关键字作为标识符。lambda 表达式本身已经是一种自动推导类型的机制，不需要使用 `auto` 关键字。直接使用 lambda 表达式作为 `std::sort` 的第三个参数即可。