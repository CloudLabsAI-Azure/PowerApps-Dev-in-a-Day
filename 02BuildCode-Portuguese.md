# Laboratório 02 - Construir um componente de código

## Duração estimada: 100 minutos

Neste laboratório, você construirá um componente de código personalizado usando o framework JavaScript React para permitir a classificação de prioridade de itens com a funcionalidade de arrastar e soltar (*drag-and-drop*) no aplicativo "PrioritZ Ask Power App". Você começará criando e configurando o componente de código, depois implementará sua lógica e o testará localmente. Em seguida, você integrará o componente ao aplicativo de tela "PrioritZ Ask", permitindo que os usuários reordenem os itens visualmente. Por fim, você adicionará o componente de código finalizado à solução "PrioritZ", garantindo que ele esteja disponível para uso em seu ambiente da Power Platform. Ao concluir esses exercícios, você ganhará experiência prática no desenvolvimento, implantação e uso de componentes de código personalizados em Power Apps.

## Objectivos do laboratório

- Exercício 1: Construir o componente de código 
- Exercício 2: Usar o componente de código 
- Exercício 3: Adicionar o componente de código à solução 

## Exercício 1 - Construir o componente de código

Neste exercício, você irá construir o componente de código.

### Tarefa 1: Criar o componente de código

1. Inicie o **Visual Studio Code** usando o atalho disponível na área de trabalho, se ainda não estiver aberto.

   ![](images/L04/vscode1.png)
   
1. Selecione a guia **Power Platform (1)** e certifique-se de que o seu perfil **DEV_ENV Auth (2)** está selecionado. 
    
   >**Observação:** A guia do Power Platform já está instalado.
    
    ![](images/L02/dv_port2_e1_g_16.png)

1. Clique no **menu de reticências ...(1)** para expandir opções adicionais. Em seguida, selecione **Terminal** **(2)** no menu suspenso e clique em **Novo Terminal** **(3)** para abrir uma sessão de terminal.

   ![](images/L01/dv_port2_e1_g_14.png)

1. Na janela Terminal, crie um novo diretório executando o comando abaixo.

    ```
    md PrioritZDnDRanking
    ```
1. Execute o comando abaixo para mudar para o diretório PrioritZDnRanking que você criou.

    ```
    cd PrioritZDnDRanking
    ```
1. Agora você deve estar no diretório que criou. Crie um novo projeto de componente e instale as dependências executando o comando abaixo.
    
     ```
     pac pcf init -ns ContosoCoffee --name PrioritZDnDRanking --template dataset --framework react --run-npm-install
     ```
     
     ![](images/L02/a_g_d_1.png)

>**Observação:** Nota: O comando acima pode levar de 2 a 3 minutos para ser concluído.

1. O projeto do framework do componente deve ser criado com sucesso.

    ![](images/L02/a_g_d_2.png)

>**Observação:** Ao instalar as dependências, você pode ver avisos como: `npm WARN deprecated @babel/plugin-proposal-object-rest-spread`, `npm WARN deprecated rimraf@3.0.2`, `npm WARN deprecated eslint@8.57.1`. Esses avisos significam que alguns pacotes estão desatualizados ou não são mais atualizados. Você pode ignorá-los com segurança, pois não afetarão o funcionamento do projeto.

1. Execute o comando abaixo para abrir o projeto.
    
    ```
    code -a.
    ```

1. Se a janela pop-up abaixo for apresentada, clique em **Sim** para confiar nos autores dos arquivos.
   
1. Revise os arquivos do componente de código criados selecionando a guia **Explorador**.
    
     ![](images/L02/dv_p3_e2_g_7.png)

1. Expanda a pasta **PrioritZDnDRanking (1)** e, em seguida, expanda a subpasta **generated**.

1. Abra o arquivo **ControlManifest.Input.xml (2)**. O manifesto é o arquivo de metadados que define um componente, incluindo as propriedades expostas ao aplicativo hospedeiro.

    ![](images/L02/dv_p3_e2_g_8.png)

