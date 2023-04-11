# Binary Search

## Preface

To calculate the middle index, here are two ways:

* `middle = (left + right) / 2`
* `middle = left + (right - left) / 2`

Since the first way may cause `add overflow`, the second way is safer.





## [69. Sqrt(x) (Easy)](https://leetcode.com/problems/sqrtx/description/)

### Solution 1 (✅)

I set the right as the input `x`.

To my surprise, this problem made me stuck for many times.

```C++
class Solution {
public:
    int mySqrt(int x) {
        if(x == 0 || x == 1)
            return x;

        int left = 0, right = x, middle = x / 2;
        while(left < right){
            if(x / middle / middle >= 1 && x / (middle+1) / (middle+1) < 1)
                return middle;
            if(x / middle / middle >= 1)
                left = middle + 1;
                middle = left + (right - left) / 2;
            if(x / middle / middle < 1)
                right = middle - 1;
                middle = left + (right - left) / 2;
        }
        return left;
    }
};
```

![69-1](Pictures/69-1.png)

---

## [744. Find Smallest Letter Greater Than Target (Easy)](https://leetcode.com/problems/find-smallest-letter-greater-than-target/)