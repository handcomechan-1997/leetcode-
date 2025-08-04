# Leetcode常见题型题解

## 回溯

### 常用模版

```python3
# 记录符合条件的答案
ans = []
# 定义dfs函数,参数保存着路径的信息
def dfs(parameters):
	# 判断路径是否到达终点
	if 到达终点：
		# 处理路径信息，把答案添加进ans中
	# 遍历所有的可能
	for i in range():
		# 添加
		# 下一层
		# 撤回
dfs(初始状态)
return ans
```

### Leetcode 46.全排列(medium)

[Leetcode 46.全排列](https://leetcode.cn/problems/permutations/)

```python3
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        # 深度优先搜索所有的可能，保存在ans中
        ans = []
        # arr保存当前的列表
        def dfs(arr):
            # 如果列表长度和nums相同，则把它保存在ans里
            if len(arr) == len(nums):
                ans.append(arr[::])
                return
            # 遍历nums中的数字，如果在arr中没有出现，则添加进去
            for num in nums:
                if num not in arr:
                    # 添加
                    arr.append(num)
                    # 下一步
                    dfs(arr)
                    # 回溯
                    arr.pop()
        dfs([])
        return ans
```



### Leetcode 78.子集(medium)

[Leetcode 78.子集](https://leetcode.cn/problems/subsets/)

```python3
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        # ans保存答案
        ans = []
        # idx表示考虑到nums中的下标为idx的数，flag表示每个数字是否出现
        def dfs(idx,flag):
            if idx == len(nums):
                tmp = []
                for i in range(len(nums)):
                    if (flag>>i)&1:
                        tmp.append(nums[i])
                ans.append(tmp[::])
                return
            # 下标为idx的数字不出现
            dfs(idx+1,flag)
            # 下标为idx的数字出现
            dfs(idx+1,flag|(1<<idx))
        dfs(0,0)
        return ans
```



### Leetcode 17.电话号码的字母组合(medium)

[Leetcode 17.电话号码的字母组合](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/)

```python3
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        if not digits:
            return []
        phone = {2:['a','b','c'],3:['d','e','f'],4:['g','h','i'],5:['j','k','l'],6:['m','n','o'],7:['p','q','r','s'],8:['t','u','v'],9:['w','x','y','z']}
        ans = []
        tmp = []
        def dfs(idx):
            if idx == len(digits):
                ans.append(''.join(tmp[::]))
                return
            for c in phone[int(digits[idx])]:
                tmp.append(c)
                dfs(idx+1)
                tmp.pop()
        dfs(0)
        return ans
```

### Leetcode 51.N皇后(hard)

[51. N 皇后](https://leetcode.cn/problems/n-queens/)

```python3
class Solution:
    def solveNQueens(self, n: int) -> List[List[str]]:
        col = set()
        down = set()
        up = set()
        ans = []
        tmp = []
        def dfs(idx):
            if idx == n:
                ans.append(tmp[::])
                return
            for i in range(n):
                if i not in col and i+idx not in down and i-idx not in up:
                    # 添加
                    col.add(i)
                    down.add(i+idx)
                    up.add(i-idx)
                    s = '.'*i+'Q'+'.'*(n-i-1)
                    tmp.append(s)
                    #下一层
                    dfs(idx+1)
                    # 撤销
                    col.remove(i)
                    down.remove(i+idx)
                    up.remove(i-idx)
                    tmp.pop()
        dfs(0)
        return ans
```

### Leetcode 39.组合总数

[Leetcode 39.组合总数](https://leetcode.cn/problems/combination-sum/)

```python3
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        arr = []
        ans = []
        def dfs(left,idx):
            if left == 0:
                ans.append(arr[::])
            for i in range(idx,len(candidates)):
                num = candidates[i]
                if num<=left:
                    arr.append(num)
                    dfs(left-num,i)
                    arr.pop()
        dfs(target,0)
        return ans
```



### Leetcode 40.组合总数Ⅱ

[Leetcode 40.组合总数Ⅱ](https://leetcode.cn/problems/combination-sum-ii/)

```python3
class Solution:
    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        candidates.sort()
        cnt = defaultdict(int)
        for num in candidates:
            cnt[num]+=1
        keys = list(cnt.keys())
        ans = []
        arr = []
        def dfs(left,idx):
            if left == 0 or idx == len(keys):
                if left == 0:
                    tmp = []
                    for j,num in enumerate(arr):
                        for i in range(num):
                            tmp.append(keys[j])
                    ans.append(tmp[::])
                return
            num = keys[idx]
            for i in range(cnt[num]+1):
                if i*num<=left:
                    arr.append(i)
                    dfs(left-i*num,idx+1)
                    arr.pop()
                else:
                    break
        dfs(target,0)
        return ans
```

### Leetcode 216.组合总数III

[Leetcode 216. 组合总和 III](https://leetcode.cn/problems/combination-sum-iii/)

```python3
class Solution:
    def combinationSum3(self, k: int, n: int) -> List[List[int]]:
        if n<(1+k)*k/2 or n>(19-k)*k/2:
            return []
        ans = []
        arr = []
        def dfs(numleft,sumleft,idx):
            if numleft == 0 or sumleft == 0 or idx == 9:
                if sumleft == 0 and numleft == 0:
                    tmp = []
                    for i,x in enumerate(arr):
                        if x == 1:
                            tmp.append(i+1)
                    ans.append(tmp[::])
                return
            if numleft>0 and sumleft>=idx+1:
                arr.append(1)
                dfs(numleft-1,sumleft-idx-1,idx+1)
                arr.pop()
                arr.append(0)
                dfs(numleft,sumleft,idx+1)
                arr.pop()
        dfs(k,n,0)
        return ans
```
### Leetcode 2522. 将字符串分割成值不超过 K 的子字符串
[Leetcode 2522. 将字符串分割成值不超过 K 的子字符串](https://leetcode.cn/contest/weekly-contest-326/problems/partition-string-into-substrings-with-values-at-most-k/)

```python
class Solution:
    def minimumPartition(self, s: str, k: int) -> int:
        # 异常检查
        if k<9:
            for c in s:
                if int(c)>k:
                    return -1
        # 字符串转换为数字
        def trans_str_to_int(s_nums):
            base = 10
            ans = 0
            for c in s_nums:
                ans = ans*base+int(c)
            return ans
        
        # dfs(idx)表示从下标为idx的字符开始的最少子字符串数量
        @cache
        def dfs(idx):
            if idx>=len(s):
                return 0
            
            tmp = 2**32
            for i in range(idx+1,len(s)+1):
                if trans_str_to_int(s[idx:i])<=k:
                    tmp = min(tmp,1+dfs(i))
                else:
                    break
            return tmp
        return dfs(0)
```

### Leetcode 200.岛屿数量(medium)

[Leetcode 200.岛屿数量](https://leetcode.cn/problems/number-of-islands/)

```python3
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        # m和n表示grid的行和列
        m,n = len(grid),len(grid[0])
        # dfs把与(i,j)相邻的陆地全部标记为'0'
        def dfs(i,j):
            # 判断坐标是否越界或者当前是海水
            if i<0 or i>=m or j<0 or j>=n or grid[i][j]=='0':
                return
            # 标记为海水
            grid[i][j] = '0'
            # 四个方向
            dfs(i+1,j)
            dfs(i-1,j)
            dfs(i,j+1)
            dfs(i,j-1)
        ans = 0
        for i in range(m):
            for j in range(n):
                if grid[i][j]=='1':
                    ans += 1
                    dfs(i,j)
        return ans
```

### Leetcode 695.岛屿的最大面积(medium)

[Leetcode 695.岛屿的最大面积](https://leetcode.cn/problems/max-area-of-island/)

```python3
class Solution:
    def maxAreaOfIsland(self, grid: List[List[int]]) -> int:
        m,n = len(grid),len(grid[0])
        # dfs(i,j)返回以(i,j)为起点的岛屿面积
        def dfs(i,j):
            if i<0 or i>=m or j<0 or j>=n or grid[i][j]==0:
                return 0
            grid[i][j] = 0
            return 1 + dfs(i+1,j) + dfs(i-1,j) + dfs(i,j+1) + dfs(i,j-1)
        ans = 0
        for i in range(m):
            for j in range(n):
                if grid[i][j]==1:
                    ans = max(ans,dfs(i,j))
        return ans
```

### Leetcode 79.单词搜索(medium)

[Leetcode 79.单词搜索](https://leetcode.cn/problems/word-search/)

```python3
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        m,n = len(board),len(board[0])
        # dfs(x,y,idx)表示从(x,y)开始匹配word[idx:]
        def dfs(x,y,idx):
            if idx == len(word):
                return True
            if x<0 or x>=m or y<0 or y>=n or board[x][y]!=word[idx]:
                return False
            # 标记已访问
            tmp = board[x][y]
            board[x][y] = '#'
            for i,j in [(x+1,y),(x-1,y),(x,y+1),(x,y-1)]:
                if dfs(i,j,idx+1):
                    board[x][y] = tmp
                    return True
            board[x][y] = tmp
            return False
        for i in range(m):
            for j in range(n):
                if dfs(i,j,0):
                    return True
        return False
```

### Leetcode 22.括号生成(medium)

[Leetcode 22.括号生成](https://leetcode.cn/problems/generate-parentheses/)

```python3
class Solution:
    def generateParenthesis(self, n: int) -> List[str]:
        # ans保存所有可行的括号组合
        ans = []
        # left和right分别表示剩余的左右括号数量
        def dfs(left, right, path):
            # 当左右括号都用完时,加入答案
            if left == 0 and right == 0:
                ans.append(''.join(path))
                return
            if left > 0:
                path.append('(')
                dfs(left-1, right, path)
                path.pop()
            if right > left:
                path.append(')')
                dfs(left, right-1, path)
                path.pop()
        dfs(n, n, [])
        return ans
```

### Leetcode 39.组合总和(medium)

[Leetcode 39.组合总和](https://leetcode.cn/problems/combination-sum/)

```python3
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        candidates.sort()
        ans = []
        # 从start开始选择,remain表示剩余和
        def dfs(start, remain, path):
            if remain == 0:
                ans.append(path[::])
                return
            for i in range(start, len(candidates)):
                if candidates[i] > remain:
                    break
                path.append(candidates[i])
                dfs(i, remain - candidates[i], path)
                path.pop()
        dfs(0, target, [])
        return ans
```

### Leetcode 40.组合总和II(medium)

[Leetcode 40.组合总和II](https://leetcode.cn/problems/combination-sum-ii/)

```python3
class Solution:
    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        candidates.sort()
        ans = []
        # 与组合总和不同,每个数字只能使用一次
        def dfs(start, remain, path):
            if remain == 0:
                ans.append(path[::])
                return
            for i in range(start, len(candidates)):
                if candidates[i] > remain:
                    break
                if i > start and candidates[i] == candidates[i-1]:
                    continue
                path.append(candidates[i])
                dfs(i+1, remain - candidates[i], path)
                path.pop()
        dfs(0, target, [])
        return ans
```

### Leetcode 77.组合(medium)

[Leetcode 77.组合](https://leetcode.cn/problems/combinations/)

```python3
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        ans = []
        # start表示当前选择的起点
        def dfs(start, path):
            if len(path) == k:
                ans.append(path[::])
                return
            for i in range(start, n+1):
                path.append(i)
                dfs(i+1, path)
                path.pop()
        dfs(1, [])
        return ans
```

### Leetcode 90.子集II(medium)

[Leetcode 90.子集II](https://leetcode.cn/problems/subsets-ii/)

```python3
class Solution:
    def subsetsWithDup(self, nums: List[int]) -> List[List[int]]:
        nums.sort()
        ans = []
        # idx表示当前枚举到的位置
        def dfs(idx, path):
            ans.append(path[::])
            for i in range(idx, len(nums)):
                if i > idx and nums[i] == nums[i-1]:
                    continue
                path.append(nums[i])
                dfs(i+1, path)
                path.pop()
        dfs(0, [])
        return ans
```

### Leetcode 131.分割回文串(medium)

[Leetcode 131.分割回文串](https://leetcode.cn/problems/palindrome-partitioning/)

```python3
class Solution:
    def partition(self, s: str) -> List[List[str]]:
        ans = []
        # start表示当前起始位置
        def dfs(start, path):
            if start == len(s):
                ans.append(path[::])
                return
            for end in range(start, len(s)):
                tmp = s[start:end+1]
                if tmp == tmp[::-1]:
                    path.append(tmp)
                    dfs(end+1, path)
                    path.pop()
        dfs(0, [])
        return ans
```

### Leetcode 257.二叉树的所有路径(easy)

[Leetcode 257.二叉树的所有路径](https://leetcode.cn/problems/binary-tree-paths/)

```python3
class Solution:
    def binaryTreePaths(self, root: Optional[TreeNode]) -> List[str]:
        ans = []
        # path保存从根到当前节点的路径
        def dfs(node, path):
            if not node:
                return
            path.append(str(node.val))
            if not node.left and not node.right:
                ans.append('->'.join(path))
            else:
                dfs(node.left, path)
                dfs(node.right, path)
            path.pop()
        dfs(root, [])
        return ans
```

### Leetcode 332.重新安排行程(hard)

[Leetcode 332.重新安排行程](https://leetcode.cn/problems/reconstruct-itinerary/)

```python3
from collections import defaultdict

class Solution:
    def findItinerary(self, tickets: List[List[str]]) -> List[str]:
        graph = defaultdict(list)
        # 逆序插入便于弹出最小的目的地
        for a, b in sorted(tickets, reverse=True):
            graph[a].append(b)
        ans = []
        def dfs(u):
            while graph[u]:
                dfs(graph[u].pop())
            ans.append(u)
        dfs('JFK')
        return ans[::-1]
```

### Leetcode 547.省份数量(medium)

[Leetcode 547.省份数量](https://leetcode.cn/problems/number-of-provinces/)

```python3
class Solution:
    def findCircleNum(self, isConnected: List[List[int]]) -> int:
        n = len(isConnected)
        vis = [False] * n
        def dfs(i):
            vis[i] = True
            for j in range(n):
                if isConnected[i][j] and not vis[j]:
                    dfs(j)
        ans = 0
        for i in range(n):
            if not vis[i]:
                ans += 1
                dfs(i)
        return ans
```

### Leetcode 733.图像渲染(easy)

[Leetcode 733.图像渲染](https://leetcode.cn/problems/flood-fill/)

```python3
class Solution:
    def floodFill(self, image: List[List[int]], sr: int, sc: int, color: int) -> List[List[int]]:
        m,n = len(image),len(image[0])
        old = image[sr][sc]
        if old == color:
            return image
        # 把与(sr,sc)相连的像素都染成color
        def dfs(i,j):
            if i<0 or i>=m or j<0 or j>=n or image[i][j]!=old:
                return
            image[i][j] = color
            dfs(i+1,j)
            dfs(i-1,j)
            dfs(i,j+1)
            dfs(i,j-1)
        dfs(sr,sc)
        return image
```
# 面试记录

## 手写快排

快排分为两个部分，一个是quicksort主体递归部分，另一个是partition分区部分。



### 为什么快排的时间复杂度是O(nlgn)?

比较笼统一点的解释：

快排的基本思想，找到一个主元(pivot)，把数组分为两个部分，一边小于主元，另一边大于主元。分为两边继续下一层的操作。

每一层的比较次数是O(n)。



# 周赛记录

## 第300场周赛

### Q1 解密消息

[2325. 解密消息](https://leetcode.cn/problems/decode-the-message/)

```python3
class Solution:
    def decodeMessage(self, key: str, message: str) -> str:
        # 第一个映射的字母是a
        tmp = ord('a')
        mapping = {}
        # 空格的映射关系不变
        mapping[' '] = ' '
        for c in key:
            # 按照出现的先后顺序完成映射
            if c not in mapping:
                # 映射成当前字符
                mapping[c] = chr(tmp)
                # 下一个字符
                tmp+=1
        ans = ''
        for c in message:
            ans+=mapping[c]
        return ans
```



### Q2 螺旋矩阵IV

[6111. 螺旋矩阵 IV](https://leetcode.cn/problems/spiral-matrix-iv/)

```python3
class Solution:
    def spiralMatrix(self, m: int, n: int, head: Optional[ListNode]) -> List[List[int]]:
        # 构造矩阵
        ans = [[-1 for _ in range(n)] for _ in range(m)]
        node = head
        # 保存已更改的点的坐标
        vis = set()
        # 起始坐标
        i,j = 0,0
        # 方向
        turn = 0
        # 方向的矩阵，分别对应右，下，左，上
        nextTurn = [(0,1),(1,0),(0,-1),(-1,0)]
        while node:
            # 改值
            ans[i][j] = node.val
            # 更新节点
            node = node.next
            vis.add((i,j))
            while True and node:
                # 下一个更新的坐标
                new_x = i+nextTurn[turn][0]
                new_y = j+nextTurn[turn][1]
                # 保证坐标在矩阵内且没更改过
                if 0<=new_x<m and 0<=new_y<n and (new_x,new_y) not in vis:
                    i,j = new_x,new_y
                    break
                else:
                    turn += 1
                    turn = turn%4
        return ans
```

### Q3 知道秘密的人数

[2327. 知道秘密的人数](https://leetcode.cn/problems/number-of-people-aware-of-a-secret/)

```python3
class Solution:
    def peopleAwareOfSecret(self, n: int, delay: int, forget: int) -> int:
        # people保存第i天知道秘密的人数
        people = [0 for _ in range(n)]
        people[0] = 1
        for i in range(n):
            for j in range(i+delay,i+forget):
                if j<n:
                    people[j]+=people[i]
                else:
                    break
        # 后forget个人数加起来
        return sum(people[-forget:]) % 1000000007
```



### Q4 网格图中递增路径的数目

[2328. 网格图中递增路径的数目](https://leetcode.cn/problems/number-of-increasing-paths-in-a-grid/)

```python3
class Solution:
    def countPaths(self, grid: List[List[int]]) -> int:
        m,n = len(grid),len(grid[0])
        @cache
        def dfs(x,y):
            tmp = 1
            for i,j in [(x+1,y),(x-1,y),(x,y-1),(x,y+1)]:
                if 0<=i<m and 0<=j<n and grid[i][j]>grid[x][y]:
                    tmp+=dfs(i,j)
            return tmp % 1000000007
        ans = 0
        for i in range(m):
            for j in range(n):
                ans+=dfs(i,j)
                ans = ans % 1000000007
        return ans
```

## 第299场周赛

### Q1 判断一个矩阵是否是一个X矩阵

[2319. 判断矩阵是否是一个 X 矩阵](https://leetcode.cn/problems/check-if-matrix-is-x-matrix/)

```python3
class Solution:
    def checkXMatrix(self, grid: List[List[int]]) -> bool:
        m = len(grid)
        for i in range(m):
            for j in range(m):
                if i+j == m-1 or i-j == 0:
                    if grid[i][j]==0:
                        print(i,j)
                        return False
                else:
                    if grid[i][j] != 0:
                        print(i,j)
                        return False
        return True
```

### Q2 统计放置房子的方式数

[2320. 统计放置房子的方式数](https://leetcode.cn/problems/count-number-of-ways-to-place-houses/)

```python3
class Solution:
    def countHousePlacements(self, n: int) -> int:
        MOD = int(1e9+7)
        @cache
        def dfs(k):
            if k == 1:
                return 2
            if k == 0:
                return 1
            return (dfs(k-1)+dfs(k-2)) % MOD
        return dfs(n)*dfs(n) % MOD
```

