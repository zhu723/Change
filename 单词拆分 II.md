# 单词拆分 II
给定一个非空字符串 s 和一个包含非空单词列表的字典 wordDict，在字符串中增加空格来构建一个句子，使得句子中所有的单词都在词典中。返回所有这些可能的句子。

**说明：**
+ 分隔时可以重复使用字典中的单词。
+ 你可以假设字典中没有重复的单词。

**示例 1:**
```
输入:
s = "catsanddog"
wordDict = ["cat", "cats", "and", "sand", "dog"]
输出:
[
  "cats and dog",
  "cat sand dog"
]
**示例 2：**

输入:
s = "pineapplepenapple"
wordDict = ["apple", "pen", "applepen", "pine", "pineapple"]
输出:
[
  "pine apple pen apple",
  "pineapple pen apple",
  "pine applepen apple"
]
解释: 注意你可以重复使用字典中的单词。
**示例 3：**

输入:
s = "catsandog"
wordDict = ["cats", "dog", "sand", "and", "cat"]
输出:
[]
```

**解题：**
 递归
 
 ```
class Solution {
public:
    vector<string> wordBreak(string s, vector<string>& wordDict) {
        unordered_map<string, vector<string>> umap;
        return helper(s, wordDict, umap);
    }
    vector<string> helper(string s, vector<string>& wordDict, unordered_map<string, vector<string>> &umap) {
        if (umap.count(s) > 0) {
            return umap[s];
        }
        if (s.empty()) {
            return {""};
        }
        vector<string> res;
        for (string word : wordDict) {
            if (s.substr(0, word.size()) != word) {
                continue;
            }
            vector<string> rem = helper(s.substr(word.size()), wordDict, umap);
            for (auto str : rem) {
                res.push_back(word + (str.empty() ? "" : " ") + str);
            }
        }
        return umap[s] = res;
    }
};



