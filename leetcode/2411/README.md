# [2411. Smallest Subarrays With Maximum Bitwise OR (Medium)](https://leetcode.com/problems/smallest-subarrays-with-maximum-bitwise-or)

<p>You are given a <strong>0-indexed</strong> array <code>nums</code> of length <code>n</code>, consisting of non-negative integers. For each index <code>i</code> from <code>0</code> to <code>n - 1</code>, you must determine the size of the <strong>minimum sized</strong> non-empty subarray of <code>nums</code> starting at <code>i</code> (<strong>inclusive</strong>) that has the <strong>maximum</strong> possible <strong>bitwise OR</strong>.</p>
<ul>
	<li>In other words, let <code>B<sub>ij</sub></code> be the bitwise OR of the subarray <code>nums[i...j]</code>. You need to find the smallest subarray starting at <code>i</code>, such that bitwise OR of this subarray is equal to <code>max(B<sub>ik</sub>)</code> where <code>i &lt;= k &lt;= n - 1</code>.</li>
</ul>
<p>The bitwise OR of an array is the bitwise OR of all the numbers in it.</p>
<p>Return <em>an integer array </em><code>answer</code><em> of size </em><code>n</code><em> where </em><code>answer[i]</code><em> is the length of the <strong>minimum</strong> sized subarray starting at </em><code>i</code><em> with <strong>maximum</strong> bitwise OR.</em></p>
<p>A <strong>subarray</strong> is a contiguous non-empty sequence of elements within an array.</p>
<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<pre><strong>Input:</strong> nums = [1,0,2,1,3]
<strong>Output:</strong> [3,3,2,2,1]
<strong>Explanation:</strong>
The maximum possible bitwise OR starting at any index is 3. 
- Starting at index 0, the shortest subarray that yields it is [1,0,2].
- Starting at index 1, the shortest subarray that yields the maximum bitwise OR is [0,2,1].
- Starting at index 2, the shortest subarray that yields the maximum bitwise OR is [2,1].
- Starting at index 3, the shortest subarray that yields the maximum bitwise OR is [1,3].
- Starting at index 4, the shortest subarray that yields the maximum bitwise OR is [3].
Therefore, we return [3,3,2,2,1]. 
</pre>
<p><strong class="example">Example 2:</strong></p>
<pre><strong>Input:</strong> nums = [1,2]
<strong>Output:</strong> [2,1]
<strong>Explanation:
</strong>Starting at index 0, the shortest subarray that yields the maximum bitwise OR is of length 2.
Starting at index 1, the shortest subarray that yields the maximum bitwise OR is of length 1.
Therefore, we return [2,1].
</pre>
<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>
<ul>
	<li><code>n == nums.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

**Companies**:
[Amazon](https://leetcode.com/company/amazon)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Binary Search](https://leetcode.com/tag/binary-search/), [Bit Manipulation](https://leetcode.com/tag/bit-manipulation/), [Sliding Window](https://leetcode.com/tag/sliding-window/)

**Similar Questions**:
* [Merge k Sorted Lists (Hard)](https://leetcode.com/problems/merge-k-sorted-lists/)
* [Bitwise ORs of Subarrays (Medium)](https://leetcode.com/problems/bitwise-ors-of-subarrays/)
* [Longest Subarray With Maximum Bitwise AND (Medium)](https://leetcode.com/problems/longest-subarray-with-maximum-bitwise-and/)

## Solution 1. Find Minimum Sliding Window

```cpp
// OJ: https://leetcode.com/problems/smallest-subarrays-with-maximum-bitwise-or
// Author: github.com/lzl124631x
// Time: O(ND) where D is the number of bits of `A[i]`
// Space: O(D)
class Solution {
public:
    vector<int> smallestSubarrays(vector<int>& A) {
        int N = A.size(), i = 0, j = 0, cnt[32] = {}, goal[32] = {};
        vector<int> ans(N);
        auto add = [](int n, int cnt[32], int sign) {
            for (int k = 0; k < 32; ++k) cnt[k] += (n >> k & 1) * sign;
        };
        auto valid = [&]() {
            for (int k = 0; k < 32; ++k) {
                if (goal[k] && !cnt[k]) return false;
            }
            return true;
        };
        for (int n : A) add(n, goal, 1);
        for (; i < N; ++i) {
            while (j <= i || !valid()) add(A[j++], cnt, 1);
            ans[i] = j - i;
            add(A[i], cnt, -1);
            add(A[i], goal, -1);
        }
        return ans;
    }
};
```