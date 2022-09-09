##### 62. 不同路径
一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为 “Start” ）。
机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为 “Finish” ）。

 ![[Pasted image 20220818170214.png]]
问总共有多少条不同的路径？

示例 1：
输入：m = 3, n = 7
输出：28
示例 2：
输入：m = 3, n = 2
输出：3
解释：
从左上角开始，总共有 3 条路径可以到达右下角。
1. 向右 -> 向下 -> 向下
2. 向下 -> 向下 -> 向右
3. 向下 -> 向右 -> 向下
示例 3：
输入：m = 7, n = 3
输出：28
示例 4：
输入：m = 3, n = 3
输出：6
 
提示：
1 <= m, n <= 100
题目数据保证答案小于等于 2 * 109

先来个递归
```java
class Solution {
    public int uniquePaths(int m, int n) {
        if(m == 1){
            return 1;
        }else if(n == 1){
            return 1;
        }
        return uniquePaths(m-1,n) + uniquePaths(m,n-1);
    }
}
```
再来个DP
```Java
class Solution {
    public int uniquePaths(int m, int n) {
        int[] dp = new int[m*n+1];
        dp[1] = 1;
        if(m ==1 || n==1)
            return 1;
        else{
            for(int i =2;i<m*n+1;i++){
                if(i<=n)
                    dp[i] = dp[i-1];
                else 
                    if(i%n == 1)
                    dp[i] = dp[i-n];
                else 
                    dp[i] = dp[i - 1] + dp[i - n];
            }   
            return dp[m*n];}
        }
}
```

##### 63. 不同路径 II
一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为 “Start” ）。
机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为 “Finish”）。
现在考虑网格中有障碍物。那么从左上角到右下角将会有多少条不同的路径？
网格中的障碍物和空位置分别用 1 和 0 来表示。
示例 1：
输入：obstacleGrid = \[\[0,0,0],\[0,1,0],\[0,0,0]]
输出：2
解释：3x3 网格的正中间有一个障碍物。
从左上角到右下角一共有 2 条不同的路径：
1. 向右 -> 向右 -> 向下 -> 向下
2. 向下 -> 向下 -> 向右 -> 向右

![[Pasted image 20220822054802.png]]

示例 2：
输入：obstacleGrid = \[\[0,1],\[0,0]]
输出：1
![[Pasted image 20220822054930.png]]
提示：
m == obstacleGrid.length
n == obstacleGrid\[i].length
1 <= m, n <= 100
obstacleGrid\[i]\[j] 为 0 或 1
```java
class Solution {
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
int m = obstacleGrid.length;
        int n = obstacleGrid[0].length;
        int[] path = new int[m*n+1];
        int[] dp = new int[m*n+1];

        int pos = m*n+1;

        for (int i =0;i<m;i++){
            for (int j=0;j<n;j++){
                if (obstacleGrid[i][j]==0){
                    path[--pos]=0;
                }
                else
                    path[--pos]=1;
            }
        }
        if (path[1]==1||path[m*n]==1)
            return 0;
        else
            dp[1] = 1;

        for (int i=2;i<m*n+1;i++){
            if (path[i]==1) {
                dp[i]=0;
                continue;
            }
            if (m==1||n==1){
                if (path[i] == 1)
                    return 0;
                else
                    return 1;
            }
            if (i<=n)
                dp[i]=dp[i-1];
            else
                if (i%n==1)
                    dp[i]=dp[i-n];
                else
                    dp[i] = dp[i-1] + dp[i-n];
        }

        return dp[m*n];
    }
}
```







##### 64. 最小路径和
给定一个包含非负整数的 m x n 网格 grid ，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。
说明：每次只能向下或者向右移动一步。
示例 1：
输入：grid = \[\[1,3,1],\[1,5,1],\[4,2,1]]
输出：7
解释：因为路径 1→3→1→1→1 的总和最小。
![[Pasted image 20220822054959.png]]
示例 2：
输入：grid = \[\[1,2,3],\[4,5,6]]
输出：12

提示：
m == grid.length
n == grid\[i].length
1 <= m, n <= 200
0 <= grid\[i]\[j] <= 100
相关标签
```java
class Solution {  
    public int minPathSum(int[][] grid) {  
        int row = grid.length;  
        int col = grid[0].length;  
        for (int i =0;i<row;i++){  
            for (int j=0;j<col;j++){  
                if(i>0&&j>0){  
                    grid[i][j] +=  
                            grid[i - 1][j]<grid[i][j - 1]?grid[i - 1][j]:grid[i][j - 1];  
                }else {  
                    if (i>0){  
                        grid[i][j] += grid[i-1][j];  
                    }  
                    if (j>0){  
                        grid[i][j] += grid[i][j-1];  
                    }  
                }  
            }  
        }  
        return grid[row - 1][col - 1];  
    }  
}
```


