# Laboratório 04 - Função Azure

## Duração estimada: 110 minutos

Trabalhando como parte da equipa de fusão PrioritZ, irá configurar um conector personalizado para uma nova API que
construir utilizando Funções Azure. A equipa decidiu mover a lógica quando um utilizador cria um novo “pedir” para
a API de Função Azure. Isto manterá a fórmula de Aplicação Power simples e permitirá que a lógica mais complexa seja
adicionado no futuro. Neste laboratório, irá criar a função, utilizar a API Dataverse e proteger a API com
O Microsoft Entra ID configura um conector personalizado para utilizar a API e altera a aplicação Power para utilizar o
conector.

Nota: Este laboratório requer uma subscrição (ou ensaio) do Azure no mesmo inquilino que o seu Dataverse
ambiente.

## Objectivo de laboratório

- Exercício 1: Criar Função Azure
- Exercício 2: implementação de funções
- Exercício 3: Publicar a Azure
- Exercício 4: Crie Conector
- Exercício 5: Utilize a função da aplicação em tela

## Exercício 1 – Criar Função Azure

Neste exercício, instala a extensão de ferramentas Azure para o Visual Studio Code e cria a função

### Tarefa 1: Instalar a extensão das ferramentas de azure

1. Iniciar **Código do estúdio do Visual** utilizando o atalho disponível na área de trabalho.

    ![](images/L04/vscode1.png)

2. Selecione o separador **Extensões**.

    ![](images/L04/vscode2.png)

3. Pesquise por **Tais ferramentas de azure (1)** e clique em **Instale (2)** para instalar a extensão Azure Tools.

    ![](images/L04/vscode3.png)

4. Aguarde que a instalação seja concluída.

5. Agora deve ver a nova extensão Azure Tools que adicionou.

    ![](images/L04/image%20(2).png)

6. Clique em **Terminal** no menu superior e selecione **Novo Terminal**.

7. Execute o comando abaixo no terminal para criar uma nova pasta.
 
    ```
    md ContosoFunções
    ```
    ![](images/L04/image%20(4).png)

### Tarefa 2: Crie uma função

1. Selecione **Ferramentas de azulo (1)** do menu de navegação esquerda e navegue até à secção **Workspace (2)**.

    ![](images/L04/vscode4.png)

1. Clique no **símbolo(1)** na image, na secção **Workspace**, clique em **Criar Função (2)** e clique em **Criar Novo Projeto**.

    ![](images/L04/functionu.png)

    >**Nota**: Se o símbolo **+** não for visível, seleccione as definições e veja se existem alguma atualização necessária para o vscode e atualize a aplicação Feche o vscode atual e abra novamente e execute o passo acima.

1. Selecione a pasta **ContosoFunctions (1)** que criou e clique em **selecionar a pasta (2)**

    ![](images/L04/L04-contoso.png)

1. Agora, será apresentado com o pop-up abaixo, clique em **Sim** para criar um novo projeto.

    ![](images/L04/vscode6.png)

1. Selecione **C#** para a linguagem.

    ![](images/L04/vscode7.png)

1. Selecione **.NET 6.0 LTS** para o tempo de execução . NET.

    ![](images/L04/vscode8.png)

1. Selecione **Acréscimo HTTP com OpenAPI** para o modelo.

    ![](images/L04/vscode9.png)

1. Introduza **CreateTopic** para o nome da função e bate **[ENTER].**

    ![](images/L04/image%20(7).png)

1. Introduza **Contoso.PrioritZ** para namespace e hit **[ENTER]**.

    ![](images/L04/vscode10.png)

1. Selecione **Anónimo** para AccessRights. Mais tarde, protegeremos a função utilizando o Microsoft Entra ID.

    ![](images/L04/vscode11.png)

9. Se for apresentado à janela abaixo, selecione **Aberto na janela atual.**

    ![](images/L04/image%20(8).png)

1. A sua função deve abrir em **Visual Studio Code**.

1. Se obteve um pop-up **Confia em autores dos ficheiros nesta pasta**, clique em **Sim, confio no autor**

1. Clique em **Terminal** no menu superior e selecione **Enfir Build Task**.

    ![](images/L04/image%20(9).png)

