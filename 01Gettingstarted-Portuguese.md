# Laboratório 01 - Introdução ao Power Apps

### Duração Estimada: 60 minutos

Neste laboratório, você configurará seu ambiente de desenvolvimento da Power Platform como parte da equipe de desenvolvimento fusion da Prioritz. Você começará importando e revisando os componentes da solução, incluindo aplicativos, fluxos e tabelas, para entender o estado atual da solução Prioritz. 

Em seguida, você aprimorará a solução adicionando uma nova coluna "My Notes" (Minhas Anotações) a uma tabela e atualizará o aplicativo Prioritz Admin para usar este novo campo. Finalmente, você verificará se o Visual Studio Code e a extensão da CLI da Power Platform estão instalados e usará a CLI para se conectar ao seu ambiente e listar as soluções. Ao concluir esses exercícios, você ganhará experiência prática com o gerenciamento de soluções, a personalização de aplicativos e as ferramentas de desenvolvedor no ecossistema da Power Platform.

## Objetivos do Laboratório

Você será capaz de concluir o seguinte:

- Exercício 1: Importar e revisar os componentes da solução
- Exercício 2: Adicionar uma coluna para "My Notes" (Minhas Anotações)
- Exercício 3: Verificar o Visual Studio Code e a extensão da CLI da Power Platform pré-instalados

## Exercício 1 - Importar e revisar os componentes da solução

Neste exercício, você importará a solução atual para o ambiente de desenvolvimento (dev) pré-criado e revisará seus componentes. Você também executará um fluxo que adicionará dados de exemplo ao seu ambiente e testará os aplicativos da solução.

>**Observação**: O ambiente Dev é já pré-criado como parte dos pré-requisitos.

### Tarefa 1: Importar, revisar os componentes da solução e executar o fluxo

1. Na LabVM, clique no atalho para o **portal do Power Apps** do navegador Microsoft Edge que está disponível na área de trabalho.

      ![azure portal.](images/L01/PAportal.png)

1. Na janela de login **Iniciar sessão**,insira o seguinte ** fornecido no e-mail de credenciais** **(1)** e clique em **Avançar** **(2)**.

   - E-mail/nome do usuário: <inject key="AzureAdUserEmail"></inject>

      ![](images/L01/cor2_1_g_1.png)

1. Agora, **Insira a senha** e clique em **Iniciar sessão**.

   - Senha: <inject key="AzureAdUserPassword"></inject>

      ![](images/L01/cor2_1_g_2.png)

1. No canto superior direito, clique em **Selecionar ambiente** **(1)** e selecione o ambiente de desenvolvimento pré-criado chamado **DEV_ENV_<inject key="Deployment ID" enableCopy="false" /> (2)**.

   ![](images/L01/DevEnv.png)

1. Agora, clique em **Soluções** **(1)** no menu do lado esquerdo e, em seguida, clique em **Importar solução** **(2)**.

   ![](images/L01/L1-T1-S16.png)

1. Na janela **Importar uma solução**, clique em **Procurar**  para selecionar o arquivo da solução.

   ![](images/L01/L1-T1-S17.png)

1. Navegue até o caminho `C:\LabFiles\Developer-in-a-day\Student\L01 - Getting started\Resources` **(1)** no explorador de arquivos, selecione o arquivo compactado **Prioritz_1_0_0_7.zip** **(2)** e clique no botão **Abrir** **(3)**.

   ![](images/L01/dv_p2_e1_g_6.png)

1. Certifique-se de que o arquivo **Prioritz_1_0_0_7.zip(1)** está selecionado e clique em **Avançar (2)**.

   ![](images/L01/L1-T1-S18.png)

1. Clique **Avançar** novamente no painel de importação da solução.

1. Na seção **Conexões**:
   - Clique no botão de reticências (...) ao lado de **Microsoft Dataverse Prioritz (1)**.
   - Certifique-se de que o e-mail do usuário **<inject key="AzureAdUserEmail"></inject> (2)** que você está usando está selecionado.
   - Clique em **Importar (3)**.

      ![](images/L01/L1-T1-S19.png)

