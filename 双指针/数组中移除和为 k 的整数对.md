------

# 数组中移除和为 k 的整数对

## 🧩 问题描述

给定一个整数数组 `nums` 和一个整数 `k`，每一步操作可以从数组中移除两个数，使它们的和为 `k`。

**目标：** 返回最多可以执行多少次这样的操作。

------

## ✅ 方法一：哈希表（HashMap）

### 🔧 思路：

1. 用 `Map<Integer, Integer>` 存储每个数字的出现次数。
2. 遍历数组，计算当前数字的补数 `k - num` 是否已出现。
3. 若存在补数，配对成功，操作数 +1，减少补数计数；
4. 若不存在，将当前数字加入 map。

### 🧠 Java 实现：

```java
public int maxOperations(int[] nums, int k) {
    Map<Integer, Integer> map = new HashMap<>();
    int operations = 0;

    for (int num : nums) {
        int complement = k - num;
        if (map.getOrDefault(complement, 0) > 0) {
            operations++;
            map.put(complement, map.get(complement) - 1);
        } else {
            map.put(num, map.getOrDefault(num, 0) + 1);
        }
    }

    return operations;
}
```

### ⏱️ 复杂度分析：

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

------

## ✅ 方法二：排序 + 双指针

### 🔧 思路：

1. 对数组升序排序。
2. 使用两个指针：
   - `left` 从左向右（小的）
   - `right` 从右向左（大的）
3. 检查 `nums[left] + nums[right]`：
   - 等于 `k`：配对成功，两个指针同时移动；
   - 小于 `k`：说明和太小，`left++`；
   - 大于 `k`：说明和太大，`right--`。

### 🧠 Java 实现：

```java
public int maxOperations(int[] nums, int k) {
    Arrays.sort(nums);
    int left = 0, right = nums.length - 1;
    int operations = 0;

    while (left < right) {
        int sum = nums[left] + nums[right];
        if (sum == k) {
            operations++;
            left++;
            right--;
        } else if (sum < k) {
            left++;
        } else {
            right--;
        }
    }

    return operations;
}
```

### ⏱️ 复杂度分析：

- 时间复杂度：`O(n log n)`（排序）
- 空间复杂度：`O(1)`（不计排序的递归栈）

------

## 🔄 双指针移动逻辑解释

| 条件                            | 操作                | 原因                 |
| ------------------------------- | ------------------- | -------------------- |
| `nums[left] + nums[right] == k` | `left++`, `right--` | 成功配对             |
| `< k`                           | `left++`            | 左边太小，往右找大数 |
| `> k`                           | `right--`           | 右边太大，往左找小数 |

------

## 🆚 方法对比

| 比较维度         | 哈希表方法 | 双指针方法 |
| ---------------- | ---------- | ---------- |
| 时间复杂度       | O(n)       | O(n log n) |
| 空间复杂度       | O(n)       | O(1)       |
| 是否破坏顺序     | 否         | 是（排序） |
| 是否使用额外内存 | 是         | 否         |
| 实现复杂度       | 稍高       | 简单直观   |
| 多次查不同 k     | 支持       | 不适合     |

------

## ✅ 总结建议

- **哈希表方法更快**（适合不改变数组、或需要多次查不同 k）
- **双指针方法更省空间**（适合空间敏感，或数组本身已排序）

------

如需更深入练习，可以在 LeetCode 搜索：**[1679. Max Number of K-Sum Pairs](https://leetcode.com/problems/max-number-of-k-sum-pairs/)**

------

