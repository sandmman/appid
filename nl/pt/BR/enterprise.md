---

copyright:
  years: 2017, 2018
lastupdated: "2018-4-24"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:tip: .tip}

# Configurando provedores de identidade corporativa
{: #enterprise}

Se você já tem um repositório do usuário existente e um jeito certificado de autenticar usuários para seus sistemas internos, é possível configurar o serviço {{site.data.keyword.appid_full}} para usar um provedor de identidade corporativa.
{: shortdesc}

## Entendendo o SAML
{: #saml}

O SAML é um padrão aberto para troca de dados de autenticação e autorização entre um provedor de identidade que declara a identidade do usuário e um provedor de serviços que consome as informações de identificação do usuário.
{: shortdesc}

Como isto funciona?

O {{site.data.keyword.appid_short_notm}} funciona como um provedor de serviços e inicia um login de conexão única (SSO) para um provedor de terceiros como o Active Directory Federation Services. O protocolo <a href="http://saml.xml.org/saml-specifications" target="_blank">SAML <img src="../../icons/launch-glyph.svg" alt="Ícone de link externo"></a> suporta diferentes perfis e opções de ligação. O {{site.data.keyword.appid_short_notm}} suporta o perfil SSO do navegador da web com a ligação de Post HTTP.

### Asserções SAML e solicitações de token de identidade

Uma asserção SAML é um pacote de informações que contêm uma ou mais instruções. A asserção contém a decisão de autorização e pode conter informações de identificação sobre o usuário.

Quando um usuário se conecta com um provedor de identidade, esse provedor envia uma asserção para o {{site.data.keyword.appid_short_notm}}. O {{site.data.keyword.appid_short_notm}} propaga as informações de identificação do usuário que são retornadas na asserção SAML para seu app como solicitações de token OIDC. O atributo SAML deve corresponder a uma das solicitações OIDC a seguir para ser incluído no token de identidade.

As solicitações a seguir podem ser incluídas:
* `nome`
* `E-mail
`
* `locale`
* `picture`

Os elementos de atributo SAML restantes que não correspondem a nenhum dos nomes padrão são ignorados.

## Configurando seu app para trabalhar com um provedor de identidade SAML externo
{: #configuring-saml}

É possível configurar o serviço {{site.data.keyword.appid_short_notm}} para usar um provedor de identidade Security Assertion Markup Language (SAML).
{: shortdesc}

### Fornecendo metadados para seu provedor de identidade

Para configurar seu app, você precisa fornecer informações para um provedor de identidade compatível com SAML. As informações são trocadas por meio de um arquivo XML de metadados que também contém dados de configuração que são usados para estabelecer confiança.

1. Na guia **Gerenciar** do painel {{site.data.keyword.appid_short_notm}}, configure **SAML 2.0** para **Ligado**. Em seguida, clique em **Editar** para definir suas configurações de SAML.
2. Clique em **Fazer download do arquivo de metadados do SAML**. Seu provedor de identidade espera as informações a seguir do arquivo.
  <table>
    <tr>
      <th> Variável </th>
      <th> Descrição </th>
    </tr>
    <tr>
      <td><code>EntityID</code></td>
      <td>O identificador que permite que o provedor de identidade saiba que o {{site.data.keyword.appid_short_notm}} emitiu a solicitação SAML.</td>
    </tr>
    <tr>
      <td><code>Location URL</code></td>
      <td>O local para o qual o provedor de identidade envia as asserções SAML após autenticar com êxito um usuário.</td>
    </tr>
    <tr>
      <td><code>Binding</code></td>
      <td>As instruções sobre como o provedor de identidade deve enviar a resposta SAML.</td>
    </tr>
    <tr>
      <td><code>NameID Format</code></td>
      <td>A maneira na qual o provedor de identidade sabe qual formato de identificador ele precisa enviar no assunto de uma asserção. A maneira na qual o {{site.data.keyword.appid_short_notm}} identifica usuários.</td>
    </tr>
    <tr>
      <td><code>WantAssertionsSigned</code></td>
      <td>A maneira na qual um provedor de identidade verifica se ele precisa assinar a asserção. O serviço espera que a asserção seja assinada, mas não suporta asserções criptografadas.</td>
    </tr>
  </table>

3. Forneça os dados para seu provedor de identidade. Se seu provedor de identidade suporta upload do arquivo de metadados, é possível fazer isso. Se não, configure as propriedades manualmente. Nem todo provedor de identidade usará as mesmas propriedades, então, se você não usar todas elas, tudo bem.

Os nomes de propriedades podem diferir entre os provedores de identidade.
{: tip}

### Fornecendo metadados para o {{site.data.keyword.appid_short_notm}}

Você precisará obter dados do provedor de identidade e fornecê-los para o {{site.data.keyword.appid_short_notm}}.

1. Navegue para a guia **SAML 2.0** do painel {{site.data.keyword.appid_short_notm}}. Insira os metadados a seguir que você obteve do provedor de identidade na seção **Fornecer metadados do SAML IdP**.
  <table>
    <tr>
      <th> Variável </th>
      <th> Descrição </th>
    </tr>
    <tr>
      <td><code>Sign-in URL</code></td>
      <td>A URL para a qual o usuário é redirecionado para autenticação. Ela é hospedada por seu provedor de identidade SAML.</td>
    </tr>
    <tr>
      <td><code>Entity ID</code></td>
      <td>O nome globalmente exclusivo para um provedor de identidade SAML.</td>
    </tr>
    <tr>
      <td><code>Signing certificate</code></td>
      <td>O certificado emitido por seu provedor de identidade SAML. Ele é usado para assinar e validar asserções SAML. Todos os provedores são diferentes, mas você pode ser capaz de fazer download do certificado de assinatura do seu provedor de identidade. O certificado deve estar no formato <code>.pem</code>.</td>
    </tr>
  </table>

2. Opcional: forneça um **Certificado secundário** que é usado se a validação da assinatura falha no certificado primário. Se a chave de assinatura permanece a mesma, o {{site.data.keyword.appid_short_notm}} não bloqueia a autenticação para certificados expirados.

3. Atualize o **Nome do provedor** e clique em **Salvar**. O nome padrão é SAML.


### Testando sua configuração

É possível testar a configuração entre seu Provedor de Identidade SAML e o {{site.data.keyword.appid_short_notm}}.

1. Certifique-se de que você salvou sua configuração.
2. Navegue para a guia **SAML 2.0** do painel {{site.data.keyword.appid_short_notm}} e clique em **Testar**. Uma nova guia é aberta.
3. Efetue login com um usuário que seu provedor de identidade já tenha autenticado.
4. Depois de concluir o formulário, você é redirecionado para outra página.
  * Autenticação bem-sucedida: a conexão entre o {{site.data.keyword.appid_short_notm}} e o Provedor de Identidade está funcionando corretamente. A página exibe [tokens de acesso e de identidade](/docs/services/appid/authorization.html#key-concepts) válidos.
  * Autenticação com falha: a conexão está quebrada. A página exibe os erros e o arquivo XML de resposta SAML.
