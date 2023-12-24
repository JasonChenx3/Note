#### 字典树

```cpp
// 插入
void insert(string s) {
    int len = s.length(), p = 0;
    for (int i = 0; i < len; i ++ ) {
        int ch = s[i] - 'a';
        if (!son[p][ch]) son[p][ch] = ++ idx;
        p = son[p][ch];
    }
    end[p] = true;
}

// 查询
bool search(string s) {
    int len = s.size(), p = 0;
    for (int i = 0; i < len; i ++ ) {
        p = son[p][s[i] - 'a'];
        if (!p) return false;
    }
    return end[p];
}
```