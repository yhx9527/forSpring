# 链表倒数第k个节点

**解题关键**： 双指针遍历链表, 前头指针先走k-1步，然后双指针一起走

**注意点**：

代码鲁棒性

1. 链表为空，k小于0
2. 链表长度小于k



```
/*function ListNode(x){
    this.val = x;
    this.next = null;
}*/
function FindKthToTail(head, k)
{
    // write code here
    //链表为空，k小于0
    if(head===null || k<=0) return null;
    
    let aheadNode = head, behindNode = null;
    for(let i=0;i<k-1;i++){
    		//链表长度小于k
        if(aheadNode.next===null){
            return null;
        } else {
            aheadNode = aheadNode.next;
        }
    }
    behindNode = head;
    while(aheadNode.next){
        behindNode = behindNode.next;
        aheadNode = aheadNode.next;
    }
    return behindNode;
}
```

