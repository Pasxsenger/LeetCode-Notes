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

### Solution 1 (❌)

The key to solving this problem is to search for the `target+1` instead of the `target`.

Here is my code:

```c++
class Solution {
public:
    char nextGreatestLetter(vector<char>& letters, char target) {
        if(target == 'z')
            return letters[0];
        //char greater = target + 1;
        int left = 0, right = letters.size(), middle = right / 2;
        char ans = target + 1;
        while(left < right){
            if(letters[middle] == ans)
                return letters[middle];
            else if(letters[middle] > ans)
                right = middle;
            else
                left = middle + 1;
            middle = left + (right - left) / 2;
        }
        return letters[left];
    }
};
```

But when it comes to this test case:

`letters = ["c","f","j"]`

`target = "a"`

I got `Runtime Error`

<img src="Pictures/744-1.png" alt="744-1" style="zoom:50%;" />

### Solution 2 (✅)

The reason is the first screening condition is not thorough enough.

After changing the first `if` statement, I got this.

```C++
class Solution {
public:
    char nextGreatestLetter(vector<char>& letters, char target) {
        if(target >= letters[letters.size()-1])
            return letters[0];
        //char greater = target + 1;
        int left = 0, right = letters.size(), middle = right / 2;
        char ans = target + 1;
        while(left < right){
            if(letters[middle] == ans)
                return letters[middle];
            else if(letters[middle] > ans)
                right = middle;
            else
                left = middle + 1;
            middle = left + (right - left) / 2;
        }
        return letters[left];
    }
};
```

![744-2](Pictures/744-2.png)



### Solution 3 (✅)

When I was checking others' solutions, I was kinda confident that mine is good enough, until I saw this——[liuchuo](https://leetcode.com/liuchuo/)’s [solution](https://leetcode.com/problems/find-smallest-letter-greater-than-target/solutions/112825/c-2-lines-solution-using-upper-bound/).

Btw, liuchuo is really famous for Chinese ACMers, at least for our team hhhhh. It's so surprised that I saw this name after years but not so surprised that she can give such a good solution, which solved this problem by **<u>ONLY TWO LINES!!!</u>**

```C++
class Solution {
public:
    char nextGreatestLetter(vector<char>& letters, char target) {
        auto it = upper_bound(letters.begin(), letters.end(), target);
        return it == letters.end() ? letters[0] : *it;
    }
};
```

The method `upper_bound()` is the punchline!

![744-3](Pictures/744-3.png)



### Solution 4 (✅)

Hold on a second! Under her solution, there is even a shorter solution, which used **<u>ONLY ONE LINE!!!!!!!</u>**

```c++
class Solution {
public:
    char nextGreatestLetter(vector<char>& letters, char target) {
        return letters[(upper_bound(letters.begin(), letters.end(), target) - letters.begin()) % letters.size()];
    }
};
```

The key is `%`.

![744-2](Pictures/744-2.png)![744-4](Pictures/744-4.png)

---

## [540. Single Element in a Sorted Array (Medium)](https://leetcode.com/problems/single-element-in-a-sorted-array/)

