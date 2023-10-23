
## leetcode 75

    双指针
    滑动窗口
    前缀和
    哈希表
    二叉树

### 605. 种花问题

    1 <= flowerbed.length <= 2 * 104
    flowerbed[i] 为 0 或 1
    flowerbed 中不存在相邻的两朵花
    0 <= n <= flowerbed.length

    输入：flowerbed = [1,0,0,0,1], 
    输出： n = 1 true / n = 2 false

    ## 跳格子解法 

    class Solution:
        def canPlaceFlowers(self, flowerbed: List[int], n: int) -> bool:
            i = 0
            while i < len(flowerbed) and n > 0:
                if flowerbed[i] == 1:
                    i+=2
                elif (i == len(flowerbed) - 1) or (flowerbed[i+1] == 0):
                    n-=1
                    i+=2
                else:
                    i+=3
            return n <= 0

### 238. 除自身以外数组的乘积


### 443. 压缩字符串

    给你一个字符数组 chars ，请使用下述算法压缩：
    从一个空字符串 s 开始。对于 chars 中的每组 连续重复字符 ：
        如果这一组长度为 1 ，则将字符追加到 s 中。
        否则，需要向 s 追加字符，后跟这一组的长度。

    压缩后得到的字符串 s 不应该直接返回 ，需要转储到字符数组 chars 中。需要注意的是，如果组长度为 10 或 10 以上，则在 chars 数组中会被拆分为多个字符。
    请在 修改完输入数组后 ，返回该数组的新长度。

    输入：chars = ["a","a","b","b","c","c","c"]
    输出：返回 6 ，输入数组的前 6 个字符应该是：["a","2","b","2","c","3"]
    解释："aa" 被 "a2" 替代。"bb" 被 "b2" 替代。"ccc" 被 "c3" 替代。

    思路
    为了实现原地压缩，我们可以使用双指针分别标志我们在字符串中读和写的位置。
    每次当读指针 read 移动到某一段连续相同子串的最右侧，我们就在写指针 write 处依次写入该子串对应的字符和子串长度即可。

    class Solution:
        def compress(self, chars: List[str]) -> int:
            def reverse(left: int, right: int) -> None:
                while left < right:
                    chars[left], chars[right] = chars[right], chars[left]
                    left += 1
                    right -= 1

            n = len(chars)
            w = left = 0
            for r in range(n):
                if r == n-1 or chars[r] != chars[r+1]:
                    chars[w] = chars[r]
                    w+=1
                    num = r - left + 1
                    if num > 1:
                        anchor = w
                        while num > 0:
                            chars[w] = str(num % 10)
                            w+=1
                            num //=10
                        reverse(anchor, w-1)
                    left = r + 1
            return w

### 1004. 最大连续1的个数 III

    给定一个二进制数组 nums 和一个整数 k，如果可以翻转最多 k 个 0 ，则返回 数组中连续 1 的最大个数 。
    输入：nums = [1,1,1,0,0,0,1,1,1,1,0], K = 2
    输出：6
    解释：[1,1,1,0,0,1,1,1,1,1,1]
    粗体数字从 0 翻转到 1，最长的子数组长度为 6。

    class Solution:
        def longestOnes(self, nums: List[int], k: int) -> int:
            right, left, tmp = 0, 0, 0
            zeros = 0
            length = len(nums)
            while right < length:
                if nums[right] == 0:
                    zeros += 1
                while zeros > k:
                    if nums[left] == 0:
                        zeros-=1
                    left += 1
                tmp = max(tmp, right - left + 1)
                right += 1
            return tmp


### 724. 寻找数组的中心下标

    给你一个整数数组 nums ，请计算数组的 中心下标 。
    数组 中心下标 是数组的一个下标，其左侧所有元素相加的和等于右侧所有元素相加的和。
    如果中心下标位于数组最左端，那么左侧数之和视为 0 ，因为在下标的左侧不存在元素。这一点对于中心下标位于数组最右端同样适用。
    如果数组有多个中心下标，应该返回 最靠近左边 的那一个。如果数组不存在中心下标，返回 -1 。

    输入：nums = [1, 7, 3, 6, 5, 6]
    输出：3
    解释：
    中心下标是 3 。
    左侧数之和 sum = nums[0] + nums[1] + nums[2] = 1 + 7 + 3 = 11 ，
    右侧数之和 sum = nums[4] + nums[5] = 5 + 6 = 11 ，二者相等。

    输入：nums = [1, 2, 3]
    输出：-1
    解释：
    数组中不存在满足此条件的中心下标。

    思路1  SUM(i左边值) + i + SUM(i右边值) = TOTAL
           SUM(i左边值) = TOTAL - i - SUM(i右边值)

    class Solution:
        def pivotIndex(self, nums: List[int]) -> int:
            n = len(nums)
            total = sum(nums)
            tmp = 0
            for i in range(n):
                if 2 * tmp + nums[i] == total:
                    return i
                tmp += nums[i]
            return -1

