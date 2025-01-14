# Laboratorio 04 - Gestión del ciclo de vida de las aplicaciones

## Duración Estimada: 95 mins

Al trabajar como parte del equipo de fusión de PrioritZ, configurará GitHub Actions con Power Platform Build Tools para automatizar y optimizar las implementaciones del equipo. Esto implica configurar canalizaciones de integración continua e implementación continua (CI/CD) para garantizar la entrega fluida y eficiente de actualizaciones a las aplicaciones de Power Platform, al mismo tiempo que administra los procesos de control de versiones, pruebas e implementación para mejorar la colaboración y mantener estándares de alta calidad en todos los proyectos del equipo.

## Objetivos del Laboratorio 

- Ejercicio 1: Configuración de una entidad de servicio 
- Ejercicio 2: Crear repositorio de GitHub 
- Ejercicio 3: Exportación y Sucursal 
- Ejercicio 4: Lanzamiento para probar 

## Ejercicio 1: Configurar una entidad de servicio

En este ejercicio, creará una entidad de servicio. El flujo de trabajo usará la entidad de servicio
acciones, por lo que no se ejecutan bajo su identidad de usuario individual.

### Tarea 1: Crear el registro de la aplicación

1. Vuelva a la pestaña del explorador en la que está abierto el Portal de Azure. Si aún no está abierto, vaya a Azure Portal mediante la siguiente dirección URL.

   ```
   https://portal.azure.com/
   ```

1. En la página principal del Portal Azure, busque **Microsoft Entra ID** ***(1)*** en la barra de búsqueda y seleccione **Microsoft Entra ID** ***(2)*** en las sugerencias.

   ![](images/dev3.png)
   
1. Seleccione **Registros de aplicaciones** ***(1)*** en la hoja lateral y haga clic en **+ Nuevo registro** ***(2)***. Este registro de aplicación se utilizará para que el conector acceda a la API protegida.

   ![](images/L05/diad5l2.png)

1. Proporcione los siguientes datos y haga clic en **Registrarse** ***(3)***.
   
   - Nombre: **GitHub Deploy<inject key="DeploymentID" enableCopy="false" />** ***(1)***
   - Tipos de cuenta admitidos: **Las cuentas en este directorio organizativo solamente (OTU WA AIW [SUFFIX] solamente - único inquilino)** ***(2)***

     ![](images/L05/diad5l3.png).
   
1. Copie el **ID de la aplicación (cliente)**, el **ID del directorio (inquilino)** y guárdelo en un bloc de notas cuando lo necesite para su uso posterior.
     
   ![](images/L05/diad5l4.png)

1. Seleccione **Certificados y secretos** en la hoja lateral y haga clic en **+ Nuevo secreto de cliente**.

   ![](images/L05/diad5l5.png)

1. Ingrese **GitHub client secret<inject key="DeploymentID" enableCopy="false" />** ***(1)*** como descripción, establezca el vencimiento en **3 meses** ***(2)* ** y haga clic en **Agregar** ***(3)***.
   
   ![](images/L05/diad5l6.png)
   
1. Copie el **valor** y guárdelo en un bloc de notas, ya que lo necesitará para usarlo más adelante.

   ![](images/L05/diad5l7.png)

    >**Nota**: Asegúrese de copiar y pegar el **ID de aplicación (cliente)**, el **ID de directorio (inquilino)** y el valor **Secreto** correctos. Copiar el valor incorrecto provocará problemas en los siguientes pasos/tareas.
    
### Tarea 2: crear un nuevo Dataverse

En esta tarea, probará nuevos entornos de Dataverse.

1. Abra una nueva ventana o pestaña del navegador y navegue hasta el Centro de administración de Power Platform utilizando la siguiente URL.

     ```
     https://admin.powerplatform.microsoft.com/environments
     ```
1. Haga clic en **+Nuevo** para crear un nuevo Dataverse.       

   ![](images/L05/newtask1.png)

