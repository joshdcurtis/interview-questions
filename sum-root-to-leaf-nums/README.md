# Sum Root to Leaf Numbers

Problem based on [this leetcode problem.](https://leetcode.com/problems/sum-root-to-leaf-numbers/)

The idea is to provide the candidate with a broken solution and have them debug it.
If they are successful, the problem requirements are changed and they then have to modify the solution.


# Broken Solution Code
```python3 []
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def sumNumbers(self, root: Optional[TreeNode]) -> int:
        path = []
        def dfs(node):
            if not node:
                return
            path.append(node.val)
            if not node.left and not node.right:
                res = 0
                for i in range(len(path)):
                    res += (10 * i) * path[i]
                return res
            else:
                path.pop()
                return dfs(node.left) + dfs(node.right)
        return dfs(root)
```

## Bug List:
1. Pops from path before finding `dfs(node.left) + dfs(node.right)`
1. `i` should be `10`'s exponent, not its multiplier.
1. Wrong index of `path` when calculating `res`. Should iterate backwards rather than forwards
1. Never pops from `path` in the base case where we've reached a leaf node

# Sample Correct Solution Code
```python3 []
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def sumNumbers(self, root: Optional[TreeNode]) -> int:
        path = []
        def dfs(node):
            if not node:
                return
            path.append(node.val)
            if not node.left and not node.right:
                res = 0
                for i in range(len(path)):
                    res += (10 ** i) * path[len(path) - 1 - i]
            else:
                res = dfs(node.left) + dfs(node.right)
            path.pop()
            return res
        return dfs(root)
```

# Problem Modification ideas

## Easy
1. Reverse the order of the digits (i.e. a path that would have previously represented 457 onw represents 754)
1. Find the maximum root-to-leaf number
1. Consider any node at depth 3 a leaf node, do not traverse deeper into the tree
1. Ignore duplicate root-to-leaf numbers if they show up within the tree

## More involved
1. Find the sum of the "level numbers", where each number's digits are composed of the values for each node on a given level of the tree (horizontally based rather than vertically).
