# 掷骰子的n种方法

**关键字**：给定骰子数，骰子的面数，求出给定值的骰子的掷法. **动态规划**

**最优解**:

1. 使用一个一维数组，其中下标表示点数，数组值表示方法种类

先理解了剑指的写法，再理解当前问题的最优解,

```
/**
 * @param {number} d
 * @param {number} f
 * @param {number} target
 * @return {number}
 */
var numRollsToTarget = function(d, f, target) {
//使用一个一维数组，其中下标表示点数，数组值表示方法种类
    let result=new Array(target+1).fill(0);
    //一个骰子时的初始值
    for(let i=1;i<=f&&i<=target;i++){
        result[i]=1;
    }
    //再掷d-1个骰子
    for(let i=1;i<d;i++){
    //从后往前进行状态推进
        for(let j=target;j>=1;j--){
            let sum=0;
            //下次循环的点数n = 上次循环的点数n-1,n-2,…n-f之和
            for(let k=1;k<=f&&j>=k;k++){
                sum=(sum+result[j-k])%1000000007;
            }
            result[j]=sum;
        }
        
    }
    return result[target]; 
};
```



**剑指的解题关键**：

1. 使用一维数组，其中下标表示骰子点数，数组保存该点数的投掷种类
2. 使用一维数组两个来实现交替，一个表示上次循环生成的，一个表示下个循环需要的
3. 下次循环的点数n = 上次循环的点数n-1,n-2,…n-f之和



```
/**
 * @param {number} d
 * @param {number} f
 * @param {number} target
 * @return {number}
 */
var numRollsToTarget = function(d, f, target) {
    if(d<=0 || f<=0 || target>d*f || target <d) return 0;
    let flag=0;
    let result=[];
    //使用一维数组，其中下标表示骰子点数，数组保存该点数的投掷方法
    //使用一维数组两个来实现交替，一个表示上次循环生成的，一个表示下个循环需要的
    result[0] = new Array(d*f+1).fill(0);
    result[1] = new Array(d*f+1).fill(0);
    //只有一个骰子的时候初始化
    for(let i=1;i<=f;i++){
        result[flag][i]=1;
    }
    //每增加一个骰子
    for(let i=2;i<=d;i++){
    		//i个骰子点数最小值应为i，故小于i的投掷方法为0
        for(let z=0;z<i;z++){
            result[1-flag][z]=0;
        }
        //i个骰子，可能的点数为i到i*f
        for(let j=i;j<=i*f;j++){
        //若之前点数存在值，则先置为0
            result[1-flag][j]=0;
            下次循环的点数j = 上次循环的点数j-1,j-2,…j-k之和
            for(let k=1;j>=k&&k<=f;k++){
                result[1-flag][j] += result[flag][j-k];
            }
        }
        //两个数组进行交替
        flag=1-flag;
    }
    //返回点数为target的投掷方法
    return result[flag][target]%(Math.pow(10, 9)+7);
};
```



