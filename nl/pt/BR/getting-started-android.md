---

copyright:
  years: 2017
lastupdated: "2017-11-02"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen:.screen}
{:codeblock: .codeblock}

# Configurando o SDK do Android
{: #android-sdk}

Construa os seus apps Android com o SDK do cliente do {{site.data.keyword.appid_short}}, inicialize o SDK, autentique usuários e faça solicitações
para recursos protegidos e desprotegidos.
{:shortdesc}


## Antes de Começar
{: #before-you-begin}

As seguintes informações são necessárias:
  * Uma instância de um serviço do {{site.data.keyword.appid_short_notm}}.
  * O seu ID do locatário.
    * Na guia **Credenciais de serviço** de seu painel de serviço, clique em **Visualizar credenciais**. Seu ID do locatário é exibido no campo **tenantID**. Esse valor é usado para inicializar o seu app.
  * A sua região do {{site.data.keyword.Bluemix}}. É possível localizar a sua região procurando na UI. O valor é usado para inicializar o seu app.
    <table> <caption> Tabela 1. Regiões e valores do SDK correspondentes do {{site.data.keyword.Bluemix_notm}} </caption>
    <tr>
      <th> Região do Bluemix </th>
      <th> Valor do SDK </th>
    </tr>
    <tr>
      <td> Sul dos Estados Unidos </td>
      <td> AppID.REGION_US_SOUTH </td>
    </tr>
    <tr>
      <td> Sydney </td>
      <td> AppID.REGION_SYDNEY </td>
    </tr>
    <tr>
      <td> United Kingdom </td>
      <td> AppID.REGION_UK </td>
    </tr>
  </table>

  * Um <a href="https://developers.google.com/web/tools/setup/" target="_blank">Projeto do Android
Studio<img src="../../icons/launch-glyph.svg" alt="ícone de Link externo"></a>, configurado para trabalhar com o Gradle.

## Instalando o SDK do cliente
{: #install-appid-sdk}

1. Crie um projeto Android Studio ou abra um projeto existente.
2.  Inclua o repositório do JitPack em seu arquivo raiz `build.gradle`.

  ```gradle
    allprojects {
	    repositories {
		    ...
		    maven { url 'https://jitpack.io' }
	    }
    }
  ```
  {: codeblock}

3. Abra o arquivo `build.gradle` para o seu aplicativo.

    **Nota**: certifique-se de abrir o arquivo para o seu app, não o arquivo `build.gradle` do projeto.
4. Localize a seção de dependências do arquivo e inclua uma dependência de compilação para o SDK do cliente do {{site.data.keyword.appid_short_notm}}.

  ```gradle
   dependencies {
       compile group: 'com.github.ibm-cloud-security:appid-clientsdk-android:1.+'
   }
  ```
  {: codeblock}

5. Localize a seção defaultConfig e inclua as linhas de código a seguir.

  ```gradle
  defaultConfig {
  ...
  manifestPlaceholders = ['appIdRedirectScheme': android.defaultConfig.applicationId]
  }
  ```
  {: codeblock}

6. Sincronizar seu projeto com o Gradle. Clique em **Ferramentas** > **Android** > **Projeto de sincronização
com Arquivos do Gradle**.

## Inicializando o client SDK
{: #initialize-client-sdk}

Inicialize o SDK do cliente passando os parâmetros de contexto, ID do locatário e região para o método de inicialização. Um local comum, mas não obrigatório, para colocar o código de inicialização está no método onCreate da atividade principal em seu aplicativo Android.

  ```java
  AppID.getInstance().initialize(getApplicationContext(), <tenantId>, AppID.REGION_UK);
  ```
  {: codeblock}

1. Substitua *tenantId* pelo tenantId do serviço {{site.data.keyword.appid_short_notm}}.
2. Substitua o AppID.REGION_UK pela sua região do {{site.data.keyword.Bluemix_notm}}.


## Autentique os usuários usando o widget de login
{: #authenticate-login-widget}

A configuração padrão do widget de login usa o Facebook e o Google como opções de autenticação. Se você configurar apenas um deles, o widget de login não será iniciado e o usuário será redirecionado para a tela de autenticação do IDP configurado.



Após o SDK do cliente do {{site.data.keyword.appid_short_notm}} ser inicializado, será possível autenticar os seus usuários executando o widget de login.

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


## Acessando atributos do usuário
{: #accessing}

Ao obter um token de acesso, é possível obter acesso ao terminal protegido de atributos do usuário. É possível obter acesso usando os métodos de API a seguir.

  ```java
  void setAttribute(@NonNull String name, @NonNull String value, UserAttributeResponseListener listener);
  void setAttribute(@NonNull String name, @NonNull String value, @NonNull AccessToken accessToken, UserAttributeResponseListener listener);

  void getAttribute(@NonNull String name, UserAttributeResponseListener listener);
  void getAttribute(@NonNull String name, @NonNull AccessToken accessToken, UserAttributeResponseListener listener);

  void deleteAttribute(@NonNull String name, UserAttributeResponseListener listener);
  void deleteAttribute(@NonNull String name, @NonNull AccessToken accessToken, UserAttributeResponseListener listener);

  void getAllAttributes(@NonNull UserAttributeResponseListener listener);
  void getAllAttributes(@NonNull AccessToken accessToken, @NonNull UserAttributeResponseListener listener);
  ```
  {: codeblock}

Quando um token de acesso não for transmitido explicitamente, o {{site.data.keyword.appid_short_notm}} usará o último token recebido.

Por exemplo, é possível chamar o código a seguir para configurar um novo atributo ou substituir um existente.

  ```java
  appId.getUserAttributeManager().setAttribute(name, value, useThisToken,new UserAttributeResponseListener() {
		@Override
		public void onSuccess(JSONObject attributes) {
			//attributes received in JSON format on successful response
		}

		@Override
		public void onFailure(UserAttributesException e) {
			//Exception occurred
		}
	});
  ```
  {: codeblock}

### Login anônimo
{: #anonymous notoc}

Com o {{site.data.keyword.appid_short_notm}} é possível efetuar login [anonimamente](/docs/services/appid/user-profile.html#anonymous).

  ```java
  appId.loginAnonymously(getApplicationContext(), new AuthorizationListener() {
		@Override
		public void onAuthorizationFailure(AuthorizationException exception) {
			//Exception occurred
		}

		@Override
		public void onAuthorizationCanceled() {
			//Authentication canceled by the user
		}

		@Override
		public void onAuthorizationSuccess(AccessToken accessToken, IdentityToken identityToken) {
			//User authenticated
		}
	});
  ```
  {: codeblock}

### Autenticação progressiva
{: #progressive notoc}

Quando o usuário mantém um token de acesso anônimo, ele pode ser identificado passando-o para o método `loginWidget.launch`.

  ```java
  void launch (@NonNull final Activity activity, @NonNull final AuthorizationListener authorizationListener, String accessTokenString);
  ```
  {: codeblock}

Após um login anônimo, a autenticação progressiva ocorrerá mesmo se o widget de login for chamado sem passar um token de acesso porque o serviço usou o último
token recebido. Se desejar limpar os tokens armazenados, execute o comando a seguir.

  ```java
  	appIDAuthorizationManager = new AppIDAuthorizationManager(this.appId);
  appIDAuthorizationManager.clearAuthorizationData();
  ```
  {: codeblock}


## Próximas etapas
{: #next-steps}

O {{site.data.keyword.appid_short_notm}} fornecerá uma configuração padrão quando você inicialmente configurar os seus provedores de identidade. É
possível usar a configuração padrão apenas no modo de desenvolvimento. Antes de publicar o seu aplicativo, [atualize a configuração padrão para as suas próprias credenciais](/docs/services/appid/identity-providers.html).
