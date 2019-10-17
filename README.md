# Cross-Domain-Cookie

一個簡單解決跨網域 cookie 請求時，電腦版 Safari 瀏覽器和 iPhone 上的 Safari、Chrome、Firefox 瀏覽器預設阻擋第三方 cookie 的問題。  
[Demo site](https://lianginger.github.io/cross-domain-cookie/)

## 專案背景

採用前、後端分離開發部屬後，前端和後端的網址可能是不一樣的，當前端向後端 API 發出請求的時候就會遇到 [CORS](https://developer.mozilla.org/zh-TW/docs/Web/HTTP/CORS) 的問題，本專案將模擬一個加入購物車的情境：

- 前端：使用 axios 發送跨網域攜帶 cookie 的請求。( 本專案 )
- 後端：Node.js + Express，使用 session 記錄使用者的購物車 id、商品 id 和商品數量( 非常粗糙的簡化，請勿使用在 Production )。( [後端專案 repository](https://github.com/Lianginger/post-cart-server-simulation) )

## 解決方法

如果發出的跨網域請求 **不需要攜帶 cookie**，[cors](https://www.npmjs.com/package/cors) 套件可以快速啟用一個 cors 的 Express middleware，簡單解決跨網域 CORS 問題。

如果發出的跨網域請求 **需要攜帶 cookie**，要額外設定：

- [前端 axios 要設定 withCredentials 屬性為 true](https://github.com/Lianginger/cross-domain-cookie/blob/ac81eff21d8c3615872102298a82e89e21a69997/index.html#L98)
- [後端 cors 要設定 credentials 屬性為 true](https://github.com/Lianginger/post-cart-server-simulation/blob/ec933c3d8b156a217d466c24ed6b893879e31ebf/app.js#L12)

除了 Safari，電腦版的其它瀏覽器已經可以正常加入購物車，但電腦版 Safari 及 iPhone 上的 Safari、Chrome 及 Firefox 手機 App 瀏覽器預設都會阻擋第三方 Cookie，這使得後端無法透過 session 辨識前端使用者是同一個人，每次的加入購物車都會創建一個新的 cartId，購物車內的商品不會累加。

一個簡單的解決方法是讓用戶去後端網址過水一下，將前端網址[重新導向後端網址](https://github.com/Lianginger/cross-domain-cookie/blob/d7148cf3858922675f6789233770f456280a9724/index.html#L82-L83)，讓瀏覽器認可後端網址存取 Cookie ( 此時後端的 Cookie 已經不是第三方 Cookie，而是第一方 Cookie，因此可以順利被瀏覽器存取 )，接著，[後端網址再重新導向前端網址](https://github.com/Lianginger/post-cart-server-simulation/blob/ec933c3d8b156a217d466c24ed6b893879e31ebf/app.js#L28-L29)，之後 axios 就可以順利發送跨網域攜帶 cookie 的請求，購物車內的商品會不斷累加。

## 參考資料

- [jQuery withCredentials not working in Safari?](https://stackoverflow.com/questions/25275595/jquery-withcredentials-not-working-in-safari)
- [IFrame 無法使用 Cookie & Session 問題](https://blog.johnwu.cc/article/cookie-session-not-working-in-iframe.html)
