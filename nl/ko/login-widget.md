---

copyright:
  years: 2017, 2018
lastupdated: "2018-02-01"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 사인인 환경 관리

로그인 위젯을 사용하면 소스 코드를 추가, 제거 또는 변경하지 않고 언제든 사인인 플로우를 업데이트할 수 있습니다. 샘플 앱에 제공된 코드를 사용하여 몇 분 안에 시작하고 실행할 수 있습니다.
{: shortdesc}

**참고**: Facebook과 같은 소셜 ID 제공자를 구성할 때 [Oauth2 권한 부여 플로우](https://oauthlib.readthedocs.io/en/stable/oauth2/grants/authcode.html)를 사용하여 로그인 위젯을 호출합니다. 

</br>

## 샘플 앱 코드 사용자 정의
{: #login-widget}

선택한 로고와 색상을 표시하도록 사전 구성된 사인인 화면을 사용자 정의할 수 있습니다. 고유하게 브랜드화한 UI를 표시하려면 [클라우드 디렉토리](/docs/services/appid/cloud-directory.html)를 확인하십시오.
{: shortdesc}

![사용자 정의된 사인인 환경 예](/images/customize.gif)

화면을 사용자 정의하려면 다음을 수행하십시오.

1. {{site.data.keyword.appid_short_notm}} 서비스 대시보드를 여십시오.
2. 회사의 브랜드에 맞게 로그인 위젯의 모양을 수정할 수 있는 **로그인 사용자 정의** 섹션을 선택하십시오.
3. 로컬 시스템에서 PNG 또는 JPG 파일을 선택하여 회사의 로고를 업로드하십시오. 권장하는 이미지 크기는 320 x 320픽셀입니다. 최대 파일 크기는 100KB입니다.
4. 색상 선택도구에서 위젯에 대한 헤더 색상을 선택하거나 다른 색상의 16진 코드를 입력하십시오.
5. 미리보기 분할창을 확인하고 사용자 정의 내용에 만족하면 **변경사항 저장**을 클릭하십시오. 확인 메시지가 표시됩니다.
6. 앱을 방문하여 변경사항을 확인하십시오. 앱을 다시 빌드하지 않아도 됩니다. 이미지는 {{site.data.keyword.appid_short}} 데이터베이스에 저장되며 다음 번에 로그인 위젯을 호출할 때 표시됩니다.


</br>

## Android SDK로 로그인 위젯 호출
{: #android}

소셜 ID 제공업체를 사용하면 Android SDK로 사전 구성된 사인인 화면을 호출할 수 있습니다.
{: shortdesc}

1. ID 제공자를 구성하십시오.
2. 코드에 다음 명령을 두십시오.
    ```java
    LoginWidget loginWidget = AppID.getInstance().getLoginWidget();
    loginWidget.launch(this, new AuthorizationListener() {
          @Override
          public void onAuthorizationFailure (AuthorizationException exception) {
            //Exception occurred
          }

          @Override
          public void onAuthorizationCanceled () {
            //Authentication canceled by the user
          }

          @Override
          public void onAuthorizationSuccess (AccessToken accessToken, IdentityToken identityToken) {
            //User authenticated
          }
        });
    ```
    {: codeblock}


</br>

## iOS Swift SDK로 로그인 위젯 호출
{: ios-swift}

소셜 ID 제공업체를 사용하면 iOS Swift SDK로 사전 구성된 사인인 화면을 호출할 수 있습니다.
{: shortdesc}

1. ID 제공자를 구성하십시오.
2. 앱 코드에 다음 명령을 추가하십시오.
  ```swift
  import BluemixAppID
  class delegate : AuthorizationDelegate {
      public func onAuthorizationSuccess(accessToken: AccessToken, identityToken: IdentityToken, response:Response?) {
          //User authenticated
      }

      public func onAuthorizationCanceled() {
          //Authentication canceled by the user
      }

      public func onAuthorizationFailure(error: AuthorizationError) {
          //Exception occurred
      }
  }

  AppID.sharedInstance.loginWidget?.launch(delegate: delegate())
  ```
  {: codeblock}


</br>

## Node.js SDK로 로그인 위젯 호출
{: #nodejs}

소셜 ID 제공업체를 사용하면 Node.js SDK로 사전 구성된 사인인 화면을 호출할 수 있습니다.
{: shortdesc}

1. ID 제공자를 구성하십시오.
2. 코드에 다음 명령을 추가하십시오.
    ```JavaScript

    var express = require('express');
    var passport = require('passport');
    var WebAppStrategy = require('bluemix-appid').WebAppStrategy;
    ```
    {: codeblock}


</br>

## Calling the login widget with the Swift SDK
{: #swift}

소셜 ID 제공자를 사용하면 Swift SDK로 사전 구성된 사인인 화면을 호출할 수 있습니다.
{: shortdesc}

WebAppKituraCredentialsPlugin은 OAuth2 권한 코드 부여 플로우를 기반으로 하고 브라우저를 사용하는 웹 애플리케이션에 사용해야 합니다. 플러그인은 권한 및 인증 플로우를 구현하기 위한 도구를 제공합니다. 또한, 플러그인은 보호된 리소스에 액세스하려는 인증되지 않은 시도를 발견하기 위한 메커니즘을 제공하며 사용자의 브라우저를 인증 페이지로 자동으로 경로 재지정합니다. 인증에 성공하면 사용자는 웹 애플리케이션의 콜백 URL로 이동하며 {{site.data.keyword.appid_short_notm}}에서 액세스 및 ID 토큰을 확보하기 위해 플러그인을 사용합니다. 해당 토큰을 확보한 후에 플러그인은 WebAppKituraCredentialsPlugin.AuthContext 아래의 HTTP 세션에 토큰을 저장합니다.

다음 코드는 `/protected` 엔드포인트를 보호하기 위해 Kitura 애플리케이션에서 WebAppKituraCredentialsPlugin을 사용하는 방법을 보여줍니다.

   ```swift
  import Foundation
  import Kitura
  import KituraSession
  import Credentials
  import SwiftyJSON
  import BluemixAppID

  // The following URLs will be used for AppID OAuth flows
  var LOGIN_URL = "/ibm/bluemix/appid/login"
  var CALLBACK_URL = "/ibm/bluemix/appid/callback"
  var LOGOUT_URL = "/ibm/bluemix/appid/logout"
  var LANDING_PAGE_URL = "/index.html"

  // Setup Kitura to use session middleware
  // Must be configured with proper session storage for production
  // environments. See https://github.com/IBM-Swift/Kitura-Session for
  // additional documentation
  let router = Router()
  let session = Session(secret: "Some secret")
  router.all(middleware: session)

  let options = [
  	"clientId": "{client-id}",
  	"secret": "{secret}",
  	"tenantId": "{tenant-id}",
  	"oauthServerUrl": "{oauth-server-url}",
  	"redirectUri": "{app-url}" + CALLBACK_URL
  ]
  let webappKituraCredentialsPlugin = WebAppKituraCredentialsPlugin(options: options)
  let kituraCredentials = Credentials()
  kituraCredentials.register(plugin: webappKituraCredentialsPlugin)

  // Explicit login endpoint
  router.get(LOGIN_URL,
  		   handler: kituraCredentials.authenticate(credentialsType: webappKituraCredentialsPlugin.name,
  												   successRedirect: LANDING_PAGE_URL,
  												   failureRedirect: LANDING_PAGE_URL
  ))

  // Callback to finish the authorization process. Will retrieve access and identity tokens from AppID
  router.get(CALLBACK_URL,
  		   handler: kituraCredentials.authenticate(credentialsType: webappKituraCredentialsPlugin.name,
  												   successRedirect: LANDING_PAGE_URL,
  												   failureRedirect: LANDING_PAGE_URL
  ))

  // Logout endpoint. Clears authentication information from session
  router.get(LOGOUT_URL, handler:  { (request, response, next) in
  	kituraCredentials.logOut(request: request)
  	webappKituraCredentialsPlugin.logout(request: request)
  	_ = try? response.redirect(LANDING_PAGE_URL)
  })

  // Protected area
  router.get("/protected", handler: kituraCredentials.authenticate(credentialsType: webappKituraCredentialsPlugin.name), { (request, response, next) in
      let appIdAuthContext:JSON? = request.session?[WebAppKituraCredentialsPlugin.AuthContext]
      let identityTokenPayload:JSON? = appIdAuthContext?["identityTokenPayload"]

      guard appIdAuthContext?.dictionary != nil, identityTokenPayload?.dictionary != nil else {
          response.status(.unauthorized)
          return next()
      }

      response.send(json: identityTokenPayload!)
      next()
  })
  var port = 3000
  if let portString = ProcessInfo.processInfo.environment["PORT"]{
      port = Int(portString)!
  }
  print("Starting on \(port)")

  // Add an HTTP server and connect it to the router
  Kitura.addHTTPServer(onPort: port, with: router)

  // Start the Kitura runloop (this call never returns)
  Kitura.run()
  ```
  {: codeblock}
