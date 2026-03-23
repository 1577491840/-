## abc449
1.D题(经典的关于绝对值的处理方法）  
即区间集合的合并  
```cpp
例如 （l,r)并上(-abs(j), abs(j)) = (L,R)  
L = max(-abs(j),l)  ,  R = min(abs(j),r)  
```
2.E题(树状数组）
```cpp
class BIT {
    int n;
    vector<int> tree;
public:
    BIT(int n_) : n(n_), tree(n_ + 1, 0) {}
    void add(int idx, int delta = 1) {
        ++idx;
        while (idx <= n) {
            tree[idx] += delta;
            idx += idx & -idx;
        }
    }
    int sum(int idx) const {
        int res = 0;
        while (idx > 0) {
            res += tree[idx];
            idx -= idx & -idx;
        }
        return res;
    }
    int lower_bound(int w) const {
        int idx = 0, bit = 1;
        while (bit <= n) bit <<= 1;
        bit >>= 1;
        while (bit) {
            int nxt = idx + bit;
            if (nxt <= n && tree[nxt] < w) {
                w -= tree[nxt];
                idx = nxt;
            }
            bit >>= 1;
        }
        return idx + 1;   // 1‑based 位置
    }
};
```
语法细节：  
tuple：可存储多个不同类型的值  
emplace_back：在容器尾部直接构造元素，避免临时对象，效率更高  
pair 排序：默认先按 first 排序，再按 second 排序。若 second 为字符串，则按字典序
```cpp
// 使用 tuple 和 emplace_back
vector<tuple<int,int,int>> queries;
queries.emplace_back(ok, x, qi);           // 直接构造

// 使用结构化绑定遍历
for (auto [ok, x, qi] : queries) {
    // ...
}

// pair 排序
vector<pair<int,string>> v = {{2,"banana"},{1,"apple"},{2,"cherry"}};
sort(v.begin(), v.end());   // 先按 int 升序，同 int 按 string 字典序
// 结果: (1,"apple"), (2,"banana"), (2,"cherry")
```
3.F题(维护区间并集长度）：线段树/多重集  
线段树：  
```cpp
