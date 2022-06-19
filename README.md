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