### 735. 行星碰撞

    给定一个整数数组 asteroids，表示在同一行的行星。
    对于数组中的每一个元素，其绝对值表示行星的大小，正负表示行星的移动方向（正表示向右移动，负表示向左移动）。每一颗行星以相同的速度移动。
    碰撞规则：两个行星相互碰撞，较小的行星会爆炸。如果两颗行星大小相同，则两颗行星都会爆炸。两颗移动方向相同的行星，永远不会发生碰撞。

    输入：asteroids = [5,10,-5]
    输出：[5,10]
    解释：10 和 -5 碰撞后只剩下 10 。 5 和 10 永远不会发生碰撞。

    输入：asteroids = [10,2,-5]
    输出：[10]
    解释：2 和 -5 发生碰撞后剩下 -5 。10 和 -5 发生碰撞后剩下 10 。

    class Solution:
        def asteroidCollision(self, asteroids: List[int]) -> List[int]:
            tmp = list()
            for num in asteroids:
                flag = True
                while flag and num < 0 and tmp and tmp[-1] > 0:
                    flag = tmp[-1] < - num
                    if tmp[-1] <= -num:
                        tmp.pop()
                if flag:
                    tmp.append(num)
                
            return tmp

### 236. 二叉树的最近公共祖先

    “对于有根树 T 的两个节点 p、q，最近公共祖先表示为一个节点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”
    输入：root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
    输出：5

    class Solution:
        def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
            if root in (None, p, q):
                return root
            left = self.lowestCommonAncestor(root.left, p, q)
            right = self.lowestCommonAncestor(root.right, p, q)
            if left and right:
                return root
            return left if left else right

### 1372. 二叉树中的最长交错路径

    给你一棵以 root 为根的二叉树，二叉树中的交错路径定义如下：

    选择二叉树中 任意 节点和一个方向（左或者右）。
    如果前进方向为右，那么移动到当前节点的的右子节点，否则移动到它的左子节点。
    改变前进方向：左变右或者右变左。

    重复第二步和第三步，直到你在树中无法继续移动。

    交错路径的长度定义为：访问过的节点数目 - 1（单个节点的路径长度为 0 ）。

    请你返回给定树中最长 交错路径 的长度。

    输入：root = [1,1,1,null,1,null,null,1,1,null,1]
    输出：4

    # Definition for a binary tree node.
    # class TreeNode:
    #     def __init__(self, val=0, left=None, right=None):
    #         self.val = val
    #         self.left = left
    #         self.right = right

    class Solution:
        def longestZigZag1(self, root: Optional[TreeNode]) -> int:
            # // reach root, the longest path from left or right
            quene = deque([(root, 0, 0)])
            num = 0
            while quene:
                # print(quene)
                node, l, r = quene.popleft()
                num = max(num, l, r)
                if node.right:
                    quene.append([node.right, r+1, 0])
                if node.left:
                    quene.append([node.left, 0, l+1])
            return num
        
        def longestZigZag2(self, root: Optional[TreeNode]) -> int:
            num = 0
            def dfs(root, flag, sum):
                if root == None:
                    return 0
                nonlocal num
                num = max(num, sum)
                if flag:
                    dfs(root.left, True, 1)
                    dfs(root.right, False, sum+1)
                else:
                    dfs(root.left, True, sum+1)
                    dfs(root.right, False, 1)
            dfs(root, True, 0)
            return num


