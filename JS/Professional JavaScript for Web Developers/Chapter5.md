>Professional JavaScript for Web Developers 

# Chapter 5

## 5.2 Array
>### 创建/检测/转换 
#### 创建
构造函数 `var arr = new Array(1,2,3);` 或者 `new Array(n)`（建立n个undefined元素）  

字面量 `var arr = [1,2,3];` 或者`[,,,3]`(前三个为undefined)  
*<warning>字面量方式中，末尾的冗余逗号将导致长度不确定性</warning>*  
*字面量方式不会调用Array构造函数*  

#### 检测
采用`arr instanceof Array`进行检测  
*<warning>当存在多个iframe，有多个全局环境（有多个Array定义），相互传递数据时会导致问题</warning>*  
采用 `Array.isArray(arr)`(HTML5)  

#### 转换为字符串
`toString()` / `toLocalString()` / `valueof()`将分别调用每个元素各自的方法，用逗号分隔。  
`join("||")`类似`toString()`但能传入参数作为自定义分隔符。

>### length属性
设置该值可以伸缩数组，伸展数组后新加的元素为undefined  
*<question>缩小数组是否会导致内存的问题？</question>*

对数组添加*整数*或*可转为整数的字符串*为名字的属性，可能会导致length增长  
对数组添加其他属性（NaN的属性），该属性在输出时隐藏，但可访问，不影响length

>### //TODO


## 5.4 RegExp  

`var exp = /pattern/flags;`以正斜杠开头。