1. En la pestaña **Nuevo entorno**.
   
   - Nombre:**DEV_ENV_TEST(1)**.
   
   - Haga de este un entorno gestionado :**Habilitar Sí(2)**.
   
   - Grupo :**Ninguno(3)**. y desplácese hacia abajo.
   
   - Escriba :**Developer(4)** y haga clic en **Siguiente (5)**.
   
   - ¿Implementar aplicaciones y datos de muestra? :**Habilite Sí(6)** y haga clic en **Guardar(7)**.
   
     ![](images/L05/newtask2.png)

     ![](images/L05/newtask3.png)

     ![](images/L05/newtask4.png)

1. Ahora puede ver el nuevo Dataverse, **DEV_ENV_TEST**, que creó.

   ![](images/L05/newtask5.png)

### Tarea 3: Crear una usuaria de aplicación en Dataverse

En esta tarea, registrará la aplicación que creó en Microsoft Entra ID en el programa de desarrollo y prueba.
Entornos de Dataverse. También se le asignará un rol de seguridad que permitirá a la entidad de servicio
implementar soluciones.

1. Abra una nueva ventana o pestaña del navegador y navegue hasta el Centro de administración de Power Platform utilizando la siguiente URL.

     ```
     https://admin.powerplatform.microsoft.com/environments
     ```

1. Haga clic en **Entornos** ***(1)*** en la hoja lateral y seleccione su entorno **DEV_ENV_<inject key="DeploymentID" enableCopy="false" />** ***(2 )***.

   ![](images/L05/env1u.png)
   
1. Desde la página de su entorno, haga clic en **Configuración**.

   ![](images/L05/diad5l9u.png)
   
1. Expanda **Usuarios + permisos** **(1)** y seleccione **Usuarios de la aplicación** **(2)**.
    
   ![](images/L05/diad5l10u.png)

1. En la página de usuarios de la aplicación, haga clic en **+ Nuevo usuario de la aplicación**.

   ![](images/L05/diad5l11u.png)
   
1. En la pestaña Crear un nuevo usuario de aplicación, haga clic en **+ Agregar una aplicación**.
      
   ![](images/L05/diad5l12.png)
   
1. Seleccione el registro de la aplicación **GitHub Deploy<inject key="DeploymentID" enableCopy="false" />** ***(1)*** que creó anteriormente y haga clic en **Agregar** **(2)**.

   ![](images/L05/diad5l13u.png)

1. Escriba **org** y seleccione su **unidad de negocio** **(1)** y en **Roles de seguridad** haga clic en **editar símbolo (2)** y seleccione 
   **Administrador del sistema(3)** y luego haga clic en **Crear (4)**.

   ![](images/L05/diad5l14u.png)

   **Nota:** Si el símbolo **#** aún está visible antes de GitHub Deploy<inject key="DeploymentID" enableCopy="false" />, haga clic en él y actualice el panel para eliminarlo.
   
1. Vuelva nuevamente a **Entornos** ***(1)*** en la hoja lateral y seleccione su **entorno de prueba** ***(2)***.

   ![](images/L05/diad5l17u.png)
   
1. Desde la página de su entorno de prueba, haga clic en **Configuración**.

   ![](images/L05/diad5l18u.png)

1. Expanda **Usuarios + permisos** ***(1)*** y seleccione **Usuarios de la aplicación** ***(2)***.
    
   ![](images/L05/diad5l19u.png)
   
1. En la página de usuarios de la aplicación, haga clic en **+ Nuevo usuario de la aplicación**.

   ![](images/L05/diad5l11uu.png)
   
1. En la pestaña **Crear un nuevo usuario de aplicación**, haga clic en **+ Agregar una aplicación**.
      
   ![](images/L05/diad5l12.png)
   
1. Seleccione el registro de la aplicación **GitHub Deploy<inject key="DeploymentID" enableCopy="false" />** ***(1)*** que creó anteriormente y haga clic en **Agregar** ***(2 )***.

   ![](images/L05/diad5l13uu.png)