### 二分查找

    def binary_search(nums: list[int], target: int) -> int:
        left, right = 0, len(nums) - 1
        while left <= right:
            mid = (left + right) // 2
            if nums[mid] == target:
                return mid
            elif nums[mid] < target:
                left = mid + 1
            else:
                right = mid - 1
        return -1
    nums = [1,2,3,4,5,6,7,8,9,10]
    print(binary_search(nums, 6))

    def binary_search_lcro(nums: list[int], target: int) -> int:
        left, right = 0, len(nums)
        while left < right:
            mid = (left + right) // 2
            if nums[mid] == target:
                return mid
            elif nums[mid] < target:
                left = mid + 1
            else:
                right = mid
        return -1

    print(binary_search_lcro(nums=nums, target=5))

### 字典树

    struct TreeNode {
        VALUETYPE value;          //结点值
        TreeNode* children[NUM];  //指向孩子结点
    };

    struct TrieNode {
        bool isEnd;         //该结点是否是一个串的结束
        TrieNode* next[26]; //字母映射表
    };

    class Trie:
        def __init__(self):
            self.children = [None] * 26
            self.flag = False

        def insert(self, word: str) -> None:
            node = self
            for ch in word:
                ch = ord(ch) - ord("a")
                if not node.children[ch]:
                    node.children[ch] = Trie()
                node = node.children[ch]
            node.flag = True
        
        def searchPrefix(self, prefix: str) -> "Trie":
            node = self
            for ch in prefix:
                ch = ord(ch) - ord("a")
                if not node.children[ch]:
                    return None
                node = node.children[ch]
            return node

        def search(self, word: str) -> bool:
            node = self.searchPrefix(word)
            return node is not None and node.flag

        def startsWith(self, prefix: str) -> bool:
            return self.searchPrefix(prefix) is not None

### 399. 除法求值

    给你一个变量对数组 equations 和一个实数值数组 values 作为已知条件，其中 equations[i] = [Ai, Bi] 和 values[i] 共同表示等式 Ai / Bi = values[i] 。每个 Ai 或 Bi 是一个表示单个变量的字符串。
    另有一些以数组 queries 表示的问题，其中 queries[j] = [Cj, Dj] 表示第 j 个问题，请你根据已知条件找出 Cj / Dj = ? 的结果作为答案。
    返回 所有问题的答案 。如果存在某个无法确定的答案，则用 -1.0 替代这个答案。如果问题中出现了给定的已知条件中没有出现的字符串，也需要用 -1.0 替代这个答案。

    注意：输入总是有效的。你可以假设除法运算中不会出现除数为 0 的情况，且不存在任何矛盾的结果。

    输入1：equations = [["a","b"],["b","c"]], values = [2.0,3.0], queries = [["a","c"],["b","a"],["a","e"],["a","a"],["x","x"]]
    输出1：[6.00, 0.50, -1.00, 1.00, -1.00]
    解释：
    条件：a / b = 2.0, b / c = 3.0
    问题：a / c = ?, b / a = ?, a / e = ?, a / a = ?, x / x = ?
    结果：[6.0, 0.5, -1.0, 1.0, -1.0 ]
    注意：x 是未定义的 => -1.0

    输入2：equations = [["a","b"],["b","c"],["bc","cd"]], values = [1.5, 2.5, 5.0], queries = [["a","c"],["c","b"],["bc","cd"],["cd","bc"]]
    输出2：[3.75, 0.40, 5.00, 0.20]

    class Solution:
        def calcEquation(self, equations: List[List[str]], values: List[float], queries: List[List[str]]) -> List[float]:
            graph = defaultdict(dict)
            for (x, y), v in zip(equations, values):
                graph[x][y] = v
                graph[y][x] = 1/v
            <!-- defaultdict(<class 'dict'>, {'a': {'b': 2.0}, 'b': {'a': 0.5, 'c': 3.0}, 'c': {'b': 0.3333333333333333}}) -->
            <!-- print(graph) -->

            def dfs(a, b, visited: Set[str]):
                if a not in graph or b not in graph:
                    return -1
                if a == b:
                    return 1
                
                for k, v in graph[a].items():
                    if k in visited:
                        continue
                    
                    visited.add(k)
                    res = dfs(k, b, visited)
                    if res != -1:
                        return v * res

                return -1
            
            return [dfs(a, b, set((a,))) for a, b in queries ]

