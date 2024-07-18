# 动态数组vector

动态数组其容量会随着需求自动增长，当向量元素增加超过当前的存储容量时，会重新分配内存，数组长度增加一倍，也就是说动态数组的长度一定是2^n

	头文件： #include <vector>;
	定义：vector <T> mp;//mp为变量名

## 元素访问

1. begin()，end()：返回向量头尾元素的迭代器
2. cbegin()，cend()：常量迭代器，不可修改数组的内容
3. front()，back()：访问第一个/最后一个元素
4. size()：返回数组长度，capacity()：返回当前存储空间能够容纳的元素数，以2^n方式增长。
5. empyt()：返回数组是否为空，为空则true，反之false
6. at()：访问指定的元素，同时进行越界检查，越界则返回 `std::out_of_range`[[异常]]


## 元素修改
1. push_back()，pop_back()：在末尾添加/删除元素
2. clear ：清楚所有内容，之后size返回值为0，capacity()不变
3. shrink_to_fit ：通过释放未使用的内存减少内存的使用，size返回值不变，capacity()可能会发生改变
4. insert ：插入元素，
5. erase ：擦除元素，参数：pos：指向要移除的元素的迭代器/first, last：要移除的元素范围
6. emplace()，emplac_back()：原位构造元素/在末尾原位构造元素。

## 示例

### 遍历

```cpp
#include <iostream>
#include <vector>
using namespace std;
int main ()
{
    vector <int> mp(20,10);
    for (auto v:mp){
        cout<<v<<" ";
    }
    cout<<endl;
    
    int t=1;
    for (auto &v:mp){
        v=t++;
    }
    for (auto v:mp){
        cout<<v<<" ";
    }
    cout<<endl;
    
    for (vector<int>::iterator it=mp.begin();it!=mp.end();it++){
        *it=1;
        cout<<*it<<" ";
    }
    cout<<endl;

    cout<<endl;
    for (auto it=mp.cbegin();it!=mp.cend();it++){
        //*it=1;//非法
        cout<<*it<<" ";
    }
    cout<<endl;
    return 0;
}
```

### insert插入

```cpp
```cpp
#include <iostream>
#include <vector>

void print_vec(const std::vector<int>& vec)
{
    for (auto x: vec) {
        std::cout << ' ' << x;
    }
    std::cout << '\n';
}

int main ()
{
    std::vector<int> vec(3,100);
    print_vec(vec);

    auto it = vec.begin();
    it = vec.insert(it, 200);
    print_vec(vec);

    vec.insert(it,2,300);
    print_vec(vec);

    // "it" 不再合法，获取新值：
    it = vec.begin();

    std::vector<int> vec2(2,400);
    vec.insert(it+2, vec2.begin(), vec2.end());
    print_vec(vec);

    int arr[] = { 501,502,503 };
    vec.insert(vec.begin(), arr, arr+3);
    print_vec(vec);
}

//输出
100 100 100
200 100 100 100
300 300 200 100 100 100
300 300 400 400 200 100 100 100
501 502 503 300 300 400 400 200 100 100 100
```

### erase擦除

```cpp
#include <vector>
#include <iostream>


