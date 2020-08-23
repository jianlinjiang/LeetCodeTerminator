## trie 树

高效的存储和查询工具。

### 情况一用于存储和查找字符串是否存在
```c++
const int N = 100010;
int trie[N][26], cnt[N], idx = 0;
void insert(char str[]){
    int p = 0;
    for(int i = 0; str[i] ; i++){
        int u = str[i] - 'a';
        if(!trie[p][u]) trie[p][u]=++idx;
        p = trie[p][u];
    }
    cnt[p]++;//表示出现的次数
}
int query(char str[]){
    int p = 0;
    for(int i = 0; str[i]; i++){
        int u = str[i] - 'a';
        if(!trie[p][u]) return 0;
        p = trie[p][u];
    }
    return cnt[p];
}
```

### 情况二 数组中最大的异或对
```c++
class Solution {
public:
    static const int N = 100010;
    int findMaximumXOR(vector<int>& nums) {
        int trie[N][2];
        int cnt[N];
        memset(trie, 0, sizeof trie);
        memset(cnt, 0, sizeof cnt);
        int idx = 0;
        for(int i = 0; i < nums.size(); i++){
            int x = nums[i];
            int p = 0;
            for(int i = 30 ; i >= 0; i--) {
                int b = (x>>i) & (1);
                if(!trie[p][b]) trie[p][b] = ++idx;
                p = trie[p][b];
            }
            cnt[p] = x;
        }
        int ans = 0;
        for(int i = 0; i < nums.size(); i++){
            int x = nums[i];
            int p = 0;
            for(int i = 30; i >= 0; i--){
                int b = (x>>i) & (1);
                int xb = b == 0 ? 1 : 0;
                // if(!trie[p][xb] && !trie[p][xb]) break;
                if(trie[p][xb]) p = trie[p][xb];    //反方向是否存在
                else p = trie[p][b];                //不存在就走相同方向
            }
            int tmp = cnt[p];
            ans = max(ans, tmp ^ x);
        }
        return ans;
    }
};
```