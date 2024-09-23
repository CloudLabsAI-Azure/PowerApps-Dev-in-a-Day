# Laboratorio 03 - Conector personalizado para API existente

## Duración Estimada: 140 minutos

Como parte del equipo de fusión de PrioritZ, configurará un conector personalizado para una API existente. El equipo desea agregar una insignia a la aplicación PrioritZ para dar crédito a los usuarios cuando hayan terminado de clasificar un elemento. El equipo identificó una API existente, pero no tiene un conector de Power Platform.

## Objetivos del Laboratorio

- Ejercicio 1: Crear Base de Datos en el Entorno Predeterminado 
- Ejercicio 2: Crear Solución 
- Ejercicio 3: Crear Conector Personalizado 
- Ejercicio 4: Añadir Código Personalizado 
- Ejercicio 5: Probar Conector Personalizado 
- Ejercicio 6: Promover la Solución al Entorno de Pruebas

## Ejercicio 1: Crear Base de Datos en el Entorno Predeterminado

En este ejercicio, creará una base de datos de Dataverse en el entorno de prueba, que se utilizará para importar la solución en los próximos ejercicios.

Cuando revise la API, verá que tiene cuatro operaciones y utiliza autenticación mediante una clave de API.


 ![](images/L03/image1.png)

### Tarea 1: Crear Base de Datos

1. Navegue hasta el portal de creación de Power Apps y seleccione su entorno de **Prueba** llamado Azure XXXXXX (predeterminado).

         https://make.powerapps.com

2. Seleccione **Soluciones** en el menú del lado izquierdo de Power Apps.

3. Haga clic en **Crear base de datos** para crear una base de datos de Dataverse.
 
    ![](images/L03/db1.png)

5. Deje los campos **Moneda** e **Idioma** en los valores predeterminados y haga clic en **Crear base de datos**.
   
    ![](images/L03/db2.png)
    
    >**Nota:** Puede dejar abierta esta pestaña del navegador y continuar con el próximo ejercicio, ya que la creación de la base de datos de Dataverse tomará algún tiempo.

## Ejercicio 2: Crear Solución

En este ejercicio, creará una solución para el conector personalizado Contoso Badges. Actualmente, los conectores personalizados deben estar en una solución separada de las aplicaciones y los flujos que los usan.

### Tarea 1: Crear una solución

1. Abra una nueva pestaña del navegador y navegue hasta el portal de creación de Power Apps y seleccione **Entornos (1)**, asegúrese de estar en su entorno de desarrollo llamado **DEV_ENV_<inject key="Deployment ID" enableCopy="false" /> (2)**.

         https://make.powerapps.com 

    ![](images/L03/dev11.png)

2. Seleccione **Soluciones** y haga clic en **+ Nueva solución**.

    ![](images/L03/L03-solution.png)

3. Escriba **Contoso Badges connector (1)** como Nombre para mostrar, seleccione **Contoso Coffee (2)** como Editor y haga clic en **Crear (3)**.
   
   ![](images/L03/image2-1.png)

## Ejercicio 3: Crear Conector Personalizado

En este ejercicio, creará un conector personalizado a partir de una API existente.

### Tarea 1: Descargar la definición de API abierta y cree un conector

1. Navegue a la siguiente URL para abrir la API Contoso Coffee Badges.

   ```
   https://contosobadgestest.azurewebsites.net/
   ```
3. Haga clic en el enlace **Open API definition file**.
   
    ![](images/L03/image3.png)

3. Haga una revisión rápida de la definición de Open API.

4. Haga clic derecho en la página y seleccione **Guardar como** o use **Ctrl + C** y nombre el archivo como **swagger.json** en su equipo. Ahora, cierre la pestaña del navegador haciendo clic en **X**.

     ![](images/L03/image4.png)


5. Navegue hasta el portal de creación de Power Apps y asegúrese de estar en su entorno de desarrollo llamado **DEV_ENV_<inject key="Deployment ID" enableCopy="false" />**.

         https://make.powerapps.com