1. Localize o elemento XML **data-set** na **linha número 21** do arquivo **ControlManifest.Input.xml**.

    ![](images/L02/image7.png)

1. Altere o **name** para **items** e o **display-name-key** para **items**. Isto define a propriedade à qual o aplicativo se vinculará para uma coleção de itens.

    ![](images/L02/image8.png)

1. Adicione as seguintes propriedades entre a tag de fechamento do elemento conjunto de dados `(</data-set>)` e a tag de abertura do elemento `<resources>`.

    > Adicione as seguintes propriedades após **linha 26** no arquivo **ControlManifest.Input.xml**.

    ```
    <property name="BackgroundColor" display-name-key="Background color" usage="input" of-type="SingleLine.Text" default-value="#F3F2F1"/>
    <property name="DragBackgroundColor" display-name-key="Drag background color" usage="input" of-type="SingleLine.Text" default-value="lightgreen"/>
    <property name="ItemHeight" display-name-key="Item height" usage="input" of-type="Whole.None" default-value="32"/>
    <property name="FontSize" display-name-key="Font size" usage="input" of-type="Whole.None" default-value="12"/>
    <property name="FontColor" display-name-key="Font color" usage="input" of-type="SingleLine.Text" default-value="#333333"/>
    ```

    ![](images/L02/image9.png)

1. Localize a secção `<resources>` e adicione o código abaixo após o **code path** para adicionar o recurso **css**. Isto garantirá que os nossos estilos sejam incluídos no pacote do componente de código quando este for implantado.

    ```
    <css path="css/PrioritZDnDRanking.css" order="1" />
    ```

    ![](images/L02/image10.png)

    >**Observação:** Certifique-se de que não remover os comentários do **resx path**, pois você enfrentará um problema na próxima tarefa ao construir o componente de código se ele estiver sem comentários.

1. Observe os dois recursos seguintes. Isso declara a dependência do componente nessas duas bibliotecas, resultado de especificar `–framework React` na inicialização.
        
     ```
     <platform-library name="React" version="16.8.6" />
     <platform-library name="Fluent" version="8.29.0" />
     ```
     
     ![](images/L02/image11.png)

1. Clique em **Arquivo** e selecione **Salvar Tudo** para salvar suas alterações.

1. Certifique-se de que o arquivo **ControlManifest.Input.xml** ainda está selecionado e clique em **Nova Pasta**.

    ![](images/L02/image12.png)

1. Nomeie a nova pasta como **css**.

1. Selecione a nova pasta **css** que criou e clique em **Novo Arquivo**

    ![](images/L02/image13.png)

1. Nomeie o novo arquivo como **PrioritZDnDranking.css**.

1. Cole o seguinte CSS no arquivo **PrioritZDnDRAnking.css**.

    ```
    .prioritydnd-scroll-container {
    box-sizing: border-box;
    padding: 2px;
    overflow-y: auto;
    overflow-x: hidden;
    position: relative;
    }
    .prioritydnd-item-container {
    user-select: none;
    display: flex;
    align-items: center;
    }
    .prioritydnd-item-column {
    margin: 8px;
    }
    ```
1. O arquivo agora deve ter a seguinte aparência:

    ![](images/L02/image14.png)

1. Clique em **Arquivo** e selecione **Salvar Tudo** para salvar suas alterações.

### Tarefa 2: Implementar a lógica do componente

1. Selecione o arquivo de componente **HelloWorld.tsx**, clique com o botão direito sobre ele e selecione **Excluir** para remover o arquivo, pois ele é criado automaticamente e não o utilizaremos.

    ![](images/L02/dv_p3_e2_g_13.png)

1. No explorador de arquivos, navegue até o caminho `C:\LabFiles\Developer-in-a-day\Student\L02 - Build a code component\Resources`.

1. Arraste o arquivo **PriorityComponent.tsx** e solte-o na pasta **PrioriZDnDranking**.