1. Aguarde até que a importação da solução seja concluída.

   ![](images/L01/dv_p2_e1_g_10.png)

1. Agora você deve ver a solução que importou na lista de soluções. Abra a solução **Prioritz** que você importou.

   ![](images/L01/dv_p2_e1_g_11.png)

1. Expanda **Tabelas (1)** e selecione a tabela **PrioritZ Topic (2)**.

   ![](images/L01/L1-T1-S21.png)

1. Na seção **Esquema**, selecione o item **Colunas** da tabela **PrioritZ Topic**.

   >**Informação**: As colunas padrão são integradas e todas as tabelas as possuem. As colunas personalizadas foram criadas pela equipe para este aplicativo.

   ![](images/L01/L1-T1-S22.png)

1. No menu **Colunas** **(1)**, selecione **Relacionamentos** **(2)** e revise como esta tabela se relaciona com outras tabelas.

   ![](images/L01/L1-T1-S23.png)

   ![](images/L01/L1-T1-S24.png)

1. No lado esquerdo, em **Objetos**, selecione **Fluxos da nuvem** **(1)** e, em seguida, clique em **Import sample data - Topics** **(2)**.

   ![](images/L01/L1-T1-S25.png)

1. Clique no botão **Editar** para revisar o fluxo.

   ![](images/L01/L1-T1-S26.png)

1. Expanda a etapa **Parse JSON** e revise os dados que este fluxo criará.

   ![](images/L01/L1-T1-S27.png)

>**Observação:** Se não conseguir expandir a etapa, clique nas reticências (...), selecione Configurações e clique em Cancelar.

1. Expanda a etapa **Apply to each topic**.

   ![](images/L01/L1-T1-S28.png)

>**Observação:** Se não conseguir expandir a etapa, clique nas reticências (...), selecione Configurações e clique em Cancelar.

1. Expanda a etapa **Apply to each topic item**.

   ![](images/L01/L1-T1-S29.png)

1. A etapa **Apply to each** deve parecer com a imagem abaixo. Esta é a lógica da automação.

   ![](images/L01/L1-T1-S30.png)

1. Clique no botão **← Import sample data - Topics**.

   ![](images/L01/L1-T1-S31.png)

1. Na lista de **Fluxos da nuvem**, clique em **Import sample data - Topics**.

   ![](images/L01/L1-T1-S32.png)

1. Na página do fluxo **Import sample data - Topics**, clique em **Executar**.

   ![](images/L01/dv_p2_e1_g_15.png)

1. Clique no botão **Executar fluxo** no painel que se abrirá para iniciar a execução.

   ![](images/L01/dv_p2_e1_g_16.png)

   > **Observação**: Se você receber o erro `Error from the token exchange: Permission denied due to missing connection` ao executar o fluxo, isso ocorre porque a conexão do **Dataverse** não está sendo adicionada corretamente. Exclua a solução importada e tente importá-la novamente, realizando os Passos 6 a 14 desta tarefa. Em seguida, tente acionar o fluxo novamente.

1. Aguarde a conclusão da execução do fluxo e clique no botão **Concluído**.

   ![](images/L01/dv_p2_e1_g_17.png)

1. O fluxo deve ser executado com sucesso. Se desejar, verificar os detalhes da execução, clique em **Todas as execuções** e selecione a linha correspondente para visualizar as ações realizadas.

   ![](images/L01/dv_p2_e1_g_18.png)

   ![](images/L01/dv_p2_e1_g_19.png)

### Tarefa 2: Teste as aplicações

1. Navegue de volta para a solução **PrioritZ** clicando em **Fluxos da nuvem**. Em alternativa, também pode abrir o portal do **Power Apps** utilizando esta URL `https://make.powerapps.com` se ainda não está aberto. Certifique-se de que o ambiente de desenvolvimento denominado **DEV_ENV_<inject key="Deployment ID" enableCopy="false" />** é selecionado.

   ![](images/L01/dv_p2_e1_g_20.png)

   >**Observação:** se um pop-up **Bem-vindo ao Power Apps Studio** aparecer, basta clicar em **Ignorar** para prosseguir.

