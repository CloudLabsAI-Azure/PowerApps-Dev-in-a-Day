# Laboratório 04 - Azure Function

## Duração estimada: 110 minutos

Trabalhando como parte da equipe de desenvolvimento da PrioritZ, você irá configurar um conector personalizado para uma nova API que você construirá usando o Azure Functions. A equipe decidiu mover a lógica de quando um usuário cria uma nova solicitação para a API do Azure Function. Isso manterá a fórmula do Power App simples e permitirá que lógicas mais complexas sejam adicionadas no futuro. Neste laboratório, você criará a função, usará a API do Dataverse, protegerá a API com o Microsoft Entra ID, configurará um conector personalizado para usar a API e alterará o Power App para usar o conector.

>**Observação:** Este laboratório requer uma assinatura (ou versão de avaliação) do Azure no mesmo tenant do seu ambiente Dataverse.

## Objectivo de Laboratório

- Exercício 1: Criar uma Azure Function
- Exercício 2: Implementação da função
- Exercício 3: Publicar no Azure
- Exercício 4: Criar o conector
- Exercício 5: Usar a Função no Aplicativo de Tela

## Exercício 1 – Criar uma Azure Function

Neste exercício, você instalará a extensão de ferramentas do Azure para o Visual Studio Code e criará a função.

### Tarefa 1: Instalar a extensão das ferramentas do Azure

1. Iniciar o **Visual Studio Code** usando o atalho disponível na área de trabalho.

    ![](images/L04/vscode1.png)

2. Selecione a guia **Extensões**.

    ![](images/L04/vscode2.png)

3. Procure por **Azure tools (1)** e clique em **Instalar (2)** para instalar a extensão.

    ![](images/L04/vscode3.png)

4. Aguarde a conclusão da instalação.

5. Você deverá ver a nova extensão Azure Tools que adicionou.

    ![](images/L04/image%20(2).png)

6. Clique em **Terminal** no menu superior e selecione **Novo Terminal**.

7. Execute o comando abaixo no terminal para criar uma nova pasta.
 
    ```
    md ContosoFunctions
    ```
    ![](images/L04/image%20(4).png)

### Tarefa 2: Crie uma função

1. Selecione **Azure tools (1)** no menu de navegação esquerdo e navegue até à seção **Workspace (2)**.

    ![](images/L04/vscode4.png)

1. Clique no **símbolo + (1)** na secção **Workspace**, clique em **Criar Função (2)** e, em seguida, **Criar Novo Projeto**.

    ![](images/L04/functionu.png)

    >**Observação**: Se o símbolo **+** não estiver visível, verifique as configurações do VS Code para atualizações pendentes. Após atualizar, feche e reabra o aplicativo.

1. Selecione a pasta **ContosoFunctions (1)** que você criou e clique em **Selecionar Pasta (2)**

    ![](images/L04/L04-contoso.png)

1. No pop-up que aparecer, clique em **Sim** para criar um novo projeto.

    ![](images/L04/vscode6.png)

1. Selecione **C#** como linguagem.

    ![](images/L04/vscode7.png)

1. Selecione **.NET 6.0 LTS** como runtime do . NET.

    ![](images/L04/vscode8.png)

1. Selecione **HTTP trigger with OpenAPI** como modelo.

    ![](images/L04/vscode9.png)

1. Insira **CreateTopic** como nome da função e pressione **[ENTER].**

    ![](images/L04/image%20(7).png)

1. Insira **Contoso.PrioritZ** como namespace e pressione **[ENTER]**.

    ![](images/L04/vscode10.png)

1. Selecione **Anonymous** para AccessRights. Mais tarde, protegeremos a função usando o Microsoft Entra ID.

    ![](images/L04/vscode11.png)

9. Se uma janela for apresentada, selecione **Abrir na janela atual.**

    ![](images/L04/image%20(8).png)

1. Sua função deve abrir no **Visual Studio Code**.

