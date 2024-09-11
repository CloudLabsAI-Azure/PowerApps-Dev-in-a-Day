# Laboratório 02 - Construir um componente de código

## Duração estimada: 90 minutos

Trabalhando como parte da equipe de fusão do PrioritZ, você foi solicitado a criar um componente de código do Power Apps para permitir a classificação de prioridade de arrastar e soltar itens no PrioritZ Ask Power App. Você criará
um componente de código usando a estrutura React JavaScript. Uma abordagem de componente de código é usada para
para atender ao requisito porque não existe um controlo semelhante já incorporado.

Você colaborou com os criadores de aplicativos para identificar as seguintes propriedades para permitir que eles
configurar o componente de código na aplicação:

- BackgroundColor
- DragBackgroundColor
- ItemHeight
- FontSize
- FontColor

O aplicativo PrioritZ Ask preparará uma coleção dos itens a serem classificados que serão vinculados como o conjunto de dados para o
o componente de código. Quando um item é arrastado e largado, o componente de código gera um evento OnSelect
que será tratado pela aplicação de alojamento. A aplicação de alojamento actualizará os itens da coleção com
a sua nova classificação. O componente de código será stateless.

## Objectivos do laboratório

- Exercício 1: Construir o componente de código 
- Exercício 2: Usar o componente de código 
- Exercício 3: Adicionar o componente de código à solução 

## Exercício 1 - Construir o componente de código

Neste exercício, você irá construir o componente de código.

### Tarefa 1: Criar o componente de código

1. Inicie o **Visual Studio Code** se ainda não estiver aberto usando o atalho disponível na área de trabalho.

   ![](images/L04/vscode1.png)
   
2. Selecione o separador **Power Platform (1)** e certifique-se de que o seu perfil **Dev Auth (2)** está selecionado. 
    
   >**Nota**: O separador Power Platform já está instalado.
    
    ![](images/L02/L01-authuu.png)

3. Clique em **Terminal (1)** e selecione **New Terminal (2)**.
     
     ![](images/L02/L02-terminaluu.png)

4. Na janela Terminal, crie um novo diretório executando o comando abaixo.

    ```
    md PrioritZDnDRanking
    ```
5. Execute o comando abaixo para mudar para o diretório PrioritZDnRanking que você criou.

    ```
    cd PrioritZDnDRanking
    ```
6. Agora você deve estar no diretório que criou. Crie um novo projeto de componente e instale as dependências executando o comando abaixo.
    
     ```
     pac pcf init -ns ContosoCoffee --name PrioritZDnDRanking --template dataset --framework react --run-npm-install
     ```
     
     ![](images/L02/image3.png)

7. O projeto do componente framework deve ser criado com sucesso.

    ![](images/L02/image4.png)

8. Execute o comando abaixo para abrir o projeto.
    ```
    code -a.
    ```

1. Se lhe for apresentado o pop-up abaixo, clique em **Sim** para confiar nos autores dos ficheiros.

    ![](images/L02/image4.1.png)

9. Reveja os ficheiros de componentes de código criados selecionando o separador **Explorer**.
    
     ![](images/L02/L02-explorer.png)

10. Expanda a pasta **PrioritZDnDRanking** e, em seguida, expanda a subpasta **generated**.