1. Para regressar ao separador **Soluções**, clique no botão **Voltar às soluções** na barra lateral esquerda.

   ![](images/L01/dv_p2_e1_g_21.png)

1. No menu de navegação à esquerda, selecione **Aplicativos** **(1)** e verifique se as aplicações **Prioritz Ask** e **Prioritz Admin** estão listadas **(2)**.

   >**Info:** A aplicação **PrioritZ Admin** é utilizada para gerir tópicos que estão a ser solicitados e a aplicação **PrioritZ Ask** permite que os utilizadores respondam.

   ![](images/L01/dv_p2_e1_g_23.png)

1. Para iniciar a aplicação **Prioritz Admin**, clique no ícone de execução localizado ao lado do nome da aplicação.

   ![](images/L01/dv_p2_e1_g_24.png)

1. Deve ver os quatro tópicos abaixo.

   ![](images/L01/EX1-T2-4-2.png)

1. Clique para abrir o tópico do banner **Event banner**.

   ![](images/L01/dv_p2_e1_g_25.png)

1. Deve ver os detalhes do tópico com alguns itens do tópico.

   ![](images/L01/EX1-T2-6-1.png)

1. Clique no botão **<** Voltar.

   > **Observação**: Deve voltar ao tela inicial.

1. Agora, clique no botão **+** para adicionar um novo tópico.

   ![](images/L01/image16.png)

1. Forneça as informações abaixo e clique em **add a picture** que está presente abaixo **Respond By**.

    - **Topic**: Digite: `Change Taco Tuesday to some other food`
    
    - **Details**: Digite: `People are tired of tacos, what should we have instead of tacos?`

1. **Responda By**: Selecione **today's date**.

      ![](images/L01/image17.png)

1. Navegue para este caminho `C:\LabFiles` no explorador de arquivos, seleccione **image.png** e clique em abrir.

1. Insira **Tamale Tuesday** no campo Choice e clique em **add a picture** que está presente abaixo do campo Choice.

      ![](images/L01/image18.png)

1. No explorador de arquivos, navegue até `C:\LabFiles` **(1)**, selecione o arquivo **image.png** **(2)** e clique no botão **Abrir** **(3)**.

      ![](images/L01/dv_p2_e1_g_28.png)

1. Clique em **+** para adicionar a escolha.

      ![](images/L01/image19.png)

1. Adicione mais algumas opções repetindo **os passos 12-14**.

    - **Choice 1**: Digite: `Steak Tuesday`

    - **Choice 2**: Digite: `Cheese and Wine Tuesday`

1. Clique no botão **Save** para salvar o tópico.

    ![](images/L01/image20.png)

1. O novo tópico deve ser guardado e deve ser navegado de volta para o tela principal.

1. Deve ver o tópico que adicionou à lista de tópicos.

    ![](images/L01/L01-taco.png)

1. Feche a aplicação PrioritZ Admin fechando o separador do navegador no qual a aplicação PrioritZ Admin está aberta.

1. No menu de navegação à esquerda, selecione **Aplicações** e clique no ícone de execução ao lado de **Prioritz Ask** para iniciar a aplicação.

    ![](images/L01/dv_p2_e1_g_31.png)

1. Deve ver uma lista de tópicos. Abra o ***Change Taco Tuesday to some other food** que criou nos passos anteriores.

    ![](images/L01/L01-list.png)

1. Clique em **up/down** ícones encomende os itens na ordem que os prefere e clique em **Vote**.

    ![](images/L01/L01-choice.png)

1. Deve ser navegado de volta para os telas principais e ver uma mensagem de notificação.

    ![](images/L01/dv_p2_e1_g_34.png)

