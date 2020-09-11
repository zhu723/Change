# 只出现一次的数字 II


给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现了三次。找出那个只出现了一次的元素。

**说明：**

你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？

```
**示例 1:**

输入: [2,2,3,2]
输出: 3
**示例 2:**

输入: [0,1,0,1,0,1,99]
输出: 99

```

![分析](https://pic.leetcode-cn.com/0381991a29b66a5eb4f1528c6c50cd6148a1284d1ea80320351443161cf1867c-2.jpg)
![分析](https://pic.leetcode-cn.com/d7fdbce1a5c3ea1cfb3bc5bce019860e8320a18151ea9b30b5ae97428aabbbd8-3.jpg)

## 代码（修改后):

**逐位考虑**
```

class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int res = 0;
        for (int i = 0; i < 32; ++i) {
            int cnt = 0;
            for (auto x : nums) {
                cnt += (x>>i)&1;
            }
            res |= (cnt%3)<<i;
        }
        return res;
    }
};

```


**位运算**
```

class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int once = 0, twice = 0;
        for (auto x : nums) {
            once = (once^x)&(~twice);
            twice = (twice^x)&(~once);
        }
        return once;
    }
};
```
