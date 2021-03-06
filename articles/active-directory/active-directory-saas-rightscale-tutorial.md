---
title: 'Tutorial: integración de Azure Active Directory con RightScale | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y RightScale.
services: active-directory
documentationcenter: ''
author: jeevansd
manager: femila
editor: ''

ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2016
ms.author: jeedes

---
# Tutorial: Integración de Azure Active Directory con RightScale
El objetivo de este tutorial es mostrar cómo integrar RightScale con Azure Active Directory (Azure AD). Integrar RightScale con Azure AD le proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a RightScale
* Puede permitir que los usuarios inicien sesión automáticamente en RightScale (inicio de sesión único) con sus cuentas de Azure AD
* Puede administrar sus cuentas en una ubicación central: el Portal de Azure clásico.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](active-directory-appssoaccess-whatis.md).

## Requisitos previos
Para configurar la integración de Azure AD con RightScale, necesita los siguientes elementos:

* Una suscripción de Azure AD
* Una suscripción habilitada para el inicio de sesión único en RightScale

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.
> 
> 

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

* No debe usar el entorno de producción, a menos que sea necesario.
* Si no dispone de un entorno de prueba de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/).

## Descripción del escenario
El objetivo de este tutorial es permitirle probar el inicio de sesión único de Azure AD en un entorno de prueba. La situación descrita en este tutorial consta de dos bloques de creación principales:

1. Adición de RightScale desde la Galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## Adición de RightScale desde la Galería
Para configurar la integración de RightScale en Azure AD, deberá agregar RightScale desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar RightScale desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo del **Portal de Azure clásico**, haga clic en **Active Directory**.
   
    ![Active Directory][1]
2. En la lista **Directory**, seleccione el directorio cuya integración desee habilitar.
3. Para abrir la vista de aplicaciones, haga clic en **Applications**, en el menú superior de la vista de directorios.
   
    ![Aplicaciones][2]
4. Haga clic en **Agregar** en la parte inferior de la página.
   
    ![Aplicaciones][3]
5. En el cuadro de diálogo **¿Qué desea hacer?**, haga clic en **Agregar una aplicación de la galería**.
   
    ![Aplicaciones][4]
6. En el cuadro de búsqueda, escriba **RightScale**.
   
    ![Creación de un usuario de prueba de Azure AD](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_01.png)
7. En el panel de resultados, seleccione **RightScale** y luego haga clic en **Completar** para agregar la aplicación.
   
    ![Selección de la aplicación en la galería](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_02.png)

## Configuración y comprobación del inicio de sesión único de Azure AD
El objetivo de esta sección es mostrar cómo configurar y probar el inicio de sesión único de Azure AD con RightScale con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de RightScale para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de RightScale.

