# Laboratório 04 - Azure Function

## Duração estimada: 110 minutos

Trabalhando como parte da equipe de desenvolvimento da PrioritZ, você irá configurar um conector personalizado para uma nova API que você construirá usando o Azure Functions. A equipe decidiu mover a lógica de quando um usuário cria uma nova solicitação para a API do Azure Function. Isso manterá a fórmula do Power App simples e permitirá que lógicas mais complexas sejam adicionadas no futuro. Neste laboratório, você criará a função, usará a API do Dataverse, protegerá a API com o Microsoft Entra ID, configurará um conector personalizado para usar a API e alterará o Power App para usar o conector.

>**Observação:** Este laboratório requer uma assinatura (ou versão de avaliação) do Azure no mesmo tenant do seu ambiente Dataverse.

## Objectivo de Laboratório

- Exercício 1: Criar uma Azure Function
- Exercício 2: Implementação da função
- Exercício 3: Publicar no Azure
- Exercício 4: Criar o conector
- Exercício 5: Usar a Função a partir de um Aplicativo de Tela

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

    * E-mail/nome de usuário: <inject key="AzureAdUserEmail"></inject>
    
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

--- PAREI QUI!!!!
11. Depois de a aplicação Função estar implantada, Clique em **Deploy to Azure** na secção **Workspaces** e escolha a Aplicação de Função que criou.

    ![](images/L04/DeployNewu.png)

    ![](images/L04/DeployNew1u.png)

12. Aguarde que a aplicação de função seja implementada e navegue até ao Portal Azure utilizando o URL abaixo.

    ```
    https://portal.azure.com/
    ```

13. Selecione **All resources**, pesquise pela aplicação de função **PrioritZFunc<inject key="Deployment ID" activãoCopy="false" />** que implantou anteriormente e clicar para o abrir.

    ![](images/L04/vscode27.1.png)

14. Selecione **Authentication (1)** do menu do lado esquerdo e clique em **Add identity provider (2)**.

    ![](images/L04/L04-auth.1.png)

15. Selecione **Microsoft** para o fornecedor de identidade e **Current tenant - Single tenant** para **Supported Account types** e clique em **Add**.

    ![](images/L04/L04-auth.2.png)

16. Abra o menu **Portal** clicando no ícone do menu Portal.

    ![](images/dev4.png)

17. Selecione **Microsoft Entra ID** na lista de recursos.

    ![](images/dev1.png)

18. Selecione **App registrations** em **Manage** do menu do lado esquerdo.

    ![](images/L04/vscode30.png)

19. Clique para abrir o **PrioritZFunc<inject key="Deployment ID" enableCopy="false" />** para abrir a aplicação.

    ![](images/L04/vscode31.png)

20. Copie o **Application (client) ID** do **PrioritZFunc<inject key="Deployment ID" activityCopy="false" />** inscrição da aplicação e guarde-o num
 nota como **ID da aplicação API PrioritZFL**. Vai precisar deste ID em passos futuros. Este ID será utilizado para configurar a proteção da API.

    ![](images/L04/image%20(33).png)

    ![](images/L04/image%20(34).png)

    >**Nota**: Certifique-se de que copia e colide o valor correto **Aplicação (cliente) ID**. Copiar o valor incorreto resultará em problemas nos próximos passos/tarefas.

21. Copie o **Directory (tenant) ID** ae mantenha-o num bloco de notas como **Tenant ID**. Vai precisar deste ID em passos futuros.

    ![](images/L04/image%20(35).png)

    >**Nota**: Certifique-se de que copia e colide o valor correto de **Directory (inquilino) ID**. Copiar o valor incorreto resultará em problemas nos próximos passos/tarefas.

22. Selecione **Certificates & secrets** em **Manage** no menu do lado esquerdo.

    ![](images/L04/vscode32.png)

23. Clique em **+ New client secret**.

    ![](images/L04/image%20(36).png)

24. Forneça uma descrição como **PrioritZ API secret (1)**, seleccione **3 months(2)** e clique em **Add(3)**.

    ![](images/L04/image38.png)

25. Copie o **Value** e mantenha-o num bloco de notas como **PrioritZFL API Secret**. Precisa desse valor em passos futuros.

    >**Nota**: Certifique-se de que copia e colhe o valor correto **Secret**. Copiar o valor incorreto resultará em problemas nos próximos passos/tarefas.

26. Selecione **API permissions** em **Manage** do menu do lado esquerdo.

    ![](images/L04/vscode33.png)

27. Clique em **+  Add a permission**.

    ![](images/L04/image%20(39).png)