1. Se aparecer um pop-up perguntando **Você confia nos autores dos arquivos nesta pasta?**, clique em **Sim, eu confio nos autores**.

1. Clique em **Terminal** no menu superior e selecione **Executar Tarefa de Build**.

    ![](images/L04/image%20(9).png)

1. Quando o build for concluído com sucesso, **pressione qualquer tecla para fechar o terminal.**.

    ![](images/L04/vscode12.png)


## Exercício 2 - Implementação da função

Neste exercício, irá implementar a função.

### Tarefa 1: Função de implementação

1. Clique em **Novo Arquivo** que está próximo de **ContosoFunctions** para adicionar um novo arquivo.

    ![](images/L04/image%20(10).png)

2. Nomeie o novo arquivo como **Model.cs**

    ![](images/L04/vscode13.png)

3. Abra o novo arquivo **Model.cs** e cole o código abaixo. Isso definirá os dados que serão enviados do Power App.

    ```
    using System;
    using Microsoft.Azure.WebJobs.Extensions.OpenApi.Core.Attributes;
    using Microsoft.OpenApi.Models;
    namespace Contoso.PrioritZ
    {
    public class TopicItemModel
    {
    public string Choice { get; set; }
    public string Photo { get; set; }
    }
    public class TopicModel
    {
    [OpenApiProperty(Nullable = false, Description = "This is a topic")]
    public string Topic { get; set; }
    public string Details { get; set; }
    public DateTime RespondBy { get; set; }
    public string MyNotes { get; set; }
    public string Photo { get; set; }
    public TopicItemModel[] Choices {get;set;}
    }
    }
    ```

4. Abra o arquivo **CreateTopic.cs**.

5. Localize os atributos do método **Run** (linha 24-27) e substitua-os pelos atributos abaixo. Isso fornecerá nomes amigáveis quando criarmos um conector para a API.

    ```
    [FunctionName("CreateTopic")]
    [OpenApiOperation(operationId: "CreateTopic", tags: new[] { "name" }, Summary = "Create Topic", Description = "Create Topic", Visibility = OpenApiVisibilityType.Important)]
    [OpenApiSecurity("function_key", SecuritySchemeType.ApiKey, Name = "code", In = OpenApiSecurityLocationType.Query)]
    [OpenApiResponseWithBody(statusCode: HttpStatusCode.OK, contentType: "application/json", bodyType: typeof(Guid), Description = "The Guid response")]
    [OpenApiRequestBody(contentType: "application/json", bodyType: typeof(TopicModel))]
    ```

6. Remova **get** dos métodos HTTP permitidos no método **Run**, deixando apenas **post**.

    ![](images/L04/image%20(14).png)

7. Abra um **Novo Terminal**.

8. Execute o comando abaixo para adicionar o pacote do **Power Platform Dataverse Client**.

    ```
    dotnet add package Microsoft.PowerPlatform.Dataverse.Client
    ```

    ![](images/L04/image%20(17).png)

9. Aguarde a adição do pacote e execute o comando abaixo para adicionar o pacote **Azure Identity**.

    ```
    dotnet add package Azure.Identity
    ```

10. Aguarde a conclusão.

11. Abra o arquivo **CreateTopic** e adicione as seguintes declarações `using` após a linha 11.

    ```
    using System;
    using Microsoft.Identity.Client;
    using Azure.Core;
    using Azure.Identity;
    using Microsoft.PowerPlatform.Dataverse.Client;
    using Microsoft.Azure.WebJobs.Extensions.OpenApi.Core.Enums;
    using Microsoft.Xrm.Sdk;
    ```

    ![](images/L04/vscode15.png)

