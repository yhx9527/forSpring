# 和为S的两个数字

**关键字**：在递增的数组数组中查找两个数使之和为S，若有多对则输出乘积最小的

**解题关键**：双指针法，一个指向头，一个指向尾

1. 当两数之和比S大时，尾指针向前
2. 当两数之和比S小时，头指针向后
3. 找到一对之后，更新两个指针继续找

```
function FindNumbersWithSum(array, sum)
{
    // write code here
    if(array.length===0) return [];
    //双指针法，一个指向头，一个指向尾
    let start=0, end=array.length-1;
    let result=[], min=99999999;
    //遍历
    while(start<end){
        let tempSum=array[start]+array[end];
        if(tempSum===sum){
            if(array[start]*array[end]<min){
                result=[array[start], array[end]];
                min=array[start]*array[end];
            }
            //找到一对之后，更新两个指针继续找
            start++;
            end--;
        }else if(tempSum > sum){
        //当两数之和比S大时，尾指针向前
            end--;
        }else{
        //当两数之和比S小时，头指针向后
            start++;
        }
    }
    return result;
}
```

