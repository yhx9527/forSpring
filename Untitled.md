# test

```
function quickSort(arr, start, end){
	if(start>=end) return;
	let index=partition(arr,start,end);
	quickSort(arr,start,index-1);
	quickSort(arr,index+1,end);
}
function partition(arr,start,end){
	let temp=arr[start];
	while(start<end){
		while(start<end&&arr[end]>temp){end--};
		arr[start]=arr[end];
		while(start<end&&arr[start]<=temp){start++};
		arr[end]=arr[start];
	}
	arr[start]=temp;
	return start;
}
```

```
function mergeSort(arr,start, end){
	if(start>=end) return;
	let middle=Math.floor((start+end)/2);
	mergeSort(arr,start,middle);
	mergeSort(arr,middle+1,end);
	merge(arr,start,middle,end);
}
function merge(arr,start,middle,end){
	let temp=[];
	let i=start,j=middle+1;
	while(i<=middle&&j<=end){
	if(arr[i]>arr[j]){
		temp.push(arr[j++]);
	}else{
		temp.push(arr[i++]);
	}
}
while(i<=middle) temp.push(arr[i++]);
while(j<=middle) temp.push(arr[j++]);
for(i=0,j=start;i<temp.length;i++,j++){
	arr[j]=temp[i];
}
}
```

```
function selectSort(arr){
	for(let i=0;i<arr.length;i++){
		let min=i;
		for(let j=i;j<arr.length;j++){
			if(arr[j]<arr[min]){
				min=j;
			}
		}
		[arr[i],arr[min]]=[arr[min],arr[i]];
	}
}
```

```
function insertSort(arr){
	for(let i=1;i<arr.length;i++){
		let temp=i;
		for(let j=i;j>=0;j--){
			if(arr[temp]<arr[j]){
				[arr[temp],arr[j]]=[arr[j],arr[temp]];
				temp=j;
			}
		}
	}
}
```

```
function heapSort(arr){
	createHeap(arr);
	for(let i=arr.length-1;i>=0;i--){
		[arr[i],arr[0]]=[arr[0],arr[i]];
		downAdjust(arr,0,i);
	}
}
function createHeap(arr){
	let index=Math.floor(arr.length/2);
	for(let i=index;i>=0;i--){
		downAdjust(arr,i, arr.length);
	}
}
function downAdjust(arr, index, len){
	let root=index,child=index*2+1;
	while(child<len){
		if(child+1<len&&arr[child+1]>arr[child]){
			child=child+1;
		}
		if(arr[child]>arr[root]){
			[arr[child],arr[root]]=[arr[root],arr[child]];
			root=child;
			child=root*2+1;
		}else{
			break;
		}
	}
}
```



```
function Node(val){
	this.val=val;
	this.left=null;
	this.right=null;
}
function createTree(inOrder,layerOrder,val, start, end){
	if(start>end) return null;
	let root = new Node(val);
	let pos=start;
	for(let i=start;i<=end;i++){
		if(inOrder[i]===val){
			pos=i;
		}
	}
	let leftRoot,rightRoot;
	if(start<pos){
		leftRoot=layerOrder.pop();
	}
	if(end>pos){
		rightRoot=layerOrder.pop();
	}
	root.left=createTree(inOrder,layerOrder,leftRoot, start, pos-1);
	root.right=createTree(inOrder,layerOrder,rightRoot, pos+1, end);
	return root;
}
```