1. Assim que a construção for sucedida, **prima qualquer tecla para fechar o terminal**.

    ![](images/L04/vscode12.png)


## Exercício 2 - Implementação de funções

Neste exercício, irá implementar a função.

### Tarefa 1: Função de implementação

1. Clique em **Novo ficheiro** que está próximo de **ContosoFunctions** para adicionar um novo ficheiro.

    ![](images/L04/image%20(10).png)

2. Refira o novo ficheiro **Model.cs**

    ![](images/L04/vscode13.png)

3. Abra o novo ficheiro **Model.cs** e cole o código abaixo. Isto definirá os dados que serão enviados
 da aplicação de energia.

    ```
    utilizando o sistema;
    utilizando o Microsoft.Azure.WebJobs.Extensions.OpenApi.Core.Attributes;
    utilizando o Microsoft.OpenApi.Models;
    namespace Contoso.PrioritZ
    {
    classe pública TopicItemModel
    {
    public string Choice { get; conjunto; }
    public string Foto { get; conjunto; }
    }
    classe pública TopicModel
    {
    [OpenApiProperty(nulable = false, Descrição = "Este é um tópico")]
    public string Tópico {get; conjunto; }
    public string Detalhes { get; conjunto; }
    public DateTime RespondBy { get; conjunto; }
    public string MyNotes { get; conjunto; }
    public string Foto { get; conjunto; }
    public TopicItemModel[] Escolhas {get;set;}
    }
    }
    ```
    Depois de adicionar o código, o seu **Model.cs** parecerá a captura de ecrã abaixo:

    ![](images/L04/vscode14.png)

4. Abra o ficheiro **CreateTopic.cs**.

5. Localize os atributos do método Executar (números de linha 24-27) que estão presentes acima do método **Run** e substitua-os pelos atributos abaixo. Isto fornece nomes fáceis de utilizar quando criamos um conector para utilizar a API.


    ```
    [Nome de Função("CreateTopic")]
    [OpenApiOperation(operationId: "CreateTopic", tags: new[] { "name" }, Summary = "Create Topic", Descrição = "Criação de Tópico", Visibilidade = OpenApiVisbilityType.Important)]
    [OpenApiSecurity("function_key", SecuritySchemeType.ApiKey, Name = "code", In = OpenApiSecurityLocationType.Query)]
    [OpenApiResponseWithBody(statusCode: HttpStatusCode. OK, contentType: "application/json", bodyType: typeof(Guid), Descrição = "The Guid response")]
    [OpenApiRequestBody(contentType: "application/json", bodyType: typeof(TopicModel))]
    ```

    Depois de adicionar os atributos, o seu método **Run** deve parecer-se com a captura de ecrã abaixo:

    ![](images/L04/image%20(13).png)

6. Remova **get** do método Run como deve ter apenas **post**.

    ![](images/L04/image%20(14).png)

7. Clique em **Terminal** no menu superior e selecione o **Novo Terminal**.

8. Execute o comando abaixo no terminal para adicionar o pacote **Power Platform Dataverse Client**.

    ```
    dotnet adicionar pacote Microsoft.PowerPlatform.Dataverse.Client
    ```

    ![](images/L04/image%20(17).png)

9. Aguarde que o pacote seja adicionado e execute o comando abaixo para adicionar o pacote **Azure Identity**.

    ```
    dotnet adicionar pacote Azure.Identidade
    ```

10. Aguarde o pacote **Azure Identity** a adicionar.

11. Abra o ficheiro **CreateTopic** e adicione as instruções abaixo após a linha número 11.

    ```
    utilizando o sistema;
    utilizando o Microsoft.Identy.Client;
    usando o Azure.Core;
    usando o Azure.Identidade;
    utilizando o Microsoft.PowerPlatform.Dataverse.Client;
    utilizando o Microsoft.Azure.WebJobs.Extensions.OpenApi.Core.Enums;
    utilizando o Microsoft.Xrm.Sdk;
    ```

    ![](images/L04/vscode15.png)

