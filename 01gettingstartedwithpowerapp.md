# Laboratorio 01 - Introducción a Power Apps

## Duración Estimada: 45 minutos

Trabajando como parte del equipo de fusión de Prioritz, configurará su entorno de desarrollo de Power Platform. Importará y revisará la solución actual y explorará el estado actual de las aplicaciones, flujos y tablas de Prioritz. También agregará una columna a una tabla y modificará la aplicación para usarla.

## Objetivos del laboratorio

Podrá completar lo siguiente:

- Ejercicio 1: Importar y revisar los componentes de la solución
- Ejercicio 2: Agregar una columna para My Notes
- Ejercicio 3: Verificar el instalador de Visual Studio Code y la extensión de la CLI de Power Platform preinstalados

## Ejercicio 1: Importar y revisar los componentes de la solución

En este ejercicio, importará la solución actual al entorno de desarrollo creado previamente y revisará los componentes de la solución. También ejecutará un flujo que agregará datos de muestra a su entorno y probará las aplicaciones en la solución.

>**Nota:** El entorno de Desarrollo (Dev) ya está creado como parte de los requisitos previos.

### Tarea 1: Importar, Revisar los componentes de la solución y ejecutar el flujo

1. En JumpVM, haga clic en el acceso directo **Power Apps Portal** del navegador Microsoft Edge que está disponible en el escritorio.

   ![azure portal.](images/L01/PAportal.png)

1. En la ventana **Iniciar sesión**, aparecerá la pantalla de inicio de sesión, introduzca el siguiente nombre de usuario **(1)** y pulse en **Siguiente** **(2)**.

   * Correo Electrónico/Nombre de Usuario: <inject key="AzureAdUserEmail"></inject>

   ![](/images/L01/signin.png)

1. Ahora, ingrese la siguiente contraseña **(1)** y haga clic en **Iniciar sesión** **(2)**. 

   * Contraseña: <inject key="AzureAdUserPassword"></inject>
   
   ![](/images/L01/signinp.png)

1. Si ve la ventana emergente **¿Desea permanecer conectado?**, haga clic en **No**.

1. Una vez que haya iniciado sesión, haga clic en **Entorno (1)** y seleccione el entorno de desarrollo creado previamente llamado **DEV_ENV_<inject key="Deployment ID" enableCopy="false" /> (2)**.   

     ![](images/L01/dev11.png)

2. Ahora, haga clic en **Soluciones(1)** en el menú del lado izquierdo y haga clic en **Importar solución(2)**.

      ![](images/L01/importsolution1.png)

3. Haga clic en **Examinar**.
    
     ![](images/L01/browse1.png)
     
1. Navegue hasta la ruta `C:\LabFiles\Developer-in-a-day\Student\L01 - Getting started\Resources` en el explorador de archivos, seleccione el archivo **Prioritz_1_0_0_7.zip** y haga clic en **Abrir**.

1. Asegúrese de que el archivo **Prioritz(1)** esté seleccionado y haga clic en **Siguiente(2)**.
    
     ![](images/L01/next1.png)
     
1. Haga clic en **Siguiente** nuevamente en la hoja de importación de solución.

1. En la sección **Conexiones**, haga clic en el botón de puntos suspensivos **...(1)** junto a **Microsoft Dataverse Priority**.

1. Asegúrese de que el **odl_user(2)** que está utilizando esté seleccionado.

1. Haga clic en **Importar(3)**.

    ![](images/L01/connection1.png)
    
1. Espere hasta que se complete la importación de la solución.

     ![](images/L01/solutionsuccess.png)
     
1. Ahora debería ver la solución que importó en la lista de soluciones.

1. Abra la solución **Prioritz** que importó.

4. Expanda **Tablas (1)** y seleccione la tabla **PrioritZ Topic (2)**.
   
     ![](images/L01/L01-table1.png)

5. Seleccione **Columnas** en Esquema y revise las columnas de la tabla **PrioritZ Topic**.

   >**Información**: Las columnas estándar están integradas y todas las tablas las tienen. El equipo creó las columnas personalizadas para esta aplicación.
 
   ![](images/L01/L01-coulumn.png)

6. Seleccione la pestaña **Relaciones** del menú desplegable Columnas y revise cómo se relaciona esta tabla con otras tablas.
 
    ![](images/L01/L01-relation.png)
 
    ![](images/L01/L01-relation1.png)

