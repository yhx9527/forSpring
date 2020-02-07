# 模拟实现数组的flat方法

要求不传depth时展开全部，传几层展开几层

```
Array.prototype.myflat=function(depth){
	let arr=this;
	return arr.reduce((acc, cur)=>{
		if(Array.isArray(cur)){
			if(typeof depth==='undefined'){
				acc.push(...cur.myflat(depth))
			}else if(depth===0){
				acc.push(cur);
			} else {
				acc.push(...cur.myflat(depth-1));
			}
		}else{
			acc.push(cur)
		}
		return acc;
	}, [])
}
```

