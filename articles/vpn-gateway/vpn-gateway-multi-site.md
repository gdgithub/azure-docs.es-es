---
title: Conectar una red virtual a varios sitios con VPN Gateway y PowerShell | Microsoft Docs
description: "Este artículo le guiará por la conexión de varios sitios locales a una red virtual con una puerta de enlace de VPN para el modelo de implementación clásica."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: azure-service-management
ms.assetid: b043df6e-f1e8-4a4d-8467-c06079e2c093
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/11/2016
ms.author: yushwang
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: c909622219128f87f05668eb9124542d2d59e1ea


---
# <a name="add-a-site-to-site-connection-to-a-vnet-with-an-existing-vpn-gateway-connection"></a>Agregar una conexión de sitio a sitio a una red virtual con una conexión de VPN Gateway existente
> [!div class="op_single_selector"]
> * [Resource Manager: portal](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
> * [Clásico: PowerShell](vpn-gateway-multi-site.md)
> 
> 

Este artículo le guiará a través del uso de PowerShell para agregar conexiones de sitio a sitio (S2S) a VPN Gateway con una conexión existente. Este tipo de conexión se denomina con frecuencia, configuración "multisitio". 

Este artículo se aplica a las redes virtuales creadas con el modelo de implementación clásica (conocido también como Administración de servicios). Estos pasos no se aplican a las configuraciones de conexión coexistentes de ExpressRoute/sitio a sitio. Consulte [conexiones coexistentes de ExpressRoute/S2S](../expressroute/expressroute-howto-coexist-classic.md) para obtener información sobre las conexiones coexistentes.

### <a name="deployment-models-and-methods"></a>Modelos de implementación y métodos
[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

Esta tabla se actualiza cada vez que hay nuevos artículos y nuevas herramientas disponibles para esta configuración. Cuando aparezca un artículo, creamos un vínculo directo a él desde esta tabla.

[!INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)]

## <a name="about-connecting"></a>Acerca de la conexión
Puede conectar varios sitios locales a una única red virtual. Esto resulta especialmente atractivo para crear soluciones híbridas en la nube. La creación de una conexión de varios sitios en la puerta de enlace de red virtual de Azure es muy similar a la creación de otras conexiones de sitio a sitio. De hecho, puede utilizar una puerta de enlace de VPN de Azure existente, siempre que la puerta de enlace sea dinámica (basada en ruta).

Si ya se ha conectado una puerta de enlace estática a la red virtual, puede cambiar el tipo de puerta de enlace a dinámico sin tener que recompilar la red virtual para dar cabida a varios sitios. Antes de cambiar el tipo de enrutamiento, asegúrese de que la puerta de enlace de VPN local admite configuraciones de VPN basadas en ruta. 

![diagrama multisitio](./media/vpn-gateway-multi-site/multisite.png "multi-site")

## <a name="points-to-consider"></a>Puntos que se deben tener en cuenta
**No podrá usar el Portal de Azure clásico para realizar cambios en esta red virtual.**  Para esta versión, tendrá que realizar cambios en el archivo de configuración de red en lugar de usar el Portal de Azure clásico. Si realiza cambios en el Portal de Azure clásico, sobrescribirán la configuración de referencia a varios sitios para esta red virtual. 

Debe sentirse bastante cómodo al usar el archivo de configuración de red en el momento en el que complete el procedimiento de varios sitios. Sin embargo, si hay más personas que trabajan en la configuración de red, tendrá que asegurarse de que todos conocen esta limitación. Esto no significa que no pueda usar el Portal de Azure clásico. Puede usarlo para todo lo demás menos para hacer cambios de configuración en esta red virtual concreta.

## <a name="before-you-begin"></a>Antes de empezar
Antes de comenzar la configuración, compruebe que dispone de lo siguiente:

* Una suscripción de Azure. Si todavía no la tiene, puede activar sus [ventajas como suscriptor de MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) o registrarse para obtener una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/).
* Hardware VPN compatible para cada ubicación local. Consulte [Acerca de los dispositivos VPN para conectividad de red virtual](vpn-gateway-about-vpn-devices.md) para comprobar si el dispositivo que quiere usar es un dispositivo que se sabe que es compatible.
* Una dirección IP IPv4 pública orientada externamente para cada dispositivo VPN. La dirección IP no se puede ubicar detrás de un NAT. Esto es un requisito.
* Deberá instalar la versión más reciente de los cmdlets de Azure PowerShell. Consulte [Cómo instalar y configurar Azure PowerShell](../powershell-install-configure.md) para más información sobre cómo instalar los cmdlets de PowerShell.
* Alguna persona con experiencia en configuración de hardware de VPN No podrá usar scripts de VPN generados automáticamente desde el Portal de Azure clásico para configurar los dispositivos de VPN. Esto significa que tendrá un conocimiento amplio de cómo configurar el dispositivo VPN o trabajar con alguien que lo tenga.
* Los intervalos de dirección IP que desea usar para la red virtual (si aún no ha creado uno). 
* Los intervalos de direcciones IP para cada uno de los sitios de red locales a los que se va a conectar. Tendrá que asegurarse de que los intervalos de dirección IP para cada uno de los sitios de red locales a los que desea conectarse no se solapan. De lo contrario, el Portal de Azure clásico o la API de REST rechazarán la configuración que se carga. 
  
    Por ejemplo, si dispone de dos sitios de red locales que contienen el intervalo de dirección IP 10.2.3.0/24 y dispone de un paquete con una dirección de destino 10.2.3.3, Azure no sabrá a qué sitio desea enviar el paquete porque se solapan los intervalos de dirección. Para evitar problemas de enrutamiento, Azure no loe permite cargar un archivo de configuración que disponga de intervalos que se solapan.