1. Seleccione **Flujo de nube (1)** y abra el flujo **Importar datos de muestra – Topics (2)**.
 
    ![](images/L01/L01-cloud1.png)

9. Haga clic en el botón **Editar** para revisar el flujo.
  
    ![](images/L01/edit21.png)

10. Expanda el paso **Parse JSON** y revise los datos que creará este flujo.

    ![](images/L01/L01-parse1.png)
    
    >>**Nota**: Si no puede expandir el paso, haga clic en los puntos suspensivos (...), luego seleccione Configuración y haga clic en Cancelar.
    
12. Expanda el paso **Apply to each topic**.
    
    ![](images/L01/L01-topic1.png)

13. Expanda el paso **Apply to each topic item**.
   
     ![](images/L01/L01-eachtopic1.png)

14. Los pasos **Apply to each** deben verse como la imagen a continuación. Esta es la lógica para la automatización.
 
    ![](images/L01/image111.png)

15. Haga clic en el botón **<- volver**.
 
    ![](images/L01/image121.png)

16. Haga clic en el nombre del flujo para abrir la pantalla de detalles del flujo.

     ![](images/L01/EX1-T1-141.png)

17. Haga clic en **Ejecutar** para ejecutar el flujo.
   
     ![](images/L01/image131.png)

18. Haga clic en el botón **Ejecutar flujo** en la hoja Ejecutar flujo.

     ![](images/L01/L01-new1.png)

     > **Nota**: Si recibe este error `Error from the token exchange: Permission denied due to missing connection` mientras ejecuta el flujo, esto se debe a que la **conexión de Dataverse** no se está agregando correctamente. Elimine la solución importada e intente volver a importar la solución realizando los **Pasos 6 a 14** de esta tarea nuevamente, luego intente desencadenar el flujo de nuevo.

19. Haga clic en **Listo** y espere a que se complete la ejecución del flujo.

     ![](images/L01/EX1-T1-181.png)

20. El flujo debería ejecutarse correctamente. Si lo desea, puede hacer clic en la fila de ejecución y se le mostrarán los detalles de lo que hizo el flujo.
   
      ![](images/L01/image141.png)

### Tarea 2: Probar las aplicaciones

1. Vuelva a la solución **PrioritZ** haciendo clic en **Flujos en la nube**. Alternativamente, también puede abrir el portal de creación de **Power Apps** utilizando esta URL `https://make.powerapps.com` si aún no está abierto. Asegúrese de que el entorno de desarrollo denominado **DEV_ENV_<inject key="Deployment ID" enableCopy="false" /> (2)** esté seleccionado.
       
   ![](images/L01/cloud1u.png)

1. Navegue hasta la hoja **Soluciones** haciendo clic en el botón **Volver a Soluciones** (<-)**.

   ![](images/L01/solutions.png)
   
2. Seleccione **Aplicaciones (1)** en el menú del lado izquierdo de Power Apps. Debería ver dos aplicaciones llamadas **PrioritZ Ask** y **PrioritZ Admin (2)**. 

     >**Información:** La aplicación **PrioritZ Admin** se utiliza para administrar los temas sobre los que se pregunta y la aplicación **PrioritZ Ask** permite que los usuarios respondan.

    ![](images/L01/EX1-T2-2_1_1u.png)

3. Inicie la aplicación **PrioritZ Admin** haciendo clic en el símbolo **iniciar**.
    
    ![](images/L01/L01-adminu1.png)

4. Debería ver los cuatro temas siguientes.

    ![](images/L01/EX1-T2-4-2u.png)

5. Haga clic para abrir el tema **Event banner**.

6. Debería ver los detalles del tema con algunos elementos del tema.

    ![](images/L01/EX1-T2-6-1u.png)

7. Haga clic en el botón **<** regresar.

    > **Nota**: Debería volver a la pantalla de inicio.

9. Ahora, haga clic en el botón **+** para agregar un nuevo tema.
    
    ![](images/L01/image16u.png)

10. Proporcione la siguiente información y haga clic en **add a picture** que se encuentra debajo del campo **Respond By**.
     
     1. **Topic**: Escriba `Change Taco Tuesday to some other food`
     
     1. **Details**: Escriba `People are tired of tacos, what should we have instead of tacos?`
     
     1. **Respond By**: Seleccione **la fecha de hoy**.
     
     ![](images/L01/image17u.png)