12. Adicione o método abaixo após o método **Run**. Este método utilizará o token passado do aplicação de chamadas para obter um novo token que permita que a função utilize a API Dataverse em nome de o utilizador chamada.

    ```
    public static async Task<string> GetAccessTokenAsync(HttpRequest req,string recursoUri)
    {
    //Obtenha o token do utilizador de chamada a partir do pedido para utilizar como UserAssertion
    var headers = req.Headers;
    var token = string. Vazio;
    if (headers.TryGetValue("Autorização", out var authHeader))
    {
    if (authHeader[0]. StartsWith("Bearer"))
    {
    token = authHeader[0].Substring(7, authHeader[0].Length -
    7);
    }
    }
    string[] escopos = new[] {$"{resourceUri}/.default" };
    string clientSecret = Environment. GetEnvironmentVarable("ClientSecret");
    string clientId = Ambiente. GetEnvironmentVarable("ClientID");
    string tennantId = Ambiente. GetEnvironmentVarable("TenantID");
    var app = ConfidencialClientApplicationBuilder. Create(clientId)
    .WithClientSecret(clientSecret)
    .WithAuthority($"https://login.microsoftonline.com/{tennantId}")
    .Construir();
    //Obtenha-se em nome do token para ligar para o utilizador
    UserAssertion userAssertion = new UserAsertion(token);
    var result = await app.AcquireTokenOnBehalfOf(scópios,
    userAssertion).ExecuteAsync();

    resultado de retorno.AccessToken;
    }

    ```

    ![](images/L04/vscode16.png)

13. Substitua o código dentro do método **Run** pelo código abaixo. Isto fornecerá uma instância do API Dataverse e utilize a função GetAccessToken que acabamos de definir.

    ```
    _logger.LogInformation("Começar o tópico Criar");
    var serviceClient = new ServiceClient(
    instânciaUrl: new Uri(Environment. GetEnvironmentVarable("DataverseUrl")),
    tokenProviderFunction: async uri => { return await
    GetAccessTokenAsync(req, Environment. GetEnvironmentVariable("DataverseUrl"));
    },
    useUniqueInstance: true,
    logger: _logger);
    if (!serviceClient.IsReady)
    {
    lançar novo Exceção("Autenticação Falha!");
    }
    ```

    ![](images/L04/vscode17.png)

14. Adicione o seguinte código após a instrução if do método **Run** para reserizar o pedido. Isto levará os dados passados ​​do chamador.

    ```
    string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
    var data = JsonConvert.DeserializeObject<TopicModel>(requestBody);
    ```
    ![](images/L04/vscode18.png)

15. Adicione o código abaixo que cria a linha ao método **Run** após o código que adicionou no passo anterior para **reserializar o pedido**. Este código cria as linhas em Dataverse é onde podemos adicionar mais lógica no futuro.

    ```
    var ask = new Entity("contoso_prioritztopic");
    ask["contoso_topic"] = data.Topic;
    ask["contoso_details"] = data.Detalhes;
    pergunta["contoso_mynotes"] = data.MyNotes;
    ask["contoso_respondby"] = data.RespondBy.Date;
    if (dados.Foto ! = nulo)
    {
    // Remover citações duplas desnecessárias,
    // Remova tudo antes da primeira vírgula (coisas incorporadas)
    ask["contoso_photo"] = Convert.FromBase64String(data. Photo.Trim('\"').Split(',')[1]);
    }
    var tópicoId = await serviceClient.CreateAsync(ask);
    foreach (escolha var aos dados. Escolhas)
    {
    var item = new Entity("contoso_prioritztopicitem");
    item["contoso_choice"] = escolha. Escolha;
    item["contoso_prioritztopic"] = novo
    EntityReference("contoso_prioritztopic", tópicoId);
    if (choice.Foto ! = nulo)
    {
    item["contoso_photo"] =
    Converter.FromBase64String(choice. Photo.Trim('\"').Split(',')[1]);
    }
    var escolhaId = await serviceClient. CreateAsync(item);
    }

    ```

    ![](images/L04/vscode19.png)

16. Adicione o código abaixo ao método **Run** para devolver o ID do tópico como JSON (obrigatório por Apps Power) após o código que adicionou no passo anterior para criar a linha ao método **Run**.

    ```
    return new OkObjectResult(topicId);
    ```

    ![](images/L04/vscode20.png)

17. Clique em **Terminal (1)** e seleccione **Entrar a Tarefa de Construção (2)**.

    ![](images/L04/vscode21.png)

