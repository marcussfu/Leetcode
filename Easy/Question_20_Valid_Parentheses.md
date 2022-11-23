# Valid Parentheses
難度: Easy (javascript)

題目: 給一個字串s，包含'(', ')', '{', '}', '[' 和 ']'

檢查這個字串s是否是相同符號的開始和結束，是的話為true。

Example1:
```
Input: s = "()[]{}"
Output: true
```
Example2:
```
Input: s = "([{}])"
Output: true
```
Example3:
```
Input: s = "(]"
Output: false
```

限制:
- 1 <= s.length <= 104
- s 只會有 '()[]{}'.

解法: 

這邊有幾點可以直接想到
- 一定是開始的括號在前面，只要關閉的括號在前就表示為false
- 看提示，要用stack堆疊(先進後出)來處理，遍尋字串s，把開啟的括號存起來，直到關閉的括號出現為止
- 逐一把關閉的括號和從stack取出的開啟括號比對，是不是一組的，如果不對就是false，直到stack為空的時候，就是true了

```
var isValid = function(s) {
    if (s.length == 1)
        return false;
    let stack = [];
    for (c in s) {
        if (s[c] === '(' || s[c] === '{' || s[c] === '[') {
            stack.push(s[c]);
        }
        else {
            lastParenteses = stack[stack.length-1];
            stack.pop();
            if ((s[c] === ')' && lastParenteses != '(') ||
                (s[c] === '}' && lastParenteses != '{') ||
                (s[c] === ']' && lastParenteses != '[')) {
                    return false;
            }
        }
    }
    return stack.length == 0;
}
```

user8331寫的，JavaScript 48 ms, faster than 96.58%

這個方式是逐一比對字串s，如果是開啟的括號，就在stack存入對應的關閉括號
然後遍尋到關閉的括號時，就從stack取出括號來比對是否一樣，不一樣則回傳false

一樣stack為空的時候，就是true了

```
var isValid = function(s) {
    const stack = [];

    for (i in s) {
        let c = s.charAt(i);
        switch(c) {
            case '(':
                stack.push(')');
                break;
            case '{':
                stack.push('}');
                break;
            case '[':
                stack.push(']');
                break;
            default:
                if (c !== stack.pop())
                    return false;
        }
    }
    return stack.length === 0;
}
```