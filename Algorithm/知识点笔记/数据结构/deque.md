### deque (双端队列)

------

#### 1 头文件及定义

```c++
#include <deque>

deque<Type> q;

deque<Type> q(dq); // 拷贝构造
```



#### 2 常用函数

```c++
q.size() // 大小
    
q.empty(); // 判空

q.clear(); // 清空

q.push_front(); // 头插

q.push_back(); // 尾插

q.pop_front(); // 弹出队头

q.pop_back(); // 弹出队尾

q.at(i); // 取出第i个元素的引用
```