1. Feche a aplicação PrioritZ Ask fechando o separador do navegador no qual a aplicação PrioritZ Ask está aberta.

> **Parabéns** por completar a tarefa! Agora, é hora de validá-lo. Aqui estão os passos:
> - Pressione o botão Validar para a tarefa correspondente. Se receber uma mensagem de êxito, pode prosseguir para a próxima tarefa. 
> - Se não, leia atentamente a mensagem de erro e tente novamente a etapa, seguindo as instruções no guia do laboratório.
> - Se precisar de ajuda, entre em contato conosco pelo cloudlabs-support@spektrasystems.com. Estamos disponíveis 24 horas por dia, 7 dias por semana para ajudar.

<validation step="43212171-0d6f-44d9-9c69-5061a4bb1b1c" />

## Exercício 2 – Adicione uma coluna para as minhas notas

Neste exercício, irá adicionar uma nova coluna **My Notes** à tabela de tópicos e atualizar o PriortZ Admin
aplicação.

### Tarefa 1: Adicione uma nova coluna

1. Navegue até ao portal do fabricante de aplicações Power, utilizando o URL abaixo, se ainda não estiver aberto. Certifique-se de que o ambiente de desenvolvimento denominado **DEV_ENV_<inject key="Deployment ID" enableCopy="false" /> (2)** é selecionado.
 
   ```
   https://make.powerapps.com
   ```

1. Selecione **Solutições (1)** do menu do lado esquerdo e abra a solução **PrioritZ (2)**.

    ![](images/L01/EX2-T1-2-1.png)

1. No painel **Objetos**, expanda **Tabelas** **(1)**, selecione a tabela **PrioritZ Topic** **(2)**, clique no botão **Criar** **(3)** e depois selecione **Coluna** em **Esquema** **(4)**.

    ![](images/L01/dv_p2_e1_g_35.png)

1. Digite o valor abaixo no campo de nomes de visualização.

   ```
   My Notes
   ```
   
1. No painel **Nova coluna**, defina o campo **Nome a apresentar** como **My Notes** **(1)**, pesquise por **Texto simples** **(2)**, selecione a opção **Várias linhas de texto** **(3)** e clique em **Guardar** **(4)**.

   ![](images/L01/dv_p2_e1_g_37.png)

   > **Observação**: Não navegue por esta página.

### Tarefa 2: Atualize a aplicação de administrador

1. No painel **Objetos**, selecione **Aplicativos** **(1)**. Na lista de aplicativos, selecione **Prioritz Admin** **(2)** e clique em **Editar** **(3)**.

    ![](images/L01/dv_p2_e1_g_38.png)

1. Na **Exibição em árvore**, selecione **Add Topic Screen** **(1)**. No menu superior, clique em **Inserir** **(2)** e selecione **Entrada de texto** **(3)**.

    ![](images/L01/cor_g_1.png)

1. Na **Exibição em árvore**, renomeie o campo recém-adicionado para **Notes textbox**.

   ```
   Notes textbox
   ```

    ![](images/L01/dv_port2_e1_g_5.png)

1. Reduza o tamanho do controle de imagem, se necessário, e mova a caixa de texto **Respond By** junto com seu rótulo para baixo. Insira a caixa de texto **Notes** entre o controle **Details** e o rótulo **Respond By**.

    ![](images/L01/image28.png)

1. Selecione o controle **Notes textbox**, abra o menu suspenso de propriedades **(1)** e escolha **HintText** **(2)** para editar o texto de sugestão.

    ![](images/L01/cor_g_2.png)

1. Altere o valor **HintText** da caixa de texto Notes para o valor abaixo.

   ```
   My notes
   ```

    ![](images/L01/cor_g_3.png)

1. Selecione o **Mode** no menu suspenso das propriedades e altere o seu valor introduzindo o texto abaixo.

   ```
   TextMode.MultiLine
   ```

    ![](images/L01/cor_g_4.png)