1. Escriba **org** y seleccione su **unidad de negocio** **(1)** y en **Roles de seguridad** haga clic en **editar símbolo (2)** y seleccione 
   **Administrador del sistema(3)** y luego haga clic en **Crear (4)**.

   ![](images/L05/diad5l14uu.png)

   **Nota:** Si el símbolo **#** aún está visible antes de GitHub Deploy<inject key="DeploymentID" enableCopy="false" />, haga clic en él y actualice el panel para eliminarlo.
   
1. Haga clic en **Entornos** ***(1)*** en la hoja lateral y seleccione su entorno **DEV_ENV_<inject key="DeploymentID" enableCopy="false" />** ***(2 )***.

   ![](images/L05/envu.png)
   
1. Copie la **URL del entorno** y guárdela en un bloc de notas; utilizará esta URL en pasos futuros.
    
   ![](images/L05/diad5l211u.png)
   
1. Vuelva nuevamente a **Entornos** ***(1)*** en la hoja lateral y seleccione su entorno **de prueba(2)**.

   ![](images/L05/diad5l17uu.png)
   
1. Copie la **URL del entorno** y guárdela en un bloc de notas; utilizará esta URL en pasos futuros.
    
   ![](images/L05/diad5l22u.png)
   
## Ejercicio 2: crear un repositorio de GitHub

En este ejercicio, creará un repositorio de GitHub y agregará secretos de repositorio.

### Tarea 1: crear un repositorio

1. Navegue a la siguiente URL e inicie sesión con sus credenciales de GitHub.

   ```
   https://github.com/
   ```
1. Haz clic en el icono de tu perfil y selecciona **Tus repositorios**.

   ![](images/L05/github1u.png)

3. Haga clic en **Nuevo repositorio** para crear un repositorio.

   ![](images/L05/github2u.png)

4. Ingrese **PrioritZ (1)** para el nombre del repositorio, seleccione **Público (2)** y marque **Agregar un archivo README (3)**.

   ![](images/L05/github3.png)

1. Haga clic en **Crear repositorio** para crearlo.

   ![](images/L05/github4.png)

5. Haga clic en **Configuración** para abrir la pestaña de configuración.
    
     ![](images/L05/Imagessettinguu.png)

6. Vaya a la sección **Seguridad**, expanda **Secretos y variables(1)** y seleccione **Acciones (2)**.
   
    > **Nota:** Los valores que proporcione no serán visibles después de crear el elemento, así que tómese su tiempo para obtener los valores correctos.
      
     ![](images/L05/github6u.png)
   
8. Haga clic en **Nuevo secreto de repositorio** para agregar un secreto.

     ![](images/L05/github7uu.png)

9. Ingrese **PowerPlatformAppID (1)** como Nombre y pegue la contraseña: **<inject key="AzureAdUserEmail"></inject> (2)** y **haga clic en Agregar secreto (3)**
    
     ![](images/L05/github8u.png)

11. Haga clic en **Nuevo secreto del repositorio** nuevamente.

12. Ingrese **PowerPlatformClientSecret (1)** como Nombre y pegue la contraseña: **<inject key="AzureAdUserPassword"></inject> (2)** y **haga clic en Agregar secreto (3)**

     ![](images/L05/github9u.png)

13. Haga clic en **Nuevo secreto del repositorio** nuevamente.

14. Ingrese **PowerPlatformTenantID (1)** para Nombre y pegue el secreto **Tenant ID (2)** de su bloc de notas que anotó anteriormente en **`Ejercicio 1 -> Tarea 1 -> Paso 5`** en el **Valor** y haga clic en **Agregar secreto (3)**.
    
     ![](images/L05/github10u.png)

16. Haga clic en **Nuevo secreto del repositorio** nuevamente.

