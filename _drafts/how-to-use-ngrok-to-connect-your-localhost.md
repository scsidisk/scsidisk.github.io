怎麼將內網的 localhost 讓外面的人都看得到呢？用用 ngrok 吧！
============================================================

為什麼用到 ngrok？
------------------

在自己電腦上開發的人通常都遇到網址只有內網才能看到，例如 `localhost:80` ， `dummy.dev:3000`啊，這樣的方式都是用內網。

雖然我們有 browsersync 的解決方式，但這個也只限於內網（又或者我設定不當只能連內網…），也就是說，若你要給其他人看的話只要網路不是同一個，就看不到囉。這聽起來很麻煩呢！以前的作法通常還要在自己電腦上測試好後還要發佈到測試站上，中間多了一個工，還滿累人的。

小編的同事李鑫前幾天提供了 ngrok 這個好物，這東西實在是太厲害了，之前都不知道有這種東西，例如小編的電腦上有 localhost:3001 ，在 terminal 輸入了 ngrok http 3001 後就自動丟出一個網址給你，讓外網的人都可以連上。真是太神了！有了這個東西之後就可以減少有些專案不用開測試站給人家看了。

### 試試看

首先使用 brew cask install ngrok  來安裝，千萬不要用 brew install ngrok 因為是上面的 ngrok 是舊版，要用 caskroom 來裝才是。

之後到假設說你有個網址是 `http://localhost:3002` ，你在 terminal 裡面輸入 `ngrok http 3002` ，就會出現一個畫面，之後會提供你網址，這個網址是你要給外面的人看的，這樣他們就連進來囉！

![ngrok-screen](https://tenten.co/blog/wp-content/uploads/2016/03/Screenshot-2016-03-21-21.53.13.png)

### 提供帳號密碼

還有更酷的！你可以以這個方式寫： `ngrok http -auth="username:password" 3002`  ，就可以讓你的頁面可以以輸入帳號密碼的方式給人家看囉。

![ngrok-password-restrict](https://tenten.co/blog/wp-content/uploads/2016/03/Screenshot-2016-03-21-21.58.45.png)

### WordPress

若在自己 local 上開發的 wordpress 給外面的人看，也不會有連結上的問題，你只要以 ngrok http -host-header=wordpress.dev 80 寫，配合 plugin 使用 [relative-url](https://wordpress.org/plugins/relative-url/)，就可以了！

廢話不多說，好用的東西試一試就知道了，(σ´∀\`)σ

更多東西就要看你[發掘](https://ngrok.com/docs)囉～

### 建議

-   去申請帳號要個 API 連結更方便，還有後台可以看
-   付費可以申請個 subdomain 讓你的網址更固定