12. Adicione o método abaixo após o método **Run**. Este método usará o token do usuário que chamou a API para obter um novo token que permitirá à função usar a API do Dataverse em nome desse usuário.

    ```
    public static async Task<string> GetAccessTokenAsync(HttpRequest req,string resourceUri)
    {
    //Get the calling user token from the request to use as UserAssertion
    var headers = req.Headers;
    var token = string.Empty;
    if (headers.TryGetValue("Authorization", out var authHeader))
    {
    if (authHeader[0].StartsWith("Bearer "))
    {
    token = authHeader[0].Substring(7, authHeader[0].Length -
    7);
    }
    }
    string[] scopes = new[] {$"{resourceUri}/.default" };
    string clientSecret = Environment.GetEnvironmentVariable("ClientSecret");
    string clientId = Environment.GetEnvironmentVariable("ClientID");
    string tenantId = Environment.GetEnvironmentVariable("TenantID");
    var app = ConfidentialClientApplicationBuilder.Create(clientId)
    .WithClientSecret(clientSecret)
    .WithAuthority($"https://login.microsoftonline.com/{tenantId}")
    .Build();
    //Get On Behalf Of Token for calling user
    UserAssertion userAssertion = new UserAssertion(token);
    var result = await app.AcquireTokenOnBehalfOf(scopes,
    userAssertion).ExecuteAsync();

    return result.AccessToken;
    }
    ```

    ![](images/L04/vscode16.png)

13. Substitua o código dentro do método **Run** pelo código abaixo. Isso fornecerá uma instância da API do Dataverse e usará a função `GetAccessToken` que acabamos de definir.

    ```    
    _logger.LogInformation("Starting Create Topic");
    var serviceClient = new ServiceClient(
    instanceUrl: new Uri(Environment.GetEnvironmentVariable("DataverseUrl")),
    tokenProviderFunction: async uri => { return await
    GetAccessTokenAsync(req, Environment.GetEnvironmentVariable("DataverseUrl"));
    },
    useUniqueInstance: true,
    logger: _logger);
    if (!serviceClient.IsReady)
    {
    throw new Exception("Authentication Failed!");
    }
    ```

    ![](images/L04/vscode17.png)

14. Adicione o código a seguir após a instrução `if` do método **Run** para desserializar a requisição e obter os dados enviados.

    ```
    string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
    var data = JsonConvert.DeserializeObject<TopicModel>(requestBody);
    ```

    ![](images/L04/vscode18.png)

15. Adicione o código abaixo para criar o registro no Dataverse, logo após o trecho que você adicionou no passo anterior.

    ```
    var ask = new Entity("contoso_prioritztopic");
    ask["contoso_topic"] = data.Topic;
    ask["contoso_details"] = data.Details;
    ask["contoso_mynotes"] = data.MyNotes;
    ask["contoso_respondby"] = data.RespondBy.Date;
    if (data.Photo != null)
    {
    // Remove unnecessary double quotes,
    // Remove everything before the first comma (embedded stuff)
    ask["contoso_photo"] = Convert.FromBase64String(data.Photo.Trim('\"').Split(',')[1]);
    }
    var topicId = await serviceClient.CreateAsync(ask);
    foreach (var choice in data.Choices)
    {
    var item = new Entity("contoso_prioritztopicitem");
    item["contoso_choice"] = choice.Choice;
    item["contoso_prioritztopic"] = new
    EntityReference("contoso_prioritztopic", topicId);
    if (choice.Photo != null)
    {
    item["contoso_photo"] =
    Convert.FromBase64String(choice.Photo.Trim('\"').Split(',')[1]);
    }
    var choiceId = await serviceClient.CreateAsync(item);
    }
    ```

    ![](images/L04/vscode19.png)

16. Adicione o código abaixo ao final do método **Run** para retornar o ID do tópico como JSON.
    
    ```
    return new OkObjectResult(topicId);
    ```

    ![](images/L04/vscode20.png)

18. Clique em **Terminal (1)** e selecione **Executar Tarefa de Build (2)**.

    ![](images/L04/vscode21.png)

