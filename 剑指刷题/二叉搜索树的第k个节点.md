# 二叉搜索树的第k个节点

**关键字**：二叉搜索树中求第k小的节点

**解题关键**： 中序遍历

二叉搜索树的中序遍历是有序的，因此我们可以在中序遍历过程中去找第k小的节点

**注意点**：

当找到之后要退出递归

```
/* function TreeNode(x) {
    this.val = x;
    this.left = null;
    this.right = null;
} */
function KthNode(pRoot, k)
{
    // write code here
    if(pRoot===null || k<=0) return;
    let result=null;
    //二叉搜索树的中序遍历是有序的，因此我们可以在中序遍历过程中去找第k小的节点
    function inOrder(pRoot){
        if(pRoot===null) return;
        //当找到之后要退出递归
        if(result===null){
           inOrder(pRoot.left); 
        }
        //当找到之后要退出递归
        if(result===null){
           k--;
            if(k===0){
                result=pRoot;
            } 
        }
        //当找到之后要退出递归
        if(result===null){
           inOrder(pRoot.right); 
        }
    }
    inOrder(pRoot);
    if(result===null) return;
    return result;
}
```

