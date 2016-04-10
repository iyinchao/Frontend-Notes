> Tricky Problems of JavaScript

## 1.
```javascript
var arr = [];
arr[0]  = 'a';
arr[1]  = 'b';
arr.foo = 'c';
alert(arr.length);
```
answer: 2  

当arr为数组，length只控制arr的数组部分,其他非数字名称的属性不管。但是，当设置`arr.length = 0`时，则**所有属性**全部消失。

## 2.
```javascript
function foo(a, b) {
    arguments[1] = 2;
    alert(b);
}
foo(1);
```
answer: alert结果为undefined  

通过名称来访问参数时，还是根据调用的情况来获得。
> *是否可以认为是调用时将有名的参数逐个复制给了arguments数组？*