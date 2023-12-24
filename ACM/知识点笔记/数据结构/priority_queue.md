### 优先队列(priority_queue)

-------

#### 特点

- 优先级高的在队头，优先出队。



#### 头文件及定义

```
#include <queue>

priority_queue<Type, Container, Functional> q;
// 数据类型，容器，排序方式

// 大顶堆
priority_queue<int> q;
priority_queue<int, vector<int>, less<int> > q;

// 小顶堆
priority_queue<int, vector<int>, greater<int> > q;

// 套 pair
priority_queue<pair<int, int> > q;

// 套 string
priority_queue<string> q;
```



#### 常用函数

```
q.top() // 取队头

q.empty() // 判空

q.size() // 元素个数

q.push() // 插队尾

q.pop() // 弹出队头
```


