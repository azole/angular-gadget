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

service 跟 factory 很像，差別在於 service 會接收一個建構子，所以當我們在第一次使用它的時候，它會做一個 new Foo(); 的動作去實例化這個物件。記住，所有類型的 service 都是 singleton，所以不管在其他任何地方又用到這個 service，都會回傳同一個物件。

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

Provider 可以說是 factory 的老大哥，剛剛 factory 的最後一個例子，就很像下面這個範例：

```javascript
app.provider('foo', function() {

  return {

    $get: function() {
      var thisIsPrivate = "Private";
      function getPrivate() {
        return thisIsPrivate;
      }

      return {
        variable: "This is public",
        getPrivate: getPrivate
      };
    }

  };

});
```
Provider 會需要一個 $get 的函式，用來被注入在其他需要用到的地方。所以當我們注入 foo 的時候，其實我們注入的是 $get。

factory 已經很簡單了，為什麼我們還需要 provider 呢？ 這是因為 provider 可以在 config 階段裡頭被設定。見以下範例：
```javascript
app.provider('foo', function() {

  var thisIsPrivate = "Private";

  return {

    setPrivate: function(newVal) {
      thisIsPrivate = newVal;
    },

    $get: function() {
      function getPrivate() {
        return thisIsPrivate;
      }

      return {
        variable: "This is public",
        getPrivate: getPrivate
      };
    }

  };

});

app.config(function(fooProvider) {
  fooProvider.setPrivate('New value from config');
});
```

這裡我們把 thisIsPrivate 搬到 $get外頭，然後新增一個 setPrivate 的函式，好在 config 階段用這個函式來改變 thisIsPrivate 的值。或許你會問，在 factory 中增加一個 setter 不是更容易嗎？這是因為，他們是有不同的目的。

我們想要注入某個物件，但我們又想要提供一個方法可以來設定他。例如，一個包著 resource 的 service，然後想要設定我們想要用的 URL。或是，當我們採用第三方開發的 service 時，像是 restangular，允許我們根據我們的目標去設定它。

這邊要特別注意的是，在 config 階段，我們必須用 *nameProvider* 去取代 *name*，使用時，才只用 *name*。

看到這邊，就能明白，我們其實已經在我們的應用中設定了某些 services 了，像是 $routeProvider 跟 $locationProvider，分別設定了 routes 跟 htmlmode。

範例程式：<a href="http://jsbin.com/ayohuz/9/edit" target="_blank">Provider</a>

### Decorator

*這個是作者提供的 bonus 1。*

如果你決定使用我給你的 foo service，但它少了你想用的 greet 函式，那你需要修改 fatory 嗎？不，你可以裝飾它：
```javascript
app.config(function($provide) {
  $provide.decorator('foo', function($delegate) {
    $delegate.greet = function() {
      return "Hello, I am a new function of 'foo'";
    };

    return $delegate;
  });
});
```
$provide 是 Angular 內部用來建立各種類型 service 的工具。我們可以手動使用它，或是在我們的模組裡頭使用它提供的函式。$provide 有一個函式 decorator，可以用來裝飾我們的 service。這個函式會接收我們想要裝飾的 service 的名字，其後的回呼函式會接收一個叫做 $delegate 的物件，這個物件就是我們原本的 service 的實例 (instance)。

在回呼函式裡我們可以把原本的 service 裝飾成我們想要的樣子。在我們這個例子中，我們增加了一個叫做 greet 的函式在我們的 service 中。然後我們回傳這個被修改過的 service。

現在，當我們使用這個 service 時，它會有 greet 這個函式。如此<a href="http://jsbin.com/ayohuz/10/edit" target="_blank">範例程式</a>中所見。

這種手動裝飾 service 的能力，讓我們在使用第三方提供的 services 時，我們可以以裝飾來取代複製修改。

注意：constant 這種 service 不可以被裝飾。

*譯者：我在看物件導向的材料時，有獨到這兩句話：1. Can be changed with minimal effort. 2. Can be extended without changing existing code. Angular 這個裝飾的能力，非常符合這兩點。*

### 建立新的 instances