19. O build deve ser bem-sucedido. Pressione qualquer tecla para parar.

    > **Observação**: Se o build falhar, verifique se você seguiu as instruções e adicionou o código corretamente nos arquivos **CreateTopic.cs** e **Model.cs**. Você pode comparar seu código com os arquivos de referência localizados em `C:\LabFiles`.
    
## Exercício 3 – Publicar no Azure

Neste exercício, você implantará a função no Azure.

### Tarefa 1: Publicar

1. Selecione **Azure Tools** do menu do lado esquerdo.

    ![](images/L04/vscode22.png)

2. Clique em **Entrar no Azure** na seção **Recursos**.

    ![](images/L04/vscode23.png)

3. Conclua o processo de login com suas credenciais.

    * E-mail/Usuário: <inject key="AzureAdUserEmail"></inject>
    
    * Senha: <inject key="AzureAdUserPassword"></inject>

4. Feche a janela do navegador após o login.

5. De volta ao VS Code, clique no **+** ao lado da sua guia **Recursos** para criar um novo Aplicativo de Função.

    ![](images/L04/NewVSazure3u.png)

6. Procure e selecione **Create Function App in Azure**.

    ![](images/L04/vscode24u.png)

7. Insira **PrioritZFunc<inject key="Deployment ID" activityCopy="false" />** como nome do aplicativo de função e pressione [ENTER].

    ![](images/L04/vscode25.png)

8. Selecione **.NET 6(LTS) In-process**.

    ![](images/L04/vscode26u.png)

9. Selecione a localização **<inject key="Region" activityCopy="false" />** da lista e aguarde a implantação.
    
    ![](images/L04/vscode27.png)

10. Após a implantação, clique em **Deploy to Azure** na seção **Workspaces** e escolha o Aplicativo de Função que você criou.

    ![](images/L04/DeployNewu.png)

    ![](images/L04/DeployNew1u.png)

11. Aguarde a implantação e navegue até o Portal do Azure.

    ```
    https://portal.azure.com/
    ```

12. Selecione **Todos os recursos**, pesquise pela aplicação de função **PrioritZFunc<inject key="Deployment ID" activãoCopy="false" />** que implantou anteriormente e clicar para o abrir.

    ![](images/L04/vscode27.1.png)

13. Selecione **Autenticação (1)** do menu esquerdo e clique em **Adicionar provedor de identidade (2)**.

    ![](images/L04/L04-auth.1.png)

14. Selecione **Microsoft** como provedor de identidade e **Inquilino atual - Inquilino único** como Tipos de conta suportados. Clique em **Adicionar**.

    ![](images/L04/L04-auth.2.png)

15. Abra o menu do **Portal** e selecione Microsoft Entra ID.

    ![](images/dev4.png)

16. Selecione **Registros de aplicativo** na seção **Gerenciar** no menu do lado esquerdo.

    ![](images/L04/vscode30.png)

17. Clique para abrir o registro do aplicativo **PrioritZFunc<inject key="Deployment ID" enableCopy="false" />**.

    ![](images/L04/vscode31.png)

18. Copie o **ID do aplicativo (cliente)** do **PrioritZFunc<inject key="Deployment ID" activityCopy="false" />** e salve-o no Bloco de Notas como **ID da aplicação API PrioritZFL**.

    ![](images/L04/image%20(33).png)

    ![](images/L04/image%20(34).png)

>**Observação:** certifique-se de copiar e colar o valor correto do ID do Aplicativo (client). Copiar o valor incorreto resultará em problemas nas próximas etapas/tarefas.

19. Copie o **ID do diretório (locatário)** e salve-o no Bloco de Notas como **Tenant ID**.

    ![](images/L04/image%20(35).png)

>**Observação:** certifique-se de copiar e colar o valor correto do ID do Diretório (tenant). Copiar o valor incorreto resultará em problemas nas próximas etapas/tarefas.

20. Selecione **Certificados e segredos** em **Gerenciar** no menu do lado esquerdo.

    ![](images/L04/vscode32.png)

