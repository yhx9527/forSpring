# 将url中的params转成对象

**描述**

```
比如输入'http://www.inode.club?name=koala&study=js&study=node';
输出
{name: "koala",study: ["js", "node"]}
```



```
  function queryString(url){
        let args = url.split('?')[1]
        let result={};
        if(args){
            result = args.split('&').map(item=>item.split('=')).reduce((acc, cur)=>{
                if(acc[cur[0]]){
                    if(Array.isArray(acc[cur[0]])){
                        acc[cur[0]].push(cur[1])
                    }else{
                        acc[cur[0]]=[acc[cur[0]], cur[1]]
                    }
                }else{
                    acc[cur[0]]=cur[1];
                }
                return acc;
            }, {})
        }
        return result;
    }
```



