Understanding Service Types
===========================

這篇文章來自 <a herf="http://angular-tips.com/blog/2013/08/understanding-service-types/" target="_blank">Understanding Service Types</a>，經原作者同意後翻譯。本人英文也不是非常好，儘量維持原意，有時候會加一些自己的理解，如果有錯誤，歡迎指正。


Angularjs 中有數種不同的service，每一種都有自己的使用情境。

有一件很重要必須要記在心裡的事情是，不管是那一種的 service 都是 singleton。

Note: singleton 是一種設計模式，他限制一個 class 的實例化 (instantiation) 只會實例化到一個物件 (object)，不管我們在什麼地方注入使用這個服務，都是在用同一個實例 (instance)。

(譯者注：實例化的過程就是利用 class 做一個 object 出來，這個 object 又可稱為 instance。)

以下就開始介紹各種不同類型的 service。