21. Clique em **+ Novo segredo do cliente**.

    ![](images/L04/image%20(36).png)

22. Forneça uma descrição como **Segredo da API PrioritZ (1)**, selecione a validade de **3 meses(2)** e clique em **Adicionar(3)**.

    ![](images/L04/image38.png)

23. Copie o **Valor** do segredo e salve-o no Bloco de Notas como **Segredo da API PrioritZFL**.

    >**Observação:** certifique-se de copiar e colar o valor correto do Secret. Copiar o valor incorreto resultará em problemas nas próximas etapas/tarefas.

24. Selecione **Permissões de API** em **Gerenciar** no menu do lado esquerdo.

    ![](images/L04/vscode33.png)

25. Clique em **+  Adicionar uma permissão**.

    ![](images/L04/image%20(39).png)

26. Selecione **Dynamics CRM** na lista. Dynamics CRM é o **Dataverse** – o portal do Azure ainda não havia sido atualizado no momento da escrita destas instruções.

    ![](images/L04/vscode34.png)

27. Marque a caixa de seleção **user_impersonation** e clique em **Adicionar permissão**.

28. Volte para **Início** e abra a aplicação de função **PrioritZFunc<inject key="Deployment ID" activityCopy="false" />**.

    ![](images/L04/vscode27.1.png)

29. Selecione **Variáveis de Ambiente (1)** em **Configurações** no menu do lado esquerdo.

30. Clique em **+ Adicionar (2)**.

    ![](images/L04/vscode36u.png)

31. Insira os seguintes dados no painel **Adicionar/Editar configuração do aplicativo** e clique em **Aplicar (3)**.

    - **Nome**: **ClientID (1)**
    
    - **Valor**: Colar o **ID** da aplicação API **PrioritZFL (2)** que você anotou anteriormente no bloco de notas.

        ![](images/L04/vscode37u.png)

32. Clique novamente em **+ Adicionar**.

33. Insira os seguintes dados no painel **Adicionar/Editar configuração do aplicativo** e clique em **Aplicar**.

    - **Nome**: **ClientSecret (1)**
    
    - **Valor**: Colar o **Segredo da API PrioritZ (2)** que notou anteriormente no bloco de notas.

        ![](images/L04/vscode38u.png)

34. Clique novamente em **+ Adicionar**.

35. Insira os seguintes dados no painel **Adicionar/Editar configuração do aplicativo** e clique em **Aplicar**.

    - **Nome**: **TennantID (1)**
    
    - **Valor**: Colar o **TenantID (2)** que você anotou anteriormente no bloco de notas.

        ![](images/L04/vscode39u.png)

36. Abra uma nova aba ou janela do navegador, acesse o **Power Platform Admin Center** e selecione **Ambientes**:

    ```
    https://admin.powerplatform.microsoft.com/environments
    ```

37. Clique para abrir o ambiente de desenvolvimento **DEV_ENV_<inject key="Deployment ID" activityCopy="false" />** que você está usando neste laboratório.

38. Copie a **URL do Ambiente** e cole-a no bloco de notas.

    ![](images/L04/image%20(47).png)

39. Clique em **+ Adicionar** mais uma vez.

40. Insira os seguintes dados no painel **Adicionar/Editar configuração do aplicativo** e clique em **Aplicar (3)**.

    - **Nome**: **DataverseURL**
    - **Valor**: Colar o **URL do Ambiente** que você anotou anteriormente no bloco de notas.

        ![](images/L04/vscode40u.png)

      >**Observação**: certifique-se de colar a **URL correta do ambiente** anotada nesta tarefa. Copiar o valor incorreto resultará em problemas nas próximas etapas/tarefas.

41. Deve ver as quatro definições de aplicativo adicionadas.

    ![](images/L04/image%20(46).png)

44. Clique em **Aplicar** e, na janela de confirmação de salvamento, clique em **Confirmar**.

