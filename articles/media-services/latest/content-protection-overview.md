---
title: Uw inhoud beveiligen met behulp van Media Services Dynamic Encryption-Azure | Microsoft Docs
description: Dit artikel geeft een overzicht van de beveiliging van inhoud met dynamische versleuteling. Er wordt ook gepraat over streaming protocollen en versleutelings typen.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 174184993e40b60dc89022d360f0c09fb31bc60b
ms.sourcegitcommit: a0b37e18b8823025e64427c26fae9fb7a3fe355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/25/2019
ms.locfileid: "68501271"
---
# <a name="protect-your-content-by-using-media-services-dynamic-encryption"></a>Uw inhoud beveiligen met behulp van Media Services dynamische versleuteling

U kunt Azure Media Services gebruiken om uw media te beveiligen vanaf het moment dat de computer de opslag, verwerking en levering verlaat. Met Media Services kunt u uw Live en on-demand inhoud dynamisch versleutelen met Advanced Encryption Standard (AES-128) of een van de drie belangrijkste Digital Rights Management (DRM)-systemen: Micro soft PlayReady, Google Widevine en Apple FairPlay. Media Services biedt ook een service voor het leveren van AES-sleutels en DRM (PlayReady, Widevine en FairPlay) licenties voor geautoriseerde clients.  

In Media Services v3 is een inhouds sleutel gekoppeld aan streaming-Locator (Zie [dit voor beeld](protect-with-aes128.md)). Als u de Media Services key delivery-service gebruikt, kunt u Azure Media Services de inhouds sleutel voor u laten genereren. U moet de inhouds sleutel zelf genereren als u de service voor de levering van sleutels gebruikt, of als u een scenario met een hoge Beschik baarheid wilt afhandelen waarbij u dezelfde inhouds sleutel moet hebben in twee data centers.

Wanneer een stroom wordt aangevraagd door een speler, Media Services maakt gebruik van de opgegeven sleutel voor het versleutelen van uw inhoud dynamisch met behulp van de lege sleutel AES of DRM-versleuteling. Voor het ontsleutelen van de stroom, vraagt de speler de sleutel van Media Services-sleutelleveringsservice of de sleutelleveringsservice die u hebt opgegeven. Om te beslissen of de gebruiker is gemachtigd om op te halen van de sleutel, de service beoordeelt wat het beleid voor het sleutels van inhoud die u hebt opgegeven voor de sleutel.

U kunt de REST-API of een Media Services-clientbibliotheek gebruiken om autorisatie en verificatie beleid voor uw licenties en sleutels te configureren.

In de volgende afbeelding ziet u de werk stroom voor Media Services inhouds beveiliging:

![Werk stroom voor Media Services beveiliging van inhoud](./media/content-protection/content-protection.svg)
  
&#42;*Dynamische versleuteling ondersteunt AES-128 Clear Key, CBCS en CENC. Zie de ondersteunings [matrix](#streaming-protocols-and-encryption-types)voor meer informatie.*

In dit artikel worden de concepten en terminologie beschreven die u helpen de beveiliging van inhoud met Media Services te begrijpen.

## <a name="main-components-of-a-content-protection-system"></a>Belangrijkste onderdelen van een systeem voor beveiliging van inhoud

Als u uw inhouds beveiligingssysteem wilt volt ooien, moet u het bereik van de inspanningen volledig begrijpen. De volgende secties geven een overzicht van de drie onderdelen die u moet implementeren. 

> [!NOTE]
> We raden u ten zeerste aan om elk deel in de volgende secties te richten en volledig te testen voordat u verdergaat met het volgende gedeelte. Als u uw inhouds beveiligingssysteem wilt testen, gebruikt u de hulpprogram ma's die in de secties zijn opgegeven.

### <a name="media-services-code"></a>Media Services code
  
Het [DRM](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM/Program.cs) -voor beeld laat zien hoe u een multi-DRM-systeem met Media Services v3 implementeert met behulp van .net. Ook wordt uitgelegd hoe u de Media Services licentie/key delivery service kunt gebruiken.   
  