6. Seleccione **Soluciones (1)** y abra la solución **Contoso Badges connector (2)** que creó.

     ![](images/L03/L03-contoso.png)

7. Haga clic en **+ Nuevo (1) | Automatización (2)** y seleccione **Conector personalizado (3)**.
     
     ![](images/L03/image5-1.png)


8. Ingrese la siguiente información en la hoja **Crear conector**.

     1. Nombre del conector: **Badges connector (1)** 
     2. Descripción: **Connector for contosobadgestest (2)**
     3. Host: **contosobadgestest.azurewebsites.net (3)** 
     4. Haga clic en **Crear conector (4)**.
    
    ![](images/L03/L03-badges1u.png)

   >**Nota**: Si se le solicita que inicie sesión, use las credenciales de ODL que se encuentran en la pestaña de ambiente ubicada a la derecha de la guía de laboratorio.

9. Seleccione **Conectores personalizados (1)** en el mapa del sitio. Haga clic en el botón **... Más acciones (2)** del conector personalizado que creó y seleccione **Actualizar desde archivo Open API (3)**

      ![](images/L03/L03-custom.png)

10. Haga clic en **Importar** para seleccionar el archivo API.

      ![](images/L03/L03-import.png)

11. Seleccione el archivo **swagger.json** que guardó en su máquina y haga clic en **Abrir**.
    
12. Haga clic en **Continuar** en la ventana emergente **Importar un archivo OpenAPI**.
    
    ![](images/L03/image8-1.png)

13. Ingrese **Connector for contosobadgestest (1)** para Descripción, **contosobadgestest.azurewebsites.net (2)** para Host, y avance a **Seguridad (3)**.
      
      ![](images/L03/image9-1u.png)

14. Revise la **configuración de seguridad (1)** y avance a **Definición (2)**.

      ![](images/L03/L03-security.png)

15. No salga de esta página.

### Tarea 2: Modificar la definición

1. Seleccione la acción **AddCredit (1)** y luego **Important (2)** para Visibility.
    
     ![](images/L03/image10-1.png)

3. Desplácese hacia abajo hasta la sección **Solicitud**, haga clic en el botón **body** y seleccione **Editar**.

     ![](images/L03/image11.png)

4. Desplácese hacia abajo y haga clic en el botón **points** y seleccione **Editar**.
    
    ![](images/L03/image12.png)

5. Seleccione **Sí** para Is required? y haga clic en el botón **Atrás**.
     
     ![](images/L03/image13.png)

6. Haga clic en el botón **recipientid** y seleccione **Editar**.

     ![](images/L03/L03-recepient.png)

7. Seleccione **Sí** para Is required? y haga clic en el botón **Atrás**.

     ![](images/L03/image13.png)

8. Haga clic en el botón **name** y seleccione **Editar**.

     ![](images/L03/L03-name.png)

9. Seleccione **Sí** para Is required? y haga clic en el botón **Atrás**.

     ![](images/L03/image13.png)

10. Haga clic en el botón **Atrás** nuevamente.

      ![](images/L03/image14.png)

11. Avance a **Complemento de IA (vista preliminar)**.
12. Avance a **Código**
 
13. Revise el código y avance a **Probar**.

      ![](images/L03/L03-test.png)

14. Haga clic en **Actualizar conector** y espere a que se actualice el conector

     ![](images/L03/image15.png)

15. No salga de esta página.

### Tarea 3: Probar el conector

1. Abra una nueva pestaña o ventana del navegador y navegue hasta la siguiente URL para abrir la API Contoso Coffee Badges. 
    ```
    https://contosobadgestest.azurewebsites.net/
    ```

2. Haga clic en el enlace para obtener la **API Key**
    
   ![](images/L03/image16.png)

3. Copie el valor de la **API Key** y guárdelo en el Bloc de notas, ya que utilizará este valor en los próximos pasos. Ahora, cierre la pestaña del navegador haciendo clic en **X**.

