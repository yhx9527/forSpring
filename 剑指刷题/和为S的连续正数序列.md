# 和为S的连续正数序列

**关键字**：输出所有和为S的连续正数序列（至少包含两个数）

**解题关键**：使用两个指针记录序列的起始和结束位置

1. 当从起始到结束的和大于S时，起始指针向后移
2. 当从起始到结束的和小于S时，结束指针向后移
3. 当从起始到结束的和等于S时，结束指针向后移，进行新一轮查找
4. 当起始指针到达和的一半时，结束循环

**注意点**：

sum小于2时特判

```
function FindContinuousSequence(sum)
{
    // write code here
    if(sum<0) return [];
    //sum小于2时特判
    if(sum<=2) return [];
    //使用两个指针记录序列的起始和结束位置
    let small=1, big=2, middle=Math.floor((sum+1)/2);
    let result=[], sequence=[small, big], total=small+big;
    //当起始指针到达和的一半时，结束循环
    while(small<middle){
        if(total===sum){
        //当从起始到结束的和等于S时，结束指针向后移，进行新一轮查找
            result.push([...sequence])
            big++;
            total+=big;
            sequence.push(big);
        }else if(total > sum){
        //当从起始到结束的和大于S时，起始指针向后移
            total-=small;
            small++;
            sequence.shift();
        } else{
        //当从起始到结束的和小于S时，结束指针向后移
            big++;
            total+=big;
            sequence.push(big);
        }
    }
    return result;
}
```

