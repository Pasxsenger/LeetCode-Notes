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

![455-1](Pictures/455-1.png)

----



## [435. Non-overlapping Intervals (Medium)](https://leetcode.com/problems/non-overlapping-intervals/)

### Solution 1 (✅)

I had no clue so I searched some solutions. According to [mrityujha](https://leetcode.com/mrityujha/)'s [solution](https://leetcode.com/problems/non-overlapping-intervals/solutions/792726/c-simple-o-nlogn-solution-with-explanation/comments/725788), I sorted the `intervals` by the **start points**.

By the way, through this problem, especially the `cmp()`, I reviewed how C++ functions **pass parameters**. Basically, there are 3 ways: `pass-by-value`, `pass-by-address(*)`, `pass-by-reference(&)(recommended)`

```c++
class Solution {
public:
    static bool cmp(vector<int> &a, vector<int> &b) {
        return a[0] < b[0];
    }
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {
        int N = intervals.size();
        if(N == 0)  return 0;

        sort(intervals.begin(), intervals.end(), cmp);
        int removed = 0, currEnd = intervals[0][1];
        for(int i = 1; i < N; i++){
            if(intervals[i][0] < currEnd){
                removed++;
                currEnd = min(currEnd, intervals[i][1]);
            }
            else    currEnd = intervals[i][1];
        }
        return removed;
    }
};
```

![435-1](Pictures/435-1.png)



### Solution 2 (✅)

However, many solutions said that the proper way is to sort `intervals` by the **end points**. And the reason is that if we pick the one ends earlier, we can get more capacity for rest intervals.

```c++
class Solution {
public:
    static bool cmp(vector<int> &a, vector<int> &b) {
        return a[1] < b[1];
    }
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {
        int N = intervals.size();
        if(N == 0)  return 0;

        sort(intervals.begin(), intervals.end(), cmp);
        int removed = 0, currEnd = intervals[0][1];
        for(int i = 1; i < N; i++){
            if(intervals[i][0] < currEnd)   removed++;
            else    currEnd = intervals[i][1];
        }
        return removed;
    }
};
```

They look basically the same.

![435-2](Pictures/435-2.png)

----

## [452. Minimum Number of Arrows to Burst Balloons (Medium)](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/)

### Solution 1 (❌)

In my mind, I compressed the `2-D` picture into `1-D` line, with overlaps (if there is any).

At first, I sorted the `points` by $x_{start}$. Then I planned to find if there was a overlap with next balloon.

If there was, I updated the left end. (But I made a mistake here: I forgot to update the right end.)

If there wasn't, I reinitialized the left end and right end as well as `arrows++`.

```c++
class Solution {
public:
    static bool cmp(vector<int> &a, vector<int> &b) {
        return a[0] == b[0] ? a[1] < b[1] : a[0] < b[0];
    }
    int findMinArrowShots(vector<vector<int>>& points) {
        sort(points.begin(), points.end(), cmp);
        int balloons = points.size(), arrows = 1, left = points[0][0], right = points[0][1];
        for(int i = 1; i < balloons; i++){
            if(points[i][0] <= right)
                left = points[i][0];
            else{
                left = points[i][0];
                right = points[i][1];
                arrows++;
            }
        }
    return arrows;
    }
};
```

<img src="Pictures/452-1.png" alt="452-1" style="zoom:50%;" />



### Solution 2 (✅)

After I updated the right end. It was AC.

```c++
class Solution {
public:
    static bool cmp(vector<int> &a, vector<int> &b) {
        return a[0] == b[0] ? a[1] < b[1] : a[0] < b[0];
    }
    int findMinArrowShots(vector<vector<int>>& points) {
        sort(points.begin(), points.end(), cmp);
        int balloons = points.size(), arrows = 1, left = points[0][0], right = points[0][1];
        for(int i = 1; i < balloons; i++){
            if(points[i][0] <= right){
                left = points[i][0];
                if(points[i][1] < right)	//fixed part
                    right = points[i][1];
            }
            else{
                left = points[i][0];
                right = points[i][1];
                arrows++;
            }
        }
    return arrows;
    }
};
```

![452-2](Pictures/452-2.png)