1. O arquivo **PriorityComponent.tsx** agora deve estar na pasta **PrioriZDnDranking**.

    ![](images/L02/image16.png)

1. Clique em **Arquivo** e salve suas alterações.

1. Abra o arquivo **PriorityComponent.tsx** e revise o conteúdo. Ele implementa o componente React que será renderizado para representar nossos itens arrastáveis.

1. Observe que a linha 9 `from react-beautiful-dnd`, tem um sublinhado vermelho. Trata-se de um pacote npm utilizado pelo componente, mas que ainda não foi referenciado.

    ![](images/L02/image17.png)

1. Execute o seguinte comando em uma janela de terminal para adicionar uma referência ao react-beautiful-dnd.

    ```
    npm install react-beautiful-dnd
    ```

    >**Observação:** Se você receber este erro **npm is not recognised**, siga os passos abaixo:

    - Abra o PowerShell e execute este comando `choco install -y --force nodejs`.
    
    - Quando a execução do comando terminar, feche e reabra o Visual Studio Code.
    
    - Execute o **Passo 8** desta tarefa novamente para instalar o pacote **react-beautiful-dnd**.

1. Execute o seguinte comando para as definições de tipo (*type definitions*).

    ```
    npm i --save-dev @types/react-beautiful-dnd
    ```

1. Observe que o sublinhado vermelho na linha 9 foi resolvido.

1. Abra o arquivo **index.ts**.

1. Remova a seguinte linha (linha 2 no arquivo Index.ts), pois não estamos mais usando o `HelloWorld`.

    ```
    import { HelloWorld, IHelloWorldProps } from "./HelloWorld";
    ```

    ![](images/L02/image18.png)

1. Adicione o código abaixo ao arquivo **index.ts** após a **linha 1**. Isso fará referência ao `PriorityComponent`.

     ```
     import { PriorityComponent, PriorityComponentProps } from './PriorityComponent';
     ```
    
     ![](images/L02/image19.png)

1. Localize a classe **Export** na **linha 5**.

     ![](images/L02/image20.png)

1. Adicione o código abaixo dentro da classe **export**. Isso define algumas variáveis ​​de trabalho que você usará na lógica da classe.

     ```
     private context: ComponentFramework.Context<IInputs>;
     private items: ComponentFramework.PropertyTypes.DataSet;
     private state: ComponentFramework.Dictionary;
     ```
            
     ![](images/L02/image21.png)

1. Localize a função **init** e remova a seguinte linha de código:

     ![](images/L02/init.png)

1. Cole o código abaixo dentro da função **init**. Esta lógica inicializa nossas variáveis de classe a partir dos valores de tempo de execução e habilita a notificação de redimensionamento.

     ![](images/L02/init1.png)

     ```
     this.context = context;
     context.mode.trackContainerResize(true);
     ```

1. Localize a função **updateView**.

    ![](images/L02/imageUpdateView.png)

1. Substitua a função **updateView** pela função abaixo. Esta lógica cria o Elemento React a partir do `PriorityComponent` e o adiciona ao DOM virtual.

    ```   
    public updateView(context: ComponentFramework.Context<IInputs>): React.ReactElement {
        const dataset = context.parameters.items;
        return React.createElement(PriorityComponent, {
            width: context.mode.allocatedWidth,
            height: context.mode.allocatedHeight,
            itemHeight: context.parameters.ItemHeight.raw,
            fontSize: context.parameters.FontSize.raw,
            fontColor: context.parameters.FontColor.raw,
            dataset: dataset,
            onReorder: this.onReorder,
            backgroundColor: this.context.parameters.BackgroundColor.raw,
            dragBackgroundColor:
            this.context.parameters.DragBackgroundColor.raw,
        } as PriorityComponentProps);
    }
    ```

    ![](images/L02/image24.png)