18. A corrida deve ter sucesso. Pressione qualquer chave para parar.

    > **Nota**:

    1. Se a operação da tarefa de construção falhar com os erros, certifique-se de que seguiu as instruções anteriores e adicionou o código corretamente nos ficheiros **CreateTopic.cs e Model.cs.**
    
    2. Além disso, pode encontrar os ficheiros **CreateTopic.cs e Model.cs** na localização **C:\LabFiles**, pode comparar o seu código com estes ficheiros e corrigir os problemas se houver algum e tentar realize **passo 17 novamente**.

## Exercício 3 – Publicar a Azure

Neste exercício, irá implementar a função no Azure.

### Tarefa 1: Publicar

1. Selecione **Ferramentas de azulo** do menu do lado esquerdo.

    ![](images/L04/vscode22.png)

2. Clique em **Inscreva-se na secção Azure** na secção **Recursos**.

    ![](images/L04/vscode23.png)

3. Conclua o processo **Sign-in** utilizando as credenciais abaixo.

    * E-mail/nome de utilizador: <inject key="AzureAdUserEmail"></inject>
    
    * Palavra-passe: <inject key="AzureAdUserPassword"></inject>

4. Feche a janela do browser de registo assim que o processo de registo estiver concluído.

5. Navegue de volta para o Código do Visual Studio e clique em **+** que está ao lado do seu separador **Recursos** para criar uma nova aplicação de funções.

    ![](images/L04/NewVSazure3u.png)

6. Agora, pesquise e seleccione **Criar Aplicação de Função no Azure**.

    ![](images/L04/vscode24u.png)

7. Introduza **PrioritZFunc<inject key="Deployment ID" activityCopy="false" />** para o nome da aplicação de função e hit [ENTER].

    ![](images/L04/vscode25.png)

8. Selecione **.NET 6(LTS) Em processo**.

    ![](images/L04/vscode26u.png)

9. Selecione a localização: **<inject key="Region" activityCopy="false" />** da lista e aguarde que a aplicação Função seja implementada.

    ![](images/L04/vscode27.png)

10. Depois de a aplicação Função estar implantada, Clique em **Deploy to Azure** na secção **Workspaces** e escolha a Aplicação de Função que criou.

    ![](images/L04/DeployNewu.png)

    ![](images/L04/DeployNew1u.png)

11. Aguarde que a aplicação de função seja implementada e navegue até ao Portal Azure utilizando o URL abaixo.

    ```
    https://portal.azure.com/
    ```

12. Selecione **Todos os recursos**, pesquise pela aplicação de função **PrioritZFunc<inject key="Deployment ID" activãoCopy="false" />** que implantou anteriormente e clicar para o abrir.

    ![](images/L04/vscode27.1.png)

13. Selecione **Autenticação (1)** do menu do lado esquerdo e clique em **Adicionar o fornecedor de identidade (2)**.

    ![](images/L04/L04-auth.1.png)

14. Selecione **Microsoft** para o fornecedor de identidade e **Rendeiro atual - Inquilino único** para **Tipos de conta suportados** e clique em **Adicionar**.

    ![](images/L04/L04-auth.2.png)

15. Abra o menu **Portal** clicando no ícone do menu Portal.

    ![](images/dev4.png)

16. Selecione **Microsoft Entra ID** na lista de recursos.

    ![](images/dev1.png)

17. Selecione **App Insuficiências** em **Gersão** do menu do lado esquerdo.

    ![](images/L04/vscode30.png)

18. Clique para abrir o **PrioritZFunc<inject key="Deployment ID" enableCopy="false" />** para abrir a aplicação.

    ![](images/L04/vscode31.png)

19. Copie o **Aplicação (cliente) ID** do **PrioritZFunc<inject key="Deployment ID" activityCopy="false" />** inscrição da aplicação e guarde-o num
 nota como **ID da aplicação API PrioritZFL**. Vai precisar deste ID em passos futuros. Este ID será utilizado para configurar a proteção da API.

    ![](images/L04/image%20(33).png)

    ![](images/L04/image%20(34).png)

    >**Nota**: Certifique-se de que copia e colide o valor correto **Aplicação (cliente) ID**. Copiar o valor incorreto resultará em problemas nos próximos passos/tarefas.

