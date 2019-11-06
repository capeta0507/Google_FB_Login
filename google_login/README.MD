# JavaScript Google Login
###### tags: `JavaScript` `Google OAuth`

現在網站提供給客戶的註冊、登入必須提供 Google 第三方登入，目前這方面的主流是 Oauth 2.0 授權、認證機制。

### OAuth
開放授權說明：
1. [百科](https://zh.wikipedia.org/wiki/%E5%BC%80%E6%94%BE%E6%8E%88%E6%9D%83)
2. [OAuth 2.0：簡介](https://lepture.com/zh/2018/oauth2-intro)

### Google Sign-in for Websites
Google提供一個系統登入的方法，讓前端工程師整合到網頁去。
[Sign-In for Websites](https://developers.google.com/identity/sign-in/web/sign-in)

步驟如下：
參考Youtube教學：[Login With Google Account Using Javascript](https://www.youtube.com/watch?v=qzq0kEUa5lU&t=3s)

1. 必須到 [Google Developers Console](https://console.developers.google.com) 去建立一個專案，並取得 OAuth client ID
2. 基本HTML撰寫登入
```htmlmixed=
<html lang="en">
  <head>
    <meta name="google-signin-scope" content="profile email">
    <meta name="google-signin-client_id" content="這裡填寫 YOUR_CLIENT_ID">
    <script src="https://apis.google.com/js/platform.js" async defer></script>
  </head>
  <body>
    <div class="g-signin2" data-onsuccess="onSignIn" data-theme="dark"></div>
    <script>
      function onSignIn(googleUser) {
        // Useful data for your client-side scripts:
        var profile = googleUser.getBasicProfile();
        console.log("ID: " + profile.getId()); // Don't send this directly to your server!
        console.log('Full Name: ' + profile.getName());
        console.log('Given Name: ' + profile.getGivenName());
        console.log('Family Name: ' + profile.getFamilyName());
        console.log("Image URL: " + profile.getImageUrl());
        console.log("Email: " + profile.getEmail());

        // The ID token you need to pass to your backend:
        var id_token = googleUser.getAuthResponse().id_token;
        console.log("ID Token: " + id_token);
      }
    </script>
  </body>
</html>
```
3. 撰寫 signOut
```htmlmixed=
<a href="#" onclick="signOut();">Sign out</a>
<script>
  function signOut() {
    var auth2 = gapi.auth2.getAuthInstance();
    auth2.signOut().then(function () {
      console.log('User signed out.');
    });
  }
</script>
```
4. 注意，必要資訊寫回後台時 ID 是敏感資料，盡量寫回 id_token。
5. 透過 GET https://oauth2.googleapis.com/tokeninfo?id_token={id_token} 可以取的Google Account 資訊(JWT)是比較恰當的方法。
```json=
{
  "iss": "accounts.google.com",
  "azp": "652169929275-4rpkherjmcm0fjr47gnauel33l6g0u59.apps.googleusercontent.com",
  "aud": "652169929275-4rpkherjmcm0fjr47gnauel33l6g0u59.apps.googleusercontent.com",
  "sub": "100929971826844105998",
  "email": "davidtpe99@gmail.com",
  "email_verified": "true",
  "at_hash": "cPwoIALRUKYlzL2TnDeK9g",
  "name": "詹森安",
  "picture": "https://lh5.googleusercontent.com/-qBRI--bdjOY/AAAAAAAAAAI/AAAAAAAAAAA/ACHi3rc3rMP-PIxxxOMtjInoxZRiob_99g/s96-c/photo.jpg",
  "given_name": "森安",
  "family_name": "詹",
  "locale": "zh-TW",
  "iat": "1573008035",
  "exp": "1573011635",
  "jti": "eaf86dfaa8f533d15af65028b8ae9eed3626999e",
  "alg": "RS256",
  "kid": "a06824b79e3982394d5ce7ac75bf92cba30a2e25",
  "typ": "JWT"
}
```
7. 一些自訂主題說明(例如自訂按鈕) https://developers.google.com/identity/sign-in/web/sign-in


davidtpe99@gmail.com 針對本傳案的測試 Client ID 資訊
client ID : 652169929275-4rpkherjmcm0fjr47gnauel33l6g0u59.apps.googleusercontent.com
client secret : cAWlD6m_BU_fZzoBnfhyuZcZ

