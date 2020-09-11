# 分割回文串 II
      给定一个字符串 s，将 s 分割成一些子串，使每个子串都是回文串。

      返回符合要求的最少分割次数。
**示例:**
```
输入: "aab"
输出: 1
解释: 进行一次分割就可将 s 分割成 ["aa","b"] 这样两个回文子串。
```

## 解题思路：
### 插入占位符


### 初始化
s ：原始的字符串。


str : s 插入占位符之后得到的字符串。


L ：算法过程中，已知的右端点最靠右的，回文子串的，左端点，初始为 0。


R ：算法过程中，已知的右端点最靠右的，回文子串的，右端点，初始为 -1。


dp ：一个数组。dp[i] 表示以 str[i] 为中心的回文子串的长度.


### 计算dp数组！
+ 从 0 开始枚举 i :
如果 i 不在 [L，R] 中，dp[i] = 1。仅含一个字符的子串肯定是回文的。
如果 i 在 [L，R] 中，那么由对称的特性可知，dp[i] = min((R-i)*2-1, dp[L+R-i])。
使用中心枚举法探测更长的子串，从 dp[i] 开始探测长度。
dp[i]确定后，若 i+dp[i]/2 > R，说明找到了右端点更靠右的子串，那么更新L，R：
+ L = i - dp[i]/2
+ R = i + dp[i]/2
+ 遍历 dp，找到最大的 dp[i]，根据 i 及 dp[i] 可以计算出 str 中最长回文子串的左右端点，然后可以计算出原始字符串 s 中最长回文子串的左右端点。

**时间复杂度**

+ 证明一下Manacher算法的时间复杂度。
当开始计算 dp[i] 时：

如果 i <= R，则可以进行加速，加速之后 dp[i] 标识的回文子串 P[i] 的右端点有两种情况：
+ P[i] 的右端点等于 R。此时需要继续探测。
+ P[i] 的右端点小于 R。此时不需要继续探测了，因为P[i] 完全内含在了 S[L:R] 中，不可能继续扩张了。
如果 R < i，则 dp[i] 要从 1 开始探测。



## 修改后代码（正确）：
```


class Manacher {
    public:
    Manacher(const std::string &s) {
        construct(s);
    };

	void getLongestPalindromeString(int &position, int &length) {
		// 找到最长的回文子串的位置与长度。
        position = -1, length = -1;
        for(int i = 0; i < len.size(); i++) {
            if(len[i] > length) {
                length = len[i];
                position = i;
            }
        }
        // 映射到原始字符串中的位置。
        position = position/2 - length/4;
        length = length/2;
        return;
    }

    // s[L:R] 是否是回文的
    bool isPalindrome(int L, int R) {
        L = L*2 + 1;
        R = R*2 + 1;
        int mid = (L+R)/2;
        if(0 <= mid && mid < len.size() && R-L+1 <= len[mid]) {
            return true;
        }
        return false;
    }

    private:
    vector<int> len;

    void construct(const std::string &s) {
        vector<char> vec;
        // 用 0 作为分隔符
        vec.resize(s.size()*2+1);
        for(int i = 0; i < s.size(); i++) {
            vec[i<<1|1] = s[i];
        }

        int L = 0, R = -1;
        len.resize(vec.size());
        
        for(int i = 0, n = vec.size(); i < n; i++) {
            if(i <= R) { // 被覆盖了，尝试加速
                len[i] = min((R-i)*2+1, len[L+R-i]);
            } else { // 未被覆盖，那就没办法加速了，从 1 开始。
                len[i] = 1;
            }
            // 尝试继续探测
            int l = i - len[i]/2 - 1;
            int r = i + len[i]/2 + 1;
            while(0 <= l && r < vec.size() && vec[l] == vec[r]) {
                --l;
                ++r;
            }
            // 更新
            len[i] = r-l-1;
            if(r > R) {
                L = l+1;
                R = r-1;
            }
        }
    }
};