20. Copie o **Directory (inquilino) ID** ae mantenha-o num bloco de notas como **Dirent ID**. Vai precisar deste ID em
 passos futuros.

    ![](images/L04/image%20(35).png)

    >**Nota**: Certifique-se de que copia e colide o valor correto de **Directory (inquilino) ID**. Copiar o valor incorreto resultará em problemas nos próximos passos/tarefas.

21. Selecione **Certificados e segredos** em **Gringe** no menu do lado esquerdo.

    ![](images/L04/vscode32.png)

22. Clique em **+ Novo segredo do cliente**.

    ![](images/L04/image%20(36).png)

23. Forneça uma descrição como **PrioritZ API secret (1)**, seleccione **3 meses (2)** e clique em **Adicionar (3)**.

    ![](images/L04/image38.png)

24. Copie o **Value** e mantenha-o num bloco de notas como **PrioritZFL API Secret**. Precisa desse valor em passos futuros.

    >**Nota**: Certifique-se de que copia e colhe o valor correto **Secreto**. Copiar o valor incorreto resultará em problemas nos próximos passos/tarefas.

25. Selecione **permissões da API** em **Gersão** do menu do lado esquerdo.

    ![](images/L04/vscode33.png)

26. Clique em **+ Adicione uma permissão**.

    ![](images/L04/image%20(39).png)

27. Selecione **Dynamics CRM** na lista de permissões da API. Dynamics CRM é Dataverse, o portal Azure simplesmente não foi atualizado no momento da escrita dessas etapas.

    ![](images/L04/vscode34.png)

28. Verifique a caixa de selecção **user_impersonation** e clique em **Adicionar permissão**.

29. Volte a **Home** e abra o **PrioritZFunc<inject key="Deployment ID" activityCopy="false" />** a aplicação de função.

    ![](images/L04/vscode27.1.png)

30. Selecione **Variáveis ​​de ambiente (1)** em **Definições** do menu do lado esquerdo..

31. Clique em **+ Adicionar (2)**

    ![](images/L04/vscode36u.png)

32. Introduza os seguintes detalhes sobre a **Adicionar/Editar a definição de aplicação** lâmina e clique em **Aplicar (3)**.

    - **Nome**: **ClientID (1)**
    
    - **Value**: Colar o **ID** da aplicação API **PrioritZFL (2)** que notou anteriormente no bloco de notas.

        ![](images/L04/vscode37u.png)

33. Clique novamente em **+ Adicionar**.

34. Introduza os seguintes detalhes sobre a **Adicionar/Editar a configuração da aplicação** lâmina e clique em **Aplicar (3)**.

    - **Nome**: **ClientSecret (1)**
    
    - **Value**: Colar o **PrioritZFL API Secret (2)** que notou anteriormente no bloco de notas.

        ![](images/L04/vscode38u.png)

35. Clique novamente em **+ Adicionar**.

36. Introduza os seguintes detalhes sobre a **Adicionar/Editar a configuração da aplicação** lâmina e clique em **Aplicar (3)**.

    - **Nome**: **TennantID (1)**
    
    - **Value**: Colar o **TenantID (2)** que notou anteriormente no bloco de notas.

        ![](images/L04/vscode39u.png)

37. Inicie uma nova janela ou tablaça de navegação no centro de administração Power Platform e selecione **Ambientes**.

    ```
    https://admin.powerplatform.microsoft.com/environments
    ```

38. Clique para abrir o ambiente Dev denominado **DEV_ENV_<inject key="Deployment ID" activityCopy="false" />** está a utilizar para este laboratório.

39. Copie o **URL** Ambiente e cole-o no bloco de notas.

    ![](images/L04/image%20(47).png)

40. Clique em **+ Adicione** mais uma vez.

41. Introduza os seguintes detalhes sobre a **Adicionar/Editar a definição de aplicação** lâmina e clique em **Aplicar**.

    - **Nome**: **DataverseURL**
    - **Value**: Colar o **URL Ambiente** que notou anteriormente no bloco de notas.

        ![](images/L04/vscode40u.png)

      >**Nota**: Certifique-se de que colide o **URL do ambiente** que observou anteriormente nesta tarefa. Copiar o valor incorreto resultará em problemas nos próximos passos/tarefas.