28. Selecione **Dynamics CRM** na lista de permissões da API. Dynamics CRM é Dataverse, o portal Azure simplesmente não foi atualizado no momento da escrita dessas etapas.

    ![](images/L04/vscode34.png)

29. Verifique a caixa de selecção **user_impersonation** e clique em **Add permission**.

30. Volte a **Home** e abra o **PrioritZFunc<inject key="Deployment ID" activityCopy="false" />** a aplicação de função.

    ![](images/L04/vscode27.1.png)

31. Selecione **Environment Variables (1)** em **Settings** do menu do lado esquerdo..

32. Clique em **+ Add (2)**

    ![](images/L04/vscode36u.png)

33. Introduza os seguintes detalhes sobre a **Add/Edit application setting** lâmina e clique em **Apply (3)**.

    - **Name**: **ClientID (1)**
    
    - **Value**: Colar o **ID** da aplicação API **PrioritZFL (2)** que notou anteriormente no bloco de notas.

        ![](images/L04/vscode37u.png)

34. Clique novamente em **+ Add**.

35. Introduza os seguintes detalhes sobre a **Add/Edit application setting** lâmina e clique em **Apply (3)**.

    - **Name**: **ClientSecret (1)**
    
    - **Value**: Colar o **PrioritZFL API Secret (2)** que notou anteriormente no bloco de notas.

        ![](images/L04/vscode38u.png)

36. Clique novamente em **+ Add**.

37. Introduza os seguintes detalhes sobre a **Add/Edit application setting** lâmina e clique em **Apply (3)**.

    - **Name**: **TennantID (1)**
    
    - **Value**: Colar o **TenantID (2)** que notou anteriormente no bloco de notas.

        ![](images/L04/vscode39u.png)

38. Inicie uma nova janela ou tablaça de navegação no centro de administração Power Platform e selecione **Environments**.

    ```
    https://admin.powerplatform.microsoft.com/environments
    ```

39. Clique para abrir o ambiente Dev denominado **DEV_ENV_<inject key="Deployment ID" activityCopy="false" />** está a utilizar para este laboratório.

40. Copie o **URL** Ambiente e cole-o no bloco de notas.

    ![](images/L04/image%20(47).png)

41. Clique em **+ Add** mais uma vez.

42. Introduza os seguintes detalhes sobre a **Add/Edit application setting** lâmina e clique em **Apply**.

    - **Name**: **DataverseURL**
    - **Value**: Colar o **Environment URL** que notou anteriormente no bloco de notas.

        ![](images/L04/vscode40u.png)

      >**Nota**: Certifique-se de que colide o **Environment URL** que observou anteriormente nesta tarefa. Copiar o valor incorreto resultará em problemas nos próximos passos/tarefas.

43. Deve ver as quatro definições de aplicação que adicionou.

    ![](images/L04/image%20(46).png)

44. Clique em **Apply** e In **Save changes** pop-up clique em **Confirm**.

45. Colar o URL abaixo no Notepad e substitua `{tenant id}` e `{api app id}` por **tenant id** e **PrioritZFL API application ID** dos seus valores do seu bloco de notas.

    ```
    https://login.microsoftonline.com/{tenant-id}/adminconsent?client_id={api app id}
    ```

    Depois de atualizar os valores, o seu URL deve ficar assim: `https://login.microsoftonline.com/2140cxxxxxxx/adminconsent?client_id=195b2axxxxxxx`

46. Após atualizar os valores, navegue até ao URL num separador de browser e inscreva-se com as credenciais abaixo.

    * E-mail/nome de utilizador: <inject key="AzureAdUserEmail"></inject>
    * Palavra-passe: <inject key="AzureAdUserPassword"></inject>

53. Clique em **Accept**.

### Tarefa 2: Registe-se a aplicação do cliente do conector

1. Navegue até ao portal do Azure e pesquise por **Microsoft Entra ID** ***(1)*** na barra de pesquisa, e seleccione **Microsoft Entra ID** ***(2)***.

    ![](images/dev3.png)

1. Selecione **App registrations** ***(1)*** da lâmina lateral e clique em **+ New registration** ***(2)***. Este registo de aplicação será utilizado para o conector para aceder à API protegida.

    ![](images/L04/diad4l2.png)

