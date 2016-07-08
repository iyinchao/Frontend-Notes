[middleware vs route handler](http://qnimate.com/express-js-middleware-tutorial/)

handlebars使用
-----

1. 自定义helpers，在express-handlebars中定义：
https://github.com/ericf/express-handlebars

2. block helpers使用{{#<name> <p1> <p2> ...}} <content> {{/<name>}}来进行访问。传入helper中的参数依次为p1, p2。最后还多传了一个参数opt，该参数有很多作用。  
opt.fn: 等于得到compile之后的content，可以继续传入当前绑定的数据，如opt.fn(this)  
opt.inverse: 得到{{else}}分割的false对应的模版，opt.fn为true的模版。
 
3. block helper中使用hash来从模版中传入非指定数量的数据
```html
<div class="entry">
    {{#list abc id="123" otherdata=abc}}
        ...
    {{/list}}
</div>
```
使用opt.hash访问

4. 当需要在helper里修改数据时，使用createFrame来隔离父helper的上下文。 
```javascript 
if (options.data) {
  var data = Handlebars.createFrame(options.data);
  data.foo = 'bar';
  options.data = data; //or options.fn({data:data})
}
```

5. 