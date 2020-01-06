# 最小的k个数

**关键字**：求数组中最小的k个数

**解题关键**：快排思想partition找到处于k-1位置的数，则其左边则是最小的k个数

**注意点**：

1. 输入特判，数组不为空，k不应小于0或大于数组长度
2. 缩小范围时，两个指针要更新

```
function GetLeastNumbers_Solution(input, k)
{
    // write code here
    //输入特判，数组不为空，k不应小于0或大于数组长度
    if((input.length===0) || (k>input.length) || (k<=0)) return [];
    let start=0, end=input.length-1;
    let index=partition(input,start, end);
    while(index!==k-1){
        if(index>k-1) {
        //缩小范围时，两个指针要更新
            end=index-1;
            index=partition(input,start, end);
        } else {
        //缩小范围时，两个指针要更新
            start=index+1;
            index=partition(input,start, end);
        }
    }
    return input.slice(0,k).sort();
}

function partition(arr, start, end){
    let temp=arr[start];
    while(start<end){
        while(start<end && arr[end]>temp) end--;
        arr[start]=arr[end];
        while(start<end && arr[start]<=temp) start++;
        arr[end] = arr[start];
    }
    arr[start]=temp;
    return start;
}
```

