# Construct Binary Tree from Preorder and Inorder Traversal
難度: Medium (javascript&Python)

題目: 給了兩個數字陣列，分別為二元樹的前序和中序排序，根據這兩個排序出這個二元樹。

Example 1:
```
            3
           / \
          9   20
              / \
             15  7

Input: preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
Output: [3,9,20,null,null,15,7]
```
Example 2:
```
Input: preorder = [-1], inorder = [-1]
Output: [-1]
```

解法:

最後合成的二元樹顯然是level order，從根節點一層一層往下。

這題雖然知道前序和中序怎麼排序，但不懂要怎麼合成出二元樹level order，參考了解答才知道箇中原因。

前序(root, left, right)的概念，就是每個節點都是先root，再left和right，所以每個節點都有機會當一次root，藉由每次從前序拿出一個節點，

中序(left, root, right)的概念，當然也是每個節點都可以是root，但不同的是，中序的排序是left，root，right，所以取出的節點，左邊就是該節點的left子樹，右邊就是該節點的right子樹，例: [9,(3),15,20,7]，9就是3的左子樹，[15,20,7]就是右子樹。

因為這樣可以看出，每次都從前序拿出一個節點，然後到中序去比對並取出該節點，再從該節點的中序分別往左子樹和右子樹遞迴往下，左子樹又會從前序取出節點，再從這個節點去中序比對，再分別往左子樹和右子樹遞迴，一直遞迴到沒有節點為止。

Time complexity - O(N) because each node is visited once.

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
 * @param {number[]} preorder
 * @param {number[]} inorder
 * @return {TreeNode}
 */
var buildTree = function(preorder, inorder) {
    if (inorder.length > 0) {
        console.log('before', preorder, inorder);
        let ind = inorder.indexOf(preorder.shift());
        let root = new TreeNode(inorder[ind]);
        console.log('after', preorder, inorder, ind, root);
        console.log("=====================");
        root.left = buildTree(preorder, inorder.slice(0, ind));
        root.right = buildTree(preorder, inorder.slice(ind+1, inorder.length));
        return root;
    }
    return null;
};

印出解題過程:

before [ 3, 9, 20, 15, 7 ] [ 9, 3, 15, 20, 7 ]
after [ 9, 20, 15, 7 ] [ 9, 3, 15, 20, 7 ] 1 TreeNode { val: 3, left: null, right: null }
=====================
before [ 9, 20, 15, 7 ] [ 9 ]
after [ 20, 15, 7 ] [ 9 ] 0 TreeNode { val: 9, left: null, right: null }
=====================
before [ 20, 15, 7 ] [ 15, 20, 7 ]
after [ 15, 7 ] [ 15, 20, 7 ] 1 TreeNode { val: 20, left: null, right: null }
=====================
before [ 15, 7 ] [ 15 ]
after [ 7 ] [ 15 ] 0 TreeNode { val: 15, left: null, right: null }
=====================
before [ 7 ] [ 7 ]
after [] [ 7 ] 0 TreeNode { val: 7, left: null, right: null }
=====================
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
    def buildTree(self, preorder: List[int], inorder: List[int]) -> Optional[TreeNode]:
        if inorder:
            ind = inorder.index(preorder.pop(0))
            root = TreeNode(inorder[ind])
            root.left = self.buildTree(preorder, inorder[0:ind])
            root.right = self.buildTree(preorder, inorder[ind+1:])
            return root
```