### 547. 省份数量

    有 n 个城市，其中一些彼此相连，另一些没有相连。如果城市 a 与城市 b 直接相连，且城市 b 与城市 c 直接相连，那么城市 a 与城市 c 间接相连。
    省份 是一组直接或间接相连的城市，组内不含其他没有相连的城市。
    给你一个 n x n 的矩阵 isConnected ，其中 isConnected[i][j] = 1 表示第 i 个城市和第 j 个城市直接相连，而 isConnected[i][j] = 0 表示二者不直接相连。

    返回矩阵中 省份 的数量。

    输入：isConnected = [[1,1,0],[1,1,0],[0,0,1]]
    输出：2

    class Solution:
        def findCircleNum1(self, isConnected: List[List[int]]) -> int:
            cities = len(isConnected)
            visited = set()
            provinces = 0

            for i in range(cities):
                if i not in visited:
                    que = deque([i])
                    while que:
                        node = que.popleft()
                        visited.add(node)
                        for k in range(cities):
                            if isConnected[node][k] == 1 and k not in visited:
                                que.append(k)
                    provinces += 1
                    
            return provinces

        def findCircleNum2(self, isConnected: List[List[int]]) -> int:
            def dfs(i: int):
                for j in range(cities):
                    if isConnected[i][j] == 1 and j not in visited:
                        visited.add(j)
                        dfs(j)
            
            cities = len(isConnected)
            visited = set()
            provinces = 0

            for i in range(cities):
                if i not in visited:
                    dfs(i)
                    provinces += 1
            
            return provinces

### 1926. 迷宫中离入口最近的出口

    给你一个 m x n 的迷宫矩阵 maze （下标从 0 开始），矩阵中有空格子（用 '.' 表示）和墙（用 '+' 表示）。同时给你迷宫的入口 entrance ，用 entrance = [entrancerow, entrancecol] 表示你一开始所在格子的行和列。
    每一步操作，你可以往 上，下，左 或者 右 移动一个格子。你不能进入墙所在的格子，你也不能离开迷宫。你的目标是找到离 entrance 最近 的出口。出口 的含义是 maze 边界 上的 空格子。entrance 格子 不算 出口。
    请你返回从 entrance 到最近出口的最短路径的 步数 ，如果不存在这样的路径，请你返回 -1 。

    输入：maze = [["+","+",".","+"],[".",".",".","+"],["+","+","+","."]], entrance = [1,2]
    输出：1
    解释：总共有 3 个出口，分别位于 (1,0)，(0,2) 和 (2,3) 。
    一开始，你在入口格子 (1,2) 处。
    - 你可以往左移动 2 步到达 (1,0) 。
    - 你可以往上移动 1 步到达 (0,2) 。
    从入口处没法到达 (2,3) 。
    所以，最近的出口是 (0,2) ，距离为 1 步。
    class Solution:
        def nearestExit(self, maze: List[List[str]], entrance: List[int]) -> int:
            m, n = len(maze), len(maze[0])
            q = deque([ ( entrance[0], entrance[1], -1 )])
            maze[entrance[0]][entrance[1]] = '+'

            while q:
                x, y, step = q.popleft()
                step += 1
                if (x == 0 or y == 0 or x == m-1 or y == n-1) and step > 0:
                    return step

                if x + 1 < m and maze[x+1][y] == '.':
                    q.append((x+1, y ,step))
                    maze[x+1][y] = '+'
                if x - 1 >= 0 and maze[x-1][y] == '.':
                    q.append((x-1, y, step))
                    maze[x-1][y] = '+'
                if y + 1 < n and maze[x][y+1] == '.':
                    q.append((x, y+1, step))
                    maze[x][y+1] = '+'
                if y - 1 >= 0 and maze[x][y-1] == '.':
                    q.append((x, y-1, step))
                    maze[x][y-1] = '+'

            return -1

        def nearestExit2(self, maze: List[List[str]], entrance: List[int]) -> int:
            m, n = len(maze), len(maze[0])
            q = deque([ ( entrance[0], entrance[1], -1 )])
            maze[entrance[0]][entrance[1]] = '+'

            dx = [1, 0, -1, 0]
            dy = [0, 1, 0, -1]
            
            while q:
                cx, cy, d = q.popleft()
                for k in range(4):
                    nx = cx + dx[k]
                    ny = cy + dy[k]
                    if 0 <= nx < m and 0 <= ny < n and maze[nx][ny] == '.':
                        if nx == 0 or nx == m - 1 or ny == 0 or ny == n-1:
                            return d+1
                        
                        maze[nx][ny] = '+'
                        q.append((nx, ny, d+1))
            return -1