17. Ingrese **PowerPlatformDevUrl (1)** para Nombre y pegue la **URL del entorno de desarrollo (2)** secreta de su bloc de notas que copió en el **`Ejercicio 1 -> Tarea 3 -> Paso 21`** en el campo **Valor** y haga clic en **Agregar secreto (3)**.

    >**Nota**: asegúrese de pegar la URL del entorno de desarrollo denominada **DEV_ENV_<inject key="DeploymentID" enableCopy="false" />** que copió en el **`Ejercicio 1 -> Tarea 3 - > Paso 17`**
   
     ![](images/L05/github11u.png) 
  
18. Haga clic en **Nuevo secreto del repositorio** una vez más.

19. Ingrese **PowerPlatformTestUrl (1)** para Nombre y pegue la **URL del entorno de prueba (2)** de su bloc de notas que copió en el **`Ejercicio 1 -> Tarea 3 -> Paso 18`** en el **Valor** y haga clic en **Agregar secreto (3)**.

     >**Nota**: Asegúrese de pegar la URL del entorno de prueba denominada **DEV_ENV_TEST** que copió en el **`Ejercicio 1 -> Tarea 3 -> Paso 23`**.
 
     ![](images/L05/L05-testurlu.png)
   
20. Ahora deberías tener **5** secretos del repositorio.
     
    ![](images/L05/Images20uu5u.png)

21. No salgas de esta página.

### Ejercicio 3: Exportación y sucursal

En este ejercicio, establecerá una acción de flujo de trabajo y agregará pasos que exportarán la solución desde el entorno de desarrollo y crearán una nueva rama.

### Tarea 1: Exportar y ramificar

En esta tarea, creará la definición del flujo de trabajo utilizando el YAML proporcionado. La acción YAML utiliza sangría de dos espacios, así que sígala cuidadosamente mientras crea la definición del flujo de trabajo. En caso de duda, revise la sangría que se muestra en las imágenes.

1. Seleccione la pestaña **Acciones** y haga clic en **Configurar un flujo de trabajo usted mismo** para crear un nuevo flujo de trabajo.
 
   ![](images/L05/Imagesworku.png)
   
1. Cambie el nombre del archivo **export-and-branch.yml**
       
1. Elimine todo del archivo de flujo de trabajo.
  
   ![](images/L05/diad5l32.png)

1. Navegue a la URL `https://raw.githubusercontent.com/CloudLabsAI-Azure/PowerApps-Dev-in-a-Day/main/export-and-branch.yml`, copie el contenido completo del archivo y péguelo en el flujo de trabajo **export-and-branch.yml**.

   ![](images/L05/diad5l28u.png)

1. Haga clic en **Confirmar cambios** y luego haga clic en **Confirmar cambios**.
    
   ![](images/L05/commit1.png)

   ![](images/L05/Images202uuu.png)

1. Vaya a la pestaña **Acciones(1)** en el lado izquierdo y luego seleccione **General(2)**.

   ![](images/L05/actionpermissionuuu.png)

1. En la sección **Permiso de flujo de trabajo**, asegúrese de que esté seleccionado **permiso de lectura y escritura** y luego haga clic en **guardar**.

   ![](images/L05/workflowpermissionuuu.png)

1. Seleccione la pestaña **Acciones** **(1)** y seleccione el **flujo de trabajo** ***(2)*** que creó.

   ![](images/L05/diad5l27.png)
   
1. Haga clic en **Ejecutar flujo de trabajo.**
      
   ![](images/L05/Images2027u.png)
   
1. Haga clic en **Ejecutar flujo de trabajo** nuevamente y espere a que se complete la ejecución del flujo de trabajo.
      
   ![](images/L05/Images2028u.png)
   
1. Seleccione la pestaña **Código** ***(1)*** y haga clic en **Sucursales** ***(2)***. Deberías ver dos ramas.
   
   ![](images/L05/diad5l29u.png)
   
1. Haga clic para abrir la rama creada por la acción del flujo de trabajo denominada Prioritz-XXXXXXX.
   
   ![](images/L05/changesbranchu.png)

