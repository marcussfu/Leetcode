# Binary Tree Inorder Traversal
難度: Easy (javascript&Python)

題目: 把給的二元樹中序(Inorder)排序後取出。

Example 1:
```
Input: root = [1,null,2,3]
Output: [1,3,2]
```
Example 2:
```
Input: root = []
Output: []
```
Example 3:
```
Input: root = [1]
Output: [1]
```

解法: 二元樹每個節點有左(left)、中(val)和右(right)，中序排序就是把中在中間。
- 方法就是先找左邊，這邊用Depth-First-Search的方式，一直左左左找到最後一個節點，這過程中把經過的節點都存起來，判斷目前節點不存在，就表示沒有左了，於是取出存在stack的中節點存在result後，再從節點的right開始找，如果也沒有了，那就取出存在stack的上一個左節點，繼續重覆到stack沒有值為止。

javascript
```
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 * @return {number[]}
 */
var inorderTraversal = function(root) {
    let result = [];
    let stack = [];
    while(root !== null || stack.length > 0) {
        if (root !== null) {
            stack.push(root);
            root = root.left;
        }
        else {
            root = stack.pop();
            result.push(root.val);
            root = root.right;
        }
    }
    return result;
};
```

python3
```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        result, stack = [], []
        while root != None or len(stack) > 0:
            if root != None:
                stack.append(root)
                root = root.left
            else:
                root = stack.pop()
                result.append(root.val)
                root = root.right
        return result
```