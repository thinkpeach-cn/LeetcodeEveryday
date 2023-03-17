# LeetCode刷题

## 346. 数据流中的移动平均值

> **给定一个整数数据流和一个窗口大小，根据该滑动窗口的大小，计算其所有整数的移动平均值。**

**示例：**

```
MovingAverage m = new MovingAverage(3)
m.next(1) = 1
m.next(10) = (1 + 10) / 2
m.next(3) = (1 + 10 + 3) / 3
m.next(5) = (10 + 3 + 5) / 3
```

```javascript
/*分析测试用例
* 1.窗口大小固定，并且为整数
* 2.调用next添加数字并且计算平均值
* 3.超过窗口大小，最先添加的数字优先移除(先进先出)
* */

/*解题思路：
* 算法原理
* 1.使用队列添加数字
* 2.通过创建成员变量来保持计算的值
* */

/*算法流程
* 1.新增整数,进入队列操作
* 2.累加整数并保存结果
* 3.如果队列大小超出窗口大小，进行出队操作，并且对成员变量进行减法
* 4.返回平均值
* */

let MovingAverage = function (size) {
    this.windowSize = size
    this.myQueue = new Queue()
    this.sum = 0
}

MovingAverage.prototype.next = function (val) {
    if (this.myQueue.getSize() > this.windowSize) {
        this.sum -= this.myQueue.getHeader()
        this.myQueue.deQueue()
    }
    this.myQueue.enQueue(val)
    this.sum += val
    return this.sum / this.myQueue.getSize()
}

class Queue {
    constructor() {
        this.queue = []
    }

    enQueue(item) {
        return this.queue.push(item)
    }

    deQueue() {
        return this.queue.shift()//第一个元素从数组中删除，并返回第一个元素的值。
    }

    getHeader() {
        return this.queue[0]
    }

    getSize() {
        return this.queue.length
    }

    isEmpty() {
        return this.getSize() === 0
    }
}
```

## [206. 反转链表](https://leetcode.cn/problems/reverse-linked-list/)

![反转链表.drawio (1)](https://cos.icehim.com/typora/%E5%8F%8D%E8%BD%AC%E9%93%BE%E8%A1%A8.drawio%20(1).svg)

```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} head
 * @return {ListNode}
 * 分析测试用例
 * 1.确定好一个链表的头节点
 * 2.输入一个反转后的链表
 *
 * 算法原理
 * 1.递归遍历链表
 * 2.遍历的过程中反转next指针
 *
 * 算法流程
 * 1.创建两个变量，一个是未反转的链表指针，另一个时反转后的链表
 * 2.递归遍历未反转的链表
 * 3.修改next指针，并且当前链表指针向前继续遍历，直到next等于null
 *
 */

var reverseList = function(head) {
    let cur = head
    let prev = null
    while (cur){
        const temp = cur.next
        cur.next = prev
        prev = cur
        cur = temp
    }
    return  prev
};
console.log(reverseList([1, 2, 3, 4, 5]));
```

## [144. 二叉树的前序遍历](https://leetcode.cn/problems/binary-tree-preorder-traversal/)

递归

```javascript
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
var preorderTraversal = function(root) {
    const res = []
    set(root)
    return res
    function set(tree) {
        //判断根节点是否存在
        if (!tree) return
        //将根节点的值添加到res
        res.push(tree.val)
        //判断、递归左节点
        tree.left && set(tree.left)
        //判断、递归右节点
        tree.right && set(tree.right)
    }
};
```

迭代

```javascript
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
class Stack {
    constructor() {
        this.stack = []
    }

    push(item) {
        return this.stack.push(item)
    }

    pop() {
        return this.stack.pop()//移除并返回最后一个元素
    }

    peek() {
        return this.stack[this.getSize() - 1]
    }

    getSize() {
        return this.stack.length
    }

    isEmpty() {
        return this.getSize() === 0;
    }
}
var preorderTraversal = function(root) {
    //创建一个栈
    let myStack = new Stack()
    let arr = []
    //根节点放入栈中
    root && myStack.push(root)
    //迭代
    while (!myStack.isEmpty()){
        //出栈
        let cur = myStack.pop()
        //将出栈的节点值添加到arr中
        arr.push(cur.val)
        //判断右节点是否存在
        cur.right && myStack.push(cur.right)
        //判断左节点是否存在
        cur.left && myStack.push(cur.left)
    }
    return arr
};
```

## [94. 二叉树的中序遍历](https://leetcode.cn/problems/binary-tree-inorder-traversal/)

```javascript
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
    const res = []
    set(root)
    return res
    function set(tree) {
        //判断根节点是否存在
        if (!tree) return
        //判断、递归左节点
        tree.left && set(tree.left)
        //将根节点的值添加到res
        res.push(tree.val)
        //判断、递归右节点
        tree.right && set(tree.right)
    }
}
```

## [145. 二叉树的后序遍历](https://leetcode.cn/problems/binary-tree-postorder-traversal/)

```javascript
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
var postorderTraversal = function(root) {
    const res = []
    set(root)
    return res
    function set(tree) {
        //判断根节点是否存在
        if (!tree) return
        //判断、递归左节点
        tree.left && set(tree.left)
        //判断、递归右节点
        tree.right && set(tree.right)
        //将根节点的值添加到res
        res.push(tree.val)
    }
};
```