42. Deve ver as quatro definições de aplicação que adicionou.

    ![](images/L04/image%20(46).png)

43. Clique em **Aplicar** e In **Guardar alterações** pop-up clique em **Confirmar**.

44. Colar o URL abaixo no Notepad e substitua `{tennant id}` e `{api application id}` por **rend id** e **PrioritZFL API da aplicação ID** dos seus valores do seu
 bloco de notas.

    ```
    https://login.microsoftonline.com/{tennant-id}/adminconsent?client_id={api app id}
    ```

    Depois de atualizar os valores, o seu URL deve ficar assim: `https://login.microsoftonline.com/2140cxxxxxxx/adminconsent?client_id=195b2axxxxxx`

45. Após atualizar os valores, navegue até ao URL num separador de browser e inscreva-se com as credenciais abaixo.

    * E-mail/nome de utilizador: <inject key="AzureAdUserEmail"></inject>
    * Palavra-passe: <inject key="AzureAdUserPassword"></inject>

53. Clique em **Aceite**.

### Tarefa 2: Registe-se a aplicação do cliente do conector

1. Navegue até ao portal Azure e pesquise por **Microsoft Entra ID** ***(1)*** na barra de pesquisa e seleccione **Microsoft Entra ID** ***(2)*** de as sugestões.

    ![](images/dev3.png)

1. Selecione **App Insuficiências** ***(1)*** da lâmina lateral e clique em **+ Novo registo** ***(2)***. Este registo de aplicação será utilizado para o conector para aceder à API protegida.

    ![](images/L04/diad4l2.png)

1. Forneça os seguintes detalhes e clique em **Registar** ***(5)***.

    - Nome: **PrioritZConnector<inject key="DeploymentID" enableCopy="false" />** ***(1)***
    - Tipos de contas suportados: **Apenas contas neste diretório organizacional (apenas TenantName - Inquilino único)** ***(2)***
    - Redirecionar URL: Selecione **Web** ***(3)*** e forneça `https://global.consent.azure-apim.net/redirect` ***(4)*** como o URL .

        ![](images/L04/diad4l3-1.png)

1. Copie o **Application (cliente) ID** e guarde-o num bloco de notas como **PrioritZ Connector Application ID**.

    ![](images/L04/diad4l4.png)

1. Selecione **Certificados e segredos** na lâmina lateral e clique em **+ Novo segredo do cliente**.

    ![](images/L04/diad4l5.png)

1. Fornecer **PrioritZsecret** ***(1)*** como descrição, defina expiração para **3 meses** ***(2)***, e clique em **Adicionar** ***(3)***.

    ![](images/L04/diad4l6.png)

1. Copie o **Valor Secreto** e guarde-o num bloco de notas como **PrioritZ Connector secert**.

    ![](images/L04/diad5l33.png)

1. Selecione **permissões API** ***(1)*** da lâmina lateral e clique em **+ Adicione uma permissão** ***(2)***.

    ![](images/L04/diad4l8.png)

1. No separador Permissão de API de pedido, seleccione o separador **As minhas APIs** ***(1)*** e seleccione **PrioritZFunc<inject key="DeploymentID" enableCopy="false" />** ***(2)***.

    ![](images/L04/diad4l9.png)

1. Toggle a caixa de selecção **user_impersonation** ***(1)*** e clique em **Adicionar permissão** ***(2)***.

    ![](images/L04/diad4l10.png)

## Exercício 4 – Crie Conector

Neste exercício, criará um novo conector personalizado.

### Tarefa 1: Crie um conector

1. Navegue até ao Portal Azure utilizando o URL abaixo.

    ```
    https://portal.azure.com/
    ```

1. Agora no portal Azure, clique em **Grupos de recursos** presentes em Navegate.

    ![](images/L04/diad4l11.png)

1. Selecione o **prioritzfunc<inject key="DeploymentID" activityCopy="false" />** Recurso Grupo da lista.

    ![](images/L04/diad4l12.png)

1. Seleccione **PrioritZFunc<inject key="DeploymentID" activityCopy="false" />** recurso de função da lista.

    ![](images/L04/diad4l13.png)

1. Na página de visão geral, Copie o **URL** da função.

    ![](images/L04/diad4l14.png)

