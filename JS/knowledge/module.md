# JavaScript 模块化
---

> 参考文章
> [伯乐在线][1]

>TODO: 名词了解：“module bundlers vs. module loaders”、“Webpack vs. Browserify”和“AMD vs. CommonJS”

## 为什么使用模块
1. 干净的全局命名空间  

>如果变量声明在顶级函数的作用域外，那么这些变量都是全局的（意味着任何地方都能读写它）。因此，造成了常见的“命名空间污染”，从而导致完全无关的代码却共享着全局变量

## 整合模块的方法

### 1. 使用匿名函数闭包

**例如：**
```javascript
var g = [1,2,3]
(function (globalVariable) {

  // Keep this variables private inside this closure scope
  var privateFunction = function() {
    console.log('Shhhh, this is private!');
    console.log("I'm an global value", g);
  }

  // Expose the below methods via the globalVariable interface while
  // hiding the implementation of the method within the
  // function() block
  // 通过 globalVariable 接口暴露下面的方法。当然，这些方法的实现则隐藏在 function() 块内
  globalVariable.each = function(collection, iterator) {
    //
  };

  globalVariable.filter = function(collection, test) {
    //
  };
  //...

 }(globalVariable));
```
类似jQuery的方法，将privateFunction定义在函数闭包中,使得在外部无法被访问到。对于数据同理。  
同时，在闭包内部可以访问到外部的变量。  
上述例子用户可以传入一个全局的变量，在闭包内为该全局变量bind我们想让外部访问的接口，这样这些成员在外部就能够访问了。以此来模拟public成员的效果。

*Notice:* 匿名函数必须被小括号包裹住，这是因为当语句以关键字 function 开头时，它会被认为是一个函数的声明语句（记住，JavaScript 中不能拥有未命名的函数声明语句，否则会报错），详情见 [stackoverflow的这个回答][2]  

> TODO: 匿名函数的一些注意事项

**再例如：**
```javascript
var myGradesCalculate = (function () {

  // Keep this variable private inside this closure scope
  var myGrades = [93, 95, 88, 0, 55, 91];

  var average = function() {
    var total = myGrades.reduce(function(accumulator, item) {
      return accumulator + item;
      }, 0);

    return'Your average grade is ' + total / myGrades.length + '.';
  };

  var failing = function() {
    var failingGrades = myGrades.filter(function(item) {
        return item &lt; 70;
      });

    return 'You failed ' + failingGrades.length + ' times.';
  };

  // Explicitly reveal public pointers to the private functions
  // that we want to reveal publicly

  return {
    average: average,
    failing: failing
  }
})();

myGradesCalculate.failing(); // 'You failed 2 times.'
myGradesCalculate.average(); // 'Your average grade is 70.33333333333333.'
```
这种方法不需要传入一个全局的对象，而是在全局中生成一个新对象，这个对象是函数闭包返回的结果。因此，只需要把要暴露的对象放入返回的对象中，而私有的对象在闭包内声明却不放在返回对象中，即可。  
当然，我们也可以在这两个例子中的闭包函数的调用中添加其他的参数作为闭包内可以使用的dependences

**总结：**
> 函数闭包的方式，使用一个全局变量将其代码封装在一个函数中，从而利用闭包作用域为自身创建一个私有的命名空间。  
> **缺点:**  
1. 仍然导致了全局命名空间的污染（slightly），因为仍然需要有一个全局的handle去绑定我们设为public的函数或变量，有可能模块的名字冲突或者同一个模块不同版本冲突  
2. 难以管理多重的依赖关系（层层相互依赖）

### 2. 使用CommonJS， AMD和CMD

> Ref: [知乎上的一个回答][3]

#### CommonJS
```javascript
//myModule.js
function myModule() {
  this.hello = function() {
    return 'hello!';
  }

  this.goodbye = function() {
    return 'goodbye!';
  }
}

module.exports = myModule;
```
```javascript
//main.js
var myModule = require('myModule');

var myModuleInstance = new myModule();
myModuleInstance.hello(); // 'hello!'
myModuleInstance.goodbye(); // 'goodbye!'
```
外部文件定义模块（的构造函数），通过module.exports=<constructor>导出模块；  
在其他文件中，使用require('<文件名（不带扩展名）>')来引入，返回构造函数。  

**优点**  
能够避免全局命名空间的污染  
让依赖关系更明确

**缺点**  
采用同步方式加载模块，按照顺序依次进行。（适合在服务器中部署，而在浏览器中会导致加载阻塞）

#### AMD (Asynchronous Module Definition)
> RequireJS 在推广过程中对模块定义的规范化产出  
> Ref: [RequireJS doc][4]  

```javascript
//myModule.js
define([], function() {
  return {
    hello: function() {
      console.log('hello');
    },
    goodbye: function() {
      console.log('goodbye');
    }
  };
});
```

```javascript
//other.js
define(['myModule', 'myOtherModule'], function(myModule, myOtherModule) {
  console.log(myModule.hello());
});
```
熟悉Angular依赖注入的应该会会心一笑。  
define第一个参数是依赖列表，AMD采取浏览器优先的方式，通过异步加载的方式完成任务。不会发生阻塞。  
接着回调函数会将加载完成后的依赖作为其参数，一一对应。  

**优点**  
异步不阻塞  
AMD模块可以是一个对象、函数、构造函数、字符串、JSON 或其它各种类型，而 CommonJS 仅支持对象作为模块  

**缺点**  
语法相对复杂  
对服务器支持不如CommonJS( 不兼容 io、文件系统（filesystem）等)  

#### CMD (Common Module Definition)
> SeaJS 在推广过程中对模块定义的规范化产出。  
> Ref:   
> [github issue - cmd 规范(中文)][5]  
> [RequireJS的异同][6]




[1]: http://web.jobbole.com/85267/
[2]: http://stackoverflow.com/questions/1634268/explain-javascripts-encapsulated-anonymous-function-syntax
[3]: https://www.zhihu.com/question/20351507  
[4]: http://requirejs.org/docs/whyamd.html
[5]: https://github.com/seajs/seajs/issues/242 "cmd 规范中文版"
[6]: https://github.com/seajs/seajs/issues/277
