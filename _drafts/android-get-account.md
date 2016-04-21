取得 Android 手機上的帳號資訊
===========================

[ 日期：2010-05-09 | 瀏覽：20,541 | 迴響：8 ]

通常使用 Android 手機的使用者都會把自己的 Google 帳號給設定在手機上，這樣一來才能使用 Google 提供的多種服務。像是 Marke、Gmail 或是 Google Talk 等等。

也有一些軟體開發者會用 Google 帳號來當做註冊的資訊使用，這樣使用者就算換了其它支 Android 手機的話，只要 Google 帳號不變就能繼續享有原先的權利。

Android SDK 2.0 中開始支援提供 AccountManager 來讓開發者能存取 Account 資訊：

    import android.accounts.Account;
    import android.accounts.AccountManager;
    import android.content.Context;

    AccountManager accountManager = AccountManager.get(context);
    Account[] accounts = accountManager.getAccounts();
    for(Account account : accounts){
        Log.i("--Get Account Example--", account.name);
        Log.i("--Get Account Example--", account.type);
    }

Account 中比較有用的資料是 name 及 type 兩個欄位：

如果手機上還有其它的帳號資料的時候，我們就能透過 type 來區分那些是 Google 的帳號資料了。剛剛是使用 getAccounts() 來取得全部的帳號資訊。若筆者只想要取得 Google 帳號的資料時，那是不是得自己去判斷 type 呢？

嘿~AccountManager 當然也有更快速的方式來針對 type 來存取帳號囉。從上一個範例中取得的帳號中可以看到屬於 Google 帳號的 type 會是 com.google，因此我們就能透過 getAccountsByType() 來取得指定 type 的帳號：

    import android.accounts.Account;
    import android.accounts.AccountManager;
    import android.content.Context;

    AccountManager accountManager = AccountManager.get(context);
    // 取得指定 type 的 Account
    Account[] accounts = accountManager.getAccountsByType("com.google");
    for(Account account : accounts){
        Log.i("--Get Account Example--", account.name);
        Log.i("--Get Account Example--", account.type);
    }

在發佈執行之前可別忘了要加上權限才行喔：

檢視原始碼 XML

    <uses-permission android:name="android.permission.GET_ACCOUNTS" />