4. Vuelva a la página de prueba del conector y haga clic en **+ Nueva conexión**.
    
     ![](images/L03/image17.png)

5. Pegue la **API Key (1)** que copió en el **paso 3** de esta tarea y haga clic en **Crear conexión (2)**.
   
    ![](images/L03/image18-1.png)

6. Haga clic en el botón **Actualizar** conexiones.
   
    ![](images/L03/image19.png)


7. La conexión que creó debe estar seleccionada.

8. Vaya a la operación **AddCredit (1)**. Ingrese su dirección de correo electrónico para recipientid, ingrese su nombre para name, ingrese **1** para points y haga clic en **Probar operación (2)**.

    ![](images/L03/image20-1u.png)

9. La prueba debería ser exitosa y la respuesta debe verse como la imagen a continuación.
     
     ![](images/L03/image21u.png)


11. Seleccione la operación **GetRecipient**.

12. Proporcione su dirección de correo electrónico como ID y haga clic en **Probar operación**.
      
     ![](images/L03/image22-1u.png)

13. La prueba debería tener éxito y debería obtener la respuesta esperada.

14. Continúe y pruebe las operaciones ListBadges y ListRecipients.
15. Todas las pruebas deberían tener éxito.
  
     ![](images/L03/image23.png)


## Ejercicio 4: Añadir Código Personalizado

En este ejercicio, agregará una nueva operación para devolver solo el nombre de la insignia actual y la URL de la imagen.
Para ello, utilizará la función de código personalizado para cambiar la forma de la respuesta de la API.

### Tarea 1: Agregar código desde la carpeta de recursos

1. Navegue hasta Power Automate utilizando la siguiente URL.
          
   ```
   https://make.powerautomate.com
   ```

2. Haga clic en **Más (1)** y seleccione **Descubrir Todo (2)**.

   ![](images/L03/L03-connectormore.png)

3. En **Datos (1)**, seleccione **Conectores personalizados (2)**.

   ![](images/L03/L03-connectoru.png)

4. Haga clic en el botón **Editar** del conector personalizado que creó.

    ![](images/L03/image24u.png)

5. Seleccione la pestaña **Definición** del menú desplegable y haga clic en **Nueva acción** en la pestaña de definición.
  
    ![](images/L03/L03-EX3.1.png)

6. Ingrese la siguiente información para agregar la acción **Get current badge**.

     1. Resumen: **Get current badge**
     2. Descripción: **Get current badge (2)** 
     3. ID de Operación: **getcurrentbadge (3)**
    
    
    ![](images/L03/image26-1.png)

7. Desplácese hacia abajo hasta la sección **Solicitud** y haga clic en **+ Importar desde muestra**.
    
    ![](images/L03/image27.png)

8. Seleccione **Get (1)** para Verbo, ingrese el valor a continuación para **URL (2)** y haga clic en **Importar (3)**.
   ```
   https://contosobadgestest.azurewebsites.net/getcurrentbadge?id={id} 
   ```

    ![](images/L03/image28-1.png)

9. Haga clic en **Actualizar conector** y espere a que se actualice el conector.
10. Seleccione la pestaña **Código** en el menú desplegable.
11. Habilite **Código (1)** y haga clic en **Cargar (2)**.
    
     ![](images/L03/image29-1.png)

12. Seleccione el archivo **CustomConnectorCode.csx** ubicado en esta ruta `C:\LabFiles\Developer-in-a-day\Student\L03 - Custom connector for existing API\Resources` y haga clic en **Abrir**.
13. Seleccione la acción **getcurrentbadge** del menú desplegable.
     
     ![](images/L03/image30.png)

14. Revise el código que acaba de agregar.
15. Haga clic en **Actualizar conector** y espere a que se actualice el conector.
16. Avance a **Probar** seleccionándolo en el menú desplegable.
17. Seleccione la acción **getcurrentbadge**.
18. Proporcione su dirección de correo electrónico como ID y haga clic en **Probar operación**.
     
     ![](images/L03/image31-1.png)


