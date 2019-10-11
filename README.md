# Cross-Domain-Cookie

一個簡單解決前端發送跨網域 cookie 請求時，cookie 被 iPhone 瀏覽器(包含：Safari、Chrome、Firefox)阻擋的問題。

採用前、後端分離開發部屬後，前端和後端的網址可能是不一樣的，當前端向後端 API 發出請求的時候就會遇到 [CORS](https://developer.mozilla.org/zh-TW/docs/Web/HTTP/CORS) 的問題，本專案將模擬一個加入購物車的情境：

- 前端：以 axios 發送跨網域攜帶 cookie 請求
- 後端：使用 session 記錄使用者的購物車 id、商品 id 和商品數量

如果你發出的請求不需要攜帶 cookie，npm 上的 [cors](https://www.npmjs.com/package/cors) 套件可以快速幫你啟用一個 cors 的 Express middleware，簡單解決跨網域 CORS 問題。

如果你發出的請求需要攜帶 cookie，
後端 cors 設定
前端 axios 設定

這時候你會發現 iPhone 的瀏覽器會阻擋第三方 cookie，包含：Safari、Chrome 及 Firefox 手機 App 都會阻擋，一個簡單的解決方式是先在前端判斷是否為手機瀏覽，如果是，將透過轉址方式將前端網址轉換成後端網址，讓瀏覽器認可後端網址存取 Cookie 後，再將後端網址轉回前端網址

https://lianginger.github.io/cross-domain-cookie/

https://stackoverflow.com/questions/25275595/jquery-withcredentials-not-working-in-safari

https://stackoverflow.com/questions/11381673/detecting-a-mobile-browser

https://blog.johnwu.cc/article/cookie-session-not-working-in-iframe.html
