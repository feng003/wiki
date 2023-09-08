
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
