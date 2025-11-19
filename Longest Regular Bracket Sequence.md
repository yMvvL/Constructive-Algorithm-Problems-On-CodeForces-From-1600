<https://codeforces.com/contest/5/problem/C>  
这道题其实感觉挺板子的，就是一个平衡括号序列，写的时候注意好细节就可以了  
我的思路是这样的：  
开一个 $stack$ （在处理括号相关的问题时很常见）用于记录'('的位置，再开一个标记数组用于标记平衡的序列  
然后遍历字符串，如果 $s_i$ 是'('那就把这个的索引推到 $stack$ 中，如果是')'那就检查一下 $stack$ 里是不是空的，如果不是空的那就说明有'('可以组成一个"()"，此时就标记下这两个括号的位置  
最后遍历一遍标记数组，寻找最长连续序列的长度和数量  
AC代码：  
```C++
void solve() {
    std::string s;
    std::cin >> s;

    int n = s.size();
    if (n < 2) {
        std::cout << 0 << " " << 1 << "\n";
        return ;
    } 

    std::vector<bool> vis(n);
    std::stack<int> stk;
    
    for (int i = 0; i < n; i++) {
        if (s[i] == '(') {
            stk.push(i);
        } else {
            if (!stk.empty()) {
                int idx = stk.top();
                stk.pop();
                vis[i] = true;
                vis[idx] = true;
            }
        }
    }

    std::vector<int> ans;
    int len = 0;
    for (int i = 0; i < n; i++) {
        if (vis[i]) {
            len++;
        } else {
            if (len != 0) {
                ans.push_back(len);
            }
            len = 0;
        }
    }

    if (len > 0) {
        ans.push_back(len);
    }

    if (ans.size() == 0) {
        std::cout << 0 << " " << 1 << "\n";
        return ;
    }
 
    std::sort(ans.rbegin(), ans.rend());

    int cnt = 0;
    for (int i = 0; i < ans.size(); i++) {
        cnt += (ans[i] == ans[0] && ans[i] != 0);
    }
    std::cout << ans[0] << " " << (cnt == 0 ? 1 : cnt) << "\n";
}
```
