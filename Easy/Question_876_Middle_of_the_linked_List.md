# Middle of the Linked List
難度: Easy (javascript&Python)

題目: 給一個串連list，回傳這個list從中間的節點以後的list。

如果是偶數的節點，那就從中間的第二個節點起算。

Example 1:
```
Input: head = [1,2,3,4,5]
Output: [3,4,5]
Explanation: The middle node of the list is node 3.
```
Example 2:
```
Input: head = [1,2,3,4,5,6]
Output: [4,5,6]
Explanation: Since the list has two middle nodes with values 3 and 4, we return the second one.
```

解法:

可以先列一下如果節點數從1到5會是怎樣變化
- ['1']: 回傳[1]
- [1,'2']: 回傳[2]
- [1,'2',3]: 回傳[2,3]
- [1,2,'3',4]: 回傳[3,4]
- [1,2,'3',4,5]: 回傳[3,4,5]

這邊用two pointer方式去解，p1就是中位節點，而p2就是每一次的尾節點 
- 這邊去判斷p2是不是還有值，是不是next還繼續有值
- 目的是讓p2保持可以和p1是中位節點的距離
  
- 例如: 節點有1個的時候，根本就沒有下個節點，就只能回傳這個
- 節點有2個的時候，照規則就是要從第二個節點起算，這邊就是判斷p2在第一個節點時，有沒有next，有所以可以往下，p1也為next的節點，但p2想要等於next的next，就沒有了，所以下個迴圈就會停掉。
- 有3個節點的時候，如果p2有值，而且next也還有值，當p1從節點1到節點2的時候，要使p1為中位數節點的話，顯然p2就要從節點1到節點3(next的next)，但判斷p2沒有next節點時，迴圈就停了，所以中位數節點p1還是在2

javascript
```
var middleNode = function(head) {
    var p1 = head;
    var p2 = head;
    while(p2 != null && p2.next != null){
        p1 = p1.next;
        p2 = p2.next.next;
    }
    return p1;
};
```

python3
```
class Solution:
    def middleNode(self, head: Optional[ListNode]) -> Optional[ListNode]:
        p1 = head
        p2 = head
        while p2 != None and p2.next != None:
            p1 = p1.next
            p2 = p2.next.next
        return p1
```