45. No bloco de notas, cole a URL abaixo e substitua `{tenant id}` e `{api app id}` pelos valores de **TenantID** e **ID do aplicativo PrioritZFL API** que você anotou:

    ```
    https://login.microsoftonline.com/{tenant-id}/adminconsent?client_id={api app id}
    ```

    Após substituir os valores, sua URL deve ficar assim:
    
`https://login.microsoftonline.com/2140cxxxxxxx/adminconsent?client_id=195b2axxxxxxx`

46. Abra essa URL em uma nova aba do navegador e entre com as credenciais abaixo:

    * E-mail/Usuário: <inject key="AzureAdUserEmail"></inject>
    * Senha: <inject key="AzureAdUserPassword"></inject>

47. Clique em **Aceitar**.

### Tarefa 2: Registrar aplicativo cliente do conector

1. No portal do Azure, pesquise Microsoft Entra ID **Microsoft Entra ID** ***(1)*** na barra de pesquisa, e seleccione **Microsoft Entra ID** ***(2)***.

    ![](images/dev3.png)

1. Selecione **Registros de aplicativos** ***(1)*** no menu lateral e clique em **+ Novo registro** ***(2)***. Este registo de aplicação será utilizado para que o conector acesse à API protegida.

    ![](images/L04/diad4l2.png)

1. Preencha os seguintes dados e clique em **Registrar** ***(5)***.

    - Nome: **PrioritZConnector<inject key="DeploymentID" enableCopy="false" />** ***(1)***
    - Tipos de conta com suporte: **Somente contas neste diretório organizacional (TenantName – locatário único)** ***(2)***
    - **URL de Redirecionamento**: Selecione **Web** ***(3)*** e forneça `https://global.consent.azure-apim.net/redirect` ***(4)*** como o URL.

        ![](images/L04/diad4l3-1.png)

1. Copie o **ID do Aplicativo (client)** e salve-o no bloco de notas como **PrioritZ Connector application ID**.

    ![](images/L04/diad4l4.png)

1. Selecione **Certificados e segredos** no menu lateral e clique em **+ Novo segredo do cliente**.

    ![](images/L04/diad4l5.png)

1. Preencha com:
    - Descrição: **PrioritZsecret** ***(1)***
    - Validade: **3 meses** ***(2)***
    - Clique em **Adicionar** ***(3)***.

    ![](images/L04/diad4l6.png)

1. Copie o **valor do segredo** e salve-o num bloco de notas como **PrioritZ Connector secret**.

    ![](images/L04/diad5l33.png)

1. Selecione **Permissões de API** ***(1)*** no menu lateral e clique em **+ Adicionar uma permissão** ***(2)***.

    ![](images/L04/diad4l8.png)

1. Na aba **Solicitar Permissões de API**, selecione **Minhas APIs** ***(1)*** e escolha **PrioritZFunc<inject key="DeploymentID" enableCopy="false" />** ***(2)***.

    ![](images/L04/diad4l9.png)

1. Ative a caixa de seleção **user_impersonation** ***(1)*** e clique em **Adicionar permissão** ***(2)***.

    ![](images/L04/diad4l10.png)

## Exercício 4 – Crie Conector

Neste exercício, criará um novo conector personalizado.

### Tarefa 1: Crie um conector

1. No navegador, acesse o portal do Azure:

    ```
    https://portal.azure.com/
    ```

1. No portal do Azure, clique em **Grupos de Recursos** na seção **Navegar**.

    ![](images/L04/diad4l11.png)

1. Na lista, selecione o grupo de recursos **prioritzfunc<inject key="DeploymentID" activityCopy="false" />**.

    ![](images/L04/diad4l12.png)

1. Na lista, seleccione a função **PrioritZFunc<inject key="DeploymentID" activityCopy="false" />**.

    ![](images/L04/diad4l13.png)

1. Na página de visão geral, copie a **URL** da função.

    ![](images/L04/diad4l14.png)

