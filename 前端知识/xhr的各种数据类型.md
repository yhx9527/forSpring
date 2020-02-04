# xhr的各种数据类型

xhr能够处理的数据类型

```
DOMString、Document、FormData、Blob、File、ArrayBuffer
```

- DOMString

xhr返回的responseText即为DOMString，可以理解为字符串

- Document

xhr返回的responseXML

- FormData

```
document.querySelector("#formData").addEventListener("submit", function(event) {
    var myFormData = new FormData(this);
    myFormData.append("token", "ce509193050ab9c2b0c518c9cb7d9556");
    var xhr = new XMLHttpRequest();
    xhr.open(this.method, this.action);
    xhr.onload = function(e) {
        if (xhr.status == 200 && xhr.responseText) {
            // 显示：'欢迎你，' + xhr.responseText;
            this.reset();
        }
    }.bind(this);
    // 发送FormData对象数据
    xhr.send(myFormData);
    // 阻止默认的表单提交
    event.preventDefault();
}, false);
```

- Blob二进制对象

```
var myblob=new Blob([数据])
myblob.size
myblob.type
myblob.slice()
```

```
var xhr = new XMLHttpRequest();    
xhr.open("get", "mm1.jpg", true);
xhr.responseType = "blob";
xhr.onload = function() {
    if (this.status == 200) {
        var blob = this.response;  // this.response也就是请求的返回就是Blob对象
        var img = document.createElement("img");
        img.onload = function(e) {
          window.URL.revokeObjectURL(img.src); // 清除释放
        };
        img.src = window.URL.createObjectURL(blob);
        eleAppend.appendChild(img);    
    }
}
xhr.send();
```

- File

**将图片转成base64**

```
FileReader.readAsDataURL()
canvas元素的toDataURL()
```

- ArrayBuffer

装有二进制数据的对象，提前将数据装入内存中，之后都不变了，需要通过DataView对象来进行解释

```
// 创建一个8字节的ArrayBuffer  
var b = new ArrayBuffer(8);  
  
// 创建一个指向b的视图v1，采用Int32类型，开始于默认的字节索引0，直到缓冲区的末尾  
var v1 = new Int32Array(b);  
  
// 创建一个指向b的视图v2，采用Uint8类型，开始于字节索引2，直到缓冲区的末尾  
var v2 = new Uint8Array(b, 2);  
  
// 创建一个指向b的视图v3，采用Int16类型，开始于字节索引2，长度为2  
var v3 = new Int16Array(b, 2, 2);  
```

```
function sendArrayBuffer() {
  var xhr = new XMLHttpRequest();
  xhr.open('POST', '/server', true);
  xhr.onload = function(e) { ... };

  var uInt8Array = new Uint8Array([1, 2, 3]);

  xhr.send(uInt8Array.buffer);
}
```