19. La prueba debería ser exitosa y debería obtener una insignia actual para el usuario que creó.
    
    ![](images/L03/image32.png)

     > **Nota**: Si la operación de prueba falla, intente actualizar el conector y luego vuelva a probarlo realizando los pasos 15 a 18.

20. Copie el JSON del **Cuerpo de respuesta**.

21. Seleccione la pestaña Definición en el menú desplegable.

22. Seleccione la acción **getcurrentbadge**.
     
     ![](images/L03/image33.png)

23. Desplácese hacia abajo hasta la sección **Response** y haga clic en **+ Agregar respuesta predeterminada.**
 
     ![](images/L03/image34.png)

24. Pegue el JSON que copió en el **Body (1)** y haga clic en **Importar (2)**.
     
     ![](images/L03/image35-1.png)

25. Haga clic en **Actualizar conector** y espere a que se actualice el conector.

26. **No** abandone esta página.

### Tarea 2: Probar código personalizado

En esta tarea, probará su código personalizado.

1. Seleccione la pestaña **Probar**.
2. Seleccione la conexión que creó anteriormente.

1. Vaya a la sección **Operaciones** y seleccione la operación **getcurrentbadge (1)**. Proporcione su correo electrónico como **id (2)** y haga clic en **Probar operación (3)**.
   
     ![](images/L03/image36-1u.png)

5. La operación debería tener éxito y el **Cuerpo** de la respuesta debería verse como la imagen a continuación.
    
    ![](images/L03/image37.png)

## Ejercicio 5: Probar Conector Personalizado

En este ejercicio, probará el conector personalizado que creó utilizando un flujo y una aplicación de canvas.

### Tarea 1: Probar conector desde la aplicación de canvas

En esta tarea, utilizará el conector personalizado que creó para mostrar la insignia actual del usuario en la aplicación de canvas PrioritZ Ask.

1. Navegue hasta el portal de creación de **Power Apps** utilizando la siguiente URL si aún no está abierto y asegúrese de estar en su entorno de desarrollo.
   ```
   https://make.powerapps.com
   ```

2. Expanda **Soluciones** y abra la solución **PrioritZ**.
3. Seleccione **Aplicaciones (1)**, seleccione la aplicación **PrioritZ Ask (2)** y haga clic en **Editar (3)**.
 
     ![](images/L03/image38-1u.png)

4. Seleccione **Datos** desde la izquierda y haga clic en **+ Agregar datos.**
     
     ![](images/L03/image39.png)

5. Expanda **Conectores** y seleccione el elemento **Badges connector** que creó.
    
    ![](images/L03/image40uu.png)


6. Haga clic en **+ Agregar una conexión**.

    ![](images/L03/L03-EX4.png)

7. Abra una nueva pestaña o ventana del navegador y navegue hasta la siguiente URL para abrir la API Contoso Coffee Badge.
    
    ```
    https://contosobadgestest.azurewebsites.net/
 
    ```
    
2. Haga clic en el enlace para abrir la **API Key**
     
    ![](images/L03/image41.png)

9. Copie el valor **API Key** y péguelo en el Bloc de notas, ya que utilizará este valor en los próximos pasos. Ahora, cierre la pestaña del navegador haciendo clic en **X**.

10. Regrese al diseñador de aplicaciones, pegue la **API Key (1)** que copió en el paso anterior y haga clic en **Conectar (2)**.
     
     ![](images/L03/image42-1u.png)


11. Seleccione la **vista de árbol**.

12. Seleccione la pestaña **Pantallas (1)**, vaya a la pestaña **Insertar (2)**, haga clic en **Medios** y, a continuación, seleccione **Imagen (3)**.
     
     ![](images/L03/L03-componentuu.png)

13. Haga doble clic en la imagen recién agregada y cambie su nombre a **User badge**.
    
     ![](images/L03/image44.png)