1. En la carretera Prioritz-XXXXXXX. rama, debería poder ver la carpeta de la solución.
      
   ![](images/L05/diad5l30u.png)
   
1. Haga clic en el botón **Contribuir** ***(1)*** y seleccione **Abrir solicitud de extracción** ***(2)***.
        
   ![](images/L05/L05-t1-1u.png)
   
23. Agregue una descripción si lo desea y luego haga clic en **Crear solicitud de extracción**.

     ![](images/L05/pr1u.png)
   
24. Ahora debería ver el resumen de la solicitud de extracción. Confirme que la rama no tenga conflictos con la rama principal y que los cambios se puedan fusionar en la rama principal automáticamente.
   
25. Haga clic en el botón de chevrón al lado del botón **Fusionar solicitud de extracción** y seleccione **Aplastar y fusionar**.
      
    ![](images/L05/Images2032u.png)

26. Haga clic en **Aplastar y fusionar**.
   
27. Haga clic en **Confirmar aplastamiento y fusión**.
   
28. La solicitud de extracción debería fusionarse correctamente.
   
     ![](images/L05/prdoneu.png)

### Ejercicio 4: Lanzamiento para prueba

En este ejercicio, creará una acción de flujo de trabajo y agregará pasos que lanzarán la solución que
exportado al entorno de prueba.

### Tarea 1: crear un flujo de trabajo

1. Ahora navegue hasta la pestaña **Acciones (1)**.
   
1. Haga clic en **Nuevo flujo de trabajo (2)**.

    ![](images/L05/nwwfu.png)
   
1. Ahora, en la página **Elija un flujo de trabajo**, haga clic en **configurar un flujo de trabajo usted mismo**.
     
    ![](images/L05/Images2033u.png)
   
1. Cambie el nombre del archivo a **release-to-test.yml**
   
    ![](images/L05/fnameu.png)
     
1. Eliminar todo del archivo de flujo de trabajo.

1. Navegue a la URL `https://raw.githubusercontent.com/CloudLabsAI-Azure/PowerApps-Dev-in-a-Day/main/release-to-test.yml` en el navegador y copie el contenido completo del archivo y péguelo en el archivo de flujo de trabajo **release-to-test.yml**.
 
   ![](images/L05/cntnu.png)
      
16. Haga clic en **Confirmar cambios** y luego haga clic en **Confirmar cambios**.

    ![](images/L05/commit1.png)
    
18. Seleccione la pestaña **Código** y asegúrese de seleccionar Prioritz-XXXXXXX.

    ![](images/L05/codeu.png)
   
19. Vaya a la sección **Versiones** y haga clic en **Crear nueva versión**.
     
     ![](images/L05/Images2047u.png)    
   
20. Haga clic en el botón **Elegir una etiqueta**, ingrese **v1.0.0** y seleccione **+ Crear nueva etiqueta al publicar**.
      
     ![](images/L05/Images2048.png)  

21. Haga clic en **Publicar comunicado**.
   
22. Seleccione la pestaña **Acciones** y supervise el flujo de trabajo.
      
     ![](images/L05/Images2049.png)

23. La liberación debería completarse exitosamente.
    
     ![](images/L05/relecomplu.png)
     
24. Vuelva al portal de PowerApps y asegúrese de estar en el entorno de prueba de PowerApps.

      ![](images/L05/lastu.png)

25. seleccione la pestaña **soluciones (1)** en el lado izquierdo y haga clic en **Administrado (2)**. Debería ver la solución implementada con el nombre de **Prioritz (3)**.

    ![](images/L05/lastuu.png)
    
## Resumen
En esta práctica de laboratorio, aprendió a promover una solución en un entorno de prueba, configurar una entidad de servicio y administrar su solución usando GitHub para el control de versiones y la automatización del flujo de trabajo.

## Has completado con éxito el laboratorio.