int main( )
{
    std::vector<int> c{0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
    for (auto &i : c) {
        std::cout << i << " ";
    }
    std::cout << '\n';

    c.erase(c.begin());

    for (auto &i : c) {
        std::cout << i << " ";
    }
    std::cout << '\n';

    c.erase(c.begin()+2, c.begin()+5);

    for (auto &i : c) {
        std::cout << i << " ";
    }
    std::cout << '\n';
}
//输出
0 1 2 3 4 5 6 7 8 9
1 2 3 4 5 6 7 8 9
1 2 6 7 8 9
```

#### shrink_to_fit

它是减少 capacity() 到 size()非强制性请求。请求是否达成依赖于实现。若发生重分配，则所有迭代器，包含尾后迭代器，和所有到元素的引用都被非法化。若不发生重分配，则没有迭代器或引用被非法化。

```cpp
#include <iostream>
#include <vector>

int main()
{
    std::vector<int> v;
    std::cout << "Default-constructed capacity is " << v.capacity() << '\n';
    v.resize(100);
    std::cout << "Capacity of a 100-element vector is " << v.capacity() << '\n';
    v.resize(50);
    std::cout << "Capacity after resize(50) is " << v.capacity() << '\n';
    v.shrink_to_fit();
    std::cout << "Capacity after shrink_to_fit() is " << v.capacity() << '\n';
    v.clear();
    std::cout << "Capacity after clear() is " << v.capacity() << '\n';
    v.shrink_to_fit();
    std::cout << "Capacity after shrink_to_fit() is " << v.capacity() << '\n';
    for (int i = 1000; i < 1300; ++i)
        v.push_back(i);
    std::cout << "Capacity after adding 300 elements is " << v.capacity() << '\n';
    v.shrink_to_fit();
    std::cout << "Capacity after shrink_to_fit() is " << v.capacity() << '\n';
}
```
### emplace构造元素

在 C++ 中，`std::vector` 是一个非常重要的容器，用于存储动态大小的数组。`std::vector` 提供了多种方法来添加新元素，其中 `emplace` 和 `emplace_back` 方法在性能优化方面尤为重要，因为它们可以直接在容器内存中就地构造元素，避免了不必要的对象复制或移动操作。

#### emplace_back()

`emplace_back` 方法用于在 `std::vector` 的末尾添加一个新元素。与 `push_back` 相比，`emplace_back` 的优势在于它可以直接在容器的末尾就地构造元素，而不是先构造一个临时对象然后复制或移动到容器中。这通过接收构造元素所需的参数，而不是元素本身，来实现。

**示例用法**：

```cpp
#include <vector>
#include <string>

int main() {
    std::vector<std::string> myVector;

    // 使用 push_back，需要创建一个临时的 std::string 对象
    myVector.push_back(std::string("Hello"));

    // 使用 emplace_back，直接在容器内构造 std::string 对象，避免了临时对象的创建和复制
    myVector.emplace_back("World");
}
```

在这个例子中，`emplace_back("World")` 直接在 `myVector` 的末尾构造了一个 `std::string` 对象，这比 `push_back(std::string("Hello"))` 更高效，因为后者需要先构造一个 `std::string` 对象，然后将其复制（或移动）到容器中。

#### emplace()

`emplace` 方法类似于 `emplace_back`，但它提供了更多的灵活性，允许在 `std::vector` 的任意位置就地构造新元素。你需要提供一个指向插入位置的迭代器，以及用于构造新元素的参数。

**示例用法**：

```cpp
#include <vector>
#include <string>

int main() {
    std::vector<std::string> myVector = {"Hello", "World"};

    // 在 vector 的开头插入新元素
    myVector.emplace(myVector.begin(), "First");

    // 现在 myVector 包含 {"First", "Hello", "World"}
}
```

在这个例子中，`emplace(myVector.begin(), "First")` 在 `myVector` 的最前面就地构造了一个新的 `std::string` 对象，将 `"First"` 插入到了 `"Hello"` 和 `"World"` 之前。

### 排序
```cpp
#include <vector>
#include <algorithm> // 包含 std::sort

int main() {
    std::vector<int> vec = {4, 1, 3, 5, 2};
    std::sort(vec.begin(), vec.end()); // 将 vec 排序
    // 现在 vec = {1, 2, 3, 4, 5}
}
```

### 总结

- `emplace_back` 和 `emplace` 都是用于在 `std::vector` 中就地构造元素的方法，避免了不必要的对象复制或移动。
- `emplace_back` 仅在容器末尾添加元素，而 `emplace` 可以在容器的任意位置插入新元素。
- 使用这些方法可以提高程序的性能，特别是在涉及到大量元素插入操作的情况下。


# 链表

STL链表是序列性容器的模板类，它将其元素保持在线性排列中，链式结构，并允许在序列中的任何位置进行有效的插入和删除。
特点：list在任何指定位置动态的添加删除效率不变，时间复杂度为O(1)，操作相比于vector比较方便。但其查找效率为O(n) ，若想访问、查看、读取数据使用vector。

## 元素访问

1. begin()，end()：返回向量头尾元素的迭代器
2. cbegin()，cend()：常量迭代器，不可修改数组的内容
3. front()，back()：访问第一个/最后一个元素
4. size()：返回数组长度，capacity()：返回当前存储空间能够容纳的元素数，以2^n方式增长。
5. empyt()：返回数组是否为空，为空则true，反之false
6. at()：访问指定的元素，同时进行越界检查，越界则返回 `std::out_of_range`[[异常]]


## 元素修改

1. push_back()、push_front()、pop_back()、pop_front()：链表头尾添加、删除。
2. clear ：清楚所有内容，之后size返回值为0
3. insert ：指定迭代器位置插入一个元素，返回的是插入的元素的迭代器。
4. erase ：删除指定迭代器位置的节点，返回的是删除节点的下一个节点的迭代器。
5. remove(const Type& \_Val )，remove_if()：将值为val的所有节点删除/移除满足特定标砖的元素。
6. emplace()，emplace_back()、emplace_front()：原位构造元素/在末尾/在头部原位构造元素。
7. unique()：将连续而相同的节点移除只剩一个。
8. sort()：对链表元素进行排序，默认升序。如果要指定排序规则，需要指定排序规则函数。`bool cmp(Type,Type);` 或 `greater()` 降序，`less()` 升序。
9. merg()：合并两个已排序链表
10. splice()：从另一个list中移动元素
12.  reverse()：链表进行翻转。

## 示例

### 排序

```cpp
#include <list>
#include <iostream>
using namespace std;
bool cmp(int a,int b){
    return a>b;
}
int main() {
    std::list<int> lst = {4, 1, 3, 5, 2};
    lst.sort(cmp); // 使用 list 的 sort 成员函数进行排序
    // 现在 lst = {1, 2, 3, 4, 5}
    for (auto v:lst){
        cout<<v<<" ";
    }
    return 0;
}
```

### emplace构造元素

在 C++ 中，`std::list` 是一个双向链表，提供了高效的元素插入和删除操作。除了常见的 `push_back`、`push_front`、`insert` 等成员函数之外，`std::list` 还提供了 `emplace_back`、`emplace_front` 和 `emplace` 方法。这些 `emplace` 方法用于在容器中直接构造元素，而不是先构造一个临时对象然后拷贝或移动到容器中。这样可以提高效率，尤其是对于那些不支持拷贝操作或拷贝成本较高的对象。

#### emplace_front

- **作用**：在 `std::list` 的开始位置直接构造一个元素。
- **用法**：`emplace_front(args...)`，其中 `args...` 表示构造新元素所需的参数。

#### emplace_back

- **作用**：在 `std::list` 的末尾直接构造一个元素。
- **用法**：`emplace_back(args...)`，其中 `args...` 表示构造新元素所需的参数。

#### emplace

- **作用**：在 `std::list` 中的指定位置前直接构造一个元素。
- **用法**：`emplace(position, args...)`，其中 `position` 表示插入点的迭代器，`args...` 表示构造新元素所需的参数。

####  示例

假设有一个 `Person` 类，我们想要将 `Person` 对象插入到 `std::list` 中：

```cpp
#include <iostream>
#include <list>

class Person {
public:
    std::string name;
    int age;
    Person(std::string n, int a) : name(n), age(a) {}
};

int main() {
    std::list<Person> myList;

    // 在列表前端直接构造元素
    myList.emplace_front("Alice", 30);

    // 在列表末端直接构造元素
    myList.emplace_back("Bob", 25);

    // 在列表中某个位置（这里是开始位置）前直接构造元素
    myList.emplace(myList.begin(), "Carol", 28);

    for (const auto& person : myList) {
        std::cout << person.name << " is " << person.age << " years old." << std::endl;
    }

    return 0;
}
```

#### 总结

使用 `emplace_front`、`emplace_back` 和 `emplace` 方法的优点在于它们可以直接在容器中的指定位置构造对象，避免了额外的复制或移动操作。这在处理大型对象或资源管理密集型对象时特别有用，能够提高程序的效率。
### insert

在 C++ 中，`std::list` 是一个双向链表，提供了灵活的元素插入功能。`std::list` 的 `insert` 成员函数允许在链表的任意位置插入一个或多个元素。这一点与其他顺序容器（如 `std::vector` 和 `std::deque`）相似，但由于 `std::list` 的内部实现是链表，所以在中间位置插入元素的效率较高，不需要移动其他元素。

#### `insert` 函数的重载版本

`std::list` 提供了几个重载版本的 `insert` 函数：

1. **在指定位置前插入单个元素**
   ```cpp
   iterator insert(iterator pos, const T& value);
   ```
   在迭代器 `pos` 指定的位置之前插入一个值为 `value` 的元素，并返回新插入元素的迭代器。

2. **在指定位置前插入单个元素（右值引用版本）**
   ```cpp
   iterator insert(iterator pos, T&& value);
   ```
   类似于上面的函数，但接受一个右值引用，允许插入临时对象，减少拷贝或移动操作。

3. **在指定位置前插入多个相同的元素**
   ```cpp
   void insert(iterator pos, size_type count, const T& value);
   ```
   在迭代器 `pos` 指定的位置之前插入 `count` 个值为 `value` 的元素。

4. **在指定位置前插入另一个容器中的一段元素**
   ```cpp
   template <class InputIterator>
   void insert(iterator pos, InputIterator first, InputIterator last);
   ```
   在迭代器 `pos` 指定的位置之前插入从 `first` 到 `last`（不包括 `last`）的元素。

5. **在指定位置前插入初始化列表中的元素**
   ```cpp
   void insert(iterator pos, std::initializer_list<T> ilist);
   ```
   在迭代器 `pos` 指定的位置之前插入初始化列表 `ilist` 中的所有元素。

#### 示例

下面是一个使用 `std::list::insert` 方法的示例：

```cpp
#include <iostream>
#include <list>

int main() {
    std::list<int> myList = {1, 2, 4, 5};

    // 在 myList 中的第三个位置前插入 3
    auto it = myList.begin();
    std::advance(it, 2); // 将迭代器移动到第三个位置
    myList.insert(it, 3); // 插入元素

    // 在 myList 的末尾插入两个 6
    myList.insert(myList.end(), 2, 6);

    // 输出 myList 的内容
    for (int n : myList) {
        std::cout << n << ' ';
    }
    std::cout << '\n';

    return 0;
}
```

输出将是：
```
1 2 3 4 5 6 6 
```

#### 注意事项

- 使用 `insert` 方法时，迭代器 `pos` 必须是有效的且属于调用 `insert` 的那个 `std::list` 实例。
- 插入操作不会使指向现有元素的迭代器、引用或指针失效，这是 `std::list` 的一个重要特性。这意味着在插入操作之后，你仍然可以使用之前获取的迭代器访问元素，这在使用 `std::vector` 时不总是成立的。
- 插入操作的时间复杂度是常数时间（O(1)），但如果需要插入多个元素，你还需要考虑到获取插入位置迭代器所需的时间。
### erase

```cpp
#include <list>
#include <iostream>
#include <iterator>

int main( ) {
    std::list<int> c{0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
    for (auto &i: c) {
        std::cout << i << " ";
    }
    std::cout << '\n';

    c.erase(c.begin());

    for (auto &i: c) {
        std::cout << i << " ";
    }
    std::cout << '\n';

    std::list<int>::iterator range_begin = c.begin();
    std::list<int>::iterator range_end = c.begin();
    //advance用于改变迭代器位置，n为向后移动距离
    std::advance(range_begin, 2);
    std::advance(range_end, 5);

    c.erase(range_begin, range_end);

    for (auto &i: c) {
        std::cout << i << " ";
    }
    std::cout << '\n';
}
```
### remove

```cpp
#include <list>
#include <iostream>

int main()
{
    std::list<int> l = { 1,100,2,3,10,1,11,-1,12 };

    l.remove(1); // 移除两个等于 1 的元素
    l.remove_if([](int n){ return n > 10; }); // 移除全部大于 10 的元素

    for (int n : l) {
        std::cout << n << ' ';
    }
    std::cout << '\n';
}
```
### unique

```cpp
#include <iostream>
#include <list>

auto print = [](auto remark, auto const& container) {
    std::cout << remark;
    for (auto const& val : container)
        std::cout << ' ' << val;
    std::cout << '\n';
};

int main()
{
    std::list<int> c = {1, 2, 2, 3, 3, 2, 1, 1, 2};
    print("Before unique():", c);
    c.unique();
    print("After  unique():", c);

    c = {1, 2, 12, 23, 3, 2, 51, 1, 2};
    print("Before unique(pred):", c);
    c.unique([mod=10](int x, int y) { return (x % mod) == (y % mod); });
    print("After  unique(pred):", c);
}
```
### merg

```cpp
#include <iostream>
#include <list>

std::ostream& operator<<(std::ostream& ostr, const std::list<int>& list)
{
    for (auto &i : list) {
        ostr << " " << i;
    }
    return ostr;
}

int main()
{
    std::list<int> list1 = { 5,9,0,1,3 };
    std::list<int> list2 = { 8,7,2,6,4 };

    list1.sort();
    list2.sort();
    std::cout << "list1:  " << list1 << "\n";
    std::cout << "list2:  " << list2 << "\n";
    list1.merge(list2);
    std::cout << "merged: " << list1 << "\n";
}
```
### splice

```cpp
#include <iostream>
#include <list>

std::ostream& operator<<(std::ostream& ostr, const std::list<int>& list)
{
    for (auto &i : list) {
        ostr << " " << i;
    }
    return ostr;
}

int main ()
{
    std::list<int> list1 = { 1, 2, 3, 4, 5 };
    std::list<int> list2 = { 10, 20, 30, 40, 50 };

    auto it = list1.begin();
    std::advance(it, 2);

    list1.splice(it, list2);

    std::cout << "list1: " << list1 << "\n";
    std::cout << "list2: " << list2 << "\n";

    list2.splice(list2.begin(), list1, it, list1.end());

    std::cout << "list1: " << list1 << "\n";
    std::cout << "list2: " << list2 << "\n";
}
```
### reverse
```cpp
#include <iostream>
#include <list>

std::ostream& operator<<(std::ostream& ostr, const std::list<int>& list)
{
    for (auto &i : list) {
        ostr << " " << i;
    }
    return ostr;
}

int main()
{
    std::list<int> list = { 8,7,5,9,0,1,3,2,6,4 };

    std::cout << "before:     " << list << "\n";
    list.sort();
    std::cout << "ascending:  " << list << "\n";
    list.reverse();
    std::cout << "descending: " << list << "\n";
}
```

# 队列

## 普通队列

`std::queue` 类是容器适配器，它给予程序员队列的功能——尤其是 FIFO （先进先出）数据结构。类模板表现为底层容器的包装器——只提供特定的函数集合。 queue 在底层容器尾端推入元素，从首端弹出元素。

### 元素访问

1. front()，back()：访问第一个/最后一个元素
2. empty()：检查容器是否为空，为空则true，反之false
3. size ()：返回元素个数

### 元素修改

1. push()：向队尾插入元素
2. pop()：删除队首元素
3. emplace()：在尾部原位构造元素

### 应用
[[有向无环图]]，[[最大连续子序列和]]

## 优先队列
在 C++ 中，优先队列是一种使用堆（通常是二叉堆）实现的抽象数据类型，**==默认是大根堆==**，它支持对元素集合进行排序，使得每次从集合中移除的总是当前集合中最大（或最小）的元素。优先队列在 C++ 标准库中通过模板类`priority_queue` 实现。

### 特点

- **自动排序**：元素在插入时会自动排序，保证队列头部始终是最大元素（或根据自定义比较函数确定的“最优”元素）。
- **高效访问**：访问最大元素的时间复杂度是 O(1)，插入和删除操作的时间复杂度通常是 O(log n)，其中 n 是队列中的元素数量。
- **不支持遍历**：与 `std::vector`、`std::deque` 和 `std::list` 不同，`std::priority_queue` 不提供直接的遍历机制，因为它是基于堆实现的，主要用于访问最大（或最小）元素。

### 常用操作

- **插入元素**：`void push(const T& value);` 将元素 `value` 添加到优先队列中。
- **原位构造元素并排序**：`emplace`原位构造元素并排序
- **移除元素**：`void pop();` 移除队列中的最大元素。
- **访问最大元素**：`const T& top() const;` 返回队列中的最大元素。
- **检查是否为空**：`bool empty() const;` 检查优先队列是否为空。
- **获取元素数量**：`size_type size() const;` 返回优先队列中的元素数量。

### 自定义优先级

`std::priority_queue` 默认使用 `std::less<T>` 作为比较函数，这意味着元素较大的优先级更高。如果你想改变元素的优先级，例如使较小的元素具有更高的优先级，你可以通过传递自定义的比较函数来实现。
#### 重载小于号

[[重载#操作符重载|运算符重载]]，是优先队列变成小根堆
```cpp
bool operator < (const node &a,const node &b){
    return a.dis>b.dis;
}
```

### 示例代码

下面是一个使用 `std::priority_queue` 的示例，展示了如何创建一个优先队列并进行基本操作：

```cpp
#include <iostream>
#include <queue>

int main() {
    std::priority_queue<int> pq;

    // 插入元素
    pq.push(30);
    pq.push(100);
    pq.push(25);
    pq.push(40);

    // 输出最大元素
    std::cout << "Top element: " << pq.top() << '\n';

    // 移除最大元素
    pq.pop();

    // 再次输出最大元素
    std::cout << "Top element after pop: " << pq.top() << '\n';

    // 检查队列是否为空
    if (!pq.empty()) {
        std::cout << "The priority queue is not empty.\n";
    }

    // 输出队列大小
    std::cout << "The size of the priority queue is: " << pq.size() << '\n';

    return 0;
}
```

### 注意事项

- `std::priority_queue` 通常用于需要快速访问最大（或最小）元素的场景，如任务调度、带权图的算法等。
- 如果你需要一个支持随机访问或遍历所有元素的容器，那么 `std::priority_queue` 可能不是最佳选择。在这种情况下，你可以考虑使用其他容器，如 `std::vector`，并使用 `std::make_heap`、`std::push_heap` 和 `std::pop_heap` 算法手动维护堆属性。


### 应用
[[最短路]]，[[最小生成树#prim算法|prim算法]]

## 双端队列

deque没有容量的观念。它是动态以分段连续空间组合而成，一旦有必要在deque的前端和尾端增加新空间，串接在整个deque的头端或尾端，deque的迭代器不是普通的指针，其复杂度比vector复杂的多。除非必要，我们应该尽量选择使用vector而非deque。deque是一种双向开口的连续性空间，可以在头尾两端分别做元素的插入和删除操作。

### 元素访问

1. at()：访问指定的元素，同时进行越界检查，越界则返回 `std::out_of_range`[[异常]]
2. begin()，end()：返回向量头尾元素的迭代器
3. cbegin()，cend()：常量迭代器，不可修改数组的内容
4. front()，back()：访问第一个/最后一个元素
5. size()：返回队列长度，
6. empyt()：返回数组是否为空，为空则true，反之false
7. shrink_to_fit()：通过释放未使用的内存减少内存的使用

### 元素修改

1. push_back()、push_front()、pop_back()、pop_front()：在队列头尾添加、删除。
2. clear ：清楚所有内容，之后size返回值为0
3. insert ：指定迭代器位置插入一个元素，返回的是插入的元素的迭代器。
4. erase ：删除指定迭代器位置的节点，返回的是删除节点的下一个节点的迭代器。
5. emplace()，emplace_back()、emplace_front()：原位构造元素/在末尾/在头部原位构造元素。

### 示例

#### shrink_to_fit

请求移除未使用的容量。它是减少使用内存而不更改序列的大小非强制性请求。请求是否达成依赖于实现。所有迭代器和引用都被非法化。尾后迭代器亦被非法化。

```cpp
#include <deque>

int main() {
    std::deque<int> nums(1000, 42);
    nums.push_front(1);
    nums.pop_front();

    nums.clear();

    // nums 现在不含项目，但它仍保有分配的内存。
    // 调用 shrink_to_fit 可能会释放任何不使用的内存。
    nums.shrink_to_fit();
}
```

### 应用
[[单调队列]]，[[背包#单调队列 优化|单调队列优化]]

# 栈

在 C++ 中，栈（Stack）是一种遵循后进先出（Last In First Out, LIFO）原则的抽象数据类型。C++ 标准库中的 `std::stack` 容器适配器提供了这种数据结构的实现。它允许只在一端（顶端）进行元素的插入和删除操作。

## 栈的基本操作

- **push**：将一个元素压入栈顶。
- **pop**：移除栈顶元素。
- **top**：访问栈顶元素。
- **empty**：检查栈是否为空。
- **size**：返回栈中元素的数量。

## 使用 std::stack

`std::stack` 在内部通常使用 `std::deque` 实现，但也可以指定其他类型的容器（比如 `std::vector` 或 `std::list`）作为其底层容器。

下面是如何使用 `std::stack` 的一个简单示例：

```cpp
#include <iostream>
#include <stack>

int main() {
    std::stack<int> stack;

    // 向栈中压入元素
    stack.push(10);
    stack.push(20);
    stack.push(30);

    // 访问栈顶元素
    std::cout << "Top element: " << stack.top() << std::endl;

    // 移除栈顶元素
    stack.pop();

    // 再次访问栈顶元素
    std::cout << "Top element after pop: " << stack.top() << std::endl;

    // 检查栈是否为空
    if (!stack.empty()) {
        std::cout << "The stack is not empty." << std::endl;
    }

    // 输出栈的大小
    std::cout << "The size of the stack is: " << stack.size() << std::endl;

    return 0;
}
```

## 注意事项

- `std::stack` 不提供遍历其元素的能力。如果你需要遍历栈中的元素，可能需要考虑使用其他容器类型。
- 栈的使用场景包括但不限于：算法中的辅助数据结构（如深度优先搜索中的递归栈）、函数调用栈、语法分析中的符号栈等。

通过使用 `std::stack`，你可以很容易地在 C++ 程序中实现基于 LIFO 原则的算法或功能。

# 集合set

在 C++ 中，`std::set` 是一个基于红黑树实现的容器，它能够自动将元素排序并保持唯一性，即不允许有重复的元素。`std::set` 属于标准模板库（STL）中的关联容器部分，提供了高效的元素查找、插入和删除操作。

## std::set 的特性

- **自动排序**：集合中的元素按照特定的顺序存储，这个顺序是通过元素类型的比较函数确定的，默认使用 `<` 操作符。因此，元素在插入到集合时会自动排序。
- **元素唯一性**：集合中不会有两个相同的元素，每个元素都是唯一的。
- **基于红黑树**：`std::set` 的实现通常基于平衡二叉搜索树（如红黑树），这保证了主要操作（如搜索、插入、删除）的时间复杂度为对数级别 O(log n)。

### 基本操作

- **插入**：`insert()` 方法用于向集合中插入新元素。
- **删除**：`erase()` 方法用于从集合中删除元素。
- **查找**：`find()` 方法用于查找集合中的元素，如果找到，则返回一个指向该元素的迭代器；否则，返回 `end()`。
- **大小**：`size()` 方法返回集合中元素的数量。
- **遍历**：可以 **==使用迭代器遍历集合中的所有元素==**，由于集合是自动排序的，遍历的顺序也是排序后的顺序。
- **原位(提示)插入**：`emplace`、`emplace_hint`，更为高效的插入方式
- **提取**： `extract`从set中移除一个元素，但不销毁，返回一个允许将该元素转移到另一个相同类型的 `set` 中的节点句柄（`node_type`）。这个操作不会复制或移动元素，因此非常高效。***c++17支持***
- **结合**：`merge`将另一个 `std::set` 中的所有元素转移到当前集合中。这个操作对于每个可以转移的元素，都是通过 `extract` 从源集合中移除然后插入到目标集合中实现的，而且这些操作都不涉及元素的复制或移动，因此效率很高。如果目标集合中已经有等价的元素，则源集合中的该元素不会被转移。***c++17支持***
- **查找**：`contsins`，用于检查 `std::set` 中是否包含一个特定的元素。它返回一个布尔值，指示该元素是否存在于集合中。***c++20支持***
- **查找区间**：`equal_range`，获取一个范围，该范围包含所有与指定值相等的元素。由于 `std::set` 中的每个元素都是唯一的，所以这个范围最多包含一个元素。`equal_range` 返回一对迭代器，表示满足条件的元素范围的开始和结束。
- **比较**：`key_comp`返回用于比较键的函数。`value_comp`返回用于在`value_type`类型的对象中比较键的函数

## 示例代码

```cpp
#include <iostream>
#include <set>

int main() {
    std::set<int> mySet;

    // 插入元素
    mySet.insert(30);
    mySet.insert(20);
    mySet.insert(10);
    mySet.insert(40);

    // 尝试插入重复的元素
    mySet.insert(10); // 不会被添加到集合中

    // 遍历集合
    for (int elem : mySet) {
        std::cout << elem << std::endl;
    }

    // 查找元素
    auto search = mySet.find(20);
    if (search != mySet.end()) {
        std::cout << "Found: " << *search << std::endl;
    } else {
        std::cout << "Not found." << std::endl;
    }

    // 删除元素
    mySet.erase(20);

    // 再次查找已删除的元素
    search = mySet.find(20);
    if (search == mySet.end()) {
        std::cout << "20 has been erased." << std::endl;
    }

    return 0;
}
```

这个示例展示了如何使用 `std::set` 来插入、删除、查找和遍历元素。注意到尝试插入重复的元素（在这个例子中是 `10`）不会对集合产生任何影响，因为 `std::set` 保证了元素的唯一性。

### emplac，emplace_hint

#### emplac

`emplace` 函数尝试在 `std::set` 中直接构造元素，而无需先创建临时对象再插入。这是通过将参数直接传递给元素类型的构造函数来实现的，从而避免了额外的复制或移动操作。如果集合中已经有了等价的元素，则不会进行任何操作。

**返回值：**
返回一个 `pair`，其 `first` 成员是指向新插入元素或已存在的等价元素的迭代器，`second` 成员是一个布尔值，指示元素是否被插入（`true` 表示插入了新元素，`false` 表示集合中已存在等价元素）。

#### emplace_hint

`emplace_hint` 函数与 `emplace` 类似，也是用于在集合中直接构造元素。不同之处在于 `emplace_hint` 允许提供一个迭代器作为“提示”位置，表明新元素可能被插入的位置。虽然 `std::set` 最终还是会将元素插入到正确的位置以保持排序，但如果提供的提示接近实际插入位置，那么插入操作可能会更高效。

**返回值：**
返回一个指向新插入元素或已存在的等价元素的迭代器。

#### 代码

```cpp
#include <iostream>
#include <set>

int main() {
    std::set<int> mySet;

    // 使用 emplace 插入元素
    auto result = mySet.emplace(10);
    if (result.second) {
        std::cout << "Inserted: " << *(result.first) << std::endl;
    }

    // 使用 emplace_hint 尝试在指定位置插入元素
    auto it = mySet.emplace_hint(mySet.end(), 20);
    std::cout << "Inserted with hint: " << *it << std::endl;

    // 尝试插入已存在的元素
    result = mySet.emplace(10);
    if (!result.second) {
        std::cout << "Element 10 already exists." << std::endl;
    }

    return 0;
}
```

### extract，merge

从 ***C++17*** 开始，`std::set` 引入了两个新的成员函数：`extract` 和 `merge`。这两个函数提供了对集合元素进行更灵活操作的能力，特别是在涉及到将元素从一个集合转移至另一个集合时。

#### extract函数

`extract` 方法用于从 `std::set` 中移除一个元素，但与 `erase` 不同的是，它不会销毁该元素，而是返回一个允许将该元素转移到另一个相同类型的 `set` 中的节点句柄（`node_type`）。这个操作不会复制或移动元素，因此非常高效。

**语法：**

- 通过指定元素的值来提取：
  ```cpp
  node_type extract(const key_type& x);
  ```
- 通过迭代器来提取：
  ```cpp
  node_type extract(const_iterator position);
  ```

**参数：**

- `x`：要提取的元素的值。
- `position`：指向要提取的元素的迭代器。

**返回值：**

返回一个 `node_type` 对象，它包含被提取的元素。如果在 `std::set` 中没有找到指定的元素，则返回的 `node_type` 对象将为空。

#### merge函数

`merge` 方法用于将另一个 `std::set` 中的所有元素转移到当前集合中。这个操作对于每个可以转移的元素，都是通过 `extract` 从源集合中移除然后插入到目标集合中实现的，而且这些操作都不涉及元素的复制或移动，因此效率很高。如果目标集合中已经有等价的元素，则源集合中的该元素不会被转移。

**语法：**

```cpp
void merge(set& source);
void merge(set&& source);
```

**参数：**

- `source`：源集合，其类型必须与目标集合相同。

**示例代码：**

```cpp
#include <iostream>
#include <set>

int main() {
    std::set<int> set1 = {1, 2, 3, 4};
    std::set<int> set2 = {3, 4, 5, 6};

    // 从 set2 中提取元素 3，并尝试将其插入到 set1 中
    auto node = set2.extract(3);
    if (!node.empty()) {
        set1.insert(move(node));
    }

    // 将 set2 中剩余的元素合并到 set1 中
    set1.merge(set2);

    // 打印 set1 的内容
    for (int elem : set1) {
        std::cout << elem << " ";
    }
    std::cout << "\n";

    // 打印 set2 的内容，应该为空
    for (int elem : set2) {
        std::cout << elem << " ";
    }
    std::cout << "\n";

    return 0;
}
```

这个示例首先从 `set2` 中提取元素 `3` 并尝试将其插入到 `set1` 中。然后，使用 `merge` 函数将 `set2` 中剩余的元素合并到 `set1` 中。注意，合并后 `set2` 会变为空，因为其所有元素都被转移到了 `set1`。

### contsins

从 C++20 开始，`std::set` 类模板引入了 `contains` 成员函数。这个函数提供了一种简单直接的方式来检查一个集合中是否存在给定的元素，使得代码更加清晰易读。

#### contains函数

`contains` 函数用于检查 `std::set` 中是否包含一个特定的元素。它返回一个布尔值，指示该元素是否存在于集合中。

**语法：**

```cpp
bool contains(const Key& key) const;
```

**参数：**

- `key`：要查找的元素的键。

**返回值：**

- 如果集合中存在一个等于 `key` 的元素，则返回 `true`。
- 如果集合中不存在这样的元素，则返回 `false`。

#### 示例

```cpp
#include <iostream>
#include <set>

int main() {
    std::set<int> mySet = {1, 2, 3, 4, 5};

    if (mySet.contains(3)) {
        std::cout << "Set contains 3" << std::endl;
    } else {
        std::cout << "Set does not contain 3" << std::endl;
    }

    if (mySet.contains(6)) {
        std::cout << "Set contains 6" << std::endl;
    } else {
        std::cout << "Set does not contain 6" << std::endl;
    }

    return 0;
}
```

在这个示例中，`contains` 函数被用来检查 `mySet` 是否包含元素 `3` 和 `6`。输出将会是：

```
Set contains 3
Set does not contain 6
```

这个功能之所以有用，是因为它提供了一种比之前版本更简洁的方式来执行这种常见操作。在 C++20 之前，要检查一个元素是否存在，你可能需要使用 `find` 方法并检查返回的迭代器是否等于 `end()` 迭代器，这种方式比直接使用 `contains` 方法要繁琐。

### equal_range

`std::set` 的 `equal_range` 成员函数用于获取一个范围，该范围包含所有与指定值相等的元素。由于 `std::set` 中的每个元素都是唯一的，所以这个范围最多包含一个元素。`equal_range` 返回一对迭代器，表示满足条件的元素范围的开始和结束。

#### equal_range函数

**语法：**

```cpp
pair<iterator,iterator> equal_range(const Key& key) const;
pair<const_iterator,const_iterator> equal_range(const Key& key) const;
```

**参数：**

- `key`：要查找范围的元素的键。

**返回值：**

返回一对迭代器，其中：
- 第一个迭代器指向第一个不小于 `key` 的元素。
- 第二个迭代器指向第一个大于 `key` 的元素。

如果 `set` 中不存在这样的元素，则两个迭代器都会等于 `set::end()`，表示该范围为空。

#### 示例

```cpp
#include <iostream>
#include <set>

int main() {
    std::set<int> mySet = {1, 2, 3, 4, 5};

    auto range = mySet.equal_range(3);

    if (range.first != mySet.end()) {
        std::cout << "Found: " << *range.first << std::endl;
    } else {
        std::cout << "Not found" << std::endl;
    }

    // 尝试查找不存在的元素
    range = mySet.equal_range(6);

    if (range.first != mySet.end()) {
        std::cout << "Found: " << *range.first << std::endl;
    } else {
        std::cout << "Not found" << std::endl;
    }

    return 0;
}
```

在这个示例中，首先尝试查找值为 `3` 的元素。由于 `3` 存在于集合中，`equal_range` 将返回一个范围，其中第一个迭代器指向该元素，而第二个迭代器指向下一个元素（即值为 `4` 的元素）。然后，尝试查找值为 `6` 的元素，由于 `6` 不在集合中，`equal_range` 返回的两个迭代器都将是 `end()`，表示范围为空。

`equal_range` 在需要根据某个值查找元素并获取其在集合中位置时非常有用，尤其是在涉及到范围查询的情况下。虽然在 `std::set` 中使用 `equal_range` 可能看起来没有太大必要，因为每个元素都是唯一的，但这个函数在 `std::multiset` 中更加实用，因为 `std::multiset` 允许存储重复的元素。

### lower_bound和upper_bound

在 C++ 中，`std::set` 是一个基于红黑树实现的有序集合，它提供了 `lower_bound` 和 `upper_bound` 方法来执行范围查询。这两个方法都是用来查找集合中与给定值相关的位置，但它们有细微的差别。

#### lower_bound

`lower_bound` 方法返回一个指向当前集合中第一个**不小于**给定值的元素的迭代器。如果所有元素都小于给定值，则返回指向集合末尾的迭代器。

**语法：**

```cpp
iterator lower_bound(const Key& key);
const_iterator lower_bound(const Key& key) const;
```

**参数：**

- `key`：要查找的键。

**返回值：**

- 返回一个迭代器，指向第一个不小于 `key` 的元素。如果这样的元素不存在，则返回 `end()`。

#### upper_bound

`upper_bound` 方法返回一个指向当前集合中第一个**大于**给定值的元素的迭代器。如果所有元素都不大于给定值，则返回指向集合末尾的迭代器。

**语法：**

```cpp
iterator upper_bound(const Key& key);
const_iterator upper_bound(const Key& key) const;
```

**参数：**

- `key`：要查找的键。

**返回值：**

- 返回一个迭代器，指向第一个大于 `key` 的元素。如果这样的元素不存在，则返回 `end()`。

#### 示例

```cpp
#include <iostream>
#include <set>

int main() {
    std::set<int> mySet = {1, 2, 3, 5, 7, 8};

    auto lb = mySet.lower_bound(4);
    auto ub = mySet.upper_bound(5);

    std::cout << "Lower bound of 4: " << (lb != mySet.end() ? std::to_string(*lb) : "Not Found") << std::endl;
    std::cout << "Upper bound of 5: " << (ub != mySet.end() ? std::to_string(*ub) : "Not Found") << std::endl;

    return 0;
}
```

输出可能如下：

```
Lower bound of 4: 5
Upper bound of 5: 7
```

在这个例子中，`lower_bound(4)` 返回指向值 `5` 的迭代器，因为 `5` 是集合中第一个不小于 `4` 的元素。`upper_bound(5)` 返回指向值 `7` 的迭代器，因为 `7` 是集合中第一个大于 `5` 的元素。

### value_comp和key_comp

在 C++ 中，`std::set` 是一个基于红黑树的有序集合，它自动根据元素的键值进行排序。`std::set` 提供了 `value_comp` 和 `key_comp` 方法，允许用户获取用于元素比较的比较对象，这有助于理解集合是如何维护其有序性的。

#### value_comp

`value_comp` 方法返回一个可用于比较集合中两个元素键值的比较对象。由于 `std::set` 中的键即值（即每个元素即是它自己的键），`value_comp` 返回的比较对象实际上用于比较元素值。

**语法：**

```cpp
value_compare value_comp() const;
```

**返回值：**

- 返回一个比较对象，该对象可用于比较两个元素的键值。

#### key_comp

`key_comp` 方法也返回一个比较对象，这个对象用于集合内部维护元素顺序的比较操作。对于 `std::set` 来说，`key_comp` 和 `value_comp` 实质上是相同的，因为集合中的每个元素都是通过其键值进行排序的。

**语法：**

```cpp
key_compare key_comp() const;
```

**返回值：**

- 返回一个比较对象，该对象可用于比较两个元素的键。

#### 示例

```cpp
#include <iostream>
#include <set>

int main() {
    std::set<int> mySet = {5, 1, 4, 3, 2};

    auto comp = mySet.value_comp(); // 获取比较器

    std::cout << "Set elements in order: ";
    for (int element : mySet) {
        std::cout << element << " ";
    }
    std::cout << std::endl;

    int previous = *mySet.begin();
    for (auto it = ++mySet.begin(); it != mySet.end(); ++it) {
        if (!comp(previous, *it)) {
            std::cout << "Set is not in order according to value_comp." << std::endl;
            return 1;
        }
        previous = *it;
    }

    std::cout << "Set is in order according to value_comp." << std::endl;

    return 0;
}
```

输出：

```
Set elements in order: 1 2 3 4 5 
Set is in order according to value_comp.
```

这个示例展示了如何使用 `value_comp` 来验证 `std::set` 的元素是否确实按照升序排列。由于 `std::set` 的实现保证了元素的有序性，因此这个检查应该总是表明集合是有序的。同样的逻辑也适用于 `key_comp`，因为对于 `std::set` 而言，键比较和值比较是等价的。

# 映射map

在 C++ 中，`std::map` 是一个非常有用的标准库容器，它存储的是键值对，并且每个键都是唯一的。`std::map` 通过红黑树实现，能够保证元素按照键的顺序排序，并提供对元素的高效访问。这里是关于 `std::map` 的一些基本用法和特性：

## 包含头文件

要使用 `std::map`，你需要包含 `<map>` 头文件：

```cpp
#include <map>
```

## 基本用法

创建一个 `std::map` 并添加一些键值对：

```cpp
std::map<std::string, int> ages;
ages["Alice"] = 30;
ages["Bob"] = 25;
ages["Charlie"] = 35;
```

## 访问元素

访问 `std::map` 中的元素可以使用 `operator[]` 或者 `at()` 方法。`operator[]` 在键不存在时会插入一个新元素，而 `at()` 会抛出一个 `std::out_of_range` 异常：

```cpp
int age_of_bob = ages["Bob"];  // 返回 25
int age_of_alice = ages.at("Alice");  // 返回 30

// 如果键不存在，at() 抛出异常
try {
    int age_of_dave = ages.at("Dave");
} catch (const std::out_of_range& e) {
    std::cout << "Dave is not in the map." << std::endl;
}
```

## 插入元素

插入元素可以使用 `insert()` 方法，这个方法不会替换已经存在的键：

```cpp
ages.insert(std::make_pair("Eve", 28));
ages.insert({"Dave", 45});  // 使用初始化列表
```

## 检查键是否存在

要检查一个键是否存在于 `std::map` 中，可以使用 `find()` 方法：

```cpp
if (ages.find("Bob") != ages.end()) {
    std::cout << "Bob is found in the map." << std::endl;
} else {
    std::cout << "Bob is not in the map." << std::endl;
}
```

## 遍历 `std::map`

遍历 `std::map` 的元素：

```cpp
for (const auto& pair : ages) {
    std::cout << pair.first << " is " << pair.second << " years old." << std::endl;
}
```

## 删除元素

删除元素可以使用 `erase()` 方法：

```cpp
ages.erase("Charlie");  // 删除键为 "Charlie" 的元素
```

## insert_or_assign

在 C++17 中引入了 `std::map` 的 `insert_or_assign` 方法，这个方法用于向 `std::map` 中插入一个新元素或者在元素已经存在时更新其值。这个方法提供了一种便捷的方式来确保键值对总是被更新到最新的值，而不需要先检查键是否存在。

### 用法

`insert_or_assign` 方法接受一个键和一个值。如果键已经存在于 `map` 中，它会更新该键对应的值。如果键不存在，它会创建一个新的键值对。方法返回一个 `pair`，其中包含一个迭代器指向元素和一个 `bool` 值，表示是否插入了新元素（`true` 表示插入了新元素，`false` 表示已存在的键被赋予了新值）。

### 示例代码

下面是一个使用 `insert_or_assign` 的示例：

```cpp
#include <iostream>
#include <map>
#include <string>

int main() {
    std::map<std::string, int> ages;

    // 插入新元素
    auto result = ages.insert_or_assign("Alice", 30);
    std::cout << "Inserted Alice: " << result.second << std::endl;  // 输出 true

    // 更新已存在的元素
    result = ages.insert_or_assign("Alice", 35);
    std::cout << "Updated Alice: " << result.second << std::endl;  // 输出 false

    // 遍历 map
    for (const auto& pair : ages) {
        std::cout << pair.first << " is " << pair.second << " years old." << std::endl;
    }

    return 0;
}
```

### 输出

```
Inserted Alice: 1
Updated Alice: 0
Alice is 35 years old.
```

### 优点

使用 `insert_or_assign` 的优点包括：

- **简化代码**：不需要先检查键是否存在再决定是调用 `insert` 还是 `operator[]`。
- **性能优化**：减少了查找键的次数，因为 `insert_or_assign` 在内部管理查找和插入或赋值的过程。

### 注意事项

- 当使用 `insert_or_assign` 更新值时，如果存储的对象类型有复杂的析构和构造过程，可能会引起性能问题，因为它可能涉及到先销毁旧对象再创建新对象的操作。
- 与 `operator[]` 不同，`insert_or_assign` 不仅仅是返回引用，它返回的是一个包含迭代器和布尔值的 `pair`，这可能会使得代码看起来稍微复杂一些，但提供了更多的控制和信息。

这个方法特别适合那些需要频繁更新键值对的场景，可以大大简化代码并提高效率。

## 特性

- **有序性**：`std::map` 中的元素总是根据键排序的。
- **唯一键**：每个键在 `std::map` 中必须是唯一的。
- **效率**：查找、插入和删除操作通常具有对数时间复杂度。

`std::map` 是一个非常强大的数据结构，适用于需要有序和快速查找功能的场景。
