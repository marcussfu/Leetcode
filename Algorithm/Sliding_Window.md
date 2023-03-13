Sliding Window
給定的input是線性資料結構，代表你可以循序存取，例如array、linked list、string

題目要求找出滿足特定條件的最長/最短的substring、subarray或一個目標值

就可以用Sliding Window。

- 在線性時間內完成(O(n))
- 透過操控two pointer找出滿足條件的區間
- 答案一定是continuous的資料，表示可以被依序存取

567. Permutation in String