## <a name="1-create-a-site-to-site-vpn"></a>1. Crear una VPN de sitio a sitio
Si ya tiene una VPN de sitio a sitio con una puerta de enlace de enrutamiento dinámico, perfecto. Puede pasar a [Exportación de la configuración de la red virtual](#export). De lo contrario, haga lo siguiente:

### <a name="if-you-already-have-a-site-to-site-virtual-network-but-it-has-a-static-policy-based-routing-gateway"></a>Si ya dispone de una red virtual de sitio a sitio, pero con una puerta de enlace de enrutamiento estático (basada en directivas):
1. Cambie el tipo de puerta de enlace a enrutamiento dinámico. Una VPN de varios sitios requiere una puerta de enlace de enrutamiento dinámico (también denominada basada en ruta). Para cambiar el tipo de puerta de enlace, tendrá que eliminar primero la puerta de enlace existente y, a continuación, crear una nueva. Para obtener instrucciones, consulte [Configuración de una puerta de enlace VPN para el modelo de implementación clásico](vpn-gateway-configure-vpn-gateway-mp.md#how-to-change-the-vpn-routing-type-for-your-gateway).  
2. Configure la nueva puerta de enlace y cree un túnel de VPN. Para obtener instrucciones, vea [Configuración de una puerta de enlace de VPN en el Portal de Azure clásico](vpn-gateway-configure-vpn-gateway-mp.md). Para empezar, cambie el tipo de puerta de enlace a enrutamiento dinámico. 

### <a name="if-you-dont-have-a-site-to-site-virtual-network"></a>Si no dispone de una red virtual de sitio a sitio:
1. Cree una red virtual de sitio a sitio con estas instrucciones: [Creación de una red virtual con una conexión de VPN de sitio a sitio en el Portal de Azure clásico](vpn-gateway-site-to-site-create.md).  
2. Configure una puerta de enlace de enrutamiento dinámico con estas instrucciones: [Configuración de una puerta de enlace de VPN](vpn-gateway-configure-vpn-gateway-mp.md). Asegúrese de seleccionar **enrutamiento dinámico** como tipo de puerta de enlace.

## <a name="a-nameexporta2-export-the-network-configuration-file"></a><a name="export"></a>2. Exportar el archivo de configuración de red
Exporte su archivo de configuración de red. El archivo que exporte se usará para configurar los ajustes de varios sitios. Si necesita instrucciones sobre cómo exportar un archivo, consulte la sección del artículo: [Cómo crear una red virtual con un archivo de configuración de red en el portal de Azure](../virtual-network/virtual-networks-create-vnet-classic-portal.md#how-to-create-a-vnet-using-a-network-config-file-in-the-azure-portal). 

## <a name="3-open-the-network-configuration-file"></a>3. Abrir el archivo de configuración de red
Abra el archivo de configuración de red que descargó en el último paso. Use el editor xml que desee. El archivo debe tener un aspecto similar al siguiente:

        <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
          <VirtualNetworkConfiguration>
            <LocalNetworkSites>
              <LocalNetworkSite name="Site1">
                <AddressSpace>
                  <AddressPrefix>10.0.0.0/16</AddressPrefix>
                  <AddressPrefix>10.1.0.0/16</AddressPrefix>
                </AddressSpace>
                <VPNGatewayAddress>131.2.3.4</VPNGatewayAddress>
              </LocalNetworkSite>
              <LocalNetworkSite name="Site2">
                <AddressSpace>
                  <AddressPrefix>10.2.0.0/16</AddressPrefix>
                  <AddressPrefix>10.3.0.0/16</AddressPrefix>
                </AddressSpace>
                <VPNGatewayAddress>131.4.5.6</VPNGatewayAddress>
              </LocalNetworkSite>
            </LocalNetworkSites>
            <VirtualNetworkSites>
              <VirtualNetworkSite name="VNet1" AffinityGroup="USWest">
                <AddressSpace>
                  <AddressPrefix>10.20.0.0/16</AddressPrefix>
                  <AddressPrefix>10.21.0.0/16</AddressPrefix>
                </AddressSpace>
                <Subnets>
                  <Subnet name="FE">
                    <AddressPrefix>10.20.0.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="BE">
                    <AddressPrefix>10.20.1.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="GatewaySubnet">
                    <AddressPrefix>10.20.2.0/29</AddressPrefix>
                  </Subnet>
                </Subnets>
                <Gateway>
                  <ConnectionsToLocalNetwork>
                    <LocalNetworkSiteRef name="Site1">
                      <Connection type="IPsec" />
                    </LocalNetworkSiteRef>
                  </ConnectionsToLocalNetwork>
                </Gateway>
              </VirtualNetworkSite>
            </VirtualNetworkSites>
          </VirtualNetworkConfiguration>
        </NetworkConfiguration>

## <a name="4-add-multiple-site-references"></a>4. Agregar referencias de varios sitios
Cuando agregue o quite la información de referencia del sitio, realizará cambios de configuración en ConnectionsToLocalNetwork/LocalNetworkSiteRef. Si se agrega una nueva referencia a sitio local se activa Azure para la creación de un nuevo túnel. En el ejemplo siguiente, la configuración de red es para una conexión de sitio único. Cuando haya terminado de realizar los cambios, guarde el archivo.

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

    To add additional site references (create a multi-site configuration), simply add additional "LocalNetworkSiteRef" lines, as shown in the example below: 

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Site2"><Connection type="IPsec" /></LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

## <a name="5-import-the-network-configuration-file"></a>5. Importar el archivo de configuración de red
Importe un archivo de configuración de red. Cuando importe este archivo con los cambios, se agregarán nuevos túneles. Los túneles usarán la puerta de enlace dinámica que creó anteriormente. Si necesita instrucciones sobre cómo importar el archivo, consulte la sección del artículo: [Cómo crear una red virtual con un archivo de configuración de red en el portal de Azure](../virtual-network/virtual-networks-create-vnet-classic-portal.md#how-to-create-a-vnet-using-a-network-config-file-in-the-azure-portal). 

## <a name="6-download-keys"></a>6. Descargar las claves
Una vez que se han agregado los nuevos túneles, use el cmdlet de PowerShell `Get-AzureVNetGatewayKey` para obtener las claves IPsec/IKE compartidas previamente para cada túnel.

Por ejemplo:

    Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site1"

    Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site2"

Si lo prefiere, también puede usar la API de REST de *Obtención de la clave compartida de puerta de enlace de la red virtual* para obtener claves compartidas previamente.

## <a name="7-verify-your-connections"></a>7. Comprobación de las conexiones
Compruebe el estado del túnel de varios sitios. Después de descargar las claves para cada túnel, querrá comprobar las conexiones. Use `Get-AzureVnetConnection` para obtener una lista de túneles de redes virtuales, tal como se muestra en el siguiente ejemplo. VNet1 es el nombre de la red virtual.

    Get-AzureVnetConnection -VNetName VNET1

    ConnectivityState         : Connected
    EgressBytesTransferred    : 661530
    IngressBytesTransferred   : 519207
    LastConnectionEstablished : 5/2/2014 2:51:40 PM
    LastEventID               : 23401
    LastEventMessage          : The connectivity state for the local network site 'Site1' changed from Not Connected to Connected.
    LastEventTimeStamp        : 5/2/2014 2:51:40 PM
    LocalNetworkSiteName      : Site1
    OperationDescription      : Get-AzureVNetConnection
    OperationId               : 7f68a8e6-51e9-9db4-88c2-16b8067fed7f
    OperationStatus           : Succeeded

    ConnectivityState         : Connected
    EgressBytesTransferred    : 789398
    IngressBytesTransferred   : 143908
    LastConnectionEstablished : 5/2/2014 3:20:40 PM
    LastEventID               : 23401
    LastEventMessage          : The connectivity state for the local network site 'Site2' changed from Not Connected to Connected.
    LastEventTimeStamp        : 5/2/2014 2:51:40 PM
    LocalNetworkSiteName      : Site2
    OperationDescription      : Get-AzureVNetConnection
    OperationId               : 7893b329-51e9-9db4-88c2-16b8067fed7f
    OperationStatus           : Succeeded

## <a name="next-steps"></a>Pasos siguientes
Para más información sobre las puertas de enlace de VPN, consulte [Acerca de las puertas de enlace de VPN](vpn-gateway-about-vpngateways.md).




<!--HONumber=Nov16_HO3-->


