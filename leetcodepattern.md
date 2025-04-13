# LeetCode Pattern Tutorial in Java

## Introduction
This tutorial will guide you through common patterns used to solve LeetCode problems in Java. Recognizing these patterns can help you approach coding challenges more effectively.

## 1. Two Pointers Pattern
Used for problems involving arrays or linked lists where you need to find pairs or subarrays that meet certain criteria.

### Example: Two Sum (LeetCode #1)
```java
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        if (map.containsKey(complement)) {
            return new int[] { map.get(complement), i };
        }
        map.put(nums[i], i);
    }
    throw new IllegalArgumentException("No two sum solution");
}
```

### Example: Remove Duplicates from Sorted Array (LeetCode #26)
```java
public int removeDuplicates(int[] nums) {
    if (nums.length == 0) return 0;
    int i = 0;
    for (int j = 1; j < nums.length; j++) {
        if (nums[j] != nums[i]) {
            i++;
            nums[i] = nums[j];
        }
    }
    return i + 1;
}
```

## 2. Sliding Window Pattern
Useful for problems involving subarrays or substrings with certain conditions.

### Example: Maximum Subarray (LeetCode #53)
```java
public int maxSubArray(int[] nums) {
    int maxSum = nums[0];
    int currentSum = nums[0];
    
    for (int i = 1; i < nums.length; i++) {
        currentSum = Math.max(nums[i], currentSum + nums[i]);
        maxSum = Math.max(maxSum, currentSum);
    }
    return maxSum;
}
```

### Example: Longest Substring Without Repeating Characters (LeetCode #3)
```java
public int lengthOfLongestSubstring(String s) {
    Map<Character, Integer> map = new HashMap<>();
    int left = 0, maxLen = 0;
    
    for (int right = 0; right < s.length(); right++) {
        char c = s.charAt(right);
        if (map.containsKey(c)) {
            left = Math.max(left, map.get(c) + 1);
        }
        map.put(c, right);
        maxLen = Math.max(maxLen, right - left + 1);
    }
    return maxLen;
}
```

## 3. Fast & Slow Pointers Pattern
Commonly used in linked list problems, especially those involving cycles.

### Example: Linked List Cycle (LeetCode #141)
```java
public boolean hasCycle(ListNode head) {
    if (head == null) return false;
    
    ListNode slow = head;
    ListNode fast = head.next;
    
    while (slow != fast) {
        if (fast == null || fast.next == null) {
            return false;
        }
        slow = slow.next;
        fast = fast.next.next;
    }
    return true;
}
```

## 4. Merge Intervals Pattern
For problems dealing with overlapping intervals.

### Example: Merge Intervals (LeetCode #56)
```java
public int[][] merge(int[][] intervals) {
    if (intervals.length <= 1) return intervals;
    
    // Sort by starting time
    Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));
    
    List<int[]> result = new ArrayList<>();
    int[] newInterval = intervals[0];
    result.add(newInterval);
    
    for (int[] interval : intervals) {
        if (interval[0] <= newInterval[1]) {
            // Overlapping intervals, update the end
            newInterval[1] = Math.max(newInterval[1], interval[1]);
        } else {
            newInterval = interval;
            result.add(newInterval);
        }
    }
    return result.toArray(new int[result.size()][]);
}
```

## 5. Cyclic Sort Pattern
Useful for problems involving arrays containing numbers in a given range.

### Example: Missing Number (LeetCode #268)
```java
public int missingNumber(int[] nums) {
    int i = 0;
    while (i < nums.length) {
        if (nums[i] < nums.length && nums[i] != i) {
            swap(nums, i, nums[i]);
        } else {
            i++;
        }
    }
    
    for (i = 0; i < nums.length; i++) {
        if (nums[i] != i) return i;
    }
    return nums.length;
}

private void swap(int[] nums, int i, int j) {
    int temp = nums[i];
    nums[i] = nums[j];
    nums[j] = temp;
}
```

## 6. Depth-First Search (DFS) Pattern
For tree and graph traversal problems.

### Example: Number of Islands (LeetCode #200)
```java
public int numIslands(char[][] grid) {
    if (grid == null || grid.length == 0) return 0;
    
    int numIslands = 0;
    for (int i = 0; i < grid.length; i++) {
        for (int j = 0; j < grid[i].length; j++) {
            if (grid[i][j] == '1') {
                numIslands++;
                dfs(grid, i, j);
            }
        }
    }
    return numIslands;
}

private void dfs(char[][] grid, int i, int j) {
    if (i < 0 || i >= grid.length || j < 0 || j >= grid[i].length || grid[i][j] == '0') {
        return;
    }
    
    grid[i][j] = '0'; // mark as visited
    dfs(grid, i + 1, j); // down
    dfs(grid, i - 1, j); // up
    dfs(grid, i, j + 1); // right
    dfs(grid, i, j - 1); // left
}
```

## 7. Breadth-First Search (BFS) Pattern
For level-order traversal or shortest path problems.

### Example: Binary Tree Level Order Traversal (LeetCode #102)
```java
public List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> result = new ArrayList<>();
    if (root == null) return result;
    
    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    
    while (!queue.isEmpty()) {
        int levelSize = queue.size();
        List<Integer> currentLevel = new ArrayList<>();
        
        for (int i = 0; i < levelSize; i++) {
            TreeNode node = queue.poll();
            currentLevel.add(node.val);
            if (node.left != null) queue.offer(node.left);
            if (node.right != null) queue.offer(node.right);
        }
        result.add(currentLevel);
    }
    return result;
}
```

## 8. Dynamic Programming Pattern
For optimization problems with overlapping subproblems.

### Example: Climbing Stairs (LeetCode #70)
```java
public int climbStairs(int n) {
    if (n == 1) return 1;
    
    int[] dp = new int[n + 1];
    dp[1] = 1;
    dp[2] = 2;
    
    for (int i = 3; i <= n; i++) {
        dp[i] = dp[i - 1] + dp[i - 2];
    }
    return dp[n];
}
```

### Example: House Robber (LeetCode #198)
```java
public int rob(int[] nums) {
    if (nums.length == 0) return 0;
    if (nums.length == 1) return nums[0];
    
    int[] dp = new int[nums.length];
    dp[0] = nums[0];
    dp[1] = Math.max(nums[0], nums[1]);
    
    for (int i = 2; i < nums.length; i++) {
        dp[i] = Math.max(dp[i - 1], dp[i - 2] + nums[i]);
    }
    return dp[nums.length - 1];
}
```