1. Na seção **Add Topic Screen**, selecione **Save topic icon**. Substitua a fórmula **OnSelect** do **Ícone Guardar Tópico** pela fórmula abaixo. O Patch cria uma nova linha na tabela Dataverse.

    ![](images/L01/cor_g_5.png)

      ```
      Set(newTopic,Patch('Prioritz Topics',Defaults('Prioritz Topics'),{'My Notes': 'Notes textbox'.Text,Topic:'Topic name textbox'.Text,Details:'Topic details textbox'.Text,'Respond By':'respond by date picker'.SelectedDate,Photo:AddTopicImage.Image}));ForAll(colAddChoices,Patch('Prioritz Topic Items',Defaults('Prioritz Topic Items'),{Choice:ThisRecord.choice,'PrioritZ Topic':newTopic,Photo:ThisRecord.photo}));Back()
      ```

1. No painel **Telas**, selecione **View Topic Screen** **(1)**, clique em **+ Inserir** **(2)** na barra superior e selecione **Rótulo de texto** **(3)** na lista de componentes populares.

      ![](images/L01/dv_port2_e1_g_6.png)

1. Clique duas vezes no rótulo recentemente adicionado e digite o valor abaixo para renomear o rótulo que acabou de adicionar.

      ```
      Notes label
      ```

    ![](images/L01/dv_port2_e1_g_7.png)

1. Altere o valor **Text** do rótulo Notes com o texto abaixo.

      ```
      'Topics gallery'.Selected.'My Notes'
      ```

      ![](images/L01/dv_port2_e1_g_8.png)

1. Rearranque os controlos e mova o rótulo **Notes** entre o rótulo de detalhes e os itens do tópico
 galeria.

    ![](images/L01/image33.png)

1. Selecione o **Home Screen** e clique em **Pré-visualizar aplicação**.

    ![](images/L01/image34.png)

1. Clique no botão **+** para adicionar um novo tópico.

    ![](images/L01/L01-taco-1_1.png)

1. Preencha o formulário fornecendo as informações abaixo e clique em **add a picture** que está presente abaixo do campo **Respond By**.

      - Tópico: **Test Notes.**

      - Detalhes: **Testing the notes.**

      - Entrada de texto: **Prioritz Admin topic.**

      - Responda por: **Today's date.**

1. Navegue para este caminho C:\LabFiles no explorador de arquivos, seleccione **image.png** e clique em abrir.

1. Digite **Test One** no campo Choice e clique em **add a picture** que esteja presente abaixo do campo Choice.

    ![](images/L01/image18.png)

1. No explorador de arquivos, navegue até `C:\LabFiles` **(1)**, selecione o arquivo **image.png** **(2)** e clique no botão **Abrir** **(3)**.

      ![](images/L01/dv_p2_e1_g_28.png)

1. Clique em **+** para adicionar a escolha.

    ![](images/L01/image19.png)

1. Adicione mais uma escolha repetindo **passos 20-22** desta tarefa.

    1. **Choice 1**: Digite: `Test Two`

1. Depois de adicionar todas as opções e detalhes do tópico, o seu tela deve corresponder à imagem abaixo.

   ![](images/L01/L01-testnotes.png)

1. Agora, clique no botão **Save**. O novo tópico deve ser **salvado**.

1. Clique para abrir o tópico **Test Notes** que acabou de criar.

1. As notas **Prioritz Admin topic** que adicionou anteriormente devem ser visíveis agora.

    ![](images/L01/image36.1.png)

1. Feche a aplicação **preview**.

1. Clique em **Publicar**.

   ![](images/L01/dv_port2_e1_g_10.png)

1. Selecione Publicar esta versão e aguarde que a publicação seja concluída.

    ![](images/L01/dv_port2_e1_g_11.png)

1. Pode fechar o **app designer**.

## Exercício 3 – Teste o CLI da Power Platform

Neste exercício, você irá revisar e testar a extensão da CLI da Power Platform no Visual Studio Code.

>**Observação**: A instalação do Visual Studio Code e da CLI da Power Platform já foi realizada como parte dos pré-requisitos.