1. Forneça os seguintes detalhes e clique em **Register** ***(5)***.

    - Nome: **PrioritZConnector<inject key="DeploymentID" enableCopy="false" />** ***(1)***
    - Tipos de contas suportados: **Accounts in this organizational directory only (TenantName only - Single tenant)** ***(2)***
    - Redirecionar URL: Selecione **Web** ***(3)*** e forneça `https://global.consent.azure-apim.net/redirect` ***(4)*** como o URL.

        ![](images/L04/diad4l3-1.png)

1. Copie o **Application (client) ID** e guarde-o num bloco de notas como **PrioritZ Connector application ID**.

    ![](images/L04/diad4l4.png)

1. Selecione **Certificates & secrets** na lâmina lateral e clique em **+ New client secret**.

    ![](images/L04/diad4l5.png)

1. Fornecer **PrioritZsecret** ***(1)*** como descrição, defina expiração para **3 months** ***(2)***, e clique em **Add** ***(3)***.

    ![](images/L04/diad4l6.png)

1. Copie o **Secret value** e guarde-o num bloco de notas como **PrioritZ Connector secret**.

    ![](images/L04/diad5l33.png)

1. Selecione **API permissions** ***(1)*** da lâmina lateral e clique em **+ Add a permission** ***(2)***.

    ![](images/L04/diad4l8.png)

1. No separador Permissão de API de pedido, seleccione o separador **My APIs** ***(1)*** e seleccione **PrioritZFunc<inject key="DeploymentID" enableCopy="false" />** ***(2)***.

    ![](images/L04/diad4l9.png)

1. Selecione **user_impersonation** ***(1)*** e clique em **Add permission** ***(2)***.

    ![](images/L04/diad4l10.png)

## Exercício 4 – Crie Conector

Neste exercício, criará um novo conector personalizado.

### Tarefa 1: Crie um conector

1. Navegue até ao Portal do Azure utilizando o URL abaixo.

    ```
    https://portal.azure.com/
    ```

1. Agora no portal Azure, clique em **Resource Groups** presente na secção **Navigate**.

    ![](images/L04/diad4l11.png)

1. Na lista, selecione o grupo de recursos **prioritzfunc<inject key="DeploymentID" activityCopy="false" />**.

    ![](images/L04/diad4l12.png)

1. Na lista, seleccione a função **PrioritZFunc<inject key="DeploymentID" activityCopy="false" />**.

    ![](images/L04/diad4l13.png)

1. Na página de visão geral, Copie o **URL** da função.

    ![](images/L04/diad4l14.png)

1. Adicione **/api/swagger.json** no final do URL e aceda a ele utilizando o browser.

    ![](images/L04/image%20(54).png)

    >**Nota**: Se as permissões surgirem, clique em **Accept** e continue.

1. Clique com o botão direito do rato no swagger seleccione **Save as** e guarde o ficheiro na máquina local: Forneça um nome para ficheiro como swag.json.

    ![](images/L04/diad4l15.png)

1. Navegue até ao portal do fabricante de aplicações de energia utilizando o URL abaixo. Certifique-se de que o ambiente de desenvolvimento é selecionado.
 
    ```
    https://make.powerapps.com
    ```

1. Expandir **Dataverse** ***(1)*** e seleccione **Custom Connectors** ***(1)***.

    ![](images/L04/diad4l16.png)

1. Clique no botão chevron junto ao Novo conector personalizado e selecione **Import an OpenAPI file**.

    ![](images/L04/diad4l17.png)

1. Introduza **PrioritZ Connector** ***(1)*** para nome e clique em **Import** ***(2)***.

    ![](images/L04/diad4l20.png)

1. Selecione o ficheiro **swagger (1)** que guardou no passo 7 desta tarefa e clique em **Continue (2)**.

    ![](images/L04/diad4l18.png)

1. Fornecer **PrioritZ<inject key="DeploymentID" activityCopy="false" /> Connector** ***(1)*** como descrição como e clique em **Security** ***(2)***.

    ![](images/L04/diad4l19.png)

16. Selecione **OAuth 2.0** ***(1)*** para o tipo de autenticação. Forneça os seguintes detalhes e clique em **Create connector** ***(8)***.

    - Identity Provider: **Azure Active Directory** ***(2)***
    - Client id: Colar **PrioritZ Connector application ID** ***(3)*** que copiou anteriormente
    - Client secret: Colar **PrioritZ Connector Secret** ***(4)*** que copiou anteriormente
    - Tenant ID: Colar o **Tenant ID** ***(5)*** que copiou anteriormente
    - Resource URL: Colar **PrioritZ API application ID** ***(6)*** que copiou anteriormente
    - Enable on-behalf-of login: **true** ***(7)***

        ![](images/L04/diad4l21.png)