U kunt elke asset met meerdere versleutelingstypen versleutelen (AES-128, PlayReady, Widevine, FairPlay). Zie [streaming protocollen en versleutelings typen](#streaming-protocols-and-encryption-types)om te zien wat zinvol is om te combi neren.

Het voorbeeld wordt getoond hoe u:

1. Een [beleid voor inhouds sleutels](content-key-policy-concept.md)maken en configureren.    

   U maakt een inhouds sleutel beleid om te configureren hoe de inhouds sleutel (die beveiligde toegang tot uw assets biedt) wordt geleverd aan eind clients:  
 
   * Geef de licentie leverings autorisatie op. Geef de logica van de autorisatie controle op op basis van claims in JSON Web Token (JWT).
   * [PlayReady](playready-license-template-overview.md)-, [Widevine](widevine-license-template-overview.md)-en/of [Fairplay](fairplay-license-overview.md) -licenties configureren. Met de sjablonen kunt u rechten en machtigingen voor elk van de DRMs configureren.

     ```
     ContentKeyPolicyPlayReadyConfiguration playReadyConfig = ConfigurePlayReadyLicenseTemplate();
     ContentKeyPolicyWidevineConfiguration widevineConfig = ConfigureWidevineLicenseTempate();
     ContentKeyPolicyFairPlayConfiguration fairPlayConfig = ConfigureFairPlayPolicyOptions();
     ```
2. Maak een [streaming-Locator](streaming-locators-concept.md) die is geconfigureerd voor het streamen van het versleutelde activum. 
  
   De streaming-Locator moet worden gekoppeld aan een [streaming-beleid](streaming-policy-concept.md). In het voor beeld is het `StreamingLocator.StreamingPolicyName` beleid ' Predefined_MultiDrmCencStreaming ' ingesteld. 
      
   De PlayReady-en Widevine-versleuteling worden toegepast en de sleutel wordt aan de client voor afspelen geleverd op basis van de geconfigureerde DRM-licenties. Als u de stroom ook wilt versleutelen met CBCS (FairPlay), gebruikt u het beleid ' Predefined_MultiDrmStreaming '.

   De streaming-Locator is ook gekoppeld aan het inhouds sleutel beleid dat u hebt gedefinieerd.
3. Maken van een test-token.

   De `GetTokenAsync` -methode laat zien hoe u een test token maakt.
4. De streaming-URL maken.

   De `GetDASHStreamingUrlAsync` -methode laat zien hoe u de streaming-URL bouwt. In dit geval wordt de streep inhoud door de URL gestreamd.

### <a name="player-with-an-aes-or-drm-client"></a>Speler met een AES-of DRM-client 

Een videospeler-app op basis van een player SDK (systeemeigen of browsergebaseerde) moet voldoen aan de volgende vereisten:

* De Player SDK ondersteunt de benodigde DRM-clients.
* De Player SDK ondersteunt de vereiste streaming-protocollen: Glad, streepje en/of HLS.
* De SDK van de Player kan verwerken van een JWT-token in een aanvraag voor het ophalen van licenties.

U kunt een speler maken met behulp van de [API van Azure Media Player](https://amp.azure.net/libs/amp/latest/docs/). Gebruik de [API van Azure Media Player ProtectionInfo](https://amp.azure.net/libs/amp/latest/docs/) om op te geven welke DRM-technologie kunt gebruiken voor verschillende DRM-platforms.

Voor testen AES of CENC (Widevine en/of PlayReady) gecodeerde inhoud, kunt u [Azure Media Player](https://aka.ms/azuremediaplayer). Zorg ervoor dat u **Geavanceerde opties** selecteert en controleer uw versleutelings opties.

Als u testen van FairPlay gecodeerde inhoud wilt, gebruikt u [deze test player](https://aka.ms/amtest). De speler ondersteunt Widevine, PlayReady en FairPlay DRMs, samen met AES-128 Clear Key encryption. 

Kies de juiste browser om verschillende DRMs te testen:

* Chrome, Opera of Firefox voor Widevine
* Micro soft Edge of Internet Explorer 11 voor PlayReady
* Safari op macOS voor FairPlay

### <a name="security-token-service"></a>Beveiligings token service

Een STS (Security Token Service) heeft JWT als het toegangs token voor back-end-toegang tot bronnen. U kunt de Azure Media Services licentie/key delivery-service gebruiken als de back-end-resource. Een STS heeft voor het definiëren van het volgende:

* Verlener en doelgroep (of bereik)
* Claims die afhankelijk van de zakelijke vereisten in de beveiliging van inhoud zijn
* Symmetrisch als asymmetrisch controle voor controle van handtekening
* Rollover van ondertekeningssleutel ondersteuning (indien nodig)

U kunt [Dit hulp programma STS](https://openidconnectweb.azurewebsites.net/DRMTool/Jwt) gebruiken om de STS te testen. Het ondersteunt alle drie typen verificatie sleutels: symmetrisch, asymmetrisch of Azure Active Directory (Azure AD) met sleutel rollover. 

## <a name="streaming-protocols-and-encryption-types"></a>Protocollen voor streaming en versleutelingstypen

U kunt Media Services gebruiken om uw inhoud dynamisch versleuteld met AES clear key- of DRM-versleuteling met behulp van PlayReady, Widevine en FairPlay te leveren. Op dit moment kunt u de HTTP Live Streaming (HLS), MPEG DASH en Smooth Streaming-indelingen versleutelen. Elk protocol ondersteunt de volgende versleutelings methoden.

### <a name="hls"></a>HLS

Het HLS-protocol ondersteunt de volgende container indelingen en versleutelings schema's.

|Containerindeling|Versleutelingsmethode|Voor beeld van URL|
|---|---|---|
|Alle|AES|`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=m3u8-aapl,encryption=cbc)`|
|MPG2-TS |CBCS (FairPlay) |`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=m3u8-aapl,encryption=cbcs-aapl)`|
|CMAF(fmp4) |CBCS (FairPlay) |`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=m3u8-cmaf,encryption=cbcs-aapl)`|
|MPG2-TS |CENC (PlayReady) |`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=m3u8-aapl,encryption=cenc)`|
|CMAF(fmp4) |CENC (PlayReady) |`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=m3u8-cmaf,encryption=cenc)`|

HLS/CMAF + FairPlay (inclusief HEVC/H. 265) wordt ondersteund op de volgende apparaten:

* iOS 11 of hoger 
* iPhone 8 of hoger
* MacOS High Sierra met Intel 7 Generation CPU

### <a name="mpeg-dash"></a>MPEG-DASH

Het MPEG-DASH-protocol ondersteunt de volgende container indelingen en versleutelings schema's.

|Containerindeling|Versleutelingsmethode|URL-voor beelden
|---|---|---|
|Alle|AES|`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=mpd-time-csf,encryption=cbc)`|
|CSF(fmp4) |CENC (Widevine + PlayReady) |`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=mpd-time-csf,encryption=cenc)`|
|CMAF(fmp4)|CENC (Widevine + PlayReady)|`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=mpd-time-cmaf,encryption=cenc)`|

### <a name="smooth-streaming"></a>Smooth Streaming

Het Smooth Streaming-Protocol ondersteunt de volgende container indelingen en versleutelings schema's.

|Protocol|Containerindeling|Versleutelingsmethode|
|---|---|---|
|fMP4|AES|`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(encryption=cbc)`|
|fMP4 | CENC (PlayReady) |`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(encryption=cenc)`|

### <a name="browsers"></a>Browsers

Algemene browsers ondersteunen de volgende DRM-clients:

|Browser|Versleuteling|
|---|---|
|Chrome|Widevine|
|Micro soft Edge, Internet Explorer 11|PlayReady|
|Firefox|Widevine|
|Opera|Widevine|
|Safari|FairPlay|

## <a name="controlling-content-access"></a>Toegang tot inhoud beheren

U kunt bepalen wie toegang heeft tot uw inhoud door het configureren van het beleid voor de inhoud van de sleutels. Media Services ondersteunt meerdere manieren om gebruikers te autoriseren die sleutels aanvragen. Het beleid voor de inhoud van de sleutels, moet u configureren. De client (speler) moet voldoen aan van het beleid voordat de sleutel kan worden geleverd aan de client. Het beleid voor de inhoud van de sleutels kan hebben *open* of *token* beperking. 

Een open-beperkt beleid voor inhouds sleutels kan worden gebruikt wanneer u een licentie wilt verlenen aan iemand zonder autorisatie. Als uw omzet bijvoorbeeld op ad of op basis van een abonnement is.  

Met een beleid met beperkte inhoud, wordt de inhouds sleutel alleen verzonden naar een client die een geldig JWT-token of een eenvoudig webtoken in de aanvraag voor licentie/sleutel levert. Dit token moet worden uitgegeven door een STS. 

U kunt Azure AD als STS gebruiken of een aangepaste STS implementeren. De STS moeten worden geconfigureerd voor het maken van een token dat is ondertekend met de opgegeven sleutel en probleem claims die u hebt opgegeven in de configuratie van de tokenbeperking. De Media Services licentie/key delivery service retourneert de aangevraagde licentie of sleutel bij de client als aan beide volgende voor waarden wordt voldaan:

* Het token is geldig. 
* De claims in het token komen overeen met die zijn geconfigureerd voor de licentie of sleutel.

Wanneer u het token beperkt beleid configureert, moet u de para meters voor de primaire verificatie sleutel, verlener en doel groep opgeven. De primaire verificatiesleutel bevat de sleutel die het token is ondertekend. De uitgever is de STS die het token uitgeeft. De doel groep, ook wel bereik genoemd, beschrijft de bedoeling van het token of de bron waartoe het token toegang verleent. De Media Services licentie/key delivery-service controleert of deze waarden in het token overeenkomen met de waarden in de sjabloon.

### <a name="token-replay-prevention"></a>Token replay voor komen

Met de functie voor het voor *komen* van tokens kunnen Media Services klanten een limiet instellen voor het aantal keren dat hetzelfde token kan worden gebruikt om een sleutel of licentie aan te vragen. De klant kan een claim van het type `urn:microsoft:azure:mediaservices:maxuses` in het token toevoegen, waarbij de waarde het aantal keren is dat het token kan worden gebruikt om een licentie of sleutel te verkrijgen. Bij alle volgende aanvragen met dezelfde token voor de sleutel levering wordt een niet-geautoriseerde reactie geretourneerd. Zie hoe u de claim toevoegt in het [DRM](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM/Program.cs#L601)-voor beeld.
 
#### <a name="considerations"></a>Overwegingen

* Klanten moeten controle hebben over het genereren van tokens. De claim moet in het token zelf worden geplaatst.
* Wanneer u deze functie gebruikt, worden aanvragen met tokens waarvan de verval tijd meer dan één uur duurt, verwijderd met een niet-geautoriseerde reactie.
* Tokens worden uniek geïdentificeerd door hun hand tekening. Elke wijziging in de payload (bijvoorbeeld bijwerken naar de verloop tijd of de claim) wijzigt de hand tekening van het token en telt als een nieuw token dat de sleutel levering niet eerder is aangetroffen.
* Afspelen mislukt als het token de `maxuses` waarde die is ingesteld door de klant heeft overschreden.
* Deze functie kan worden gebruikt voor alle bestaande beveiligde inhoud (alleen het token dat is uitgegeven moet worden gewijzigd).
* Deze functie werkt met zowel de JWT-als de SWT.

## <a name="using-a-custom-sts"></a>Een aangepaste STS gebruiken

Een klant kan ervoor kiezen om een aangepaste STS te gebruiken om tokens op te geven. Redenen zijn:

* STS biedt geen ondersteuning voor de id-provider die wordt gebruikt door de klant. In dit geval een aangepaste STS mogelijk een optie.
* De klant mogelijk meer flexibele of betere controle STS integreren met de klant abonnee factureringssysteem. Bijvoorbeeld, een operator MVPD biedt mogelijk meerdere OTT-abonnee pakketten, zoals basic, premium en sport. De operator wilt overeenkomen met de claims in een token met de abonnee pakket zodat alleen de inhoud in een specifiek pakket beschikbaar worden gesteld. In dit geval een aangepaste STS biedt de benodigde flexibiliteit en controle.
* Om aangepaste claims in het token op te nemen om te selecteren tussen verschillende ContentKeyPolicyOptions met verschillende DRM-licentie parameters (een abonnements licentie versus een huur licentie).
* Om een claim op te geven met de id van de inhouds sleutel van de sleutel waartoe het token toegang verleent.

Wanneer u een aangepaste STS, moet twee worden gewijzigd:

* Wanneer u een service voor het leveren van licenties voor een asset configureert, moet u opgeven de beveiligingssleutel gebruikt voor verificatie door de aangepaste STS in plaats van de huidige sleutel van Azure AD.
* Wanneer een JTW-token wordt gegenereerd, een beveiligingssleutel is opgegeven in plaats van de persoonlijke sleutel van de huidige X509 certificaat in Azure AD.

Er zijn twee soorten sleutels:

* Symmetrische sleutel: Dezelfde sleutel wordt gebruikt om een JWT te genereren en te verifiëren.
* Asymmetrische sleutel: Een combi natie van open bare en persoonlijke sleutels in een x509-certificaat wordt gebruikt met een persoonlijke sleutel voor het versleutelen/genereren van een JWT en met de open bare sleutel om het token te verifiëren.

Als u .NET Framework / C# als uw ontwikkelplatform, de X509 certificaat dat wordt gebruikt voor een asymmetrische beveiligingssleutel moet een sleutellengte van ten minste 2048 hebben. Dit is een vereiste van de klasse System.IdentityModel.Tokens.X509AsymmetricSecurityKey in .NET Framework. Anders is de volgende uitzondering opgetreden: IDX10630: De ' System. Identity model. tokens. X509AsymmetricSecurityKey ' voor ondertekening mag niet kleiner zijn dan ' 2048 ' bits.

## <a name="custom-key-and-license-acquisition-url"></a>Aangepaste sleutel en licentie voor aanschaf-URL

Gebruik de volgende sjablonen als u een andere licentie/key delivery service wilt opgeven (niet Media Services). De twee Vervang bare velden in de sjablonen zijn daar, zodat u uw streaming-beleid kunt delen in veel assets in plaats van een streaming-beleid per activum te maken. 

* `EnvelopeEncryption.CustomKeyAcquisitionUrlTemplate`: De sjabloon voor de URL van de aangepaste service die sleutels levert aan spelers van eind gebruikers. Het is niet vereist wanneer u Azure Media Services gebruikt voor het uitgeven van sleutels. 

   De sjabloon ondersteunt vervangbaar tokens die door de service worden bijgewerkt tijdens runtime met de waarde die specifiek is voor de aanvraag.  De momenteel ondersteunde token waarden zijn:
   * `{AlternativeMediaId}`, die wordt vervangen door de waarde van StreamingLocatorId. AlternativeMediaId.
   * `{ContentKeyId}`, die wordt vervangen door de waarde van de id van de aangevraagde sleutel.
* `StreamingPolicyPlayReadyConfiguration.CustomLicenseAcquisitionUrlTemplate`: De sjabloon voor de URL van de aangepaste service die licenties levert aan spelers van eind gebruikers. Het is niet vereist wanneer u Azure Media Services gebruikt voor het uitgeven van licenties. 

   De sjabloon ondersteunt vervangbaar tokens die door de service worden bijgewerkt tijdens runtime met de waarde die specifiek is voor de aanvraag. De momenteel ondersteunde token waarden zijn:  
   * `{AlternativeMediaId}`, die wordt vervangen door de waarde van StreamingLocatorId. AlternativeMediaId.
   * `{ContentKeyId}`, die wordt vervangen door de waarde van de id van de aangevraagde sleutel. 
* `StreamingPolicyWidevineConfiguration.CustomLicenseAcquisitionUrlTemplate`: Hetzelfde als de vorige sjabloon, alleen voor Widevine. 
* `StreamingPolicyFairPlayConfiguration.CustomLicenseAcquisitionUrlTemplate`: Hetzelfde als de vorige sjabloon, alleen voor FairPlay.  

Bijvoorbeeld:

```csharp
streamingPolicy.EnvelopEncryption.customKeyAcquisitionUrlTemplate = "https://mykeyserver.hostname.com/envelopekey/{AlternativeMediaId}/{ContentKeyId}";
```

`ContentKeyId`heeft een waarde van de aangevraagde sleutel. U kunt gebruiken `AlternativeMediaId` als u de aanvraag aan een entiteit aan uw zijde wilt toewijzen. `AlternativeMediaId` Kan bijvoorbeeld worden gebruikt om u te helpen bij het opzoeken van machtigingen.

 Zie [streaming-beleid-maken](https://docs.microsoft.com/rest/api/media/streamingpolicies/create)voor rest-voor beelden die gebruikmaken van url's voor aangepaste licentie/sleutel overname.

## <a name="troubleshoot"></a>Problemen oplossen

Als u de `MPE_ENC_ENCRYPTION_NOT_SET_IN_DELIVERY_POLICY` fout melding krijgt, moet u ervoor zorgen dat u het juiste streaming-beleid opgeeft.

Als er fouten optreden die eindigen `_NOT_SPECIFIED_IN_URL`op, zorg er dan voor dat u de versleutelings indeling opgeeft in de URL. Een voorbeeld is `…/manifest(format=m3u8-cmaf,encryption=cbcs-aapl)`. Zie [streaming protocollen en versleutelings typen](#streaming-protocols-and-encryption-types).

## <a name="ask-questions-give-feedback-get-updates"></a>Vragen stellen, feedback geven, updates ophalen

Bekijk het [Azure Media Services Community](media-services-community.md) -artikel voor verschillende manieren om vragen te stellen, feedback te geven en updates te ontvangen over Media Services.

## <a name="next-steps"></a>Volgende stappen

* [Beveiligen met AES-versleuteling](protect-with-aes128.md)
* [Beveiligen met DRM](protect-with-drm.md)
* [Multi-DRM-inhouds beschermings systeem ontwerpen met toegangs beheer](design-multi-drm-system-with-access-control.md)
* [Versleuteling van opslag side](storage-account-concept.md#storage-side-encryption)
* [Veelgestelde vragen](frequently-asked-questions.md)