11. Navegue hasta la ruta C:\LabFiles en el explorador de archivos, seleccione **image.png** y haga clic en abrir.

12. Escriba **Tamale Tuesday** en el campo Choice y haga clic en **add a picture** que se encuentra debajo del campo Choice.
     
      ![](images/L01/image18u.png)

11. Navegue hasta la ruta `C:\LabFiles` en el Explorador de Archivos, seleccione **image.png** y haga clic en Abrir.

13. Haga clic en **+** para agregar la opción.
     
      ![](images/L01/image191.png)

14. Agregue un par de opciones más repitiendo los **pasos 12 a 14**.
       
       1. **Choice 1** : Escriba `Steak Tuesday`
       
       2. **Choice 2**: Escriba `Cheese and Wine Tuesday`

15. Haga clic en el botón **Save** para guardar el tema.
    
    ![](images/L01/image20u.png)

16. El nuevo tema debería estar guardado y debería volver a la pantalla principal.

17. Debería ver el tema que agregó a la lista de temas.

     ![](images/L01/L01-tacou.png)

18. Cierre la aplicación PrioritZ Admin cerrando la pestaña del navegador en la que está abierta la aplicación PrioritZ Admin.

19. Seleccione **Aplicaciones (1)** en el menú del lado izquierdo de Power Apps e inicie la aplicación **PrioritZ Ask (2)** haciendo clic en el símbolo iniciar.
     
     ![](images/L01/L01-prioritzasku.png)

20. Debería ver una lista de temas. Abra el tema **Change Taco Tuesday to some other food** que creó en los pasos anteriores.

     ![](images/L01/L01-listu.png)

21. Haga clic en los íconos **arriba/abajo** para ordenar los elementos en el orden que prefiera y haga clic en **Vote**.
     
      ![](images/L01/L01-choiceuu.png)

22. Debería volver a las pantallas principales y debería ver un mensaje de notificación.

      ![](images/L01/TVU.png)
    
23. Cierre la aplicación PrioritZ Ask cerrando la pestaña del navegador en la que está abierta la aplicación PrioritZ Ask.

## Ejercicio 2: Agregar una columna para My Notes

En este ejercicio, agregará una nueva columna **My Notes** a la tabla de temas y actualizará la aplicación PriortZ Admin.

### Tarea 1: Agregar una nueva columna

1. Navegue hasta el portal de creación de Power Apps utilizando la siguiente URL si aún no está abierta. Asegúrese de que el entorno de desarrollo denominado **DEV_ENV_<inject key="Deployment ID" enableCopy="false" /> (2)** esté seleccionado.
    ```
    https://make.powerapps.com
   ```
2. Seleccione **Soluciones (1)** en el menú del lado izquierdo de Power Apps y abra la solución **PrioritZ (2)**.

   ![](images/L01/EX2-T1-2-1u.png)

3. Expanda **Tablas(1)** y seleccione la tabla **PrioritZ Topic(2)**.

4. Seleccione la pestaña **Columnas** que se encuentra debajo de **+ Nuevo(3)** y haga clic en **columna(4)**.

    ![](images/L01/EX2-T1-4u.png)

5. Ingrese el siguiente valor en el campo Nombre para mostrar.

   ```
   My Notes
   ```
1. Ahora, busque **plain text (1)** en Tipo de datos, luego seleccione el que se encuentra como **Multiple lines of text (2)** y haga clic en **Guardar (3)**.

    ![](images/L01/L01-notesu.png)
   
   > **Nota**: No abandone esta página.

### Tarea 2: Actualizar la aplicación de administración

1. Asegúrese de que todavía se encuentra en la solución **PrioritZ**. Seleccione **Aplicaciones (1)** en **Objetos** y seleccione la aplicación **PrioritZ Admin (2)** y haga clic en **Editar (3)**.
    
    ![](images/L01/L01-admineditu.png)

2. Seleccione **Add Topic Screen(1)**.

3. Haga clic en **+ Insertar(2)** y seleccione **Entrada de texto(3)**.
   
     ![](images/L01/tinputu.png)