14. Establezca el valor **Image** de User badge con la fórmula que se muestra a continuación.

    ```
    ContosoBadges.getcurrentbadge({id:User().Email}).image
    ```
    ![](images/L03/image45u.png)

15. Establezca el valor Tooltip de User badge con la fórmula que se muestra a continuación.

    ```
    ContosoBadges.getcurrentbadge({id:User().Email}).name
    ```
    ![](images/L03/L03-EX4-T1u.png)

16. Achique la imagen y muévala a la esquina superior derecha de la pantalla.

17. El elemento User badge ahora debería verse como la imagen que se muestra a continuación.
      
     ![](images/L03/image46.png)

18. Seleccione la pestaña **Pantallas** en la vista de árbol. Haga clic en el botón **Iniciar**.

19. Pase el cursor sobre la insignia para ver el nombre de la insignia.

      ![](images/L03/image47.png)

20. Cierre la vista previa.

1. Seleccione **Publicar**.

23. Seleccione **Publicar esta versión**.

24. Vuelva a la solución haciendo clic en el botón  **Atrás**.
     
      ![](images/L03/imagee47.png)

25. No salga de esta página.

### Tarea 2: Probar el conector desde el flujo

1. Asegúrese de que todavía se encuentra en la solución **PrioritZ**.

2. Haga clic en **+ Nuevo** y seleccione **Automatización | Flujo de nube | Instantáneo**.

    ![](images/L03/image48.png)

3. Ingrese **Test add credit** como nombre del flujo, seleccione **Activar manualmente un flujo** y haga clic en **Crear**.
     
    ![](images/L03/edd%20(1).png)

4. Haga clic en **+ Nuevo paso**.

    ![](images/L03/L03-EX3-T2.png)

5. Seleccione la pestaña **Personalizado** y luego seleccione la acción **Add credit**.
   
    ![](images/L03/edd%20(2).png)
    
6. Ingrese **Test connection**, pegue la **API Key** que copió anteriormente en el **paso 9** de la tarea 1 de este ejercicio y haga clic en **Crear**.
  
    ![](images/L03/edd%20(3).png)

7. Haga clic en el campo **recipientId**. En Activar manualmente un panel de flujo, seleccione **User email**.
    
     ![](images/L03/image49u.png)

8. Haga clic en el campo **name**. En Activar manualmente un panel de flujo, y seleccione **User name**.

9. Ingrese **1** para points y haga clic en **Guardar**. Espere a que se guarde el flujo.
   
     ![](images/L03/image50.png)

10. Haga clic en **Probar**.

     ![](images/L03/L03-EX4-test.png)

11. Seleccione **Manualmente** y haga clic en **Probar** nuevamente.

     ![](images/L03/L03-EX3-manually.png)

12. Haga clic en **Continuar**.

13. Haga clic en **Ejecutar flujo**.

14. Haga clic en **Listo**.

15. La ejecución del flujo debería ser exitosa. Una vez que se haya realizado correctamente, haga clic en el botón **Atrás**.
    
     ![](images/L03/image51.png)

17. Seleccione **Flujos en la nube** y abra el flujo que creó.
     
     ![](images/L03/image52.png)

18. Abra una nueva ventana del navegador y navegue hasta el portal de creación de Power Apps.
19. Asegúrese de que está en el entorno correcto.
20. Seleccion **Aplicaciones** e inicie la aplicación **PrioritZ Ask**.
21. La aplicación debería mostrar ahora **First Badge**.
  
     ![](images/L03/image53.png)

22. Vuelve al flujo y ejecútelo dos veces más.

     ![](images/L03/L03-EX4-run1.png)

23. Vuelva a la aplicación **PrioritZ Ask** y actualice la página.
24. Ahora debería ver la insignia **Team Player**.
  
     ![](images/L03/image54.png)

25. Vaya al flujo y ejecútelo dos veces más.
26. Vuelva a la aplicación **PrioritZ Ask** y actualice la página.
27. Ahora debería ver la insignia **Champ**
   
     ![](images/L03/image55.png)