解析
前言
今天通过「最小路径和」问题来分享如何进行「维度合并」来降低“出错风险”和“Debug 难度”。

这也是 DP 问题中的一个常用技巧：通过「问题等价变换」来降低编码难度。

动态规划
这道题是在 62. 不同路径 的基础上，增加了路径成本概念。

我们可以根据问题来调整我们的「状态定义」：

定义 f\[i]\[j]f\[i]\[j] 为从 (0,0)(0,0) 开始到达位置 (i,j)(i,j) 的最小总和。

那么 f\[m-1]\[n-1]就是我们最终的答案，f\[0]\[0]=grid\[0]\[0]是一个显而易见的起始状态。

由于题目限定了我们只能「往下」或者「往右」移动，因此我们按照 当前位置可由哪些位置转移过来 进行分析：

当前位置只能通过「往下」移动而来，即有 f\[i]\[j] = f\[i-1]\[j] + grid\[i]\[j]

当前位置只能通过「往右」移动而来，即有 f\[i]\[j] = f\[i]\[j-1] + grid\[i]\[j]

当前位置既能通过「往下」也能「往右」移动，即有 f[i][j] = min(f\[i]\[j-1],f\[i-1]\[j]) + grid\[i]\[j]

代码：
```Java
class Solution {
    public int minPathSum(int[][] grid) {        
        int m = grid.length, n = grid[0].length;
        int[][] f = new int[m][n];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (i == 0 && j == 0) {
                    f[i][j] = grid[i][j];
                } else {
                    int top  = i - 1 >= 0 ? f[i - 1][j] + grid[i][j] : Integer.MAX_VALUE;
                    int left = j - 1 >= 0 ? f[i][j - 1] + grid[i][j] : Integer.MAX_VALUE;
                    f[i][j] = Math.min(top, left);
                }
            }
        }
        return f[m - 1][n - 1];
    }
}
```
时间复杂度：O(n\*m)
空间复杂度：O(n\*m)
进阶
如果要输出总和最低的路径呢（如果有多个答案，返回其中之一即可）？
从原问题我们知道，我们需要从 (0,0(0,0) 一步步转移到 (m-1,n-1)(m−1,n−1)。

也就是我们需要扫描完整个方块（转移完所有的状态），才能得到答案。

那么显然，我们可以使用额外的数据结构来记录，我们是如何一步步转移到 f\[m-1]\[n-1]的。

当整个 DP 过程结束后，我们再用辅助记录的数据结构来回推我们的路径。

同时，由于我们原有的 DP 部分已经创建了一个二维数组来存储状态值，这次用于记录「上一步」的 g 数组我们改用一维数组来记录。

代码：

```Java
class Solution {
    int m, n;
    public int minPathSum(int[][] grid) {        
        m = grid.length;
        n = grid[0].length;
        int[][] f = new int[m][n];
        int[] g = new int[m * n];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (i == 0 && j == 0) {
                    f[i][j] = grid[i][j];
                } else {
                    int top  = i - 1 >= 0 ? f[i - 1][j] + grid[i][j] : Integer.MAX_VALUE;
                    int left = j - 1 >= 0 ? f[i][j - 1] + grid[i][j] : Integer.MAX_VALUE;
                    f[i][j] = Math.min(top, left);
                    g[getIdx(i, j)] = top < left ? getIdx(i - 1, j) : getIdx(i, j - 1);
                }
            }
        }
        
        // 从「结尾」开始，在 g[] 数组中找「上一步」
        int idx = getIdx(m - 1, n - 1);
        // 逆序将路径点添加到 path 数组中
        int[][] path = new int[m + n][2];
        path[m + n - 1] = new int[]{m - 1, n - 1};
        for (int i = 1; i < m + n; i++) {
            path[m + n - 1 - i] = parseIdx(g[idx]);
            idx = g[idx];
        }
        // 顺序输出位置
        for (int i = 1; i < m + n; i++) {
            int x = path[i][0], y = path[i][1];
            System.out.print("(" + x + "," + y + ") ");
        }
        System.out.println(" ");
        
        return f[m - 1][n - 1];
    }
    int[] parseIdx(int idx) {
        return new int[]{idx / n, idx % n};
    }
    int getIdx(int x, int y) {
        return x * n + y;
    }
}
```
也许你会觉得「输出」方案的代码太麻烦了。

这是因为我们找路径的过程是「倒着」找，而输出方案的时候则是「顺着」输出。

如果希望简化找路径的过程，我们需要对原问题进行等价转换：

将 「(0,0)到 (m-1,n-1)的最短路径」转换为「从 (m-1,n-1) 到 (0,0)的最短路径」，同时移动方向从「向下 & 向右」转换为「向上 & 向左」。

这样我们就能实现「找路径」的顺序和「输出」顺序同向。

调整定义 f[i][j]f[i][j] 为从 (m-1,n-1)开始到达位置 (i,j)的最小总和。

代码：

```Java

class Solution {
    int m, n;
    public int minPathSum(int[][] grid) {        
        m = grid.length;
        n = grid[0].length;
        int[][] f = new int[m][n];
        int[] g = new int[m * n];
        for (int i = m - 1; i >= 0; i--) {
            for (int j = n - 1; j >= 0; j--) {
                if (i == m - 1 && j == n - 1) {
                    f[i][j] = grid[i][j];
                } else {
                    int bottom = i + 1 < m ? f[i + 1][j] + grid[i][j] : Integer.MAX_VALUE;
                    int right  = j + 1 < n ? f[i][j + 1] + grid[i][j] : Integer.MAX_VALUE; 
                    f[i][j] = Math.min(bottom, right);
                    g[getIdx(i, j)] = bottom < right ? getIdx(i + 1, j) : getIdx(i, j + 1);
                }
            }
        }

        int idx = getIdx(0,0);
        for (int i = 1; i <= m + n; i++) {
            if (i == m + n) continue;
            int x = parseIdx(idx)[0], y = parseIdx(idx)[1];
            System.out.print("(" + x + "," + y + ") ");
            idx = g[idx];
        }
        System.out.println(" ");

        return f[0][0];
    }
    int[] parseIdx(int idx) {
        return new int[]{idx / n, idx % n};
    }
    int getIdx(int x, int y) {
        return x * n + y;
    }
}
```
如果方块中「存在负权」，如何求解？
如果只是增加负权值的条件，走动规则不变（只能往下或往右），那么 DP 仍然有效。仍然能够得到「总成本最小」的路径，但不确保成本必然为负权，也不确保必然会经过负权位置。

如果走动规则调整为「可以往任意方向」且「每个位置最多只能访问一次」，如何求解？
这时候问题就转换为「图论最短路」问题，而且是从「特定源点」到「特定汇点」的「单源最短路」问题。

需要根据是否存在「负权边」来分情况讨论：

不存在负权边：使用 Dijkstra 算法求解,存在负权边：使用 Bellman Ford 或 SPFA 求解

总结
今天，除了 LeetCode 的原问题以外，我还给介绍了两个「进阶」问题。
在「进阶一」输出方案问题中，我给你介绍了如何使用「一维数组」存储「二维信息」，这是一个常见的手段。以及如何通过 问题等价变换 来降低编码难度。
通过「进阶二」我向你展示了，同一道题换了一个前提条件，求解方法将截然不同。
改了一个前提条件之后，原本的解法对应的证明将会失效，原本的算法也就不能正确求解了。
类似的问题我在 \[第一讲] 的「思考」中也问过。
这就是我们做算法题一定要讲「证明」的原因，搞清楚本质了才是真正会做。

##### 120. 三角形最小路径和
给定一个三角形 triangle ，找出自顶向下的最小路径和。
每一步只能移动到下一行中相邻的结点上。相邻的结点 在这里指的是 下标 与 上一层结点下标 相同或者等于 上一层结点下标 + 1 的两个结点。也就是说，如果正位于当前行的下标 i ，那么下一步可以移动到下一行的下标 i 或 i + 1 。
示例 1：

输入：triangle = \[\[2],\[3,4],\[6,5,7],\[4,1,8,3]]
输出：11
解释：如下面简图所示：
   2
  3 4
 6 5 7
4 1 8 3
自顶向下的最小路径和为 11（即，2 + 3 + 5 + 1 = 11）。
示例 2：

输入：triangle = \[\[-10]]
输出：-10
 

提示：

1 <= triangle.length <= 200
triangle\[0].length == 1
triangle\[i].length == triangle\[i - 1].length + 1
-104 <= triangle\[i]\[j] <= 104
进阶：
你可以只使用 O(n) 的额外空间（n 为三角形的总行数）来解决这个问题吗？

```Java
class Solution {  
    public int minimumTotal(List<List<Integer>> triangle) {  
    int n = 0;  
    int m = 0;  
    int num = (triangle.size() + 1) * triangle.size() / 2;  
    int dp[] = new int[num];  
    int ans = Integer.MAX_VALUE;  
    for(int i = 0;i < num;i++){  
        List temp = triangle.get(n);  
        if(n == 0) dp[i] =(Integer) temp.get(m);  
        if(m == 0 && n != 0) dp[i] =(Integer) temp.get(m) + dp[i - n];  
        if(m == n && n != 0) dp[i] =(Integer) temp.get(m) + dp[i - n - 1];  
        if(m != n && m != 0)  
            dp[i] =(Integer) temp.get(m) + Math.min(dp[i - n], dp[i - n - 1]);  
        m++;  
        if (m == n + 1) {  
            m = 0;  
            n++;  
        }  
        if (i >= num - n - 1) {  
            if (dp[i] < ans)  
                ans = dp[i];  
       }  
    }  
    return ans;  
    }  
}
```