4. Haga doble clic en la **Entrada de texto** recién agregada e ingrese el valor a continuación para cambiar el nombre de la entrada de texto.

    ```
    Notes textbox
    ```
    
     ![](images/L01/image27u.png)

5. Si es necesario, reduzca el tamaño del control add picture y mueva el cuadro de texto y la etiqueta **Respond By** hacia abajo y coloque el cuadro de texto **Notes** entre el control Details y la etiqueta Respond by.
   
    ![](images/L01/image28u.png)

6. Seleccione el **Cuadro de texto Notes** y luego **HintText** en el menú desplegable de propiedades.

    ![](images/L01/hintextu.png)

7. Cambie el valor **HintText** del cuadro de texto Notes por el siguiente valor.

    ```
    My notes
    ```
   
    ![](images/L01/image29u.png)

8. Seleccione **Mode** en el menú desplegable de propiedades y cambie su valor ingresando el texto que aparece a continuación.

    ```
    TextMode.MultiLine
    ```

    ![](images/L01/L01-modeu.png)

9. Seleccione **Save topic icon** en la sección **Add Topics Screen**.
     
     ![](images/L01/image30u.png)

10. Reemplace la fórmula **OnSelect** de **Save topic icon** con la fórmula que aparece a continuación. Patch crea una nueva fila en la tabla Dataverse.
     
     ![](images/L01/image31u.png)

    ```
    Set(newTopic,Patch('Prioritz Topics',Defaults('Prioritz Topics'),{'My Notes': 'Notes textbox'.Text,Topic:'Topic name textbox'.Text,Details:'Topic details textbox'.Text,'Respond By':'respond by date picker'.SelectedDate,Photo:AddTopicImage.Image}));ForAll(colAddChoices,Patch('Prioritz Topic Items',Defaults('Prioritz Topic Items'),{Choice:ThisRecord.choice,'PrioritZ Topic':newTopic,Photo:ThisRecord.photo}));Back()
    ```
11. Seleccione **View Topic Screen (1)** en la pestaña **Pantallas**.

12. Haga clic en la pestaña **+ Insertar(2)** y seleccione **Etiqueta de texto(3)**.

    ![](images/L01/tlabelu.png)

13. Haga doble clic en la etiqueta recién agregada e ingrese el valor a continuación para cambiar el nombre de la etiqueta que acaba de agregar.

     ```
     Notes label
     ```
     
    ![](images/L01/L01-labelu.png)

14. Cambie el valor **Text** de la etiqueta Notes por el texto a continuación.

     ```
     'Topics gallery'.Selected.'My Notes'
     ```

15. Reorganice los controles y mueva la **etiqueta Notes** entre la etiqueta de detalles y la galería de elementos de Temas.

16. Seleccione **Home Screen(1)** y haga clic en **Previsualizar la app(2)**.
      
      ![](images/L01/image34u.png)

17. Haga clic en el botón **+** para agregar un nuevo tema.

      ![](images/L01/L01-taco-1_1u.png)

18. Complete el formulario proporcionando la siguiente información y haga clic en **add a picture** que se encuentra debajo del campo **Respond By**.

       1. Topic: `Test Notes` (1)
       
       2. Details: `Testing the notes` (2)
       
       3. Text input: `Prioritz Admin topic` (3)
       
       4. Respond By: **La fecha de Hoy** (4)
      
      
19. Navegue hasta esta ruta C:\LabFiles en el explorador de archivos, seleccione **image.png** y haga clic en abrir.

20. Escriba **Test One** en el campo Choice y haga clic en **add a picture** que se encuentra debajo del campo Choice.
     
      ![](images/L01/image18uu.png)

21. Navegue hasta esta ruta `C:\LabFiles` en el Explorador de Archivos, seleccione **image.png** y haga clic en abrir.

22. Haga clic en **+** para agregar la opción.
     
      ![](images/L01/image19u.png)

23. Agregue una opción más repitiendo los **pasos 20 a 22** de esta tarea.
       
       1. **Choice 1** : Escriba `Test Two`
24. Después de agregar todas las opciones y los detalles del tema, su pantalla debería verse como la siguiente captura de pantalla.

   ![](images/L01/L01-testnotesu.png)
      
25. Ahora, haga clic en el botón **Save**. El nuevo tema debería estar **guardado**.

26. Haga clic para abrir el tema **Test Notes** que acaba de crear.

