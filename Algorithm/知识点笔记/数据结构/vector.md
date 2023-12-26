# vector用法

## 1 头文件

```c++
#include <vector>
using namespace std;
```

## 2 初始化

- 注意使用**圆括号**，方括号是开二维的。

```c++
1
vector<type> name(int nSize);
//创建一个名为name的vector,元素个数为 nSize,元素类型为 type

如：
vector<int> a(10);

2
vector<type> name(int nSize, const T& t);
//创建一个名为name的vector
//元素个数为 nSize
//元素类型为 type
//元素的值均为 t

如：
vector<int> a(10, 1);

3  
vector<type> name(const vector<type>&);
//拷贝创建

如：
vector<int> a(b);
//a是b的一份拷贝

vector<int> a(b.begin(), b.begin() + 3);
//a是b前三个元素的拷贝

int b[] = {1, 2, 3, 4, 5};
vector<int> a(b, b + 5);

4    
vector<type> name[N];
// 创建N个vector
// name[0] name[1]来访问    
```

## 3 函数

```c++
vector<int> a;

a.back();   //返回最后一个元素
a.front();  //返回第一个元素
a[i];       //返回a中第i个元素

a.begin();  //返回第一个元素的迭代器指针
a.end();    //返回最后一个的下一个迭代器指针

a.empty();  //判断是否为空，空返回true，不空返回false
a.clear();  //清空a中的元素
a.size();   //返回a的元素个数
a.erase(beg, end);  //删除[beg, end)之间的元素
a.erase(pos);  //删除pos位置的元素

a.insert(pos, elem);  //在pos处插入elem的拷贝
a.push_back(elem);  //在a的最后添加一个elem的拷贝
a.pop_back();    //删除最后一个元素

a.assign(begin, end);  //将[beg, end)的值赋给a
a.assign(n, elem);  //将n个elem的拷贝赋给a

a.swap(b);  //a与b进行整体交换

a == b;  //判断a与b是否相等   与之类似有!= >= <= > <

```

## 4 遍历

```c++
添加元素
    
vector<int> a;
for (int i = 0; i < num; i ++ ) a.push_back(i);

int a[6] = {1, 2, 3, 4, 5, 6};
vector<int> b;
for (int i = 0; i < 6; i ++ ) b.push_back(a[i]);

int a[6] = {1, 2, 3, 4, 5, 6};
vector<int> b;
vector<int> c(a, a + 4);
for (auto it = c.begin(); it < c.end(); it ++) b.push_back(*it);

for (auto t : b) cout << t << endl;
```

## 5 配合algorithm

```c++
sort(a.begin(), a.end());
//  [begin, end)排序

reverse(a.begin(), a.end())
//  [begin, end)倒置
    
lower_bound(a.begin(), a.end(), n);
//在[begin, end)找到第一个 >= n 的值，返回其迭代器指针

upper_bound(a.begin(), a.end(), n);
//在[begin, end)找到第一个 > n 的值，返回其迭代器指针
```

## 6 易错点

- ```c++
  vector<int> a;
  for (int i = 0; i < n; i ++ ) a[i] = i;
  //错误点:
  //1. 下标只能访问存在的元素。
  //2. 不能通过赋值更改内部数据。
  ```

  

## 7 调试代码

```c++
#include <bits/stdc++.h>

using namespace std;

int main()
{
	
//	vector<int> a(5, 0);
//	int b[] = {1, 2, 3, 4, 5};
//	cout << "Before" << endl;
//	for (auto x : a) cout << x << " ";
//	cout << endl;
//	cout << "After" << endl;
//	a.assign(b,b + 5);
//	for (auto x : a) cout << x << " ";
	
//	vector<int> a({1, 2, 3, 4});
//	a.erase(a.begin());
//	for (auto x : a) cout << x << " ";
//	cout << endl;
	
//	vector<int> a(10, 1);
//	find(a.begin(), a.end(), 10);
	
//	int a[6] = {1, 2, 3, 4, 5, 6};
//	vector<int> b;
//	vector<int> c(a, a + 4);
//	for (auto it = c.begin(); it < c.end(); it ++) b.push_back(*it);
//	for (auto x : b) cout << x << " ";
//	cout << endl;
	
//	第一部分
//	int b[] = {1, 2, 3, 4, 5};
//	vector<int> a(b, b + 5);
//	for (auto x : a) cout << x << " ";
//	cout << endl;
	
//	vector<int> b(10, 2);
//	vector<int> a(b.begin(), b.begin() + 3);
//	for (auto x : a) cout << x << " ";
//	cout << endl;
	
//	vector<int> b(10, 2);
//	vector<int> a(b);
//	for (auto x : a) cout << x << " ";
//	cout << endl;
	
//	vector<int> a(10, 1);
//	for (auto x : a) cout << x << " ";
//	cout << endl;
	
//	vector<int> a(10);
//	for (auto x : a) cout << x << " ";
//	cout << endl; 
	
    return 0;
}

```

