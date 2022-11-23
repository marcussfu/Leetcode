# Merge Two Sorted Lists
難度: Easy (javascript)

題目: 把兩個已經排序好(從小到大)的linked list合成一個從小到大排序的linked list。

Example 1:
```
Input: list1 = [1,2,4], list2 = [1,3,4]
Output: [1,1,2,3,4,4]
```
Example 2:
```
Input: list1 = [], list2 = []
Output: []
```
Example 3:
```
Input: list1 = [], list2 = [0]
Output: [0]
```

解法: 
可以分為使用recursion和while兩種

recursion: 
- 用遞迴的方式來比較兩個list的val大小，如果list1比較小，那就讓list1的next和list2的節點一起再用同樣的方法(mergeTwoLists)重新比較，並回傳list1目前節點
- 如果換成是list2的val比較小，那就換list2的next與list1的節點(mergeTwoLists)重新比較，並回傳list2目前節點
- 一直比較到list1或list2其中一個空了，就可以從遞迴逐一回算(return)到最前面return的list1或list2
```
var mergeTwoLists = function(list1, list2) {
    if (list1 === null)
        return list2;
    if (list2 === null)
        return list1;

    if (list1.val <= list2.val) {
        list1.next = mergeTwoLists(list1.next, list2);
        return list1;
    }
    else {
        list2.next = mergeTwoLists(list1, list2.next);
        return list2;
    }
};
```

while:
- 先產生兩個node，mergeHead和crt(複製mergeHead)
- mergeHead這個節點就是存放整個listed list最一開始的節點，以供後來回傳答案用
- crt則是while迴圈每次檢查都使其一直保持在最前面的節點
- while檢查每次list1和list2都要有值的狀態下，比較list1的val和list2的val的大小，取出比較小的
- 把比較小的存放到crt的next節點，並將被存放的list1或list2轉為自己的next節點接著下一次的比較
- 當然crt也要轉為自己的next節點，保持在最前面的節點
- 直到while的判斷，list1或list2已經有其中一個沒有值了，那就可以取出最後還有值的list，放到crt的next節點
- 因為crt已經把整個list都存好了，最後當然是回傳之前就存好的mergeHead這個最前面節點的next，就可以得到整個排序好的linked list。

```
var mergeTwoLists = function(list1, list2) {
    let mergeHead = {val: -1, next: null},
        crt = mergeHead;
    
    while(list1 && list2) {
        if (list1.val > list2.val) {
            crt.next = list2;
            list2 = list2.next;
        }
        else {
            crt.next = list1;
            list1 = list1.next;
        }
        crt = crt.next;
    }
    crt.next = list1 || list2;
    return mergeHead.next;
};
```