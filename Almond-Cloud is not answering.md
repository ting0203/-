# [Almond-Cloud is not answering](https://community.almond.stanford.edu/t/almond-cloud-is-not-answering/367)

`URL: https://community.almond.stanford.edu/t/almond-cloud-is-not-answering/367`

## Question



![Screenshot 2020-12-10 203816](https://community.almond.stanford.edu/uploads/default/optimized/1X/f50dabfcadd36b91556adc704b384f521b0e30ae_2_690x346.png)



Hi, we installed our almond-cloud and set the back and front end up.

We tried the Web Almond only configuration. The Thingpedia and Almond NLP should be included in this. But our almond doesn’t answer to any of our questions. It just doesn’t respond. Although the Miscellaneous Interface and the Weather skill are implemented.

Any idea to fix this?

``` 
嗨，我們安裝了杏仁雲並設置了後端和前端。

我們嘗試了 Web Almond only 配置。Thingpedia 和 Almond NLP 應該包含在其中。但是我們的杏仁不能回答我們的任何問題。它只是沒有反應。雖然實現了雜項界面和天氣技能。

有什麼想法可以解決這個問題嗎？
```

-----

## Answer

Are there any errors in the terminal?

In any case, if you use the master version of almond-cloud, you should have access to the skills in almond-dev.stanford.edu (aka [Almond 1.99 1](https://wiki.almond.stanford.edu/en/release-planning/one-point-ninetynine)), which at the moment include Weather, Yelp, Spotify, and Miscellaneous. Other skills are still WIP. If you want to use the older version of Almond, which talks to the older, larger Thingpedia, you should check out one of the older tags in the almond-cloud repo.

```
終端中是否有任何錯誤？

在任何情況下，如果您使用杏仁雲的主版本，您應該可以訪問almond-dev.stanford.edu（又名Almond 1.99）中的技能 1)，目前包括 Weather、Yelp、Spotify 和 Miscellaneous。其他技能還在WIP中。如果您想使用舊版本的 Almond，它與更舊的、更大的 Thingpedia 對話，您應該查看杏仁雲存儲庫中的舊標籤之一。
```

-----

Thanks for your answer!

There are no errors in the Terminal. We have no idea what the problem is and why almond is not working.
Currently we use the the Almond version 1.99. But we will try the older version 1.8. Maybe this will solve the problem.

Thanks for your help!

```
感謝您的回答！

終端中沒有錯誤。我們不知道問題是什麼以及為什麼杏仁不起作用。
目前我們使用的是 Almond 版本 1.99。但我們會嘗試舊版本 1.8。也許這會解決問題。

謝謝你的幫助！
```

-----

Are there errors in the browser console? If you don’t see a message like “Hello! How can I help you?” the browser is not connected to the backend correctly.

```
瀏覽器控制台中是否有錯誤？如果您沒有看到類似“您好！我怎麼幫你？” 瀏覽器未正確連接到後端。
```

-----

![截圖英文](https://community.almond.stanford.edu/uploads/default/optimized/1X/94d823ec3438b2b2fec4b108744b75aa62b5ae73_2_690x344.png)

Again thanks for your answer.
We have this error in the browser console:
Loading failed for the script with source “http://127.0.0.1:8080/assets/javascripts/conversation-bundle.js”

```
再次感謝您的回答。
我們在瀏覽器控制台中出現此錯誤：
源為“ http://127.0.0.1:8080/assets/javascripts/conversation-bundle.js”的腳本加載失敗1.
```

-----

Yeah that sums up the problem. Does the file conversation-bundle.js exist in public/javascripts in your almond-cloud git clone? It gets created when you run yarn.
Also, maybe look at the network tab as well? It will tell you if the error is a 404, a server error, or some other kind of error.

```
是的，總結了這個問題。您的杏仁雲 git 克隆中的 public/javascripts 中是否存在 session-bundle.js 文件？它是在您運行 yarn 時創建的。
另外，也許還要看看網絡選項卡？它會告訴您錯誤是 404、服務器錯誤還是其他類型的錯誤。
```

------

![圖片2](https://community.almond.stanford.edu/uploads/default/optimized/1X/db4704434bc1c4c1b6faaa93eda42e992506decf_2_690x52.png)

The file conversation-bundle.js was not created in public/javascripts.
And it is a 404 error.
Any idea how to solve this?

```
文件 session-bundle.js 不是在 public/javascripts 中創建的。
這是一個 404 錯誤。
知道如何解決這個問題嗎？
```

-----

Hi gcampax

I’m helping Felix to fix this. You are asking for -bundle files in public/javascripts in our almound-clound git clone. That’s probably the issue: We followed the [Cloud Almond Installation and Configuration Guide 1](https://almond.stanford.edu/doc/installing-almond-cloud.md) on the Stanford pages. This guide does not mention cloning git. Instead it suggests using yarn. So our installation is based on yarn, not on git.

Looking at public/javascripts I see the following files:

```
ubuntu@ubuntu2004:~$ ls -1 ~/.config/yarn/global/node_modules/almond-cloud/public/javascripts2fa_setup.jsackee-min.jsadmin.jsapps.jsdevice.jsdevices-create.jsdevice-selector.jsdocsearch.jsget-involved.jsindex.jsjsoneditor.min.jsjsonlint.jsjstz.min.jsmturk_check.jsprofile.jsqrcode.jsregister.jsshared.jsstatus.jsvalidator.min.js
```

There are no `-bundle` files at all. It seems, as if yarn didn’t create them. Is this an issue in the yarn installation procedure or in the documentation? How can we recreate them?

We would appreciate, if you could shed some light on that. Many thanks in advance
FrankyB

```
嗨 gcampax

我正在幫助菲利克斯解決這個問題。您在我們的 almound-clound git clone 中要求公共/javascripts 中的 -bundle 文件。這可能是問題所在：我們遵循了Cloud Almond 安裝和配置指南 1在斯坦福頁面上。本指南沒有提到克隆 git。相反，它建議使用紗線。所以我們的安裝基於yarn，而不是git。

查看 public/javascripts 我看到以下文件：

ubuntu@ubuntu2004:~$ ls -1 ~/.config/yarn/global/node_modules/almond-cloud/public/javascripts
2fa_setup.js
ackee-min.js
admin.js
apps.js
device.js
devices-create.js
device-selector.js
docsearch.js
get-involved.js
index.js
jsoneditor.min.js
jsonlint.js
jstz.min.js
mturk_check.js
profile.js
qrcode.js
register.js
shared.js
status.js
validator.min.js

根本沒有-bundle文件。看起來，好像紗線沒有創造它們。這是紗線安裝程序或文檔中的問題嗎？我們如何重新創建它們？

如果您能對此有所了解，我們將不勝感激。提前
非常感謝FrankyB
```

------

It might be a bug in yarn if the file was not generated.
Perhaps the most reliable way to install almond-cloud is to clone the almond-cloud git repository, install the dependencies and build the missing files with `yarn`, and then make the `almond-cloud` command available using `yarn link`. This should solve your problem.

```
如果未生成文件，則可能是 yarn 中的錯誤。
也許安裝杏仁雲最可靠的方法是克隆杏仁雲 git 存儲庫，安裝依賴項並使用 構建丟失的文件yarn，然後almond-cloud使用yarn link. 這應該可以解決您的問題。
```

----

Thanks for your help. This solved the problem:)

```
謝謝你的幫助。這解決了問題:)
```



