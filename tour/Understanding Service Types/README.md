Understanding Service Types
===========================

*這篇文章來自 <a href="http://angular-tips.com/blog/2013/08/understanding-service-types/" target="_blank">Understanding Service Types</a>，經原作者同意後翻譯。本人英文也不是非常好，儘量維持原意，有時候會加一些自己的理解，如果有錯誤，歡迎指正。*

- - -

Angularjs 中有數種不同的service，每一種都有自己的使用情境。

有一件很重要必須要記在心裡的事情是，不管是那一種的 service 都是 singleton。

Note: singleton 是一種設計模式，他限制一個 class 的實例化 (instantiation) 只會實例化到一個物件 (object)，不管我們在什麼地方注入使用這個服務，都是在用同一個實例 (instance)。

*(譯者注：實例化的過程就是利用 class 做一個 object 出來，這個 object 又可稱為 instance。)*

以下就開始介紹各種不同類型的 service。

### Constant
範例：
```javascript
app.constant('fooConfig', {     
  config1: true,     
  config2: "Default config2"     
});     
```
Constant 經常用來作為 directives 之間的預設值設定。所以當你想要建立一個 directive，想要傳入一些選項，同時又想要傳入一些預設設定時，constant 就很好用。
  
當我們把值放在 constant 時，這些值是不能被修改的。constant 可以是 primitive 或是 物件。

範例程式：<a href="http://jsbin.com/ayohuz/2/edit">Constant</a>    

### Value
範例：```javascript
app.value('fooConfig', {    
  config1: true,    
  config2: "Default config2 but it can changes"    
});
```
value 非常像 constant，只是他的值是可以被修改的。通常也用來當做 directives 的設定值。value 也很像 factory，只是 value 只能放值，不能計算。

範例程式：<a href="http://jsbin.com/ayohuz/5/edit">Value</a>
  
### Factory
### Service
### Provider
### Decorator