1. Adicione **/api/swagger.json** no final do URL e aceda a ele utilizando o browser.

    ![](images/L04/image%20(54).png)

    >**Nota**: Se as permissões surgirem, clique em **Aceite** e continue.

1. Clique com o botão direito do rato no swagger seleccione **Guarrear as** e Guardar o ficheiro na máquina local: Forneça um nome para ficheiro como swag.json.

    ![](images/L04/diad4l15.png)

1. Navegue até ao portal do fabricante de aplicações de energia utilizando o URL abaixo. Certifique-se de que o ambiente de desenvolvimento é selecionado.
 
    ```
    https://make.powerapps.com
    ```

1. Expandir **Dataverse** ***(1)*** e seleccione **Conectores personalizados** ***(1)***.

    ![](images/L04/diad4l16.png)

1. Clique no botão chevron junto ao Novo conector personalizado e selecione **Importar um ficheiro OpenAPI**.

    ![](images/L04/diad4l17.png)

1. Introduza **Conector PrioritZ** ***(1)*** para nome e clique em **Importar** ***(2)***.

    ![](images/L04/diad4l20.png)

1. Selecione o ficheiro **swagger (1)** que guardou no passo 7 desta tarefa e clique em **Continuar (2)**.

    ![](images/L04/diad4l18.png)

1. Fornecer **PrioritZ<inject key="DeploymentID" activityCopy="false" /> Conector** ***(1)*** como descrição como e clique em **Segurança** ***(2)***.

    ![](images/L04/diad4l19.png)

16. Selecione **OAuth 2.0** ***(1)*** para o tipo de autenticação. Forneça os seguintes detalhes e clique em **Criar conector** ***(8)***.

    - Fornecedor de identidade: **Azure Active Directory** ***(2)***
    - ID do cliente: Colar **ID da aplicação do conector PrioritZ** ***(3)*** que copiou anteriormente
    - Segredo do cliente: Colar **PrioritZ Connector Secret** ***(4)*** que copiou anteriormente
    - ID do inquilino: Colar o **ID do inquilino** ***(5)*** que copiou anteriormente
    - URL de recursos: Colar **ID da aplicação API PrioritZ** ***(6)*** que copiou anteriormente
    - Ativar o login em nome: **true** ***(7)***

        ![](images/L04/diad4l21.png)

### Tarefa 2: Conector de teste

1. Agora navegue até ao conector que acabou de criar e clique no botão **edit**. Selecione o separador **Test** ***(1)*** no menu suspenso e clique em **+ Nova ligação** ***(2)***.

    ![](images/L04/diad4l22.png)

1. Clique em **Criar**.

    ![](images/L04/diad4l23.png)

1. Se o prompt de login aparecer entre nas credenciais abaixo e clicar em **Aceite** para aceitar os termos.

    * E-mail/nome de utilizador: <inject key="AzureAdUserEmail"></inject>
    * Palavra-passe: <inject key="AzureAdUserPassword"></inject>

7. Depois de criado o conector, selecione **Conectores personalizados (1)** no menu do lado esquerdo e clique em **Editar (2)** no **Conector PrioritZ**.

    ![](images/L04/L04-custom.png)

7. Selecione o **Teste** no separador menu suspenso.

    ![](images/tstcnt.png)

9. Certifique-se de que a ligação que criou está selecionada.

    ![](images/cntcr.png)

11. Ligue **Raw Body** e forneça o JSON abaixo e clique em **Teste operar**.

    ```
    {
    "tópico": "Topico de Teste",
    "detalhes": "Da função Azure",
    "respondeBy": "2022-11-01",
    "myNotes": "Fominou",
    "escolhas": [
    {
    "escolha": "Escolha 1"
    }
    ]
    }
    ```
    ![](images/L04/diad4l24.png)

11. O teste de operação deve ter sucesso e a resposta deve parecer a image abaixo.

    ![](images/L04/image%20(65).png)

### Exercício 5 – Utilize a função de aplicação em tela

Neste exercício, irá utilizar a função Azure que criou através do conector personalizado do PrioritZ
Aplicação em tela de administrador.

### Tarefa 1: Utilize a função