27. Las notas **Prioritz Admin topic** que agregó anteriormente ahora deberían estar visibles.
 
     ![](images/L01/image36.1.png)

28. Cierre la **vista previa** de la aplicación.

29. Haga clic en **Publicar**.

    ![](images/L01/publish.png)

30. Seleccione Publicar esta versión y espere a que se complete la publicación.

     ![](images/L01/NewUipublish1u.png)

31. Puede cerrar el **diseñador de aplicaciones**.

## Ejercicio 3: Probar la CLI de Power Platform

En este ejercicio, revisará y probará la extensión de la CLI de Power Platform en Visual Studio Code.

>**Nota**: La instalación de Visual Studio Code y la CLI de Power Platform ya se realizó como parte de los requisitos previos.

1. Navegue hasta el centro de administración de Power Platform mediante la siguiente URL y seleccione **Ambientes**.
      ```
        https://admin.powerplatform.microsoft.com/environments
      ```

1. Si ve la ventana emergente **¿Desea permanecer conectado?**, haga clic en **No**.

2. Haga clic para abrir su entorno de desarrollo llamado **DEV_ENV_<inject key="Deployment ID" enableCopy="false" />**.

3. Haga clic con el botón derecho en el valor **Environment URL** y péguelo en el Bloc de notas.
 
    >**Nota**: Asegúrese de que el valor Environment URL se copie junto con **https**. El valor copiado debería verse parecido a `https://orgxxxxxx.crm.dynamics.com/`

    ![](images/L01/image37u.png)

4. En JumpVM, inicie **Visual Studio Code** usando el acceso directo disponible en el escritorio.

   ![](images/L04/vscode1.png)
   
6. Haga clic en **Terminal** y seleccione **Nuevo terminal**.

    ![](images/L01/image42u.png)

7. Ejecute el siguiente comando en la terminal.
   ```
   pac
   ```
   
   > **Información:** Si encuentra un error después de usar el comando **pac**, descargue la CLI de Power Platform desde la URL **https://aka.ms/PowerAppsCLI**, abra el instalador y complete la instalación. Luego, vuelva a intentar el paso.

8. Reemplace `<your environment URL>` en el siguiente comando con el valor de la URL del entorno que copió anteriormente y luego ejecute el comando.
   ```
   pac auth create --name DevAuth --url <your environment URL>
   ```

   > **Información:** Después de agregar la URL del entorno, el comando se verá así: `pac auth create --name DevAuth--url https://org32172839283.crm.dynamics.com/`
  
    ![](images/L01/Eeditpac.png)

1. Complete el proceso de **Inicio de sesión**, utilizando las siguientes credenciales.

      * Correo Electrónico/Nombre de Usuario: <inject key="AzureAdUserEmail"></inject>
      * Contraseña: <inject key="AzureAdUserPassword"></inject>

9. Seleccione la herramienta **Power Platform (1)**, ahora debería tener al menos un **perfil de autenticación (2)**. Si tiene más de un perfil, asegúrese de que el perfil que creó esté seleccionado.
   
    ![](images/L01/L01-authu.png)

    > **Nota**: Si puede ver el **Perfil Universal** en lugar del perfil **DeVAuth**, se debe a que agregó el valor **Environment URL** incorrecto en el comando **pac auth create** en el paso 9. Para solucionar este problema, siga los pasos a continuación:

      1. Elimine el **Perfil Universal** de Visual Studio Code haciendo clic en el botón Eliminar.
      2. Copie el valor **Environment URL** correcto siguiendo el **Paso 4** de esta tarea.
      3. Realice el **Paso 8** de esta tarea nuevamente para crear el perfil de autenticación.

10. Haga clic en **Terminal** y seleccione **Nuevo terminal** si aún no está abierta.

     ![](images/L01/image42.png)

11. Ejecute el siguiente comando para ver una lista de soluciones.

      ```
      pac solution list
      ```
12. Debería ver una lista de soluciones instaladas en su entorno.
    
    ![](images/L01/sollistu.png)

## Resumen

En este laboratorio, aprendió a importar y ejecutar una solución de inicio, a personalizarla agregando una nueva columna y actualizando la aplicación de administración, y a verificar la funcionalidad mediante la CLI de Power Platform.

## Ha completado el laboratorio con éxito