### Tarefa 2: Conector de teste

1. Agora navegue até ao conector que acabou de criar e clique no botão **edit**. Selecione o separador **Test** ***(1)*** no menu suspenso e clique em **+ New connection** ***(2)***.

    ![](images/L04/diad4l22.png)

1. Clique em **Create**.

    ![](images/L04/diad4l23.png)

1. Se o prompt de login aparecer entre nas credenciais abaixo e clicar em **Accept** para aceitar os termos.

    * E-mail/nome de utilizador: <inject key="AzureAdUserEmail"></inject>
    * Palavra-passe: <inject key="AzureAdUserPassword"></inject>

7. Depois de criado o conector, selecione **Custom connectors (1)** no menu do lado esquerdo e clique em **Edit (2)** no **PrioritZ connector**.

    ![](images/L04/L04-custom.png)

7. Selecione o **Test** no separador menu suspenso.

    ![](images/tstcnt.png)

9. Certifique-se de que a ligação que criou está selecionada.

    ![](images/cntcr.png)

11. Ligue **Raw Body** e forneça o JSON abaixo e clique em **Test operation**.

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

11. O teste de operação deve ter sucesso e a resposta deve parecer a image abaixo.

    ![](images/L04/image%20(65).png)

### Exercício 5 – Utilize a função de aplicação em tela

Neste exercício, você usará a função do Azure que você criou por meio do conector personalizado da aplicação de canvas PrioritZ Admin.

### Tarefa 1: Utilize a função

1. Navegue até ao portal do fabricante do Power Apps e certifique-se de que está no ambiente correto.
2. Selecione Aplicações, selecione a aplicação **PrioritZ Admin** e clique em **Edit**.

    ![](images/L04/image%20(66).png)

3. Selecione **Data** , clique em **+ Add data**, pesquise por conector prioritz e selecione o **PrioritZ Connector**
 criou.

    ![](images/L04/image%20(67).png)

4. Adicione o conector clicando nele novamente.
5. Clique no botão **... More actions** do conector que acabou de adicionar e seleccionar **Rename**.

    ![](images/L04/image%20(68).png)

6. Renomeie o conector **PrioritZFunction**.

    ![](images/L04/image%20(69).png)

7. Selecione a **Tree view** e expanda o **Add Topic Screen**.

    ![](images/tree.png)

9. Selecione o **Add choice icon**.

    ![](images/L04/image%20(70).png)

9. Substitua a fórmula **OnSelect** do **Add choice icon** com a fórmula abaixo. Isto ajusta o
 nomes de colunas para corresponder à API e codifica as fotos.

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

10. Selecione **Save topic icon**.

11. Substitua a fórmula **OnSelect** do **Save topic icon** com a fórmula abaixo. Isto muda para
 fazer com que a API crie o “pergunte”.


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

12. Clique em **Save**.

1. Clique em **Publish**.

1. Selecione **Publish this version** e aguarde que a publicação seja concluída.

1. Não navegue por esta página.

### Tarefa 2: Aplicação de teste

1. Selecione o **Home Screen** e clique em **Preview the app**.

    ![](images/L04/image%20(75).png)

2. Clique no botão **+** adicionar.
3. Introduza **Function Test** para o Tópico, **Testing the function** para Detalhes. **Note for testing the function** para
 Note que selecione uma data para Resposta por e clique em **Add a picture**.

    ![](images/L04/image%20(76).png)

4. Navegue para este caminho **C:\LabFiles** no ficheiro explorador de ficheiros, seleccione **image.png** e clique em abrir.

5. Introduza **Test choice one** para Escolha e clique em **add a picture**.
6. Navegue para este caminho **C:\LabFiles** no ficheiro explorer, seleccione **image.png** e clique em **+**.

    ![](images/L04/image%20(77).png)

7. Introduza **Test choice two** para Escolha e clique em **add a picture**.

8. Navegue para este caminho **C:\LabFiles** no ficheiro explorer, seleccione **image.png** e clique em **+**.

9. Clique em **Save**.

    ![](images/L04/image%20(78).png)

10. O novo tópico deve ser guardado e deve ser navegado de volta para o ecrã principal.
11. Localize o novo tópico que criou e abra.

    ![](images/L04/image80.png)

12. Deve ver as duas opções que adicionou ao tópico.

    ![](images/L04/image81.png)

## Resumo

Neste laboratório, aprendeu a criar, implementar e publicar uma Função Azure, criar um conector para ele e testar a sua integração nas aplicações Power Platform.

## Concluiu o laboratório com sucesso