1. Adicione **/api/swagger.json** ao final da URL e acesse no navegador.

    ![](images/L04/image%20(54).png)

    >**Observação**: se aparecer uma solicitação de permissões, clique em **Accept** e **continuar**.

1. Clique com o botão direito no swagger, selecione **Salvar como** e salve o arquivo em sua máquina local como **swag.json**.

    ![](images/L04/diad4l15.png)

1. Acesse o portal do Power Apps. Certifique-se de que o ambiente de desenvolvimento correto esteja selecionado.
 
    ```
    https://make.powerapps.com
    ```

1. Expanda **Dataverse** ***(1)*** e selecione **Conectores Personalizados** ***(1)***.

    ![](images/L04/diad4l16.png)

1. Clique na seta ao lado de **Novo conector personalizado** e selecione **Importar um arquivo OpenAPI.**.

    ![](images/L04/diad4l17.png)

1. Digite **PrioritZ Connector** ***(1)*** como nome e clique em **Importar** ***(2)***.

    ![](images/L04/diad4l20.png)

1. Selecione o arquivo **swagger (1)** salvo anteriormente e clique em **Continuar (2)**.

    ![](images/L04/diad4l18.png)

1. Digite **PrioritZ<inject key="DeploymentID" activityCopy="false" /> Connector** ***(1)*** como descrição e clique em **Segurança** ***(2)***.

    ![](images/L04/diad4l19.png)

16. Selecione **OAuth 2.0** ***(1)*** como tipo de autenticação. Forneça os seguintes dados e clique em **Criar conector** ***(8)***.

    - Provedor de Identidade: **Azure Active Directory** ***(2)***
    - Client id: cole o **PrioritZ Connector application ID** ***(3)***
    - Client secret: cole o **PrioritZ Connector Secret** ***(4)***
    - Tenant ID: cole o **Tenant ID** ***(5)***
    - Resource URL: cole o **PrioritZ API application ID** ***(6)***
    - Enable on-behalf-of login: **true** ***(7)***

        ![](images/L04/diad4l21.png)

### Tarefa 2: Testar o conector

1. No conector recém-criado, clique em **Editar**. Selecione a aba **Testar** ***(1)*** no menu suspenso e clique em **+ Nova conexão** ***(2)***.

    ![](images/L04/diad4l22.png)

1. Clique em **Criar**.

    ![](images/L04/diad4l23.png)

1. Se aparecer a tela de login, insira as credenciais abaixo e clique em **Aceitar** para os termos:

    * E-mail/Usuário: <inject key="AzureAdUserEmail"></inject>
    * Senha: <inject key="AzureAdUserPassword"></inject>

7. Depois que o conector for criado, selecione **Conectores personalizados (1)** no menu do lado esquerdo e clique em **Editar (2)** no **PrioritZ connector**.

    ![](images/L04/L04-custom.png)

7. Selecione a aba **Testar** no menu suspenso.

    ![](images/tstcnt.png)

9. Confirme que a conexão criada esteja selecionada.

    ![](images/cntcr.png)

11. Ative **Raw Body**, insira o JSON abaixo e clique em **Testar operação**.

    ```
    {
    "topic": "Test Topic",
    "details": "From Azure Function",
    "respondBy": "2022-11-01",
    "myNotes": "It worked",
    "choices": [
    {
    "choice": "Choice 1"
    }
    ]
    }
    ```
    
    ![](images/L04/diad4l24.png)

11. O teste deve ser concluído com sucesso, e a resposta deve ser semelhante à da imagem de exemplo.

    ![](images/L04/image%20(65).png)

### Exercício 5 – Usar a Função no Aplicativo de Tela

### Tarefa 1: Usar a função

1. Acesse o portal do Power Apps e certifique-se de que está no ambiente correto.

2. Selecione Aplicativos, selecione o aplicativo **PrioritZ Admin** e clique em **Editar**.

    ![](images/L04/image%20(66).png)

