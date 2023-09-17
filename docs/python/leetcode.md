
## leetcode 75

    双指针
    滑动窗口
    前缀和
    哈希表

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