## Ejercicio 6: Promover la Solución al Entorno de Pruebas

En este ejercicio, exportará la solución del conector Contoso Badges desde el entorno de desarrollo y la importará al entorno de prueba.

### Tarea 1: Exportar la solución.

1. Navegue hasta el portal de creación de Power Apps y asegúrese de estar en su entorno de desarrollo.

     ```
     https://make.powerapps.com
     ```

2. Seleccione **Soluciones**.
3. Seleccione la solución del conector **Contoso Badges** y haga clic en **Exportar solución**.
   
     ![](images/L03/L03-EX4-export.png)

4. En la hoja **Antes de exportar**, haga clic en **Publicar** y espere a que se complete la publicación.
5. Una vez publicada, haga clic en **Siguiente**.
6. Seleccione **Administrado** y haga clic en **Exportar**.
7. Espere a que se exporte la solución.
8. Haga clic en el botón Descargar en la parte superior derecha de la pantalla. Haga clic en Descargar Solución.
 
    ![](images/L03/SolutionDown1.png)

### Tarea 2: Importar la solución

1. Navegue hasta el portal de creación de Power Apps si aún no está abierto y seleccione su entorno de **Prueba** llamado Azure XXXXXX (predeterminado).

     ```
     https://make.powerapps.com
     ```

3. Haga clic en **Importar Solución**.
    
     ![](images/L03/L03-EX5.png)
     
     >**Nota:** Intente actualizar el navegador si las soluciones no están abiertas.

4. Haga clic en **Explorar**.
5. Seleccione la solución que exportó desde el entorno de desarrollo y haga clic en **Abrir**.
6. Haga clic en **Siguiente**.
7. Haga clic en **Importar** y espere a que se complete la importación.
8. La solución debería importarse correctamente. **No** salga de esta página.

### Tarea 3: Probar el conector

1. Haga clic para abrir la solución que acaba de importar.

2. Haga clic para abrir **Badges connector**.
  
    ![](images/L03/image58.png)

    >**Nota**: Si recibe el mensaje de error **could not retrieve the connector data**, espere unos minutos (5 a 10 minutos) para que se actualicen los datos del conector. Si eso no funciona, puede eliminar el conector importado y realizar los **pasos 5 a 10** de la tarea **Tarea 2: Importar solución** nuevamente y luego intente abrir el conector.

3. Haga clic en **Editar**.
4. Seleccione la pestaña **Probar** en el menú desplegable.

     ![](images/L03/L03-EX5-default.png)

5. Haga clic en **+ Nueva conexión**. Se abrirá una nueva pestaña del navegador para crear una conexión.
6. Abra una nueva ventana o pestaña del navegador y navegue hasta la siguiente URL para abrir la API Contoso Coffee Badges.

   ```
   https://contosobadgestest.azurewebsites.net/
   ```

7. Haga clic en el enlace **Get an API Key**.
  
     ![](images/L03/image60.png)

8. Copie el valor de la **API Key**.
9. Vuelva al editor de conectores, pegue la API Key que copió en el paso anterior y haga clic en **Crear conexión**. Ahora, cierre la pestaña del navegador haciendo clic en **X**.
   
     ![](images/L03/image61.png)

10. Haga clic en **Actualizar** conexiones.
     
      ![](images/L03/image62.png)

11. Vaya a la sección **Operaciones** y seleccione la operación **addcredit**.
12. Proporcione su correo electrónico para **recipientid**, proporcione el **name**, ingrese **1** para **points** y haga clic en **Probar operación**.
     
     ![](images/L03/image63u.png)

13. La prueba debería tener éxito y la respuesta debería verse como la imagen a continuación.
      
      ![](images/L03/image64u.png)

## Resumen

En este laboratorio, aprendió a crear y modificar un conector personalizado utilizando una definición de Open API, probar su funcionalidad e integrarlo con aplicaciones de canvas y flujos dentro de Power Platform.

## Ha completado el laboratorio con éxito