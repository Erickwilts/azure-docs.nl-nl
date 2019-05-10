---
title: Opmerkingen bij de release - spraakservices
titlesuffix: Azure Cognitive Services
description: Zie een actief logboek van de functie-versies, verbeteringen, opgeloste fouten en bekende problemen voor Azure Speech Services.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: wolfma
ms.custom: seodec18
ms.openlocfilehash: fa722d749ec27a72a8be3bf8fcfd8097a1404458
ms.sourcegitcommit: e6d53649bfb37d01335b6bcfb9de88ac50af23bd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/09/2019
ms.locfileid: "65465603"
---
# <a name="release-notes"></a>Releaseopmerkingen

## <a name="speech-sdk-150-2019-may-release"></a>Speech SDK 1.5.0: 2019 mei release

**Nieuwe functies**

* Wake word (sleutelwoord ontdekken/KWS)-functionaliteit is nu beschikbaar voor Windows en Linux. KWS functionaliteit mogelijk in combinatie met elk type zijn microfoon, officiële KWS, maar ondersteunen is momenteel beperkt tot de microfoon matrices gevonden in de Azure Kinect DK-hardware of de SDK van de apparaten spraak.
* Woordgroep hint functionaliteit is beschikbaar via de SDK. Zie voor meer informatie, [hier](how-to-phrase-lists.md).
* Conversatie transcriptie functionaliteit is beschikbaar via de SDK. Zie [hier](conversation-transcription-service.md).
* Ondersteuning voor virtuele voice-first-assistenten met behulp van het kanaal directe regel spraak toevoegen.

**Voorbeelden**

* Voorbeelden toegevoegd voor nieuwe functies of nieuwe services die worden ondersteund door de SDK.

**Verbeteringen / gewijzigd**

* Verschillende eigenschappen voor herkenning om aan te passen servicegedrag of resultaten van de service (zoals maskeren grof taalgebruik en anderen) toegevoegd.
* U kunt nu de herkenning via de standaard configuratie-eigenschappen configureren, zelfs als u de herkenning gemaakt `FromEndpoint`.
* Objective-c `OutputFormat` eigenschap is toegevoegd aan SPXSpeechConfiguration.
* De SDK biedt nu ondersteuning voor Debian 9 als een Linux-distributie.

**Oplossingen voor problemen**

* Een probleem opgelost waarbij de sprekerherkenning-resource is destructed te vroeg in tekst naar spraak.
## <a name="speech-sdk-142"></a>Spraak SDK 1.4.2

Dit is een opgelost probleem release en alleen gevolgen heeft voor de SDK systeemeigen of worden beheerd. Het is niet van invloed op de JavaScript-versie van de SDK.

## <a name="speech-sdk-141"></a>Spraak SDK 1.4.1

Dit is een alleen-JavaScript-versie. Er zijn geen functies zijn toegevoegd. De volgende oplossingen zijn aangebracht:

* Web pack voorkomen dat het laden van https-proxy-agent.

## <a name="speech-sdk-140-2019-april-release"></a>Speech SDK 1.4.0: 2019 April release

**Nieuwe functies** 

* De SDK biedt nu ondersteuning voor de Text to Speech-service als een beta-versie. Dit wordt ondersteund in Windows en Linux-bureaublad van C++ en C#. Raadpleeg voor meer informatie de [Text to Speech overzicht](text-to-speech.md#get-started-with-text-to-speech).
* De SDK biedt nu ondersteuning voor MP3- en Opus/OGG audiobestanden als invoerbestanden stream. Deze functie is alleen beschikbaar op Linux via C++ en C# en is momenteel in de bètafase (meer informatie [hier](how-to-use-codec-compressed-audio-input-streams.md)).
* De spraak-SDK voor Java, .NET core, C++ en Objective-C hebben opgedaan met ondersteuning voor macOS. De Objective-C-ondersteuning voor macOS is momenteel in de bètafase bevindt.
* iOS: De spraak-SDK voor iOS (Objective-C) is nu ook gepubliceerd als een CocoaPod.
* JavaScript: Ondersteuning voor niet-standaard microfoon als een apparaat voor invoer.
* JavaScript: Proxy-ondersteuning voor Node.js.