3. Selecione **Dados** , clique em **+ Adicionar dados**, pesquise por **prioritz connector** e selecione o conector criado.

    ![](images/L04/image%20(67).png)

4. Clique novamente para adicioná-lo.

5. No conector adicionado, clique em **... Mais ações** e selecione **Renomear**.

    ![](images/L04/image%20(68).png)

6. Renomeie para **PrioritZFunction**.

    ![](images/L04/image%20(69).png)

7. Abra a **Árvore de Exibição** e expanda **Add Topic Screen**.

    ![](images/tree.png)

9. Selecione o ícone **Add choice**.

    ![](images/L04/image%20(70).png)

9. Substitua a fórmula **OnSelect** do ícone **Add choice** pela fórmula abaixo. Essa alteração ajusta os nomes das colunas para que correspondam aos da API e codifica as fotos.

    ![](images/L04/image%20(72).png)

    ```    
    Collect(
    colAddChoices,
    {
    choice: 'Choice name textbox'.Text,
    photoRaw: UploadedImage1.Image,
    photo: JSON(
    UploadedImage1.Image,
    JSONFormat.IncludeBinaryData
    )
    }
    );
    Reset('Choice name textbox');
    Reset(AddMediaButton2)
    ```

10. Selecione o ícone **Save topic**.

11. Substitua a fórmula **OnSelect** do ícone **Save topic** pela fórmula abaixo. Essa alteração faz com que a API passe a ser responsável por criar a **solicitação**.


    ![](images/L04/image%20(74).png)

    ```    
    Set(returnGuid, PrioritZFunction.CreateTopic({
    topic: 'Topic name textbox'.Text,
    details: 'Topic details textbox'.Text,
    respondBy: 'respond by date picker'.SelectedDate,
    myNotes: 'Notes textbox'.Text,
    photo: JSON(AddTopicImage.Image, JSONFormat.IncludeBinaryData),
    choices: ShowColumns(colAddChoices, "choice", "photo")
    }));
    Notify("Topic created! " & returnGuid, NotificationType.Success, 5000);
    Back();
    ```

12. Clique em **Salvar**.

1. Clique em **Publicar**.

1. Selecione **Publicar esta versão** e aguarde que a publicação seja concluída.

1. Não saia desta página até o processo terminar.

### Tarefa 2: Testar o aplicativo

1. Selecione o **Home Screen** e clique em **Preview the app**.

    ![](images/L04/image%20(75).png)

2. Clique no botão **+** Adicionar.

3. Insira **Function Test** para o Tópico, **Testing the function** para Detalhes. **Note for testing the function** em
 "Nota", selecione uma data para "Resposta até" e clique em **Add a picture**.

    ![](images/L04/image%20(76).png)

4. Navegue até a pasta **C:\LabFiles** no explorador de arquivos, selecione **image.png** e clique em **Abrir**.

5. Insira **Test choice one** no campo "Choice" e clique em **add a picture**.

6. Navegue até a pasta **C:\LabFiles** no explorador de arquivos, selecione **image.png** e clique em **+**.

    ![](images/L04/image%20(77).png)

7. Insira **Test choice two** no campo "Choice" e clique em **add a picture**.

8. Navegue até a pasta **C:\LabFiles** no explorador de arquivos, selecione **image.png** e clique em **+**.

9. Clique em **Salvar**.

    ![](images/L04/image%20(78).png)

10. O novo tópico será salvo e você será redirecionado à tela principal.

11. Localize o novo tópico criado e abra-o.

    ![](images/L04/image80.png)

12. Veja as duas opções que adicionou ao tópico.

    ![](images/L04/image81.png)

## Resumo

Neste laboratório, você aprendeu a criar, implementar e publicar uma função no Azure, criar um conector personalizado para ela e testar sua integração em aplicativos da Power Platform.

## Concluiu o laboratório com sucesso