1. Navegue até ao portal do fabricante do Power Apps e certifique-se de que está no ambiente correto.
2. Selecione Aplicações, selecione a aplicação **PrioritZ Admin** e clique em **Editar**.

    ![](images/L04/image%20(66).png)

3. Selecione **Data** , clique em **+ Adicionar dados**, pesquise por conector prioritz e selecione o **Conector PrioritZ**
 criou.

    ![](images/L04/image%20(67).png)

4. Adicione o conector clicando nele novamente.
5. Clique no botão **... Mais ações** do conector que acabou de adicionar e seleccionar **Renomear**.

    ![](images/L04/image%20(68).png)

6. Renomeie o conector **PrioritZFunction**.

    ![](images/L04/image%20(69).png)

7. Selecione a **Árvore ver** e expanda o **Add Tópico Ecrã**.

    ![](images/tree.png)

9. Selecione o **Adicionar ícone de escolha**.

    ![](images/L04/image%20(70).png)

9. Substitua a fórmula **OnSelect** do **Adicionar ícone de escolha** com a fórmula abaixo. Isto ajusta o
 nomes de colunas para corresponder à API e codifica as fotos.

    ![](images/L04/image%20(72).png)

    ```
    Recolher(
    colAddChoices,
    {
    escolha: 'Escolha o nome de textbox'. Texto,
    photoRaw: UploadedImage1.Image,
    foto: JSON(
    UploadedImage1.image,
    JSONFormat.IncludeBinaryData
    )
    }
    );
    Reset('Escolha na caixa de texto do nome');
    Reset(AddMediaButton2)
    ```

10. Selecione **Guardar ícone do tópico**.

11. Substitua a fórmula **OnSelect** do **Guardar o ícone do tópico** com a fórmula abaixo. Isto muda para
 fazer com que a API crie o “pergunte”.


    ![](images/L04/image%20(74).png)

    ```
    Set(returnGuid, PrioritZFunction. CreateTopic({
    tópico: 'Nome do tópico caixa de texto'. Texto,
    detalhes: 'Caixa de texto de detalhes tópico'. Texto,
    responder: 'respond by data selecker'.SelectedDate,
    myNotes: 'Notes textbox'.Texto,
    foto: JSON(AddTopicImage.Image, JSONFormat.IncludeBinaryData),
    escolhas: ShowColumns(colAddChoices, "choice", "foto")
    }));
    Notificar("Topic criado!" & returnGuid, NotificationType.Success, 5000);
    Costas();
    ```

12. Clique em **Guardar**.

1. Clique em **Publique**.

1. Selecione **Publique esta versão** e aguarde que a publicação seja concluída.

1. Não navegue por esta página.

### Tarefa 2: Aplicação de teste

1. Selecione o **Ecrã Esclareza** e clique em **Própria a aplicação**.

    ![](images/L04/image%20(75).png)

2. Clique no botão **+** adicionar.
3. Introduza **Teste de Função** para o Tópico, **Testando a função** para Detalhes. **Nota para testar a função** para
 Note que selecione uma data para Resposta por e clique em **Adicione uma image**.

    ![](images/L04/image%20(76).png)

4. Navegue para este caminho **C:\LabFiles** no ficheiro explorador de ficheiros, seleccione **image.png** e clique em abrir.

5. Introduza **Escolha de teste um** para Escolha e clique em **adicionar uma image**.
6. Navegue para este caminho **C:\LabFiles** no ficheiro explorer, seleccione **image.png** e clique em **+**.

    ![](images/L04/image%20(77).png)

7. Introduza **Teste de escolha duas** para Escolha e clique em **adicionar uma image**.

8. Navegue para este caminho **C:\LabFiles** no ficheiro explorer, seleccione **image.png** e clique em **+**.

9. Clique em **Guardar**.

    ![](images/L04/image%20(78).png)

10. O novo tópico deve ser guardado e deve ser navegado de volta para o ecrã principal.
11. Localize o novo tópico que criou e abra.

    ![](images/L04/image80.png)

12. Deve ver as duas opções que adicionou ao tópico.

    ![](images/L04/image81.png)

## Resumo

Neste laboratório, aprendeu a criar, implementar e publicar uma Função Azure, criar um conector para ele e testar a sua integração nas aplicações Power Platform.

## Concluiu o laboratório com sucesso
