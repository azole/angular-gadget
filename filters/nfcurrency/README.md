nfcurrency
===============

在 angular 中，使用 currency 會出現兩位小數點，如下：
```
{{amount | currency}}  // $1,234.00
```

如果有引入 i18n，例如：http://code.angularjs.org/1.2.3/i18n/angular-locale_zh-tw.js
則會出現：
```
{{amount | currency}}  // NT$1,234.00
```

在台幣很少有需要出現後兩位小數點的時候，於是根據網路上找到的資料做了 nfcurrency，效果如下：
```
{{amount | nfcurrency}}  // $1,234
```

參考資料：

1. <a href="http://docs.angularjs.org/api/ng.filter:currency" target="_blank">AngularJS: currency</a>

2. <a href="http://stackoverflow.com/questions/14782439/removing-angularjs-currency-filter-decimal-cents" target="_blank">stackoverflow</a>