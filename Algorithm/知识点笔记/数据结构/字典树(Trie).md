### 字典树(Trie)

字典树又称，$Trie$ 树，单词查找树，主要用于统计、排序、和存储大量的字符串（但不限于字符串）。

<img src="D:\资料\算法笔记\img\trie结构.png" style="zoom:60%;" />

##### 优点

- 利用字符串的公共前缀来减少查询时间，最大限度减少无谓的字符串比较，查询效率比哈系树高。

##### 基本操作

- 创建、查找、插入和删除（极少使用）。

##### 插入代码

时间复杂度 $O(n)$ ，$n$ 为插入字符串的长度。

```cpp
void insert(string s) {
    int len = s.length(), p = 0;
    for (int i = 0; i < len; i ++ ) {
        int ch = s[i] - 'a';
        if (!son[p][ch]) son[p][ch] = ++ idx;
        p = son[p][ch];
    }
    end[p] = true;
}
```

##### 查询代码

复杂度同插入。

```cpp
bool search(string s) {
    int len = s.size(), p = 0;
    for (int i = 0; i < len; i ++ ) {
        p = son[p][s[i] - 'a'];
        if (!p) return false;
    }
    return end[p];
}
```

#### 应用

- 字符串检索。
- 前缀统计。
- 最长公共前缀。
- 排序。
- 作为其他数据结构与算法的辅助。