Para configurar y probar el inicio de sesión único de Azure AD con RightScale, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-single-sign-on)**: para permitir a los usuarios usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)**: para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de RightScale](#creating-a-rightscale-test-user)**: para tener un homólogo de Britta Simon en RightScale que esté vinculado a la representación de ella en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)**: para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#testing-single-sign-on)**: para comprobar si funciona la configuración.

### Configuración del inicio de sesión único de Azure AD
El objetivo de esta sección es habilitar el inicio de sesión único de Azure AD en el Portal clásico y configurar el inicio de sesión único en la aplicación RightScale.

**Para configurar el inicio de sesión único de Azure AD con RightScale, realice los pasos siguientes:**

1. En el Portal clásico, en la página de integración de aplicaciones de **RightScale**, haga clic en **Configurar inicio de sesión único** para abrir el cuadro de diálogo **Configurar inicio de sesión único**.
   
    ![Configurar inicio de sesión único][6]
2. En la página **¿Cómo desea que los usuarios inicien sesión en RightScale?**, seleccione **Inicio de sesión único de Azure AD** y después haga clic en **Siguiente**.
   
    ![Configurar inicio de sesión único](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_03.png)
3. En la página de diálogo **Configurar las opciones de la aplicación**, si desea configurar la aplicación en el **modo iniciado por el proveedor de identidades**, siga estos pasos y haga clic en **Siguiente**:
   
    ![Configurar inicio de sesión único](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_04.png)

    a. En el cuadro de texto URL de respuesta, escriba la dirección URL con el siguiente patrón: `https://login.rightscale.com/login/saml2/consume`

    b. Haga clic en **Siguiente**.

1. Si quiere configurar la aplicación en el **modo iniciado por el proveedor de servicios**, en la página de diálogo **Configurar las opciones de la aplicación**, haga clic en **"Mostrar la configuración avanzada (opcional)"** y después escriba la **URL de inicio de sesión** y haga clic en **Siguiente**.
   
    ![Configurar inicio de sesión único](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_06.png)
   
    a. En el cuadro de texto URL de inicio de sesión, escriba la dirección URL que usan los usuarios para iniciar sesión en su aplicación de RightScale con el siguiente patrón: `https://login.rightscale.com/`
   
    b. Haga clic en **Siguiente**.
2. En la página **Configurar inicio de sesión único en RightScale**, lleve a cabo estos pasos y haga clic en **Siguiente**:
   
    ![Configurar inicio de sesión único](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_05.png)
   
    a. Haga clic en **Descargar certificado** y luego guarde el archivo de certificado codificado en base 64 en el equipo.
   
    b. Haga clic en **Siguiente**.
3. Para configurar SSO para la aplicación, debe iniciar sesión en su inquilino de RightScale como administrador.
   
    a. En el menú de la parte superior, haga clic en la pestaña **Configuración** y seleccione **Inicio de sesión único**.
   
    ![Configurar inicio de sesión único](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_001.png)
   
    b. Haga clic en el botón "**nuevo**" para agregar **sus proveedores de identidades SAML**.
   
    ![Configurar inicio de sesión único](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_002.png)
   
    c. En el cuadro de texto **Nombre para mostrar**, escriba el nombre de la compañía.
   
    ![Configurar inicio de sesión único](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_003.png)
   
    d. Seleccione **Permitir SSO iniciado por RightScale con una sugerencia de detección** y escriba el **nombre de dominio** en el cuadro de texto siguiente.
   
    ![Configurar inicio de sesión único](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_004.png)
   
    e. Copie la dirección URL de SSO de SAML de Azure AD para el **punto de conexión de SSO de SAML** en RightScale.
   
    ![Configurar inicio de sesión único](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_005.png)
   
    ![Configurar inicio de sesión único](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_006.png)
   
    f. Copie el identificador de entidad de Azure AD a **SAML EntityID** en RightScale.
   
    ![Configurar inicio de sesión único](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_007.png)
   
    ![Configurar inicio de sesión único](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_008.png)
   
    g. Haga clic en el botón **Explorador** para cargar el certificado que descargó en el paso 4.
   
    ![Configurar inicio de sesión único](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_009.png)
   
    h. Haga clic en **Save**.
4. En el portal clásico, seleccione la confirmación de la configuración de inicio de sesión único y haga clic en **Siguiente**.
   
    ![Inicio de sesión único de Azure AD][10]
5. En la página **Confirmación del inicio de sesión único**, haga clic en **Completar**.
   
    ![Inicio de sesión único de Azure AD][11]

### Creación de un usuario de prueba de Azure AD
El objetivo de esta sección es crear un usuario de prueba en el Portal clásico llamado Britta Simon.

![Creación de un usuario de Azure AD][20]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel de navegación izquierdo del **Portal de Azure clásico**, haga clic en **Active Directory**.
   
    ![Creación de un usuario de prueba de Azure AD](./media/active-directory-saas-rightscale-tutorial/create_aaduser_09.png)
2. En la lista **Directory**, seleccione el directorio cuya integración desee habilitar.
3. Para mostrar la lista de usuarios, en el menú de la parte superior, haga clic en **Usuarios**.
   
    ![Creación de un usuario de prueba de Azure AD](./media/active-directory-saas-rightscale-tutorial/create_aaduser_03.png)
4. Para abrir el diálogo **Agregar usuario**, en la barra de herramientas de la parte inferior, haga clic en **Agregar usuario**.
   
    ![Creación de un usuario de prueba de Azure AD](./media/active-directory-saas-rightscale-tutorial/create_aaduser_04.png)
5. En la página de diálogo **Proporcione información sobre este usuario**, realice los pasos siguientes:
   
    ![Creación de un usuario de prueba de Azure AD](./media/active-directory-saas-rightscale-tutorial/create_aaduser_05.png)
   
    a. En **Tipo de usuario**, seleccione **Nuevo usuario de la organización**.
   
    b. En el cuadro de texto **Nombre de usuario**, escriba **BrittaSimon**.
   
    c. Haga clic en **Next**.
6. En la página de diálogo **Perfil de usuario**, realice los siguientes pasos:
   
   ![Creación de un usuario de prueba de Azure AD](./media/active-directory-saas-rightscale-tutorial/create_aaduser_06.png)
   
   a. En el cuadro de texto **Nombre**, escriba **Britta**.
   
   b. En el cuadro de texto **Apellidos**, escriba **Simon**.
   
   c. En el cuadro de texto **Nombre para mostrar**, escriba **Britta Simon**.
   
   d. En la lista **Rol**, seleccione **Usuario**.
   
   e. Haga clic en **Siguiente**.
7. En la página de diálogo **Obtener contraseña temporal**, haga clic en **Crear**.
   
    ![Creación de un usuario de prueba de Azure AD](./media/active-directory-saas-rightscale-tutorial/create_aaduser_07.png)
8. En la página de diálogo **Obtener contraseña temporal**, realice los pasos siguientes:
   
    ![Creación de un usuario de prueba de Azure AD](./media/active-directory-saas-rightscale-tutorial/create_aaduser_08.png)
   
    a. Anote el valor del campo **Nueva contraseña**.
   
    b. Haga clic en **Completo**.

### Creación de un usuario de prueba de RightScale
En esta sección, creará un usuario denominado Britta Simon en RightScale. Trabaje con el equipo de soporte técnico de RightScale a través de support@rightscale.com para agregar los usuarios en la plataforma de RightScale.

### Asignación del usuario de prueba de Azure AD
El objetivo de esta sección es permitir que Britta Simon use el inicio de sesión único de Azure concediéndole acceso a RightScale.

![Asignar usuario][200]

**Para asignar a Britta Simon a RightScale, realice los pasos siguientes:**

1. En el Portal clásico, para abrir la vista de aplicaciones, en la vista del directorio, haga clic en **Aplicaciones** en el menú superior.
   
    ![Asignar usuario][201]
2. En la lista de aplicaciones, seleccione **RightScale**.
   
    ![Configurar inicio de sesión único](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_50.png)
3. En el menú de la parte superior, haga clic en **Usuarios**.
   
    ![Asignar usuario][203]
4. En la lista Usuarios, seleccione **Britta Simon**.
5. En la barra de herramientas de la parte inferior, haga clic en **Asignar**.
   
    ![Asignar usuario][205]

### Prueba del inicio de sesión único
El objetivo de esta sección es probar la configuración del inicio de sesión único de Azure AD mediante el panel de acceso. Al hacer clic en el icono de RightScale en el Panel de acceso, debería iniciar sesión automáticamente en su aplicación RightScale.

## Recursos adicionales
* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](active-directory-saas-tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_205.png

<!---HONumber=AcomDC_0921_2016-->