1. Adicione o código abaixo após a função **destroy**. Esta lógica lida com o evento `onReorder` do `PriorityComponent` e identifica os itens envolvidos para o aplicativo hospedeiro como itens selecionados.

    ```
    onReorder = (sourceIndex: number, destinationIndex: number): void => {
    const dataset = this.context.parameters.items;
    const sourceId = dataset.sortedRecordIds[sourceIndex];
    const destinationId = dataset.sortedRecordIds[destinationIndex];
    // raise the OnSelect event
    this.context.parameters.items.openDatasetItem(dataset.records[sourceId].getNamedReference());
    // set the SelectedItems property
    this.context.parameters.items.setSelectedRecordIds([sourceId, destinationId]);
    };
    ```

    ![](images/L02/image25.png)

    > **Observação:**  A função **Destroy** estará no final da classe **PrioritZDnDranking**.

1. Após concluir todos os passos, para evitar erros, substitua o código existente em seu arquivo `index.ts` pelo código a seguir. Em seguida, pressione `CTRL + S` para salvar o arquivo:

    ```
    import { IInputs, IOutputs } from "./generated/ManifestTypes";
    import { PriorityComponent, PriorityComponentProps } from './PriorityComponent';
    import * as React from "react";

    export class PrioritZDnDRanking implements ComponentFramework.ReactControl<IInputs, IOutputs> {
        private context: ComponentFramework.Context<IInputs>;
        private items: ComponentFramework.PropertyTypes.DataSet;
        private state: ComponentFramework.Dictionary;
        private theComponent: ComponentFramework.ReactControl<IInputs, IOutputs>;
        private notifyOutputChanged: () => void;

        /**
        * Empty constructor.
        */
        constructor() { }

        /**
        * Used to initialize the control instance. Controls can kick off remote server calls and other initialization actions here.
        * Data-set values are not initialized here, use updateView.
        * @param context The entire property bag available to control via Context Object; It contains values as set up by the 
        * customizer mapped to property names defined in the manifest, as well as utility functions.
        * @param notifyOutputChanged A callback method to alert the framework that the control has new outputs ready to be retrieved 
        * asynchronously.
        * @param state A piece of data that persists in one session for a single user. Can be set at any point in a controls life 
        * cycle by calling 'setControlState' in the Mode interface.
        */
        public init(
            context: ComponentFramework.Context<IInputs>,
            notifyOutputChanged: () => void,
            state: ComponentFramework.Dictionary
        ): void {
            this.context = context;
            context.mode.trackContainerResize(true);
            this.notifyOutputChanged = notifyOutputChanged;
        }

        /**
        * Called when any value in the property bag has changed. This includes field values, data-sets, global values such as container 
        * height and width, offline status, control metadata values such as label, visible, etc.
        * @param context The entire property bag available to control via Context Object; It contains values as set up by the customizer 
        * mapped to names defined in the manifest, as well as utility functions.
        * @returns ReactElement root react element for the control
        */
        public updateView(context: ComponentFramework.Context<IInputs>): React.ReactElement {
            const dataset = context.parameters.items;
            return React.createElement(PriorityComponent, {
                width: context.mode.allocatedWidth,
                height: context.mode.allocatedHeight,
                itemHeight: context.parameters.ItemHeight.raw,
                fontSize: context.parameters.FontSize.raw,
                fontColor: context.parameters.FontColor.raw,
                dataset: dataset,
                onReorder: this.onReorder,
                backgroundColor: this.context.parameters.BackgroundColor.raw,
                dragBackgroundColor: this.context.parameters.DragBackgroundColor.raw,
            } as PriorityComponentProps);
        }

        /**
        * It is called by the framework prior to a control receiving new data.
        * @returns an object based on nomenclature defined in manifest, expecting object[s] for property marked as "bound" or "output"
        */
        public getOutputs(): IOutputs {
            return { };
        }

        /**
        * Called when the control is to be removed from the DOM tree. Controls should use this call for cleanup.
        * i.e. cancelling any pending remote calls, removing listeners, etc.
        */
        public destroy(): void {
            // Add code to cleanup control if necessary
        }

        onReorder = (sourceIndex: number, destinationIndex: number): void => {
            const dataset = this.context.parameters.items;
            const sourceId = dataset.sortedRecordIds[sourceIndex];
            const destinationId = dataset.sortedRecordIds[destinationIndex];
            // raise the OnSelect event
            this.context.parameters.items.openDatasetItem(dataset.records[sourceId].getNamedReference());
            // set the SelectedItems property
            this.context.parameters.items.setSelectedRecordIds([sourceId, destinationId]);
        };
    } 
    ```
 
