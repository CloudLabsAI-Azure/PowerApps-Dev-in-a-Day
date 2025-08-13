# Low Code for Pro-Dev in a Day
para Desenvolvedores Profissionais em um Dia
## Duração Estimada: 8 Horas

## Visão Geral

Neste laboratório, você obterá experiência prática com vários aspectos da Power Platform, desde a importação e execução de soluções iniciais até a construção e o uso de componentes de código personalizados. Você criará e modificará conectores personalizados, integrará Azure Functions e promoverá soluções entre ambientes. Cada laboratório desenvolve habilidades fundamentais, como a criação de componentes do Power Apps, a configuração de conectores personalizados e o gerenciamento de soluções com o GitHub. Seguindo instruções detalhadas e auxílios visuais, você aprenderá a desenvolver, testar e implantar soluções robustas da Power Platform de forma eficaz, garantindo uma compreensão abrangente das capacidades e melhores práticas da plataforma.

## Objetivo

Após concluir estes laboratórios, você saberá como importar e personalizar soluções na Power Platform, construir componentes de código personalizados e criar e integrar conectores personalizados. Ao final deste laboratório, você terá obtido conhecimento sobre:

 - **Introdução ao Power Apps:** Aprender a importar uma solução inicial, adicionar uma nova coluna, atualizar o aplicativo de administração e testar a CLI da Power Platform.
 - **Construir um componente de código:** Criar um componente de código, implementar sua lógica, integrá-lo a um aplicativo de tela e adicioná-lo a uma solução.
 - **Conector personalizado para API existente:** Criar, modificar e testar um conector personalizado usando uma definição Open API, e integrá-lo com aplicativos de tela e fluxos.
 - **Gerenciamento do ciclo de vida de aplicativos (ALM):** Promover uma solução para um ambiente de teste, configurar uma entidade de serviço (service principal), gerenciar com o GitHub e configurar um fluxo de trabalho (workflow) para o lançamento.
   
## Pré-requisitos

Os participantes devem ter:

- Conhecimento básico da Power Platform e do Power Apps.
- Compreensão de APIs e desenvolvimento de componentes personalizados.
- Familiaridade com GitHub e operações Git.
- Estar preparado com os arquivos, definições de API e ferramentas de desenvolvimento necessárias.

## Arquitetura

Neste laboratório, você seguirá um processo estruturado para dominar aspectos-chave do desenvolvimento e gerenciamento da Power Platform. Você começará importando uma solução pré-construída, executando um fluxo para adicionar dados de exemplo, personalizando-a com uma nova coluna e testando a CLI da Power Platform com o VS Code. Em seguida, você construirá um componente de código com o VS Code, o integrará a um aplicativo de tela e o adicionará a uma solução. Depois, você criará um conector personalizado usando uma definição Open API, o aprimorará com código personalizado e o testará tanto em fluxos quanto em aplicativos de tela. Após isso, você criará, implementará e publicará uma Azure Function, criará um conector para ela e, opcionalmente, a integrará a um aplicativo de tela. Finalmente, você promoverá uma solução para um ambiente de teste, configurará uma entidade de serviço, gerenciará a solução usando um repositório GitHub e a liberará para testes. Cada passo é detalhado com instruções e auxílios visuais para garantir que você ganhe experiência prática com os recursos da Power Platform.

## Diagrama da Arquitetura

 ![](./images/low_code_in_a_day_Architecture_diagram.JPG)

## Explicação dos Componentes
A arquitetura para este laboratório envolve os seguintes componentes principais:

