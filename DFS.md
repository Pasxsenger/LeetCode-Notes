# DFS

[695. Max Area of Island (Medium)](#695)



[点击跳转](#jump)



## <span id="jump">[695. Max Area of Island (Medium)](https://leetcode.com/problems/max-area-of-island/)</span>

### Solution 1 (❌)

I used DFS to serach the biggest island. And used `visited` to record the visited squares.

```c++
class Solution {
public:
    int maxAreaOfIsland(vector<vector<int>>& grid) {
        int M = grid.size(), N = grid[0].size();
        int maxArea = 0;
        vector<vector<int>> directions = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
        vector<bool> visited(M * N, false);
        for(int i = 0; i < M; i++){
            for(int j = 0; j < N; j++){
                maxArea = max(maxArea, DFS(grid, directions, visited, M, N, i, j));
            }
        }
        return maxArea;
    }
    int DFS(vector<vector<int>>& grid, vector<vector<int>> directions, vector<bool>& visited, int M, int N, int row, int column) {
        if(row < 0 || row >= M || column < 0 || column >= N || grid[row][column] == 0 || visited[row * M + column])
            return 0;
        
        visited[row * M + column] = true;
        int area = 1;
        for(auto d : directions)
            area += DFS(grid, directions, visited, M, N, row + d[0], column + d[1]);
        return area;
    }
};
```

But I failed on this test case.

<img src="Pictures/695-1.png" alt="695-1" style="zoom:50%;" />



### Solution 2 (✅)

After debugging, I found out that the problem was `visited[row * M + column]`.

And I changed it to `visited[row * N + column]`.

```c++
class Solution {
public:
    int maxAreaOfIsland(vector<vector<int>>& grid) {
        int M = grid.size(), N = grid[0].size();
        int maxArea = 0;
        vector<vector<int>> directions = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
        vector<bool> visited(M * N, false);
        for(int i = 0; i < M; i++){
            for(int j = 0; j < N; j++){
                maxArea = max(maxArea, DFS(grid, directions, visited, M, N, i, j));
            }
        }
        return maxArea;
    }
    int DFS(vector<vector<int>>& grid, vector<vector<int>> directions, vector<bool>& visited, int M, int N, int row, int column) {
        if(row < 0 || row >= M || column < 0 || column >= N || grid[row][column] == 0 || visited[row * N + column])
            return 0;
        
        visited[row * N + column] = true;
        int area = 1;
        for(auto d : directions)
            area += DFS(grid, directions, visited, M, N, row + d[0], column + d[1]);
        return area;
    }
};
```

Hmmm... It seems not efficient.

![695-2](Pictures/695-2.png)



### Solution 3 (✅)

Then I tried [caihao0727mail](https://leetcode.com/caihao0727mail/)'s [solution](https://leetcode.com/problems/max-area-of-island/solutions/108533/java-c-straightforward-dfs-solution/) in my own way.

```c++
class Solution {
public:
    int maxAreaOfIsland(vector<vector<int>>& grid) {
        int max_area = 0, M = grid.size(), N = grid[0].size();
        for(int i = 0; i < M; i++)
            for(int j = 0; j < N; j++)
                if(grid[i][j] == 1)
                    max_area = max(max_area, AreaOfIsland(grid, M, N, i, j));
        return max_area;
    }
    
    int AreaOfIsland(vector<vector<int>>& grid, int M, int N, int row, int column){
        if(row >= 0 && row < M && column >= 0 && column < N && grid[row][column] == 1){
            grid[row][column] = 0;
            return 1 + AreaOfIsland(grid, M, N, row+1, column) + AreaOfIsland(grid, M, N, row-1, column) + AreaOfIsland(grid, M, N, row, column-1) + AreaOfIsland(grid, M, N, row, column+1);
        }
        return 0;
    }
};
```

The reason why this one performs much better is that it does not use `visited`.

Instead, it directly **changes the original data**.

Sure it will be faster. 

But I alwasys have some concerns that changing original data should be avoided.

![695-3](Pictures/695-3.png)

---

## [200. Number of Islands (Medium)](https://leetcode.com/problems/number-of-islands/)

### Solution 1 (❌)

This one is similar to last one. But I thought it's easier.

However, I still got wrong answer.

```c++
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        int islands = 0, M = grid.size(), N = grid[0].size();
        vector<bool> visited(M * N, false);
        vector<vector<int>> directions = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
        for(int i = 0; i < M; i++){
            for(int j = 0; j < N; j++){
                if(grid[i][j] == '1' && !visited[i * N + j]){
                    islands++;
                    DFS(grid, visited, directions, M, N, i, j);
                }
            }
        }
        return islands;
    } 
    void DFS(vector<vector<char>> grid, vector<bool>& visited, vector<vector<int>> directions, int M, int N, int row, int column) {
        if(row < 0 || row >= M || column < 0 || column >= N || grid[row][column] == '0' || visited[row * N + column])
            return;
        visited[row * N + column] = true;
        for(auto d : directions)
            DFS(grid, visited, directions, M, N, row+d[0], column+d[1]);
    }
};
```

It failed on a big test case.

![200-1](Pictures/200-1.png)



### Solution 2 (✅)

So I thought I might have to change the original data to shorten the runtime.

Then I deleted the `visited` and added the `grid[row][column] = '0'`.

```c++
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        int islands = 0, M = grid.size(), N = grid[0].size();
        vector<vector<int>> directions = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
        for(int i = 0; i < M; i++){
            for(int j = 0; j < N; j++){
                if(grid[i][j] == '1'){
                    islands++;
                    DFS(grid, directions, M, N, i, j);
                }
            }
        }
        return islands;
    } 
    void DFS(vector<vector<char>>& grid, vector<vector<int>> directions, int M, int N, int row, int column) {
        if(row < 0 || row >= M || column < 0 || column >= N || grid[row][column] == '0')
            return;
        grid[row][column] = '0';
        for(auto d : directions)
            DFS(grid, directions, M, N, row+d[0], column+d[1]);
    }
};
```

The efficiency was quite poor.

![200-2](Pictures/200-2.png)

### Solution 3 (✅)

From [cryptx_](https://leetcode.com/cryptx_/)'s[solution](https://leetcode.com/problems/number-of-islands/solutions/501000/c-simple-dfs-beats-100-in-memory-detailed-explanantion/), I realized that the `directions` will cost lots of time and memory.

So I replaced it with

```c++
DFS(grid, M, N, row+1, column);
DFS(grid, M, N, row-1, column);
DFS(grid, M, N, row, column+1);
DFS(grid, M, N, row, column-1);
```



```c++
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        int islands = 0, M = grid.size(), N = grid[0].size();
        for(int i = 0; i < M; i++){
            for(int j = 0; j < N; j++){
                if(grid[i][j] == '1'){
                    islands++;
                    DFS(grid, M, N, i, j);
                }
            }
        }
        return islands;
    } 
    void DFS(vector<vector<char>>& grid, int M, int N, int row, int column) {
        if(row < 0 || row >= M || column < 0 || column >= N || grid[row][column] == '0')
            return;
        grid[row][column] = '0';
        DFS(grid, M, N, row+1, column);
        DFS(grid, M, N, row-1, column);
        DFS(grid, M, N, row, column+1);
        DFS(grid, M, N, row, column-1);
    }
};
```



**Guess what!**

![200-3](Pictures/200-3.png)

**SOOOO EFFICIENT!!!**

---

## [547. Number of Provinces (Medium)](https://leetcode.com/problems/number-of-provinces/)