1. Abra o arquivo **package.json**.

1. Localize o objeto JSON **dependencies**.

    ![](images/L02/image26.png)

1. Substitua **dependencies** pelo JSON abaixo. Em seguida, pressione **CTRL + S** para salvar o arquivo.

    ```
    "dependencies": {
    "@fluentui/react": "8.29.0",
    "eslint-config-prettier": "^8.5.0",
    "eslint-plugin-prettier": "^4.0.0",
    "eslint-plugin-react": "^7.29.4",
    "eslint-plugin-react-hooks": "^4.4.0",
    "eslint-plugin-sonarjs": "^0.13.0",
    "prettier": "^2.6.1",
    "react": "16.8.6",
    "react-beautiful-dnd": "^13.1.0",
    "react-dom": "16.8.6"
    },
    ```

1. Navegue até o arquivo **.eslintric.json(1)** na navegação à esquerda para adicionar a nova regra de lint. Localize **rules(2)** na **linha 22** e substitua o código seguindo as regras especificadas abaixo.

    ```
    "no-unused-vars": ["off"],
    "no-undef": ["off"]
    ```

    ![](images/L02/eslint.png)

1. Clique em **Arquivo** e salve todas as suas alterações.

1. Clique nas **Reticências (...)** **(1)**, em **Terminal** **(2)** e selecione **Novo Terminal** **(3)**, se ainda não estiver aberto.

   ![](images/L01/dv_port2_e1_g_14.png)

1. Execute o comando abaixo. Isso irá construir o seu componente e identificará quaisquer problemas.

    ```
    npm run-script build
    ```

    > **Observação:** Se a operação de compilação falhar com o erro **`Root element is missing`**, certifique-se de que o **resx path** está comentado no arquivo `ControlManifest.Input.xml` e tente construir o componente novamente.

1. O *build* deve ser bem-sucedido. Se houver algum erro, resolva-os antes de prosseguir.

     ![](images/L02/image28.png)

1. Execute o comando abaixo para iniciar o ambiente de teste.

    ```
    npm start
    ```

1. O ambiente de teste deve iniciar; caso contrário, copie o endereço e cole-o em uma nova janela do navegador Edge. Tente arrastar os itens e veja se o comportamento funciona como esperado.

    ![](images/L02/imagee29u.png)

    > **Observação:**  Se você receber um pop-up do Internet Explorer, feche-o, copie a URL `localhost` e cole-a em uma nova aba no Edge.
    > **Observação:** Se o ambiente de teste não iniciar como esperado e você não conseguir ver o resultado esperado, verifique se seguiu as instruções anteriores e adicionou o código corretamente nos arquivos **Manifest** e **Index**.

1. Feche o ambiente de teste fechando a aba do navegador.

1. Pare a execução pressionando a tecla **[CONTROL]** + **C**.

1. Digite **Y** e pressione [ENTER].

    ![](images/L02/image30.png)