- **Ambiente Power Platform:** O espaço de trabalho central onde você importa, gerencia e personaliza soluções dentro da Power Platform. Ele fornece as ferramentas e a interface necessárias para desenvolver e testar vários aplicativos e componentes.
- **Visual Studio Code:** Um editor de código versátil usado para desenvolver componentes de código personalizados e Azure Functions. Ele oferece extensões e integrações poderosas para otimizar a codificação e a depuração no ecossistema da Power Platform.
- **Componente de Código:** Elementos criados para estender a funcionalidade dos Power Apps. Esses componentes envolvem a escrita e integração de lógica personalizada, o que aprimora as capacidades e a flexibilidade dos aplicativos de tela.
- **Conector Personalizado:** Ferramentas que permitem que os aplicativos da Power Platform se conectem a fontes de dados externas via APIs. Esses conectores possibilitam a integração perfeita de dados e serviços externos nas aplicações da Power Platform.
- **Azure Function:** Serviços de computação sem servidor (serverless) que executam código sob demanda para realizar várias tarefas. As Azure Functions são integradas aos aplicativos da Power Platform para adicionar capacidades avançadas e escaláveis e para lidar com operações específicas.
- **GitHub:** Um sistema de controle de versão para gerenciar e rastrear alterações em soluções e código.

##  Getting Started
 
Bem-vindo ao seu workshop "Low-Code para Desenvolvedores Profissionais em um Dia"! Preparamos um ambiente integrado para você explorar e aprender sobre os serviços do Azure. Vamos começar aproveitando ao máximo esta experiência:
 
## Acessando seu Ambiente de Laboratório
 
Quando estiver pronto para começar, sua máquina virtual e o guia estarão ao seu alcance dentro do seu navegador.

![](./images/GS6.png)

### Máquina Virtual e Guia
 
No ambiente integrado, a VM do laboratório serve como o espaço de trabalho designado, enquanto o guia do laboratório fica acessível no lado direito da tela.

## Explorando Seus Recursos de Laboratório
 
Para entender melhor seus recursos de laboratório e credenciais, navegue até a aba **Environment Details** tab.

![](./images/GS20.png)
 
## Utilizando o Recurso de Dividir Janela
 
Para sua conveniência, você pode abrir o guia do laboratório em uma janela separada selecionando o botão **Dividir Janela** no canto superior direito.
 
![](./images/GS8.png)
 
## Gerenciando Sua Máquina Virtual
 
Sinta-se à vontade para **Iniciar, Parar ou Reiniciar** sua máquina virtual conforme necessário a partir da aba **Recursos**. A experiência está em suas mãos!
 
![](./images/GS5.png)
 
## Let's Get Started with Azure Portal
 
1. On your virtual machine, click on the Azure Portal icon as shown below:
 
   ![](./images/GS1.png)
 
2. You'll see the **Sign into Microsoft Azure** tab. Here, enter your credentials:
 
   - **Email/Username:** <inject key="AzureAdUserEmail"></inject>
 
     ![](./images/GS2.png "Enter Email")
 
3. Next, provide your password:
 
   - **Password:** <inject key="AzureAdUserPassword"></inject>
 
     ![](./images/GS3.png "Enter Password")
 
4. If you see the pop-up **Stay Signed in?**, click **No**.

   ![](./images/GS9.png)

5. If you see the pop-up **You have free Azure Advisor recommendations!**, close the window to continue the lab.

6. If a **Welcome to Microsoft Azure** popup window appears, click **Maybe Later** to skip the tour.

By completing these exercises, You will import and customize Power Platform solutions, create and implement custom code components with Visual Studio Code, develop and integrate custom connectors, create and deploy Azure Functions, and manage solutions using GitHub for source control.
   
## Contato para Suporte

A equipe de suporte do CloudLabs está disponível 24 horas por dia, 7 dias por semana, 365 dias por ano, via e-mail e chat ao vivo para garantir assistência contínua a qualquer momento. Oferecemos canais de suporte dedicados e personalizados tanto para alunos quanto para instrutores, garantindo que todas as suas necessidades sejam atendidas de forma rápida e eficiente.

**Contatos de Suporte para Alunos:**

- Suporte por E-mail: cloudlabs-support@spektrasystems.com
- Suporte por Chat ao Vivo: https://cloudlabs.ai/labs-support
  
Agora, clique em Avançar no canto inferior direito para ir para a próxima página.

![](./images/GS4.png)

## Bons estudos!!






 