**Voorbeelden**

* Voorbeelden voor het gebruik van de spraak-SDK met C++ en Objective-C in macOS zijn toegevoegd.
* Voorbeelden van het gebruik van de Text to Speech-service zijn toegevoegd.

**Verbeteringen / gewijzigd**

* Python: Aanvullende eigenschappen van de resultaten zijn nu beschikbaar via de `properties` eigenschap.
* Voor aanvullende ontwikkel- en ondersteuning voor foutopsporing, kunt u SDK-logboekregistratie en diagnostische gegevens in een logboekbestand omleiden (meer informatie [hier](how-to-use-logging.md)).
* JavaScript: De van audio-verwerkingsprestaties verbeteren.

**Oplossingen voor problemen**

* Mac/iOS: Een bug die hebben geleid tot een lange wachttijd wanneer een verbinding met de Speech-Service kan niet worden gemaakt, is opgelost.
* Python: de foutafhandeling voor argumenten in Python callbacks verbeteren.
* JavaScript: Vaste juiste status voor spraak reporting is op RequestSession beëindigd.

## <a name="speech-sdk-131-2019-february-refresh"></a>Speech SDK 1.3.1: Februari 2019 vernieuwen

Dit is een opgelost probleem release en alleen gevolgen heeft voor de SDK systeemeigen of worden beheerd. Het is niet van invloed op de JavaScript-versie van de SDK.

**Opgelost probleem**

* Een geheugenlek opgelost bij het gebruik van de invoer van de microfoon. Stream op basis van of het bestand invoer wordt niet beïnvloed.

## <a name="speech-sdk-130-2019-february-release"></a>Speech SDK 1.3.0: Release van februari 2019

**Nieuwe functies**

