# Symmetric Tree
難度: Easy (javascript&Python)

題目: 檢查樹是否對稱。
遞迴和迭代兩種方法都要

Example 1:

         1
        /  \
       2    2
      / \  / \
     3  4 4   3

Input: root = [1,2,2,3,4,4,3]
Output: true

Example 2:

         1
        /  \
       2    2
        \    \
         3    3

Input: root = [1,2,2,null,3,null,3]
Output: false

限制:
- The number of nodes in the tree is in the range [1, 1000]

解法:

分為遞迴和迭代兩種方式來遍行檢查樹。

遞迴的方式，就是每次去比較對稱的兩個節點值有沒有相同
有的話，就繼續往下遞迴找節點，直到沒有節點或其中一邊沒有節點為止。

- 可以看到下面的code，將root的左右節點傳入遞迴的方法
- 判斷如果兩邊都沒有節點了，那也是對稱，回傳true
- 如果只有一邊沒有節點，就不對稱了，回傳false
- 如果有節點，但節點的值(val)不同，一樣不對稱
- 判斷節點的值相同的話，就表示目前的節點是對稱的，往下遞迴找下面的節點
- 往下找節點，也要找對稱的，像是(左邊的左節點，跟右邊的右節點)，(左邊的右節點，跟右邊的左節點)
- 等到遞迴回傳，確認都對稱，則為true，否則為false

時間複雜度為: O(n), 因為我們遍尋了每個節點。

python3
```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def isSymmetric(self, root: TreeNode) -> bool:
        if not root: return True
        return self.isSym(root.left, root.right)
    
    def isSym(self, l: TreeNode, r: TreeNode) -> bool:
        if not l and not r: return True
        if not l or not r: return False
        return l.val == r.val and self.isSym(l.left, r.right) and self.isSym(l.right, r.left)
```

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
 * @return {boolean}
 */
var isSymmetric = function(root) {
    if (root === null) {
        return true;
    }
    return isSym(root.left, root.right);
};

const isSym = (l, r) => {
    if (l === null && r === null) {
        return true;
    }
    if (l === null || r === null) {
        return false;
    }
    return l.val === r.val && isSym(l.left, r.right) && isSym(l.right, r.left);
}
```

迭代的方式，就是改成while迴圈來處理
- 這邊用stack儲存需處理的對稱節點
- 後進先出的方式，一直pop出去(DFS)
- 如果取出的兩個對稱兩個節點都為None，也是對稱，就continue下去，直到不對稱或stack沒有值可取為止。
- 如果兩個對稱的節點有一個不是None，或者節點的值不同，就表示不對稱，回傳false。
- 往stack增加往下的對稱節點(左邊的左節點，跟右邊的右節點)，(左邊的右節點，跟右邊的左節點)
- 如果整個stack都檢查空了，就表示都為對稱，回傳true。

時間複雜度跟遞迴一樣為O(n)，因為遍尋了每個節點


python3
```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isSymmetric(self, root: Optional[TreeNode]) -> bool:
        if not root: return True
        stack = [(root.left, root.right)]

        while stack:
            l,r = stack.pop() # DFS
            # l,r = stack.pop(0) BFS
            if l == r == None: continue
            if not (l and r) or l.val != r.val: return False
            stack += (l.left, r.right), (l.right, r.left)
        return True
```

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
 * @return {boolean}
 */
var isSymmetric = function(root) {
    if (root === null) return true;
    stack = [{left: root.left, right: root.right}];
    while (stack.length > 0) {
        const node = stack.pop();//DFS
        // const node = stack.shift(); BFS
        if (node.left === null && node.right === null) continue;
        if ((node.left === null || node.right === null) || 
            (node.left.val !== node.right.val))
            return false;
        stack.push({left: node.left.left, right: node.right.right});
        stack.push({left: node.left.right, right: node.right.left});
    }
    return true;
};
```

也可以換成BTS廣度優先搜尋的方式，這邊的方式就是從stack取值的方式改成先進先出

但其實也是遍尋每個節點，時間複雜度一樣為O(n)
  