1.  Navegue até ao centro de administração do Power Platform utilizando o URL abaixo e selecione **Ambientes**.
 
      ```
      https://admin.powerplatform.microsoft.com/environments
      ```

1. Se aparecer o pop-up **Permanecer conectado?**, clique em **Não**.

1. Clique para abrir o seu ambiente de desenvolvimento chamado **DEV_ENV_<inject key="Deployment ID" enableCopy="false" />**.

1. Clique com o botão direito na **URL** do Ambiente, copie o valor e cole-o no Bloco de Notas.

   >**Observação**: a: Certifique-se de que o valor da URL do Ambiente seja copiado juntamente com o **https**. Seu valor copiado deve ser semelhante a este:`https://orgxxxxxx.crm.dynamics.com/`.

   ![](images/L01/dv_port2_e1_g_13.png)

1. Na LabVM, inicie o **Visual Studio Code** usando o atalho disponível na área de trabalho.

   ![](images/L04/vscode1.png)

1. Clique nas Reticências (...) **(1)**, em **Terminal** **(2)** e depois clique em **Novo Terminal** **(3)**.

   ![](images/L01/dv_port2_e1_g_14.png)

1. Execute o comando abaixo no terminal.

   ```
   pac
   ```

   > **Info:** Se você encontrar um erro após usar o comando `pac`, baixe a CLI da Power Platform do link `https://aka.ms/PowerAppsCLI`, abra o instalador e conclua a instalação. Em seguida, tente o passo novamente.

1. Substitua `<a sua URL de ambiente>` no comando abaixo pelo valor da URL do ambiente que você copiou anteriormente e, em seguida, execute o comando.

   ```
   pac auth create --name DevAuth --url <your environment URL>
   ```

   > **Observação:** Após adicionar a URL do ambiente, o comando ficará assim: `pac auth create --name DevAuth--url https://org32172839283.crm.dynamics.com/`

   ![](images/L01/dv_port2_e1_g_15.png)

1. Conclua o processo de **Login**, usando as credenciais abaixo.

   - E-mail/Usuário: <inject key="AzureAdUserEmail"></inject>
   
   - Senha: <inject key="AzureAdUserPassword"></inject>

      >**Observação:** Se um Aviso de Segurança do Windows aparecer, clique em **Sim** para prosseguir.

   ![](images/L01/cor2_1_g_3.png)

1. Selecione **Power Platform (1)**. Agora você deve ter pelo menos um **perfil de autenticação (2)**. Se tiver mais de um perfil, certifique-se de que o perfil que você criou está selecionado.

   ![](images/L01/dv_port2_e1_g_16.png)

   > **Observação**: Se você estiver vendo o  **Perfil Universal** em vez do perfil de **DeVAuth**, é porque adicionou o valor incorreto da URL do Ambiente no comando `pac auth create` no Passo 8. Para corrigir esse problema, siga os passos abaixo:

   - Exclua o **Perfil Universal** do Visual Studio Code clicando no botão de exclusão.

   - Copie o valor correto da **URL do Ambiente** seguindo o **Passo 4** desta tarefa.
   
   - Execute o **Passo 8** desta tarefa novamente para criar o perfil de autenticação.

1. Clique nas **Reticências (...) (1)**, em **Terminal (2)** e selecione **Novo Terminal (3)**, se ainda не estiver aberto.

    ![](images/L01/image42.png)

1. Execute o comando abaixo para ver uma lista de soluções.

      ```
      pac solution list
      ```

1. Você deverá ver uma lista de soluções instaladas em seu ambiente.

    ![](images/L01/dv_port2_e1_g_17.png)

## Resumo

Neste laboratório, você aprendeu a importar e executar uma solução inicial, personalizá-la adicionando uma nova coluna e atualizando o aplicativo de administração, e a verificar a funcionalidade usando a CLI da Power Platform.

## Concluiu o laboratório com sucesso. Prossiga para a próxima página.

![](./images/NextPage.png)
