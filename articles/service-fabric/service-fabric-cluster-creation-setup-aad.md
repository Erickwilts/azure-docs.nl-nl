---
title: Azure Active Directory instellen voor verificatie van Service Fabric | Microsoft Docs
description: Meer informatie over het instellen van Azure Active Directory (Azure AD) om clients voor Service Fabric-clusters te verifiëren.
services: service-fabric
documentationcenter: .net
author: athinanthny
manager: chackdan
editor: chackdan
ms.assetid: 15d0ab67-fc66-4108-8038-3584eeebabaa
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 6/28/2019
ms.author: atsenthi
ms.openlocfilehash: 6c195357c4a037534307571a53589b2ae861d88b
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67486011"
---
# <a name="set-up-azure-active-directory-for-client-authentication"></a>Azure Active Directory instellen voor clientverificatie

Voor clusters die worden uitgevoerd op Azure, Azure Active Directory (Azure AD) aanbevolen voor beveiligde toegang tot eindpunten voor beheer.  Dit artikel wordt beschreven hoe tot het instellen van Azure AD om clients voor een Service Fabric-cluster te verifiëren, die moet worden uitgevoerd voordat [het maken van het cluster](service-fabric-cluster-creation-via-arm.md).  Azure Active Directory maakt het beheren van toegang tot toepassingen door organisaties (bekend als tenants) mogelijk. Toepassingen worden onderverdeeld in die een webgebaseerde UI voor aanmelden en die met een systeemeigen client-ervaring. 

Een Service Fabric-cluster biedt verschillende toegangspunten bij de management-functionaliteit, met inbegrip van de webconsole [Service Fabric Explorer][service-fabric-visualizing-your-cluster] and [Visual Studio][service-fabric-manage-application-in-visual-studio]. Als gevolg hiervan, maakt u twee Azure Active Directory-toepassingen voor het beheren van toegang tot het cluster: een webtoepassing en een systeemeigen toepassing.  Nadat de toepassingen zijn gemaakt, wijst u gebruikers toe aan de rollen alleen-lezen en beheerder.

> [!NOTE]
> U moet de volgende stappen uitvoeren voordat u het cluster maakt. Omdat de scripts clusternamen en eindpunten verwachten, moeten de waarden worden gepland en geen waarden zijn die u al hebt gemaakt.

## <a name="prerequisites"></a>Vereisten
In dit artikel nemen we aan dat u al een tenant hebt gemaakt. Als u dit nog niet hebt gedaan, begint u door [Een Azure Active Directory-tenant verkrijgen][active-directory-howto-tenant] te lezen.

Ter vereenvoudiging van enkele van de stappen voor het configureren van Azure Active Directory met een Service Fabric-cluster, hebben we een set Windows PowerShell-scripts gemaakt.

