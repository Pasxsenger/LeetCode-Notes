# Greedy

## [455. Assign Cookies (Easy)](https://leetcode.com/problems/assign-cookies/)

### Solution 1 (✅)

In this problem, my idea is to sort each vector and use two pointers to count how many children can be contented.

```c++
class Solution {
public:
    int findContentChildren(vector<int>& g, vector<int>& s) {
        sort(g.begin(), g.end());
        sort(s.begin(), s.end());
        int contented = 0, sizeg = g.size(), sizes = s.size();
        for(int indexg = 0, indexs = 0; indexg < sizeg && indexs < sizes; ){
            if(s[indexs] >= g[indexg]){
                indexs++;
                indexg++;
                contented++;
            }
            else    indexs++;
        }
        return contented;
    }
};
```

![image-20230112150142174](Pictures/455-1.png)

----



## [435. Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/)

### Solution 1 (✅)