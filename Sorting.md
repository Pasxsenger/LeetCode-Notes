# Sorting

## [215. Kth Largest Element in an Array (Medium)](https://leetcode.com/problems/kth-largest-element-in-an-array/)

### Version 1 (✅)

Just kidding hhhhh...

```c++
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        sort(nums.begin(), nums.end(), greater<int>());
        return nums[k-1];
    }
};
```

![image-20230112150142174](Pictures/215-1.png)



### Version 2 (✅)

With [jianchao-li](https://leetcode.com/jianchao-li/)'s [solutions](https://leetcode.com/problems/kth-largest-element-in-an-array/solutions/60309/c-stl-partition-and-heapsort/), I learned `min-heap sort` built with `priority queue`.

```c++
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        priority_queue<int, vector<int>, greater<int>> pq;
        for(int num: nums){
            pq.push(num);
            if(pq.size() > k)
                pq.pop();
        }
        return pq.top();
    }
};
```

It can be foreseen that it's not as efficient as sort. But it's not the point.

![image-20230112150142174](Pictures/215-2.png)

----

## [347. Top K Frequent Elements (Medium)](https://leetcode.com/problems/top-k-frequent-elements/)

### Version 1 (✅)

With [k_joshi](https://leetcode.com/k_joshi/)'s [solution](https://leetcode.com/problems/top-k-frequent-elements/solutions/1927997/easy-and-simple-c-code-bucket-sort-o-n-linear-time-complexity/), I used `bucket sort`, which costed a lot of space.

```c++
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        int N = nums.size();
        unordered_map<int, int> mp;
        for(int num: nums)
            mp[num]++;
        
        vector<vector<int>> buckets(N+1);
        for(auto m: mp)
            buckets[m.second].push_back(m.first);
        
        vector<int> ans;
        for(int i = N; i > 0; i--){		//attention: it starts with N instead of N-1
            int vecLen = buckets[i].size();
            for(int j = 0; j < vecLen; j++){
                if(ans.size() < k)
                    ans.push_back(buckets[i][j]);
            }
            if(ans.size() == k)
                return ans;
        }
        return ans;
    }
};
```

![image-20230112150142174](Pictures/347-1.png)



### Version 2 (✅)

This is just my experiment. I changed `auto` to `pair` to observe the result. 

```c++
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        int N = nums.size();
        unordered_map<int, int> mp;
        for(int num: nums)
            mp[num]++;
        
        vector<vector<int>> buckets(N+1);
        for(pair m: mp)
            buckets[m.second].push_back(m.first);
        
        vector<int> ans;
        for(int i = N; i > 0; i--){
            int vecLen = buckets[i].size();
            for(int j = 0; j < vecLen; j++){
                if(ans.size() < k)
                    ans.push_back(buckets[i][j]);
            }
            if(ans.size() == k)
                return ans;
        }
        return ans;
    }
};
```

It turned out that it got faster.

![image-20230112150142174](Pictures/347-2.png)



### Version 3 (✅)

With [sxycwzwzq](https://leetcode.com/sxycwzwzq/)'s [solution](https://leetcode.com/problems/top-k-frequent-elements/solutions/81624/c-o-n-log-n-k-unordered-map-and-priority-queue-maxheap-solution/), I used `max-heap` to find out the top K elements, which saved lots of space.

```c++
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int, int> mp;
        for(int num: nums)
            mp[num]++;
        
        vector<int> ans;
        priority_queue<pair<int, int>> pq;
        for(auto it = mp.begin(); it != mp.end(); it++){
            pq.push(make_pair(it->second, it->first));
            if(pq.size() > mp.size()-k){
                ans.push_back(pq.top().second);
                pq.pop();
            }
        }
        return ans;
    }
};
```

![image-20230112150142174](Pictures/347-3.png)

----