1. [Kloon de opslagplaats](https://github.com/Azure-Samples/service-fabric-aad-helpers) op uw computer.
2. [Zorg ervoor dat u alle vereiste onderdelen hebt](https://github.com/Azure-Samples/service-fabric-aad-helpers#getting-started) voor de scripts die zijn geïnstalleerd.

## <a name="create-azure-ad-applications-and-assign-users-to-roles"></a>Azure Active Directory-toepassingen maken en gebruikers toewijzen aan rollen

We de scripts gebruiken om twee Azure AD-toepassingen voor het beheren van toegang tot het cluster te maken: een webtoepassing en een systeemeigen toepassing. Nadat u toepassingen voor uw cluster hebt gemaakt, maakt u gebruikers voor de [rollen die worden ondersteund door Service Fabric](service-fabric-cluster-security-roles.md): alleen-lezen en -beheerder.

Voer `SetupApplications.ps1` uit, en geef de tenant-ID, de naam van het cluster en de antwoord-URL van de webtoepassing op als parameters.  Geef ook gebruikersnamen en wachtwoorden voor de gebruikers op. Bijvoorbeeld:

```powershell
$Configobj = .\SetupApplications.ps1 -TenantId '0e3d2646-78b3-4711-b8be-74a381d9890c' -ClusterName 'mysftestcluster' -WebApplicationReplyUrl 'https://mysftestcluster.eastus.cloudapp.azure.com:19080/Explorer/index.html' -AddResourceAccess
.\SetupUser.ps1 -ConfigObj $Configobj -UserName 'TestUser' -Password 'P@ssword!123'
.\SetupUser.ps1 -ConfigObj $Configobj -UserName 'TestAdmin' -Password 'P@ssword!123' -IsAdmin
```

> [!NOTE]
> Voor nationale clouds (bijvoorbeeld Azure Government, Azure China, Azure Duitsland), moet u ook de parameter `-Location` opgeven.

U vindt uw *TenantId* door het uitvoeren van de PowerShell-opdracht `Get-AzureSubscription`. Deze opdracht wordt uitgevoerd, wordt de tenant-id voor elk abonnement weergegeven.

De *Clusternaam* wordt gebruikt voor als prefix aan de Azure Active Directory-toepassingen die zijn gemaakt door het script. Deze hoeft niet precies overeen te komen met de naam van het daadwerkelijke cluster. De naam is uitsluitend bedoeld om het eenvoudiger te maken dat Azure Active Directory-artefacten worden toegewezen aan het Service Fabric-cluster waarmee ze worden gebruikt.

*WebApplicationReplyUrl* wordt het standaardeindpunt dat Azure Active Directory retourneert naar uw gebruikers nadat ze klaar zijn aanmelden. Stel dit eindpunt in als het Service Fabric Explorer-eindpunt voor uw cluster, dit is standaard:

https://&lt;cluster_domain&gt;:19080/Explorer

U wordt gevraagd of u zich aanmeldt bij een account dat beheerdersrechten voor de Azure Active Directory-tenant heeft. Nadat u zich hebt aangemeld, maakt het script de web- en systeemeigen toepassingen voor uw Service Fabric-cluster. Als u kijkt naar de toepassingen van de tenant in de [Azure-portal][azure-portal], ziet u twee nieuwe vermeldingen:

   * *ClusterName*\_Cluster
   * *ClusterName*\_Client

Het script af te drukken de JSON die is vereist voor de Azure Resource Manager-sjabloon wanneer u [maken van het cluster](service-fabric-cluster-creation-create-template.md#add-azure-ad-configuration-to-use-azure-ad-for-client-access), dus is het een goed idee om Houd het PowerShell-venster geopend.

```json
"azureActiveDirectory": {
  "tenantId":"<guid>",
  "clusterApplication":"<guid>",
  "clientApplication":"<guid>"
},
```

## <a name="troubleshooting-help-in-setting-up-azure-active-directory"></a>Problemen bij het instellen van Azure Active Directory
Instellen van Azure AD en het gebruik van het kunnen lastig zijn, zodat hier een aantal tips zijn over wat u doen kunt om op te sporen van het probleem.

### <a name="service-fabric-explorer-prompts-you-to-select-a-certificate"></a>Service Fabric Explorer vraagt u om een certificaat te selecteren
#### <a name="problem"></a>Probleem
Nadat u zich hebt aangemeld met succes aan Azure AD in Service Fabric Explorer, geeft de browser naar de startpagina, maar een bericht u vraagt om een certificaat selecteren.

![Dialoogvenster SFX-certificaat][sfx-select-certificate-dialog]

#### <a name="reason"></a>Reason
De gebruiker is niet een rol in de Azure AD-toepassing voor cluster toegewezen. Azure AD-verificatie mislukt dus op Service Fabric-cluster. Service Fabric Explorer gebruikgemaakt van verificatie via certificaat.

#### <a name="solution"></a>Oplossing
Volg de instructies voor het instellen van Azure AD en gebruikersrollen toewijzen. Ook aangeraden u inschakelen 'Gebruikerstoewijzing vereist voor toegang tot app,' als `SetupApplications.ps1` heeft.

### <a name="connection-with-powershell-fails-with-an-error-the-specified-credentials-are-invalid"></a>Verbinding met PowerShell mislukt met fout: "De opgegeven referenties zijn ongeldig."
#### <a name="problem"></a>Probleem
Wanneer u PowerShell gebruiken om verbinding met het cluster met behulp van de beveiligingsmodus "AzureActiveDirectory", nadat u zich hebt aangemeld met succes aan Azure AD, wordt de verbinding met een fout mislukt: "De opgegeven referenties zijn ongeldig."

#### <a name="solution"></a>Oplossing
Deze oplossing is hetzelfde als de vorige waarde.

### <a name="service-fabric-explorer-returns-a-failure-when-you-sign-in-aadsts50011"></a>Service Fabric Explorer, wordt er een fout retourneert wanneer u zich aanmeldt: "AADSTS50011"
#### <a name="problem"></a>Probleem
Wanneer u zich aanmeldt bij Azure AD in Service Fabric Explorer, wordt een fout geretourneerd door de pagina: "AADSTS50011: Het antwoordadres op dat &lt;url&gt; komt niet overeen met de antwoordadressen die is geconfigureerd voor de toepassing: &lt;guid&gt;. "

![Antwoordadres op dat u SFX komt niet overeen met][sfx-reply-address-not-match]

#### <a name="reason"></a>Reason
Het (web)-clustertoepassing met Service Fabric Explorer probeert te verifiëren bij Azure AD en biedt als onderdeel van de aanvraag de geretourneerde Omleidings-URL. Maar de URL is niet opgenomen in de Azure AD-toepassing **antwoord-URL** lijst.

#### <a name="solution"></a>Oplossing
Selecteer 'App-registraties' in AAD-pagina, selecteert u uw toepassing voor het cluster en selecteer vervolgens de **antwoord-URL's** knop. Op de pagina 'Antwoord-URL's ', de URL van de Service Fabric Explorer toevoegen aan de lijst of een van de items in de lijst met vervangen. Wanneer u klaar bent, sla de wijziging.

![Web application antwoord-url][web-application-reply-url]

### <a name="connect-the-cluster-by-using-azure-ad-authentication-via-powershell"></a>Verbinding maken met het cluster met behulp van Azure AD-verificatie via PowerShell
Als u wilt verbinding maken met de Service Fabric-cluster, gebruikt u het volgende voorbeeld van de PowerShell-opdracht:

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <endpoint> -KeepAliveIntervalInSec 10 -AzureActiveDirectory -ServerCertThumbprint <thumbprint>
```

Zie voor meer informatie, [cmdlet Connect-ServiceFabricCluster](https://docs.microsoft.com/powershell/module/servicefabric/connect-servicefabriccluster).

### <a name="can-i-reuse-the-same-azure-ad-tenant-in-multiple-clusters"></a>Kan ik dezelfde Azure AD-tenant in meerdere clusters gebruiken?
Ja. Maar vergeet niet de URL van de Service Fabric Explorer toevoegen aan uw (web)-clustertoepassing. Anders, Service Fabric Explorer werkt niet.

### <a name="why-do-i-still-need-a-server-certificate-while-azure-ad-is-enabled"></a>Waarom kan ik nog steeds moet een certificaat voor Azure AD is ingeschakeld?
FabricClient en FabricGateway uit te voeren een wederzijdse verificatie. Tijdens de verificatie van Azure AD, Azure AD-integratie zorgt voor de identiteit van een client naar de server en het servercertificaat wordt gebruikt om te controleren of de identiteit van de server. Zie voor meer informatie over Service Fabric certificaten [X.509-certificaten en Service Fabric][x509-certificates-and-service-fabric].

## <a name="next-steps"></a>Volgende stappen
Na het instellen van Azure Active Directory-toepassingen en functies van de instelling voor gebruikers, [configureren en implementeren van een cluster](service-fabric-cluster-creation-via-arm.md).


<!-- Links -->
[azure-CLI]:https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest
[azure-portal]: https://portal.azure.com/
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[active-directory-howto-tenant]:../active-directory/develop/quickstart-create-new-tenant.md
[service-fabric-visualizing-your-cluster]: service-fabric-visualizing-your-cluster.md
[service-fabric-manage-application-in-visual-studio]: service-fabric-manage-application-in-visual-studio.md
[sf-aad-ps-script-download]:http://servicefabricsdkstorage.blob.core.windows.net/publicrelease/MicrosoftAzureServiceFabric-AADHelpers.zip
[x509-certificates-and-service-fabric]: service-fabric-cluster-security.md#x509-certificates-and-service-fabric

<!-- Images -->
[sfx-select-certificate-dialog]: ./media/service-fabric-cluster-creation-setup-aad/sfx-select-certificate-dialog.png
[sfx-reply-address-not-match]: ./media/service-fabric-cluster-creation-setup-aad/sfx-reply-address-not-match.png
[web-application-reply-url]: ./media/service-fabric-cluster-creation-setup-aad/web-application-reply-url.png
