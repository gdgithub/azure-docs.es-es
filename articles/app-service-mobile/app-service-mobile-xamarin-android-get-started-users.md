---
title: "Introducción a la autenticación de Aplicaciones móviles en Xamarin Android"
description: "Obtenga información acerca de cómo utilizar Aplicaciones móviles para autenticar usuarios de su aplicación Xamarin Android a través de una variedad de proveedores de identidad, incluidos AAD, Google, Facebook, Twitter y Microsoft."
services: app-service\mobile
documentationcenter: xamarin
author: adrianhall
manager: dwrede
editor: 
ms.assetid: 570fc12b-46a9-4722-b2e0-0d1c45fb2152
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.author: adrianha
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 4c0ba82ca010f1ee571424fa3d650718e5acdd8b


---
# <a name="add-authentication-to-your-xamarinandroid-app"></a>Adición de la autenticación a la aplicación Xamarin.Android
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

En este tema se muestra cómo autenticar usuarios de una aplicación móvil desde la aplicación cliente. En este tutorial podrá agregar la autenticación al proyecto de inicio rápido mediante un proveedor de identidades compatible con Aplicaciones móviles de Azure. Una vez que la aplicación móvil finalice la autenticación y autorización correctamente, se mostrará el valor del identificador de usuario.

Este tutorial se basa en el inicio rápido de aplicaciones móviles. Primero debe completar el tutorial [Creación de una aplicación Xamarin.Android]. Si no usa el proyecto de servidor de inicio rápido descargado, debe agregar el paquete de extensión de autenticación al proyecto. Para obtener más información acerca de los paquetes de extensión de servidor, consulte [Trabajar con el SDK del servidor back-end de .NET para Aplicaciones móviles de Azure](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

## <a name="a-nameregisteraregister-your-app-for-authentication-and-configure-app-services"></a><a name="register"></a>Registro de la aplicación para la autenticación y configuración de Servicios de aplicaciones
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="a-namepermissionsarestrict-permissions-to-authenticated-users"></a><a name="permissions"></a>Restricción de los permisos para los usuarios autenticados
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

En Visual Studio o Xamarin Studio, ejecute el proyecto de cliente en un dispositivo o emulador. Compruebe que se produce una excepción no controlada con el código de estado 401 (No autorizado) después de iniciarse la aplicación. Esto sucede porque la aplicación intenta obtener acceso al back-end de la aplicación móvil como usuario sin autenticar. La tabla *TodoItem* ahora requiere autenticación.

Luego, actualizará la aplicación cliente para solicitar recursos del back-end de la aplicación móvil con un usuario autenticado.

## <a name="a-nameadd-authenticationaadd-authentication-to-the-app"></a><a name="add-authentication"></a>Incorporación de autenticación a la aplicación
La aplicación se actualiza para requerir a los usuarios que pulsen el botón **Iniciar sesión** y que se autentiquen para que se muestren los datos.

1. Agregue el siguiente código a la clase **TodoActivity** :
   
        // Define a authenticated user.
        private MobileServiceUser user;
        private async Task<bool> Authenticate()
        {
                var success = false;
                try
                {
                    // Sign in with Facebook login using a server-managed flow.
                    user = await client.LoginAsync(this,
                        MobileServiceAuthenticationProvider.Facebook);
                    CreateAndShowDialog(string.Format("you are now logged in - {0}",
                        user.UserId), "Logged in!");
   
                    success = true;
                }
                catch (Exception ex)
                {
                    CreateAndShowDialog(ex, "Authentication failed");
                }
                return success;
        }
   
        [Java.Interop.Export()]
        public async void LoginUser(View view)
        {
            // Load data only after authentication succeeds.
            if (await Authenticate())
            {
                //Hide the button after authentication succeeds.
                FindViewById<Button>(Resource.Id.buttonLoginUser).Visibility = ViewStates.Gone;
   
                // Load the data.
                OnRefreshItemsSelected();
            }
        }
   
    Esto crea un método nuevo para autenticar un usuario y un controlador de método para un nuevo botón **Iniciar sesión** . El usuario del código de ejemplo anterior se autentica mediante el inicio de sesión en Facebook. Se usa un cuadro de diálogo para mostrar el identificador de usuario una vez autenticado.
   
   > [!NOTE]
   > Si usa un proveedor de identidades que no sea una cuenta de Facebook, cambie el valor que usó con **LoginAsync** por uno de los siguientes: *MicrosoftAccount*, *Twitter*, *Google* o *WindowsAzureActiveDirectory*.
   > 
   > 
2. En el método **OnCreate** , elimine o convierta en comentario la siguiente línea de código:
   
        OnRefreshItemsSelected ();
3. En el archivo Activity_To_Do.axml, agregue la siguiente definición del botón *LoginUser* antes del botón *AddItem* existente:
   
          <Button
            android:id="@+id/buttonLoginUser"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:onClick="LoginUser"
            android:text="@string/login_button_text" />
4. Agregue el siguiente elemento al archivo de recursos Strings.xml:
   
        <string name="login_button_text">Sign in</string>
5. En Visual Studio o Xamarin Studio, ejecute el proyecto de cliente en un dispositivo o emulador, e inicie sesión con el proveedor de identidades elegido.
   
       When you are successfully logged-in, the app will display your login ID and the list of todo items, and you can make updates to the data.

<!-- URLs. -->
[Creación de una aplicación Xamarin.Android]: app-service-mobile-xamarin-android-get-started.md



<!--HONumber=Nov16_HO3-->


