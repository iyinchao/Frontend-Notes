>Professional JavaScript for Web Developers

# Chapter 5

## 5.2 Array
### 创建/检测/转换
- **创建**  

构造函数 `var arr = new Array(1,2,3);` 或者 `new Array(n)`（建立n个undefined元素）  

字面量 `var arr = [1,2,3];` 或者`[,,,3]`(前三个为undefined)  
<w>字面量方式中，末尾的冗余逗号将导致长度不确定性</w>  
<w>字面量方式不会调用Array构造函数</w>

- **检测**

采用`arr instanceof Array`进行检测  
<w>当存在多个iframe，有多个全局环境（有多个Array定义），相互传递数据时会导致问题</w>  

采用 `Array.isArray(arr)`(HTML5)  

- **转换为字符串**

`toString()` / `toLocalString()` / `valueof()`将分别调用每个元素各自的方法，用逗号分隔。  
`join("<divider>")`类似`toString()`但能传入参数作为自定义分隔符。

### length属性
设置该值可以伸缩数组，伸展数组后新加的元素为undefined  

<q>缩小数组是否会导致内存的问题？</q>

对数组添加*整数*或*可转为整数的字符串*为名字的属性，可能会导致length增长  
对数组添加其他属性（NaN的属性），该属性在toString时隐藏，但可访问，不影响length

>TODO


## 5.4 RegExp  
字面量定义方式，以正斜杠开头:  
`var exp = /pattern/flags;`

>TODO

## 5.5 Function
### 本质
Function实质上是对象（实现了[[call]]方法的），函数名为对象的指针。  

不存在原生重载功能，因为给同一个函数名（为指针）定义新的函数，只能导致其指向新定义的那个函数，老的定义被覆盖。  

### 函数声明/表达式
函数声明会被hoisting，而表达式不会。

表达式：
```JavaScript
var expFun = function () { /**/};
!function(){}();
```

### Function API
- **Function类构造函数**  

```JavaScript
var sum = new Function("v1", "v2", "return v1 + v2");
```
最后一个参数为函数体（内部会eval），前面其他参数为sum的参数列表。  
<dont>不要使用，会导致两次代码解析，性能差</dont>  

> TODO: 构造函数干的三件事（this）

- **arguments 和 this**

都可在函数内部直接访问  
**this:** 指向函数的调用者（函数所绑定的对象，全局定义的为Global对象），或者对构造函数来说为将要新建的对象。  

**arguments:** 该函数的参数，伪数组。  
`arguments.callee`可访问arguments所在的函数。  
<do>可用于解耦，抽象地在函数内部引用函数本身。（因为函数名可以任意赋值）</do>  <w class="use-strict">严格模式下不可用</w>  

- **length 和 caller**
