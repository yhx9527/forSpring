# 21. 包含min函数的栈

**关键字**：O(1)的时间复杂度实现min函数

**解题关键**：

使用辅助栈在push时同步数据栈记录最小值

1. 当辅助栈为空或者node值小于辅助栈栈顶，则node入栈
2. node值大于辅助栈栈顶，则栈顶值接着入栈，使之与数据栈保持同步
3. 辅助栈栈顶始终保持是当前的最小值

```
const data_stack=[];
const min_stack=[];
function push(node)
{
    // write code here
    data_stack.push(node);
    //使用辅助栈在push时同步数据栈记录最小值
    //当辅助栈为空或者node值小于辅助栈栈顶，则node入栈
    if(min_stack.length===0 || top()>node){
        min_stack.push(node);
    } else {
    //node值大于辅助栈栈顶，则栈顶值接着入栈，使之与数据栈保持同步
        min_stack.push(top());
    }
}
function pop()
{
    // write code here
    if(data_stack.length>0 && min_stack.length>0){
        data_stack.pop();
        min_stack.pop();
    }
}
function top()
{
    if(min_stack.length>0){
        return min_stack[min_stack.length-1];
    }
    return;
    // write code here
}
//辅助栈栈顶始终保持是当前的最小值
function min()
{
    // write code here
    return top();
}


```