1. Execute o comando abaixo para enviar o componente para o seu ambiente.
    
    ```
    pac pcf push --publisher-prefix contoso
    ```

 > **Notas de Solução de Problemas:** 
    
    > 1. Se você encontrar a mensagem de erro **"Error: Missing required tool: MSBuild.exe/dotnet.exe. Please add MSBuild.exe/dotnet.exe"**, siga estes passos:
        
    > - Navegue até o diretório `C:\LabFiles` e abra **dotnet-sdk-8.0.100-win-x64**.
           
    > - Na janela de configuração, selecione **Repair** e aguarde a conclusão.

    > - Após o reparo, feche e reabra o Visual Studio Code, execute o comando de `build` do passo 28 e, em seguida, execute o comando push novamente.

    > - If the issue persists, uninstall **dotnet-sdk-8.0.100-win-x64** and install the latest version from [the .NET download page](https://dotnet.microsoft.com/en-us/download). Close Visual Studio Code, reopen it, run the build command from step 28, and then execute the above command again.

    > 2. Se o erro de **push** **Sorry, the app encountered a non-recoverable error...** ocorrer, certifique-se de que seguiu as instruções anteriores e adicionou o código corretamente nos arquivos **Manifest* e **Index**. Você pode encontrar os arquivos de referência em `C:\LabFiles` para comparar seu código.

    > 3. Se a execução falhar com um erro de pacote Nuget, execute o comando abaixo no PowerShell e tente o comando `push` novamente:
    
        ```
        dotnet nuget add source https://api.nuget.org/v3/index.json -n nuget.org --configfile $env:APPDATA\NuGet\NuGet.Config
        ```
1. Aguarde a solução ser importada e publicada em seu ambiente.

    ![](images/L02/image31uu.png)

### Tarefa 3: Confirme que o controle foi adicionado ao ambiente

1. Navegue até o portal de criação do Power Apps usando a URL abaixo, se ainda não estiver aberto. Certifique-se de que o ambiente de desenvolvimento chamado **DEV_ENV_<inject key="Deployment ID" enableCopy="false" /> (2)** esteja selecionado.

    ```
    https://make.powerapps.com/
    ```

1. No painel de navegação à esquerda, selecione **Soluções** **(1)** e abra a solução **PowerAppsTools_contoso** **(2)**.

    ![](images/L02/dv_p3_e2_g_17.png)

1. Clique em **Tudo** e confirme que o controle personalizado está nesta solução.

    ![](images/L02/cor_g_6.png)

> **Parabéns** por concluir a tarefa! Agora, é hora de validá-la. Aqui estão os passos:
> - Pressione o botão Validar para a tarefa correspondente. Se receber uma mensagem de êxito, pode prosseguir para a próxima tarefa. 
> - Se não, leia atentamente a mensagem de erro e tente novamente a etapa, seguindo as instruções no guia do laboratório.
> - Se precisar de ajuda, entre em contato conosco pelo cloudlabs-support@spektrasystems.com. Estamos disponíveis 24 horas por dia, 7 dias por semana para ajudar.

<validation step="aae02cab-a129-491c-b38b-29c66f2f2547" />

## Exercício 2 – Usar o Componente de Código

Neste exercício, você usará o componente de código que criou na aplicação de tela **PrioritZ Ask**.

### Tarefa 1: Permitir o framework de componentes do Power Apps

Nesta tarefa, você permitirá a publicação de aplicativos de tela com componentes de código em seu ambiente.

1. Navegue até ao centro de administração do Power Platform utilizando a URL abaixo e selecione ambientes.

    ```
    https://admin.powerplatform.microsoft.com/environments
    ```

1. Abra o ambiente de desenvolvimento chamado **DEV_ENV_<inject key="Deployment ID" enableCopy="false" />** que você está usando para este laboratório.

    ![](images/L02/dv_p3_e2_g_18.png)

1. Clique em **Configurações** no menu superior.

    ![](images/L02/dv_p3_e2_g_19.png)

1. Na página **Configurações**, expanda a seção **Produto** **(1)** e clique em **Recursos** **(2)** para visualizar as opções disponíveis.

    ![](images/L02/dv_p3_e2_g_20.png)

1. Ative a opção **Permitir a publicação de aplicativos de tela com componentes de código**. Role para baixo e clique em  deslizando o botão para **Ativado** **(1)** e clique em **Salvar** **(2)** para aplicar as alterações.

    ![](images/L02/dv_p3_e2_g_21.png)

>> COntinuar aqui !!!
 ### Tarefa 2: Editar aplicação em tela

Nesta tarefa, irá editar a aplicação PrioritZ Ask em tela para utilizar o componente de código que criou.

1. Navegue até ao portal do fabricante de aplicações Power, utilizando o URL abaixo, se ainda não estiver aberto. Certifique-se de que o ambiente de desenvolvimento denominado **DEV_ENV_<inject key="Deployment ID" enableCopy="false" />** é selecionado.

    ```
    https://make.powerapps.com/
    ```

1. No menu de navegação à esquerda, clique em **Soluções** **(1)** e selecione a solução **Prioritz** **(2)** para abri-la.

    ![](images/L02/dv_p3_e2_g_22.png)

1. No painel **Objetos**, selecione **Aplicações (1)**. Em seguida, clique na aplicação **Prioritz Ask (2)** e selecione o botão **Editar (3)** na parte superior.

    ![](images/L02/dv_p3_e2_g_23.png)

1. No separador **Componentes (1)**, clique no ícone de seta para trás **(2)** para importar componentes.

    ![](images/L02/dv_p3_e2_g_24.png)

1. No separador **Código (1)**, selecione o componente **PrioritZDnDRanking (2)** e clique em **Importar (3)**.

    ![](images/L02/dv_p3_e2_g_25_cor.png)

1. Selecione o separador **Ecras**.

1. Expanda **Vote Screen (1)** e selecione **Votes gallery (2)**.

    ![](images/L02/dv_p3_e2_g_26.png)

1. Selecione a **Width** do menu suspenso das propriedades.

    ![](images/L02/L02-votescreen1u.png)

1. Defina o valor de **Width** da galeria Votes para **570**.

    ![](images/L02/L02-widthu.png)

1. O ecrã deve agora parecer a image abaixo.

    ![](images/L02/image40u.png)

1. Selecione o **Votes Screen** e clique em **+ Insert**.

    ![](images/L02/image41u.png)

1. Selecione o componente **PrioritZDnDranking** em **Componentes de codigo**.

    ![](images/L02/dv_p3_e2_g_27.png)

1. Vá até à aba de **Vista de árvore** e selecione o item **PrioritZDnDRanking** que acabou de adicionar.

1. Defina o valor **Items** do componente **PrioritZDnDRAnking** para a fórmula abaixo.

    ```
    'Votes gallery'.AllItems
    ```

    ![](images/L02/dv_p3_e2_g_28.png)

1. Selecione o **PrioritZDnDRanking**, no painel **Propriedades** à direita, vá ao separador **Apresentar (1)**, defina o valor de **Item height** como **160 (2)** e clique em **Editar (3)**.

    ![](images/L02/cor_g_7.png)

1. Clique em **+ Add field** para adicionar um novo campo.

1. Selecione **Rank (1)** e clique em **Add (2)**.

    ![](images/L02/cor_g_8.png)

1. A classificação deve agora mostrar-se no controlo, mas está resolvido descendente.

1. Selecione a galeria **Votes**, selecione a propriedade **Items** do menu suspenso da propriedade e altere a ordem de classificação para **Ascending**.

1. A classificação deve agora classificar ascendente.

1. Selecione o **Pcomponente rioritZDnDRAnking** e depois **X** propriedade do menu suspenso da propriedade.

    ![](images/L02/image47.1.png)

1. Defina o valor **X** do componente **PrioritZDnDRAnking** na fórmula abaixo.

    ```
    'Votes gallery'.Width
    ```

1. Selecione a propriedade **Width** do componente **PrioritZDnDRAnking** do menu suspenso da propriedade e defina o seu valor para **60**.

    ![](images/L02/a_g_u_1.png)

1. Selecione a propriedade **Height** do componente **PrioritZDnDRAnking** do menu suspenso da propriedade e defina o seu valor com a fórmula abaixo.

    ```
    'Votes gallery'.Height
    ```

    ![](images/L02/a_g_u_2.png)

1. Selecione a propriedade **ItemHeight** do componente **PrioritZDnDRAnking** do suspensão da propriedade e defina o seu valor com a fórmula abaixo

    ```
    'Votes gallery'.TemplateHeight
    ```

    ![](images/L02/a_g_u_3.png)

1. Selecione a propriedade **BackgroundColor** do componente **PrioritZDnDRAnking** do menu suspenso da propriedade e defina o seu valor para **"#A70202"**

    ![](images/L02/a_g_u_5.png)

1. Selecione a propriedade **DragBackgroundColor** do componente **PrioritZDnDranking** do menu suspenso da propriedade e defina o seu valor para **"LightBlue"**

    ![](images/L02/a_g_u_4.png)

1. Selecione a propriedade **Y** do componente **PrioritZDnDRAnking** do suspensão da propriedade e defina o seu valor com a fórmula abaixo.

    ```
    'Votes gallery'.Y
    ```

1. Selecione a propriedade **OnSelect** do componente **PrioritZDnDRAnking** do suspensão da propriedade e defina o seu valor com a fórmula abaixo.

    ```
    With(
        {
            sourceRank: First(Self.SelectedItems).Rank,
            destinationRank: Last(Self.SelectedItems).Rank
        },
        If(
            sourceRank < destinationRank,
     // Moving Up
            UpdateIf(
                colVotes,
                Rank >= sourceRank && Rank <= destinationRank,
                {
                    Rank: If(
                        Rank <> sourceRank,
                        Rank - 1,
                        destinationRank
                    )
                }
            );
        );
        If(
            sourceRank > destinationRank,
     // Moving Down
            UpdateIf(
                colVotes,
                Rank >= destinationRank && Rank <= sourceRank,
                {
                    Rank: If(
                        Rank <> sourceRank,
                        Rank + 1,
                        destinationRank
                    )
                }
            );
        );

    );
    ```

1. Selecione o **Home Screen** e clique em **Play**.

1. Selecione um dos **topics**.

1. Pode visualizar como está no ecrã de um telefone utilizando o emulador.

    ![](images/L02/dv_p3_e2_g_36.png)

1. Arraste um dos itens do tópico e deixe-o ativá-lo num local diferente.

    ![](images/L02/image49.png)

1. O arrastar/desembarque deve funcionar como esperado.

1. Feche a antevisão.

1. Clique em **Publish**.

    ![](images/L02/publish.png)

1. Selecione **Publish this version** e aguarde que a publicação seja concluída.

1. Pode **close** o estúdio de aplicações em tela.

## Exercício 3 – Adicionar Componente de Código à Solução

Neste exercício, irá adicionar o componente de código que criou à solução PrioritZ.

### Tarefa 1: Adicione o componente à solução

1. Navegue até ao portal do fabricante de aplicações Power, utilizando o URL abaixo, se ainda não estiver aberto. Certifique-se de que o ambiente de desenvolvimento denominado **DEV_ENV_<inject key="Deployment ID" enableCopy="false" /> (2)** é selecionado.

    ```
    https://make.powerapps.com/
    ```

1. Selecione **solução** e abra a solução **PrioritZ**.

1. Clique em **Adicionar existente (1)**, selecione **Mais (2)** → **Programador (3)** → **Controlo personalizado (4)**.

    ![](images/L02/dv_p3_e2_g_37.png)

1. Selecione **contoso_ContosoCoffee.PrioritZDnDRanking** e clique em **Adicionar**.

    ![](images/L02/dv_p3_e2_g_38.png)

1. Clique em **Publicar todas as personalizações**.

    ![](images/L02/dv_p3_e2_g_39.png)

## Resumo

Neste laboratório, aprendeu a criar um componente de código, a implementar a sua lógica, a integrá-lo numa aplicação de lona e a adicioná-la a uma solução dentro da Power Platform.

## Concluiu o laboratório com sucesso.Prossiga para a próxima página.

![](./images/NextPage.png)
