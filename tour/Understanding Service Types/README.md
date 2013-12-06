Understanding Service Types
===========================

*這篇文章來自 <a href="http://angular-tips.com/blog/2013/08/understanding-service-types/" target="_blank">Understanding Service Types</a>，經原作者同意後翻譯。本人英文也不是非常好，儘量維持原意，有時候會加一些自己的理解，如果有錯誤，歡迎指正。*

---

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
範例：
```javascript
app.value('fooConfig', {    
  config1: true,    
  config2: "Default config2 but it can changes"    
});
```
value 非常像 constant，只是他的值是可以被修改的。通常也用來當做 directives 的設定值。value 也很像 factory，只是 value 只能放值，不能計算。

範例程式：<a href="http://jsbin.com/ayohuz/5/edit">Value</a>

### Factory
範例：
```javascript
app.factory('foo', function() {
  var thisIsPrivate = "Private";
  function getPrivate() {
    return thisIsPrivate;
  }

  return {
    variable: "This is public",
    getPrivate: getPrivate
  };
});

// or..

app.factory('bar', function(a) {
  return a * 2;
});
```
factory 是最常用的 service，也是最容易理解的。

factory 會回傳任何型別。如果是回傳物件，作者建議採用 <a href="http://addyosmani.com/resources/essentialjsdesignpatterns/book/#revealingmodulepatternjavascript" target="_blank">Revealing module pattern</a>。當然，你可以用任何你想用的方式使用它。

如同之前所言，所有類型的 service 都是 singleton。所以當我們修改 foo.variable 時，所有其他地方也都會被修改。

範例程式：<a href="http://jsbin.com/ayohuz/7/edit" target="_blank">Factory</a>

### Service

```javascript
app.service('foo', function() {
  var thisIsPrivate = "Private";
  this.variable = "This is public";
  this.getPrivate = function() {
    return thisIsPrivate;
  };
});
```
*譯者：Constant, Value, Factory, Service, Provider 都是 Service 的一種，這邊的兩個 Service 不是同一個。很討厭吧...*

service 跟 factory 很像，差別在於 service 會接收一個建構子，所以當我們在第一次使用他的時候，他會做一個 new Foo(); 的動作去實例化這個物件。記住，所有類型的 service 都是 singleton，所以不管在其他任何地方又用到這個 service，都會回傳同一個物件。

事實上，跟下面這段範例很像：
```javascript
app.factory('foo2', function() {
  return new Foobar();
});


function Foobar() {
  var thisIsPrivate = "Private";
  this.variable = "This is public";
  this.getPrivate = function() {
    return thisIsPrivate;
  };
}
```
Foobar 是一個 *class*，當我們第一次使用的時候，會在我們的 factory 中實例化它，然後回傳。如同 service，這個 Foobar 類別只會被實例化一次，當下次還要用到這個 factory 時，它將會回傳同一個物件。

如果已經有一個類別，而我們想在我們的 service 中使用時，可以這樣寫：
```javascript
app.service('foo3', Foobar);
```
範例程式：<a href="http://jsbin.com/ayohuz/8/edit" target="_blank">Service</a>

### Provider
### Decorator
