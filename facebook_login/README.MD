# JavaScript Facebook Login
###### tags: `JavaScript` `Facebook OAuth`

現在網站提供給客戶的註冊、登入必須提供 Facebook 第三方登入，目前這方面的主流是 Oauth 2.0 授權、認證機制。

### Facebook Sign-in for WWW
Facebook 提供一個系統登入的方法，讓前端工程師整合到網頁去。
1. [搭配 JavaScript SDK 的網頁版「Facebook 登入」](https://developers.facebook.com/docs/facebook-login/web?locale=zh_TW)
2. [關於OAuth 2.0-以Facebook為例](https://medium.com/@justinlee_78563/%E9%97%9C%E6%96%BCoauth-2-0-%E4%BB%A5facebook%E7%82%BA%E4%BE%8B-6f78a4a55f52)
2. [使用 FB API 處理登入登出功能](https://www.wfublog.com/2018/12/fb-api-log-in-out-button-example.html)

步驟如下：
```htmlmixed=
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Facebook Account Login</title>
</head>
<style>

</style>
<body>
  <h1>Welcome to Facebook OAuth 2.0 Login demo</h1>
  
  <fb:login-button scope="public_profile,email" autologoutlink="true" onlogin="checkLoginState();"></fb:login-button>

目前狀態：<span id="FB_STATUS_1"></span>

<script>
window.fbAsyncInit = function() {
  FB.init({
    appId: '1247542705453879', // 填入 FB APP ID
    cookie: true,
    xfbml: true,
    version: 'v3.1'
  });

  FB.getLoginStatus(function(response) {
    statusChangeCallback(response);
  });
};

// 處理各種登入身份
function statusChangeCallback(response) {
  console.log(response);
  var target = document.getElementById("FB_STATUS_1"),
  html = "";

  // 登入 FB 且已加入會員
  if (response.status === 'connected') {
    html = "已登入 FB，並加入 WFU BLOG DEMO 應用程式<br/>";

    FB.api('/me?fields=id,name,email', function(response) {
      console.log(response);
      html += "會員暱稱：" + response.name + "<br/>";
      html += "會員 email：" + response.email;
      target.innerHTML = html;
    });
  }

  // 登入 FB, 未偵測到加入會員
  else if (response.status === "not_authorized") {
    target.innerHTML = "已登入 FB，但未加入 WFU BLOG DEMO 應用程式";
  }

  // 未登入 FB
  else {
    target.innerHTML = "未登入 FB";
  }
}

function checkLoginState() {
  FB.getLoginStatus(function(response) {
    statusChangeCallback(response);
  });
}

// 載入 FB SDK
(function(d, s, id) {
  var js, fjs = d.getElementsByTagName(s)[0];
  if (d.getElementById(id)) return;
  js = d.createElement(s);
  js.id = id;
  js.src = "https://connect.facebook.net/zh_TW/sdk.js";
  fjs.parentNode.insertBefore(js, fjs);
}(document, 'script', 'facebook-jssdk'));
</script>

</html>
```

davidtpe@seed.net.tw 針對本傳案的測試 資訊
1. 應用程式編號 : 1247542705453879
2. 應用程式密鑰 : 14c7a223e3450c7c4deee04f6c98f37d
3. 應用程式網域 : http://localhost:5500

------

### 另一種方法
參考文件：https://medium.com/@justinlee_78563/%E9%97%9C%E6%96%BCoauth-2-0-%E4%BB%A5facebook%E7%82%BA%E4%BE%8B-6f78a4a55f52

```bash
https://www.facebook.com/v2.10/dialog/oauth?client_id=1247542705453879&redirect_uri=http://localhost:5500/fb.html

https://graph.facebook.com/v2.10/oauth/access_token?client_id=1247542705453879&redirect_uri=http://localhost:5500/fb.html&client_secret=14c7a223e3450c7c4deee04f6c98f37d&code=AQAcwvNjk2YU2WGloDlpkNlwBSm__YffxMvknyMSkZ3O7JR4ZnvaRceYCCX0oZWaIfAou92yYHAp49Tn5EsFZ3OV4YWhS87r26T1lgJWN-u9FHJa35ibsMJzuHVe1UT6nqzLvneXP-rWrgZxN7wV86CZ16vvkso_jxcVqru44pJZwRkBi9FnnycF1F3tgO9NWt5Y7Vf7bTfQvnZ5ezRuJq9dUZ-sDCI68eGvCivkKd2wr0x7VL0As05nNOtOOvNsQQF1y0EsdLukL5hGC43eW9c80lf0eVNdLkN84S_NCIjM-LgXEPfMRuazCn9sobVJFlwrGjtyVbyNJz_c1Qzqj6jt#_

```