* De spraak-SDK biedt ondersteuning voor selectie van de invoer microfoon via de klasse AudioConfig. Hiermee kunt u audio om gegevens te streamen met de spraak-Services via een microfoon niet-standaard. Zie voor meer informatie de documentatie beschrijven [audio-invoer Apparaatselectie](how-to-select-audio-input-devices.md). Deze functie is nog niet beschikbaar is via JavaScript.
* De spraak-SDK biedt nu ondersteuning voor Unity in een bètaversie. Feedback geven via de sectie probleem in de [voorbeeldopslagplaats in GitHub](https://aka.ms/csspeech/samples). Deze release biedt ondersteuning voor Unity op Windows x86 en x64 (desktop of Universal Windows Platform-toepassingen), en Android (ARM32/64, x86). Meer informatie vindt u in onze [Unity-snelstartgids](quickstart-csharp-unity.md).
* Het bestand `Microsoft.CognitiveServices.Speech.csharp.bindings.dll` (geleverd in eerdere versies) wordt niet meer nodig hebt. De functionaliteit is nu geïntegreerd in de core-SDK.


**Voorbeelden**

De volgende nieuwe inhoud is beschikbaar in onze [voorbeeldopslagplaats](https://aka.ms/csspeech/samples):

* Meer voorbeelden voor AudioConfig.FromMicrophoneInput.
* Aanvullende voorbeelden van Python intentieherkenning en voor omzetting.
* Als u meer voorbeelden voor het gebruik van het verbindingsobject in iOS.
* Meer Java-voorbeelden voor vertaling met de audio-uitvoer.
* Nieuwe voorbeeld voor het gebruik van de [Batch transcriptie REST-API](batch-transcription.md).

**Verbeteringen / gewijzigd**

* Python
  * Verbeterde parameter verificatie en foutberichten in SpeechConfig.
  * Ondersteuning toevoegen voor het verbindingsobject.
  * Ondersteuning voor 32-bits Python (x86) op Windows.
  * De spraak-SDK voor Python, valt buiten beta.
* iOS
  * De SDK is nu gebouwd op basis van de iOS SDK versie 12.1.
  * De SDK biedt nu ondersteuning voor iOS versie 9.2 en hoger.
  * Referentiedocumentatie voor verbeteren en op te lossen verschillende namen van eigenschappen.
* JavaScript
  * Ondersteuning toevoegen voor het verbindingsobject.
  * Type definitiebestanden voor gebundelde JavaScript toevoegen
  * Eerste ondersteuning en de implementatie voor woordgroep hints.
  * Eigenschappen van verzameling met JSON-service voor opname geretourneerd
* Windows-DLL's bevat nu een versie-resource.
* Als u een kenmerk maakt `FromEndpoint` kunt u parameters rechtstreeks aan de eindpunt-URL toevoegen. Met behulp van `FromEndpoint` kunt u de herkenning via de standaard configuratie-eigenschappen niet configureren.

**Oplossingen voor problemen**

* Lege proxygebruikersnaam en wachtwoord voor proxy zijn niet correct verwerkt. Met deze release, als u de proxygebruikersnaam en wachtwoord voor proxy hebt ingesteld op een lege tekenreeks zullen ze niet worden verzonden bij het verbinden met de proxy.
* De sessie-id die zijn gemaakt door de SDK zijn niet altijd volledig willekeurige voor sommige talen&nbsp;/ omgevingen. De initialisatie van de generator van willekeurige om dit probleem te verhelpen toegevoegd.
* De verwerking van de verificatietoken verbeteren. Als u een verificatietoken gebruiken wilt, Geef in het SpeechConfig en laat u de abonnementssleutel leeg. Vervolgens maakt u de herkenning zoals gebruikelijk.
* In sommige gevallen de verbinding is niet correct object uitgebracht. Dit probleem is opgelost.
* De JavaScript-voorbeeld is ter ondersteuning van de audio-uitvoer voor vertaling synthese ook op Safari opgelost.

## <a name="speech-sdk-121"></a>Spraak SDK 1.2.1

Dit is een alleen-JavaScript-versie. Er zijn geen functies zijn toegevoegd. De volgende oplossingen zijn aangebracht:

* Einde van de stroom op turn.end, niet op de speech.end gestart.
* Los de fout in audio pomp noodzakelijk dat geen gepland vervolgens verzonden heeft als de huidige is mislukt verzenden.
* Herkenning van continue met-verificatietoken oplossen.
* Opgelost probleem voor verschillende herkenning / eindpunten.
* Verbeteringen in de documentatie bij.

## <a name="speech-sdk-120-2018-december-release"></a>Speech SDK 1.2.0: Release van December 2018

**Nieuwe functies**

* Python
  * De bètaversie van Python-ondersteuning (3.5 en hoger) is beschikbaar in deze versie. Zie voor meer informatie, here](quickstart-python.md).
* JavaScript
  * De spraak-SDK voor JavaScript is open-source. De broncode is beschikbaar op [GitHub](https://github.com/Microsoft/cognitive-services-speech-sdk-js).
  * We bieden nu ondersteuning voor Node.js, kunt u meer informatie vinden [hier](quickstart-js-node.md).
  * De lengtebeperking voor audio-sessies is verwijderd, opnieuw verbinden gebeurt automatisch onder de dekking.
* Verbindingsobject
  * U kunt een verbindingsobject openen van het kenmerk. Dit object kunt u expliciet de serviceverbinding tot stand brengen en zich inschrijven voor het verbinding maken en gebeurtenissen te verbreken.
    (Deze functie nog niet beschikbaar is via JavaScript en Python.)
* Ondersteuning voor Ubuntu 18.04.
* Android
  * Ingeschakelde ProGuard ondersteuning tijdens het genereren van APK.

**Verbeteringen**

* Verbeteringen in het gebruik van interne thread, het aantal threads, vergrendelingen, mutexen te verminderen.
* Verbeterde foutrapportage / gegevens. In enkele gevallen zijn foutberichten niet is doorgegeven om helemaal uit.
* Ontwikkeling van afhankelijkheden in JavaScript, kunnen gebruikmaken van recente modules bijgewerkt.

**Oplossingen voor problemen**

* Vaste geheugenlekken vanwege een niet-overeenkomend gegevenstype in RecognizeAsync.
* In sommige gevallen zijn uitzonderingen worden gelekt.
* Geheugenlek gecorrigeerd in translation gebeurtenisargumenten.
* Vaste een vergrendelingsprobleem op herstellen in lange sessies uitgevoerd.
* Er is een probleem die leiden kan tot het uiteindelijke resultaat voor mislukte vertalingen ontbreekt opgelost.
* C#: Als een asynchrone bewerking is niet in de hoofdthread gestopt, is het mogelijk dat het kenmerk kan worden verwijderd voordat de asynchrone taak is voltooid.
* Java: Een probleem opgetreden bij het leidt tot een crash van de Java-virtuele machine vast.
* Objective-c Toewijzing van vaste enum; RecognizedIntent is geretourneerd in plaats van RecognizingIntent.
* JavaScript: Set standaarduitvoerindeling in een eenvoudig in SpeechConfig.
* JavaScript: Inconsistentie tussen de eigenschappen van het object config in JavaScript en andere talen wordt verwijderd.

**Voorbeelden**

* Bijgewerkt en vaste enkele voorbeelden van (bijvoorbeeld uitvoer stemmen voor vertaling, enz.).
* Voorbeelden van Node.js in toegevoegd de [voorbeeldopslagplaats](https://aka.ms/csspeech/samples).

## <a name="speech-sdk-110"></a>Spraak SDK 1.1.0

**Nieuwe functies**

* Ondersteuning voor Android x86/x64.
* Proxy-ondersteuning: U kunt nu een functie voor het instellen van de proxy-informatie (hostnaam, poort, gebruikersnaam en wachtwoord) in het object SpeechConfig aanroepen. Deze functie is nog niet beschikbaar op iOS.
* Verbeterde foutcode en berichten. Als een erkenning heeft een fout geretourneerd, deze al ingesteld `Reason` (in geannuleerde gebeurtenis) of `CancellationDetails` (in herkenningsresultaat) naar `Error`. De geannuleerde gebeurtenis bevat nu twee aanvullende leden, `ErrorCode` en `ErrorDetails`. Als de server heeft aanvullende foutinformatie met de gemelde fout geretourneerd, is het nu zijn beschikbaar in de nieuwe leden.

**Verbeteringen**

* Aanvullende verificatie toegevoegd in de configuratie voor de herkenning en toegevoegde extra foutbericht.
* Verbeterde verwerking van ervaren stilte in het midden van een geluidsbestand.
* NuGet-pakket: .NET Framework-projecten, voorkomt u dat bouwen met configuratie/platform.

**Oplossingen voor problemen**

* Verschillende uitzonderingen gevonden in de kenmerken die zijn opgelost. Bovendien zijn uitzonderingen onderschept en geconverteerd naar geannuleerde gebeurtenis.
* Een geheugenlek in de eigenschap management oplossen.
* Probleem opgelost waarbij een audio-invoerbestand de herkenning kan vastlopen.
* Een bug opgelost waar de gebeurtenissen na een sessie stop-gebeurtenis kunnen worden ontvangen.
* Sommige racevoorwaarden in threading opgelost.
* Een iOS compatibiliteitsprobleem dat leiden een crash tot kan opgelost.
* Stabiliteitsverbeteringen voor ondersteuning van Android microfoon.
* Een bug opgelost waarbij een kenmerk in JavaScript de opname-taal zou negeren.
* Er is een bug te voorkomen dat de EndpointId instellen (in sommige gevallen) opgelost in JavaScript.
* Gewijzigde parametervolgorde in AddIntent in JavaScript en toegevoegde ontbrekende AddIntent JavaScript-code.

**Voorbeelden**

* C++ toegevoegd en C# samplea voor gebruik in de pull-abonnementen en push de stroom in de [voorbeeldopslagplaats](https://aka.ms/csspeech/samples).

## <a name="speech-sdk-101"></a>Speech SDK 1.0.1

Verbeteringen van de betrouwbaarheid en oplossingen voor problemen:

* Vaste mogelijke fatale fout vanwege een racevoorwaarde in herkenning wordt verwijderd
* Vaste mogelijke fatale fout in het geval van niet-ingestelde eigenschappen.
* Toegevoegde extra fout en controleren van de parameter.
* Objective-c Vaste mogelijk fatale fout veroorzaakt door de naam in NSString overschrijven.
* Objective-c Aangepaste zichtbaarheid van de API
* JavaScript: Vaste met betrekking tot de gebeurtenissen en de nuttige informatie.
* Verbeteringen in de documentatie bij.

In onze [voorbeeldopslagplaats](https://aka.ms/csspeech/samples), een nieuwe steekproef voor JavaScript is toegevoegd.

## <a name="cognitive-services-speech-sdk-100-2018-september-release"></a>Cognitive Services Speech SDK 1.0.0: Release van September 2018

**Nieuwe functies**

* Ondersteuning voor Objective-C in iOS. Bekijk onze [Objective-C-Snelstartgids voor iOS](quickstart-objectivec-ios.md).
* Ondersteuning voor JavaScript in browser. Bekijk onze [JavaScript-snelstartgids](quickstart-js-browser.md).

**Belangrijke wijzigingen**

* Met deze release zijn een aantal belangrijke wijzigingen zijn geïntroduceerd.
  Controleer [deze pagina](https://aka.ms/csspeech/breakingchanges_1_0_0) voor meer informatie.

## <a name="cognitive-services-speech-sdk-060-2018-august-release"></a>Cognitive Services Speech SDK 0.6.0: Release van augustus 2018

**Nieuwe functies**

* UWP-apps die zijn gemaakt met de spraak-SDK nu kunnen de Windows App Certification Kit (WACK) doorgeven.
  Bekijk de [UWP-snelstartgids](quickstart-csharp-uwp.md).
* Ondersteuning voor .NET Standard 2.0 op Linux (Ubuntu 16.04 x 64).
* Experimentele: Ondersteuning voor Java 8 in Windows (64-bits) en Linux (Ubuntu 16.04 x 64).
  Bekijk de [Java Runtime Environment snelstartgids](quickstart-java-jre.md).

**Functionele wijzigen**

* Aanvullende foutgegevens details op verbindingsfouten worden blootgesteld.

**Belangrijke wijzigingen**

* Java (Android), de `SpeechFactory.configureNativePlatformBindingWithDefaultCertificate` functie vereist niet langer een padparameter. Het pad wordt nu automatisch gedetecteerd op alle ondersteunde platforms.
* De get-accessor van de eigenschap `EndpointUrl` in Java en C# is verwijderd.

**Oplossingen voor problemen**

* In Java, is het resultaat audio synthese op de vertaling herkenning nu geïmplementeerd.
* Een opgelost dat leiden niet-actieve threads en een toenemend aantal open en niet-gebruikte sockets tot kan.
* Een probleem opgelost waar een langlopende opname in het midden van de overdracht kan beëindigd.
* Herkenning afsluiten, een zeldzame situatie vast.

## <a name="cognitive-services-speech-sdk-050-2018-july-release"></a>Cognitive Services Speech SDK 0.5.0: Release van juli 2018

**Nieuwe functies**

* Ondersteuning voor Android-platform (API 23: Android 6.0 Marshmallow of hoger). Bekijk de [Android snelstartgids](quickstart-java-android.md).
* Ondersteuning voor .NET Standard 2.0 op Windows. Bekijk de [.NET Core-snelstartgids](quickstart-csharp-dotnetcore-windows.md).
* Experimentele: Ondersteuning voor UWP op Windows (versie 1709 of hoger).
  * Bekijk de [UWP-snelstartgids](quickstart-csharp-uwp.md).
  * Opmerking: UWP-apps die zijn gebouwd met de spraak-SDK nog doorgegeven niet de Windows App Certification Kit (WACK).
* Ondersteuning voor langlopende met automatisch opnieuw verbinden.

**Functionele wijzigingen**

* `StartContinuousRecognitionAsync()` biedt ondersteuning voor langlopende herkenning.
* Het herkenningsresultaat bevat meer velden. Ze worden verschoven ten opzichte van de audio begin en de duur (zowel in tikken) van de herkende tekst en aanvullende waarden die staan voor herkenning van status, bijvoorbeeld `InitialSilenceTimeout` en `InitialBabbleTimeout`.
* Ondersteuning voor AuthorizationToken voor het maken van factory-exemplaren.

**Belangrijke wijzigingen**

* Herkenning van gebeurtenissen: Gebeurtenistype NoMatch is samengevoegd met de fout-gebeurtenis.
* Uitvoerindeling om te blijven met C++ uitgelijnde is SpeechOutputFormat in C# gewijzigd.
* Het retourtype van sommige methoden van de `AudioInputStream` interface enigszins gewijzigd:
   * In Java, de `read` methode nu retourneert `long` in plaats van `int`.
   * In C#, de `Read` methode nu retourneert `uint` in plaats van `int`.
   * In C++ kunt de `Read` en `GetFormat` methoden nu terug `size_t` in plaats van `int`.
* C++: Exemplaren van invoer audiostreams nu kunnen worden doorgegeven als een `shared_ptr`.

**Oplossingen voor problemen**

* Onjuiste retourwaarden, het resultaat vast wanneer `RecognizeAsync()` een time-out optreedt.
* De afhankelijkheid van media foundation-bibliotheken op Windows is verwijderd. De SDK gebruikt nu Core Audio-API's.
* Documentatie fix: Toegevoegd een [regio's](regions.md) pagina voor het beschrijven van de ondersteunde regio's.

**Bekend probleem**

* De spraak-SDK voor Android rapporteren niet spraak synthese resultaten voor vertaling. Dit probleem wordt opgelost in de volgende release.

## <a name="cognitive-services-speech-sdk-040-2018-june-release"></a>Cognitive Services Speech SDK 0.4.0: Release van juni 2018

**Functionele wijzigingen**

- AudioInputStream

  Een kenmerk kan nu een stroom gebruiken als de audiobron. Zie voor meer informatie het gerelateerde [gebruiksaanwijzing](how-to-use-audio-input-streams.md).

- De indeling van gedetailleerde uitvoer

  Bij het maken van een `SpeechRecognizer`, kunt u aanvragen `Detailed` of `Simple` uitvoerindeling. De `DetailedSpeechRecognitionResult` bevat een betrouwbaarheidsscore, herkende tekst, onbewerkte lexicale vorm, genormaliseerde formulier en genormaliseerde formulier met gemaskeerd grof taalgebruik.

**Belangrijke wijziging**

- Gewijzigd in `SpeechRecognitionResult.Text` van `SpeechRecognitionResult.RecognizedText` in C#.

**Oplossingen voor problemen**

- Een mogelijke retouraanroep-probleem opgelost in de laag USP tijdens het afsluiten.

- Als een kenmerk een geluidsbestand invoer gebruikt, is het bedrijf op naar de bestandsingang die langer dan nodig.

- Verschillende impassen tussen de pomp bericht en de herkenning verwijderd.

- Fire een `NoMatch` leiden tot wanneer het antwoord van service is een time-out.

- De media foundation-bibliotheken op Windows zijn vertraging geladen. Deze bibliotheek is vereist voor de microfoon alleen.

- De uploadsnelheid van audiogegevens is beperkt tot over twee keer de oorspronkelijke audio snelheid.

- Op Windows, zijn C# .NET-assembly's nu sterke naam.

- Documentatie fix: `Region` is vereiste informatie om te maken van een kenmerk.

Meer voorbeelden zijn toegevoegd en worden voortdurend bijgewerkt. Zie voor de meest recente set voorbeelden, de [spraak SDK voorbeelden van GitHub-opslagplaats](https://aka.ms/csspeech/samples).

## <a name="cognitive-services-speech-sdk-0212733-2018-may-release"></a>Cognitive Services Speech SDK 0.2.12733: 2018-mei release

Deze release is de eerste openbare preview-versie van de Cognitive Services Speech SDK.
