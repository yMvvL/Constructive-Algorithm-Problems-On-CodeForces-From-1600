<https://codeforces.com/contest/1334/problem/C>  
这个题我一开始的思路有一些问题，我想的是可能需要给每个怪物都先吃几颗子弹来达到连锁爆炸的效果，但是实际上不用这样  
我们不妨这样考虑，所有的怪物构成了一个环，我们把这个环剪开，那么除了最开始那个怪物，其余的任何一个怪物需要的子弹数都是 $max(0,a_{i+1}-b_i)$  
然后对于最开头的那个怪物，要消灭它需要额外的子弹（因为最开头的不会受到爆炸的波及），这个数是多少呢？不难发现是 $a_i-max(0,a_{i+1}-b_i)$ （也就是它的生命值减去爆炸效果下的子弹数）  
那么我们的目的就很明确了，找到这个式子的最小值就可以了，我们可以逐个遍历一遍，在计算总子弹数的同时更新最小值，最后的结果就是二者之和  
AC代码： 
```C++
void solve() {
    int n;
    std::cin >> n;

    std::vector<long long> a(n), b(n);
    for (int i = 0; i < n; i++) {
        std::cin >> a[i] >> b[i];
    }

    long long ans = 0, mi = LLONG_MAX / 3;
    for (int i = 0; i < n; i++) {
        int idx = (i + 1) % n;
        long long val = std::min(a[idx], b[i]);
        ans += a[idx] - val;
        mi = std::min(mi, val);
    }

    std::cout << ans + mi << "\n";
}

```
