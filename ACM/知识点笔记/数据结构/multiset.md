```cpp
#include <set>  // 头文件

/*
实现方式是红黑树，复杂度是logn.
*/

multiset<int> S;  // 定义，默认升序

// 操作
S.size();  // 返回集合内元素数量
S.empty();  // 返回集合是否为空的布尔值

S.count(x);  // 返回x的个数
S.find(x);  // 返回等于x的迭代器，没有则返回end()
S.lower_bound(x);  // 返回第一个>=x的迭代器，没有则返回end()
S.upper_bound(x);  // 返回第一个>x的迭代去，没有则返回end()

S.begin();  // 头迭代器
*S.begin(); // 第一个元素
S.rbegin();  // 最后一个元素的迭代器
*S.rbegin();  // 最后一个元素

S.insert(x); // 插入x
S.erase(x); // 将所有的x删除
S.erase(it);  // 将迭代器所指元素删除

// 删除一个元素
void remove(int x) {
    auto it = S.find(x);
    S.erase(it);
}
```

