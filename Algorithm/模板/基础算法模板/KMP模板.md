### 代码（下标从0开始）

$next[j]$ ：串的长度为 $j$ 时，相等前缀和后缀的==最大长度==。

规定 $next[0] = -1$ 。

由含义知：

- $next[1] = 0$ ，前缀和后缀不能包括自身。



```cpp
void get_ne(string t) {
    int j = 0, k = -1;
    ne[0] = -1;
    while (j < t.size()) {
        while (k != -1 && t[j] != t[k]) k = ne[k];
        ne[++ j] = ++ k;
    }
}

int kmp(string s, string t, int pos) {
    int i = pos, j = 0, slen = s.size(), tlen = t.size();
    
    while (i < slen && j < tlen) {
        if (j == -1 || s[i] == t[j]) i ++ , j ++ ;
        else j = ne[j];
        
        if (j == tlen) return i - tlen + 1;  // 返回匹配成功时的下标
    }
    
    return -1;  //匹配失败
}
```





### 代码（下标从1开始）

$next[i]$ ：在匹配串中，以 $t[i]$ 结尾的后缀，能够匹配从 $1$ 开始的非平凡前缀的的最大长度。

```c++
// s 自身求 next[]
void get_next()
{
    // 因为 ne[1] = 0，所以i从2开始循环。
    for (int i = 2, j = 0; i <= n; i ++ )
    {
        while (j && s[i] != s[j + 1]) j = ne[j];
        
        if (s[i] == s[j + 1]) j ++ ;
        
        ne[i] = j;
    }
}

// s母串 p匹配串
void get_next()
{
    for (int i = 1, j = 0; i <= n; i ++ )
    {
        while (j && s[i] != p[j + 1]) j = ne[j];
        
        if (s[i] == p[j + 1]) j ++ ;
        
        ne[i] = j;
    }
}

// 匹配过程
// s 母串 p匹配串
for (int i = 1, j = 0; i <= n; i ++ )
{
    while (j && s[i] != p[j + 1]) j = ne[j];
    
    if (s[i] == p[j + 1]) j ++ ;
    
    if (j == m)
    {
        // 匹配成功
        j = ne[j]; // 顺着上一次的接着匹配
    }
}
```