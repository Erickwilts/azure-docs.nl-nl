---
title: Getimede meta gegevens Azure Media Services signalering in live-streaming | Microsoft Docs
description: Deze specificatie bevat een overzicht van methoden voor het Signa leren van getimede meta gegevens bij het opnemen en streamen naar Azure Media Services. Dit omvat ondersteuning voor algemene, getimede meta gegevens signalen (ID3), en SCTE-35-Signa lering voor AD-invoeging en splice-voor waarden.
services: media-services
documentationcenter: ''
author: johndeu
manager: femila
editor: johndeu
ms.assetid: 265b94b1-0fb8-493a-90ec-a4244f51ce85
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/2/2019
ms.author: johndeu
ms.openlocfilehash: 444d5ca996c014bdbf2e62cacf2563c7b63372e4
ms.sourcegitcommit: 5d6c8231eba03b78277328619b027d6852d57520
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2019
ms.locfileid: "69015725"
---
# <a name="signaling-timed-metadata-in-live-streaming"></a>Getimede meta gegevens in live streamen signalering 

Laatst bijgewerkt: 2019-07-02

### <a name="conformance-notation"></a>Conformiteit-notatie

De sleutel woorden ' moet ', ' mag niet ', ' vereist ', ' moet ', ' mag niet ', ' moet ', ' mogen ', ' is ', ' mag niet ', ' wordt aanbevolen ', ' mei ' en ' optioneel ' in dit document moeten worden geïnterpreteerd zoals beschreven in RFC 2119

## <a name="1-introduction"></a>1. Inleiding 

Om het invoegen van advertenties of aangepaste meta gegevens gebeurtenissen op een client speler te Signa leren, maken broadcasters vaak gebruik van getimede meta gegevens die zijn Inge sloten in de video. Media Services biedt ondersteuning voor het Trans Port van getimede meta gegevens van het opname punt van het live streaming-kanaal naar de client toepassing om deze scenario's in te scha kelen.
Deze specificatie bevat een overzicht van verschillende modi die worden ondersteund door Media Services voor getimede meta gegevens binnen live streaming-signalen.

1. [SCTE-35] Signa lering die voldoet aan de standaarden die worden beschreven door [SCTE-35], [SCTE-214-1], [SCTE-214-3] en [RFC8216]

2. [SCTE-35] Signa lering die voldoet aan de verouderde specificatie [Adobe-Primetime] voor RTMP AD-Signa lering.
   
3. Een algemene seingevings modus voor meta gegevens, voor berichten die **niet** [SCTE-35] zijn en die [id3v2] of andere aangepaste schema's kunnen bevatten die zijn gedefinieerd door de ontwikkelaar van de toepassing.

## <a name="11-terms-used"></a>1,1 gebruikte termen

| Begrip              | Definitie |
|-------------------|------------|
| AD-afbreek punt          | Een locatie of tijdstip waarop een of meer advertenties mogen worden gepland voor levering; hetzelfde als beschik bare verkoop kansen. |
| AD-besluit service| externe service die bepaalt welke AD ('s) en de duur van de gebruiker worden weer gegeven. De services worden doorgaans door een partner verschaft en zijn buiten het bereik van dit document.|
| WMS               | Indicatie van de tijd en para meters van de aanstaande AD-afbreek periode. Houd er rekening mee dat hints kunnen wijzen op een overschakeling van een AD-eind punt, in afwachting van de overstap naar de volgende AD binnen een AD-out en wachtende switch van een AD-afbreek bewerking naar de hoofd inhoud. |
| Packager          | De Azure Media Services ' streaming-eind punt ' biedt dynamische verpakkings mogelijkheden voor DASH en HLS en wordt ' packager ' genoemd in de media branche. 
| Presentatie tijd | De tijd dat een gebeurtenis wordt weer gegeven aan een viewer. De tijd vertegenwoordigt het tijdstip op de media tijdlijn dat een viewer de gebeurtenis zou zien. Zo is de presentatie tijd van een SCTE-35 splice_info ()-opdracht bericht de splice_time (). |
| Aankomst tijd      | Het tijdstip waarop een gebeurtenis bericht binnenkomt. De tijd is doorgaans verschillend van de presentatie tijd van de gebeurtenis, omdat gebeurtenis berichten worden verzonden vóór de presentatie tijd van de gebeurtenis.                                     |
| Sparse track      | Media spoor die niet doorlopend is en tijd is gesynchroniseerd met een bovenliggend of controle spoor.                                                                                                                                    |
| Oorsprong            | De Azure media streaming-service                                                                                                                                                                                                |
| Kanaal filter      | De Azure Media Live streaming-service                                                                                                                                                                                           |
| HLS               | Apple HTTP Live Streaming-Protocol                                                                                                                                                                                               |
| LIGGENDE              | Dynamisch adaptief streamen via HTTP                                                                                                                                                                                             |
| Vloeiend            | Smooth Streaming-Protocol                                                                                                                                                                                                        |
| MPEG2-TS          | MPEG 2-transport stromen                                                                                                                                                                                                         |
| RTMP              | Real-time multimedia Protocol                                                                                                                                                                                                    |
| uimsbf            | Niet-ondertekend geheel getal, meest significante bit eerst.                                                                                                                                                                                    |

---

## <a name="12-normative-references"></a>1,2 normatieve verwijzingen

De volgende documenten bevatten voorzieningen, die door de verwijzing in deze tekst de bepalingen van dit document vormen. Alle documenten zijn onderworpen aan de herziening door de standaard instellingen en lezers worden aangemoedigd om de mogelijkheid van de meest recente versies van de hieronder vermelde documenten te onderzoeken. Er wordt ook aandacht besteed aan het feit dat nieuwere versies van de documenten waarnaar wordt verwezen, mogelijk niet compatibel zijn met deze versie van de getimede meta gegevens specificatie voor Azure Media Services.


|Standard  |Definitie  |
|---------|---------|
|[Adobe-Primetime] | [Primetime Digital-invoeg signalerings specificatie 1,2](https://www.adobe.com/content/dam/acom/en/devnet/primetime/PrimetimeDigitalProgramInsertionSignalingSpecification.pdf) |
|[Adobe-Flash-AS] | [Naslag informatie voor FLASH action script-taal](https://help.adobe.com/archive/en_US/as2/flashlite_2.x_3.x_aslr.pdf) |
| [AMF0]            | ["Actie bericht indeling AMF0"](https://download.macromedia.com/pub/labs/amf/amf0_spec_121207.pdf) |
| [STREEPJE-IF-ONTDUBBELINGS IOP]     | Richt 4,2 lijnen voor samen werking met DASH Industry-Forum[https://dashif-documents.azurewebsites.net/DASH-IF-IOP/master/DASH-IF-IOP.html](https://dashif-documents.azurewebsites.net/DASH-IF-IOP/master/DASH-IF-IOP.html) |
| [HLS-TMD]         | Getimede meta gegevens voor HTTP Live Streaming-[https://developer.apple.com/streaming](https://developer.apple.com/streaming) |
| [ID3v2]           | ID3-code versie 2.4.0[http://id3.org/id3v2.4.0-structure](http://id3.org/id3v2.4.0-structure) |
| [ISO-14496-12]    | ISO/IEC 14496-12: Deel 12 ISO base-basis media bestands indeling, FourthEdition 2012-07-15  |
| [MPEGDASH]        | Informatie technologie--dynamisch adaptief streamen via HTTP (DASH)--deel 1: Beschrijving van media presentatie en segment indelingen. Mei 2014. Gemaakt. URL https://www.iso.org/standard/65274.html |
| [MPEGCMAF]        | Informatie technologie: multi media-toepassings indeling (MPEG-A)--deel 19: Algemene media toepassings indeling (CMAF) voor gesegmenteerde media. Januari 2018. Gemaakt. URL https://www.iso.org/standard/71975.html |
| [MPEGCENC]        | Informatie technologie--MPEG-systeem technologieën--deel 7: Veelvoorkomende versleuteling in ISO-basis bestanden voor bestands indeling. Februari 2016. Gemaakt. URL https://www.iso.org/standard/68042.html |
| [MS-SSTR]         | ["Micro soft Smooth Streaming-Protocol", 15 mei 2014](https://docs.microsoft.com/openspecs/windows_protocols/ms-sstr/8383f27f-7efe-4c60-832a-387274457251) |
| [MS-SSTR-opname]  | [Azure Media Services gefragmenteerde MP4 Live opname-specificatie](https://docs.microsoft.com/azure/media-services/media-services-fmp4-live-ingest-overview) |
| [RFC8216]         | R. Pantos, ed.; W. mei. HTTP Live Streaming. 2017 augustus. Informatief. [https://tools.ietf.org/html/rfc8216](https://tools.ietf.org/html/rfc8216) |
| [RFC4648]         |De Base16-, Base32-en base64-gegevens codering-[https://tools.ietf.org/html/rfc4648](https://tools.ietf.org/html/rfc4648) |
| RTMP            |["Het real-time Messa ging-Protocol van Adobe", december 21, 2012](https://www.adobe.com/devnet/rtmp.html)  |
| [SCTE-35-2019]    | SCTE 35: 2019-cueing bericht over digitale programma-invoeg toepassing voor kabel- https://www.scte.org/SCTEDocs/Standards/ANSI_SCTE%2035%202019r1.pdf  |
| [SCTE-214-1]      | SCTE 214-1 2016 – MPEG DASH voor op IP gebaseerde kabel Services, deel 1: MPD-beperkingen en-extensies |
| [SCTE-214-3]      | SCTE 214-3 2015 MPEG-DASH voor IP-gebaseerde kabel Services, deel 3: STREEPJE/FF-profiel |
| [SCTE-224]        | SCTE 224 2018r1 – gebeurtenis planning en meldings interface |
| [SCTE-250]        | API voor gebeurtenis-en signalerings beheer (ESAM) |

---


## <a name="2-timed-metadata-ingest"></a>2. Getimede meta gegevensopname

## <a name="21-rtmp-ingest"></a>2,1 RTMP-opname

Met [RTMP] kunnen getimede meta gegevens signalen worden verzonden als [AMF0]-Hint berichten die zijn Inge sloten in de [RTMP]-stroom. De hint berichten kunnen ergens worden verzonden voordat de werkelijke gebeurtenis of het AD-Splice-signaal van [SCTE35] moet worden uitgevoerd. Ter ondersteuning van dit scenario wordt de werkelijke tijd van de gebeurtenis verzonden binnen het bericht hint. Zie [AMF0] voor meer informatie.

In de volgende tabellen wordt de indeling van de AMF-bericht lading beschreven die Media Services neemt voor zowel de bericht modus eenvoudig als [SCTE35].

De naam van het bericht [AMF0] kan worden gebruikt om meerdere gebeurtenis stromen van hetzelfde type te onderscheiden.  Voor zowel berichten van [SCTE-35] als de modus ' Simple ' moet de naam van het AMF-bericht ' onAdCue ' zijn, zoals vereist in de [Adobe-Primetime]-specificatie.  Alle velden die hieronder niet worden vermeld, worden genegeerd door Azure Media Services op opname.

## <a name="211-rtmp-signal-syntax"></a>2.1.1 RTMP-signaal syntaxis

Azure Media Services kunt verschillende bericht typen van [AMF0] Luis teren en erop reageren die kunnen worden gebruikt om verschillende realtime gesynchroniseerde meta gegevens in de live stream te Signa leren.  De specificatie [Adobe-Primetime] definieert twee Cue-typen met de naam ' eenvoudige ' en ' SCTE-35-modus. Voor de eenvoudige modus ondersteunt Media Services één AMF Cue-bericht met de naam ' onAdCue ' met een Payload die overeenkomt met de onderstaande tabel die is gedefinieerd voor het signaal ' eenvoudige modus '.  

De volgende sectie bevat de RTMP-' eenvoudige ' modus ' payload, die kan worden gebruikt om een Basic ' spliceOut '-ad-signaal te Signa leren dat wordt uitgevoerd naar het client manifest voor HLS, DASH en micro soft Smooth Streaming. Dit is zeer nuttig voor scenario's waarbij de klant geen complexe AD signalerings-of invoegings systeem op basis van SCTE-35 heeft en gebruikmaakt van een Basic on-premises encoder om via een API te verzenden in het bericht hint. De on-premises Encoder biedt doorgaans ondersteuning voor een REST-API om dit signaal te activeren. de video stroom wordt ook wel ' splice-condition ' door een IDR-frame in de video ingevoegd en er wordt een nieuwe GOP terug gestart.

## <a name="212--simple-mode-ad-signaling-with-rtmp"></a>punt 2.1.2 in eenvoudige modus AD-Signa lering met RTMP

| Veldnaam | Veldtype | Vereist? | Beschrijvingen                                                                                                             |
|------------|------------|----------|--------------------------------------------------------------------------------------------------------------------------|
| WMS        | Tekenreeks     | Vereist | Het gebeurtenis bericht.  Is ' SpliceOut ' om een eenvoudige modus-Splice aan te wijzen.                                              |
| id         | Tekenreeks     | Vereist | Een unieke id waarmee de Splice of het segment wordt beschreven. Identificeert dit exemplaar van het bericht                            |
| duration   | Number     | Vereist | De duur van de splice. Eenheden zijn fractie seconden.                                                                |
| elapsed    | Number     | Optioneel | Wanneer het signaal wordt herhaald om afstemming in te ondersteunen, is dit veld de hoeveelheid presentatie tijd die is verstreken sinds de gestarte splice. Eenheden zijn fractie seconden. Wanneer u de eenvoudige modus gebruikt, mag deze waarde de oorspronkelijke duur van de Splice niet overschrijden.                                                  |
| time       | Number     | Vereist | Is de tijd van de splices, in de presentatie tijd. Eenheden zijn fractie seconden.                                     |

---
 
## <a name="213-scte-35-mode-ad-signaling-with-rtmp"></a>2.1.3 SCTE-35-modus AD-Signa lering met RTMP

Wanneer u werkt met een meer geavanceerde productie werk stroom voor broadcasten waarvoor het volledige SCTE-Payload 35-bericht moet worden uitgevoerd naar het HLS of het streep-manifest, is het het beste om de ' SCTE-35-modus ' van de specificatie [Adobe-Primetime] te gebruiken.  Deze modus ondersteunt in-band SCTE-35-signalen die rechtstreeks naar een on-premises Live Encoder worden verzonden, waarna de signalen worden gecodeerd in de RTMP-stroom met behulp van de ' SCTE-35-modus ' die is opgegeven in de [Adobe-Primetime]-specificatie. 

Normaal gesp roken SCTE-35-berichten kunnen alleen voor komen in MPEG-2-transport stroom (TS) op een on-premises encoder. Neem contact op met de fabrikant van uw coderings programma voor meer informatie over het configureren van een Trans Port stream-opname die SCTE-35 bevat en schakel deze in voor Pass-Through naar RTMP in de Adobe SCTE-35-modus.

In dit scenario moet de volgende Payload worden verzonden vanuit het on-premises coderings programma met behulp van het bericht type **' onAdCue '** [AMF0].

| Veldnaam | Veldtype | Vereist? | Beschrijvingen                                                                                                             |
|------------|------------|----------|--------------------------------------------------------------------------------------------------------------------------|
| WMS        | Tekenreeks     | Vereist | Het gebeurtenis bericht.  Voor [SCTE-35] berichten moet dit de met base64 gecodeerde [RFC4648] binaire splice_info_section () zijn om berichten te verzenden naar HLS-, Smooth-en dash-clients.                                              |
| type       | Tekenreeks     | Vereist | Een URN of URL die het bericht schema aangeeft. Voor [SCTE-35] berichten **moet** dit **' scte35 '** zijn om berichten te verzenden naar HLS-, Smooth-en dash-clients, in overeenstemming met [Adobe-Primetime]. De URN: scte: scte35:2013: bin kan eventueel ook worden gebruikt om een bericht [SCTE-35] te Signa leren. |
| id         | Tekenreeks     | Vereist | Een unieke id waarmee de Splice of het segment wordt beschreven. Identificeert dit exemplaar van het bericht.  Berichten met een gelijkwaardige semantiek moeten dezelfde waarde hebben.|
| duration   | Number     | Vereist | De duur van de gebeurtenis of het AD-segment, indien bekend. Als dit onbekend is, **moet** de waarde 0 zijn.                                                                 |
| elapsed    | Number     | Optioneel | Wanneer het AD-signaal [SCTE-35] wordt herhaald om in te stellen, is dit veld de hoeveelheid presentatie tijd die is verstreken sinds de gestarte splice. Eenheden zijn fractie seconden. In de modus [SCTE-35] kan deze waarde groter zijn dan de oorspronkelijke opgegeven duur van de Splice of het segment.                                                  |
| time       | Number     | Vereist | De presentatie tijd van de gebeurtenis of AD-splice.  De tijd en duur van de presentatie **moeten** worden uitgelijnd met Stream Access points (SAP) van het type 1 of 2, zoals gedefinieerd in [ISO-14496-12] bijlage I. Voor uitgaand HLS **moeten** tijd en duur worden uitgelijnd met de grenzen van het segment. De presentatie tijd en duur van verschillende gebeurtenis berichten binnen dezelfde gebeurtenis stroom mogen elkaar niet overlappen. Eenheden zijn fractie seconden.

---
## <a name="214-elemental-live-oncuepoint-ad-markers-with-rtmp"></a>2.1.4-ad-markeringen (onCuePoint) met RTMP

De elementaire Live on-premises encoder ondersteunt advertentie markeringen in het RTMP-signaal. Azure Media Services ondersteunt momenteel alleen het AD-markerings type ' onCuePoint ' voor RTMP.  Dit kan worden ingeschakeld in de instellingen van de Adobe RTMP-groep in de instellingen of API voor het Live-programma voor de code ring van media, door '**ad_markers**' in te stellen op ' onCuePoint '.  Raadpleeg de documentatie over elementaire Live voor meer informatie. Als u deze functie inschakelt in de RTMP-groep, worden SCTE-35-signalen door gegeven aan de Adobe RTMP-uitvoer die moeten worden verwerkt door Azure Media Services.

Het bericht type ' onCuePoint ' is gedefinieerd in [Adobe-Flash-AS] en heeft de volgende Payload-structuur wanneer deze wordt verzonden vanuit de elementaire Live RTMP-uitvoer.


| Eigenschap  |Description  |
|---------|---------|
|  name     | De naam moet '**scte35**' zijn door elementair Live. |
|time     |  De tijd in seconden waarop het actie punt tijdens de tijd lijn is opgetreden in het video bestand |
| type     | Het type actie punt moet worden ingesteld op**gebeurtenis**. |
| parameters | Een associatieve matrix met naam/waarde-paar teken reeksen die de informatie uit het SCTE-35-bericht, inclusief id en duur, bevatten. Deze waarden worden geparseerd door Azure Media Services en opgenomen in de label manifest decoratie.  |


Wanneer deze modus van AD-markering wordt gebruikt, is de HLS-manifest uitvoer vergelijkbaar met de eenvoudige modus van Adobe. 

### <a name="215-cancellation-and-updates"></a>2.1.5 annuleren en updates

Berichten kunnen worden geannuleerd of bijgewerkt door meerdere berichten te verzenden met dezelfde presentatie tijd en ID. De presentatie tijd en-ID identificeren de gebeurtenis uniek en het laatste bericht dat is ontvangen voor een specifieke presentatie tijd die voldoet aan de vooraf ingestelde beperkingen is het bericht dat wordt verwerkt. De bijgewerkte gebeurtenis vervangt alle eerder ontvangen berichten. De vooraf-rollen beperking is vier seconden. Berichten die ten minste vier seconden vóór de presentatie tijd zijn ontvangen, worden op de gewenste actie uitgevoerd.

## <a name="22-fragmented-mp4-ingest-smooth-streaming"></a>2,2 gefragmenteerde MP4-opname (Smooth Streaming)

Raadpleeg [MS-SSTR-opname] voor vereisten voor Live Stream-opname. De volgende secties bevatten informatie over de opname van meta gegevens van de time-out voor presentaties.  De time-outmetagegevens van de presentatie worden opgenomen als een sparse track, die is gedefinieerd in het manifest van de live-server (Zie MS-SSTR) en het vak Movie (' Moov ').  

Elk sparse fragment bestaat uit een film fragment box (' moof ') en media Data Box (' mdat '), waarbij het vak ' mdat ' het binaire bericht is.

Het coderings programma moet het fragment splitsen op het moment van de presentatie waar de hint moet worden ingevoegd om een frame nauw keurige invoeging van advertenties te krijgen.  Er moet een nieuw fragment worden gemaakt dat begint met een nieuw gemaakt IDR frame of met SAP (stream Access Points) van het type 1 of 2, zoals gedefinieerd in [ISO-14496-12] bijlage I. Op die manier kan de Azure media packager een HLS-manifest en een DASH-manifest voor meerdere Peri Oden genereren waarin de nieuwe periode begint op het frame-nauw keurigere Splice voor de presentatie.

### <a name="221-live-server-manifest-box"></a>Dialoog venster 2.2.1 Live server

De sparse track **moet** worden gedeclareerd in het manifest van de live-server met een **\<TextStream\>** -vermelding en **moet** de volgende kenmerken hebben ingesteld:

| **Kenmerk naam** | **Veld type** | **Vereist?** | **Beschrijving**                                                                                                                                                                                                                                                 |
|--------------------|----------------|---------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| systemBitrate      | Number         | Vereist      | **Moet** ' 0 ' zijn, wat een track met een onbekende variabele bitrate aangeeft.                                                                                                                                                                                                 |
| parentTrackName    | Tekenreeks         | Vereist      | **Moet** de naam zijn van het bovenliggende spoor, waarbij de tijd codes van de sparse track worden uitgelijnd. Het bovenliggende spoor kan geen sparse track zijn.                                                                                                                    |
| manifestOutput     | Boolean-waarde        | Vereist      | **Moet** ' True ' zijn, om aan te geven dat de sparse track wordt Inge sloten in het Smooth client-manifest.                                                                                                                                                               |
| Subtype            | Tekenreeks         | Vereist      | **Moet** de vier teken code ' data ' zijn.                                                                                                                                                                                                                        |
| Niveaus             | Tekenreeks         | Vereist      | **Moet** een urn of URL zijn die het bericht schema aangeeft. Voor [SCTE-35] berichten **moet** dit ' urn: SCTE: scte35:2013: bin ' zijn om berichten te verzenden naar HLS-, Smooth-en dash-clients in overeenstemming met [SCTE-35]. |
| trackName          | Tekenreeks         | Vereist      | **Moet** de naam van de sparse track zijn. De tracknaam kan worden gebruikt om meerdere gebeurtenis stromen te onderscheiden met hetzelfde schema. Elke unieke gebeurtenis stroom **moet** een unieke Track naam hebben.                                                                           |
| timescale          | Number         | Optioneel      | **Moet** de tijd schaal van het bovenliggende spoor zijn.                                                                                                                                                                                                                      |

---

### <a name="222-movie-box"></a>2.2.2 movie box

Het vak film (' Moov ') volgt het manifest van Live server als onderdeel van de stream-header voor een sparse track.

Het vak ' Moov ' **moet** een **TrackHeaderBox (' tkhd ')** bevatten zoals gedefinieerd in [ISO-14496-12] met de volgende beperkingen:

| **Veld naam** | **Veld type**          | **Vereist?** | **Beschrijving**                                                                                                |
|----------------|-------------------------|---------------|----------------------------------------------------------------------------------------------------------------|
| duration       | 64-bits geheel getal zonder teken | Vereist      | **Moet** 0 zijn, omdat het vakje track nul voor beelden heeft en de totale duur van de voor beelden in het vak bijhouden 0 is. |

---

Het vak ' Moov ' **moet** een **HandlerBox (' hdlr ')** bevatten, zoals gedefinieerd in [ISO-14496-12] met de volgende beperkingen:

| **Veld naam** | **Veld type**          | **Vereist?** | **Beschrijving**   |
|----------------|-------------------------|---------------|-------------------|
| handler_type   | 32-bits geheel getal zonder teken | Vereist      | **Moet** ' meta ' zijn. |

---

Het vak ' stsd ' **moet** een MetaDataSampleEntry bevatten met een coderings naam zoals gedefinieerd in [ISO-14496-12].  Bijvoorbeeld: voor SCTE-35 berichten **moet** de naam van de code ' SCTE ' zijn.

### <a name="223-movie-fragment-box-and-media-data-box"></a>2.2.3 film fragment en media Data Box

Sparse track fragmenten bestaan uit een film fragment box (' moof ') en een media Data Box (' mdat ').

> [!NOTE]
> Het coderings programma moet het fragment splitsen op het moment van de presentatie waar de hint moet worden ingevoegd om een frame nauw keurige invoeging van advertenties te krijgen.  Er moet een nieuw fragment worden gemaakt dat begint met een nieuw gemaakt IDR frame of met SAP (stream Access Points) van het type 1 of 2, zoals gedefinieerd in [ISO-14496-12] bijlage I
> 

Het vak MovieFragmentBox (' moof ') **moet** een **TrackFragmentExtendedHeaderBox (' uuid ')** bevatten, zoals gedefinieerd in [MS-SSTR] met de volgende velden:

| **Veld naam**         | **Veld type**          | **Vereist?** | **Beschrijving**                                                                               |
|------------------------|-------------------------|---------------|-----------------------------------------------------------------------------------------------|
| fragment_absolute_time | 64-bits geheel getal zonder teken | Vereist      | **Moet** de aankomst tijd van de gebeurtenis zijn. Deze waarde Lijnt het bericht uit met het bovenliggende nummer.   |
| fragment_duration      | 64-bits geheel getal zonder teken | Vereist      | **Moet** de duur van de gebeurtenis zijn. De duur kan nul zijn om aan te geven dat de duur onbekend is. |

---


Het MediaDataBox-vak (' mdat ') **moet** de volgende indeling hebben:

| **Veld naam**          | **Veld type**                   | **Vereist?** | **Beschrijving**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|-------------------------|----------------------------------|---------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| version                 | 32-bits geheel getal zonder teken (uimsbf) | Vereist      | Bepaalt de indeling van de inhoud van het vak ' mdat '. Niet-herkende versies worden genegeerd. Momenteel is de enige ondersteunde versie 1.                                                                                                                                                                                                                                                                                                                                                      |
| id                      | 32-bits geheel getal zonder teken (uimsbf) | Vereist      | Identificeert dit exemplaar van het bericht. Berichten met een gelijkwaardige semantiek moeten dezelfde waarde hebben; dat wil zeggen dat het verwerken van elk gebeurtenis bericht met dezelfde id voldoende is.                                                                                                                                                                                                                                                                                                            |
| presentation_time_delta | 32-bits geheel getal zonder teken (uimsbf) | Vereist      | De som van de fragment_absolute_time, opgegeven in de TrackFragmentExtendedHeaderBox en de presentation_time_delta **moet** de presentatie tijd van de gebeurtenis zijn. De tijd en duur van de presentatie **moeten** worden uitgelijnd met Stream Access points (SAP) van het type 1 of 2, zoals gedefinieerd in [ISO-14496-12] bijlage I. Voor uitgaand HLS **moeten** tijd en duur worden uitgelijnd met de grenzen van het segment. De presentatie tijd en duur van verschillende gebeurtenis berichten binnen dezelfde gebeurtenis stroom **mogen** elkaar niet overlappen. |
| message                 | bytematrix                       | Vereist      | Het gebeurtenis bericht. Voor [SCTE-35] berichten is het bericht de binaire splice_info_section (). Voor [SCTE-35] berichten **moet** dit de splice_info_section () zijn om berichten te verzenden naar HLS-, Smooth-en dash-clients in overeenstemming met [SCTE-35]. Voor [SCTE-35] berichten is de binaire splice_info_section () de payload van het vak ' mdat ' en is het **niet** base64-gecodeerd.                                                            |

---


### <a name="224-cancellation-and-updates"></a>2.2.4 annuleringen en updates

Berichten kunnen worden geannuleerd of bijgewerkt door meerdere berichten te verzenden met dezelfde presentatie tijd en ID.  De presentatie tijd en-ID identificeren de gebeurtenis uniek. Het laatste bericht dat is ontvangen voor een specifieke presentatie tijd die voldoet aan de voor waarden voor de roll, is het bericht dat wordt verwerkt. Het bijgewerkte bericht vervangt alle eerder ontvangen berichten.  De vooraf-rollen beperking is vier seconden. Berichten die ten minste vier seconden vóór de presentatie tijd zijn ontvangen, worden op de gewenste actie uitgevoerd. 


## <a name="3-timed-metadata-delivery"></a>3 getimede levering van meta gegevens

De gegevens van de gebeurtenis stroom zijn dekkend om te Media Services. Media Services geeft slechts drie stukjes informatie door tussen het opname-en eind punt van de client. De volgende eigenschappen worden aan de client geleverd, in overeenstemming met [SCTE-35] en/of het streaming-protocol van de client:

1.  Schema: een URN of URL die het schema van het bericht aangeeft.
2.  Presentatie tijd: de presentatie tijd van de gebeurtenis op de media tijdlijn.
3.  Duur: de duur van de gebeurtenis.
4.  ID: een optionele unieke id voor de gebeurtenis.
5.  Bericht: de gegevens van de gebeurtenis.

## <a name="31-microsoft-smooth-streaming-manifest"></a>3,1 micro soft Smooth Streaming-manifest  

Raadpleeg de verwerking van sparse track [MS-SSTR] voor meer informatie over het format teren van een sparse-berichten spoor. Voor [SCTE35]-berichten voert Smooth Streaming de met base64 gecodeerde splice_info_section () uit naar een sparse fragment.
De StreamIndex **moet** een subtype van ' data ' hebben en de CustomAttributes **moet** een kenmerk met name = "schema" en value = "urn: scte: scte35:2013: bin" bevatten.

#### <a name="smooth-client-manifest-example-showing-base64-encoded-scte35-splice_info_section"></a>Glad client manifest voorbeeld met base64-gecodeerde [SCTE35] splice_info_section ()
~~~ xml
<?xml version=”1.0” encoding=”utf-8”?>
<SmoothStreamingMedia MajorVersion=”2” MinorVersion=”0” TimeScale=”10000000” IsLive=”true” Duration=”0”
  LookAheadFragmentCount=”2” DVRWindowLength=”6000000000”>

  <StreamIndex Type=”video” Name=”video” Subtype=”” Chunks=”0” TimeScale=”10000000”
    Url=”QualityLevels({bitrate})/Fragments(video={start time})”>
    <QualityLevel Index=”0” Bitrate=”230000”
      CodecPrivateData=”250000010FC3460B50878A0B5821FF878780490800704704DC0000010E5A67F840” FourCC=”WVC1”
      MaxWidth=”364” MaxHeight=”272”/>
    <QualityLevel Index=”1” Bitrate=”305000”
      CodecPrivateData=”250000010FC3480B50878A0B5821FF87878049080894E4A7640000010E5A67F840” FourCC=”WVC1”
      MaxWidth=”364” MaxHeight=”272”/>
    <c t=”0” d=”20000000” r=”300” />
  </StreamIndex>
  <StreamIndex Type=”audio” Name=”audio” Subtype=”” Chunks=”0” TimeScale=”10000000”
    Url=”QualityLevels({bitrate})/Fragments(audio={start time})”>
    <QualityLevel Index=”0” Bitrate=”96000” CodecPrivateData=”1000030000000000000000000000E00042C0”
      FourCC=”WMAP” AudioTag=”354” Channels=”2” SamplingRate=”44100” BitsPerSample=”16” PacketSize=”4459”/>
    <c t=”0” d=”20000000” r=”300” />
  </StreamIndex>
  <StreamIndex Type=”text” Name=”scte35-sparse-stream” Subtype=”DATA” Chunks=”0” TimeScale=”10000000”
    ParentStreamIndex=”video” ManifestOutput=”true” 
    Url=”QualityLevels({bitrate})/Fragments(captions={start time})”>
    <QualityLevel Index=”0” Bitrate=”0” CodecPrivateData=”” FourCC=””>
      <CustomAttributes>
        <Attribute Name=”Scheme” Value=”urn:scte:scte35:2013:bin”/>
      </CustomAttributes>
    </QualityLevel>
    <!-- The following <c> and <f> fragments contains the base64-encoded [SCTE35] splice_info_section() message -->
    <c t=”600000000” d=”300000000”>    <f>PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz48QWNxdWlyZWRTaWduYWwgeG1sbnM9InVybjpjYWJsZWxhYnM6bWQ6eHNkOnNpZ25hbGluZzozLjAiIGFjcXVpc2l0aW9uUG9pbnRJZGVudGl0eT0iRVNQTl9FYXN0X0FjcXVpc2l0aW9uX1BvaW50XzEiIGFjcXVpc2l0aW9uU2lnbmFsSUQ9IjRBNkE5NEVFLTYyRkExMUUxQjFDQTg4MkY0ODI0MDE5QiIgYWNxdWlzaXRpb25UaW1lPSIyMDEyLTA5LTE4VDEwOjE0OjI2WiI+PFVUQ1BvaW50IHV0Y1BvaW50PSIyMDEyLTA5LTE4VDEwOjE0OjM0WiIvPjxTQ1RFMzVQb2ludERlc2NyaXB0b3Igc3BsaWNlQ29tbWFuZFR5cGU9IjUiPjxTcGxpY2VJbnNlcnQgc3BsaWNlRXZlbnRJRD0iMzQ0NTY4NjkxIiBvdXRPZk5ldHdvcmtJbmRpY2F0b3I9InRydWUiIHVuaXF1ZVByb2dyYW1JRD0iNTUzNTUiIGR1cmF0aW9uPSJQVDFNMFMiIGF2YWlsTnVtPSIxIiBhdmFpbHNFeHBlY3RlZD0iMTAiLz48L1NDVEUzNVBvaW50RGVzY3JpcHRvcj48U3RyZWFtVGltZXM+PFN0cmVhbVRpbWUgdGltZVR5cGU9IkhTUyIgdGltZVZhbHVlPSI1MTUwMDAwMDAwMDAiLz48L1N0cmVhbVRpbWVzPjwvQWNxdWlyZWRTaWduYWw+</f>
    </c>
    <c t=”1200000000” d=”400000000”>      <f>PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz48QWNxdWlyZWRTaWduYWwgeG1sbnM9InVybjpjYWJsZWxhYnM6bWQ6eHNkOnNpZ25hbGluZzozLjAiIGFjcXVpc2l0aW9uUG9pbnRJZGVudGl0eT0iRVNQTl9FYXN0X0FjcXVpc2l0aW9uX1BvaW50XzEiIGFjcXVpc2l0aW9uU2lnbmFsSUQ9IjRBNkE5NEVFLTYyRkExMUUxQjFDQTg4MkY0ODI0MDE5QiIgYWNxdWlzaXRpb25UaW1lPSIyMDEyLTA5LTE4VDEwOjE0OjI2WiI+PFVUQ1BvaW50IHV0Y1BvaW50PSIyMDEyLTA5LTE4VDEwOjE0OjM0WiIvPjxTQ1RFMzVQb2ludERlc2NyaXB0b3Igc3BsaWNlQ29tbWFuZFR5cGU9IjUiPjxTcGxpY2VJbnNlcnQgc3BsaWNlRXZlbnRJRD0iMzQ0NTY4NjkxIiBvdXRPZk5ldHdvcmtJbmRpY2F0b3I9InRydWUiIHVuaXF1ZVByb2dyYW1JRD0iNTUzNTUiIGR1cmF0aW9uPSJQVDFNMFMiIGF2YWlsTnVtPSIxIiBhdmFpbHNFeHBlY3RlZD0iMTAiLz48L1NDVEUzNVBvaW50RGVzY3JpcHRvcj48U3RyZWFtVGltZXM+PFN0cmVhbVRpbWUgdGltZVR5cGU9IkhTUyIgdGltZVZhbHVlPSI1MTYyMDAwMDAwMDAiLz48L1N0cmVhbVRpbWVzPjwvQWNxdWlyZWRTaWduYWw+</f>
    </c>
  </StreamIndex>
</SmoothStreamingMedia>
~~~

## <a name="32-apple-hls-manifest-decoration"></a>3,2 Apple HLS-manifest decoratie

Azure Media Services ondersteunt de volgende HLS-manifest Tags voor de Signa lering van AD beschik bare gegevens tijdens een live-of on-demand-gebeurtenis. 

- EXT-X-DATERANGE zoals gedefinieerd in Apple HLS [RFC8216]
- EXT-X-CUE zoals gedefinieerd in [Adobe-Primetime]-deze modus wordt als ' verouderd ' beschouwd. Klanten moeten indien mogelijk de tag EXT-X-DATERANGE aannemen.

De gegevens uitvoer naar elke tag kan variëren op basis van de gebruikte opname signaal modus. Bijvoorbeeld, RTMP-opname met de eenvoudige modus van Adobe bevat niet de volledige SCTE-35-nettolading met base64-code ring.

## <a name="321-apple-hls-with-adobe-primetime-ext-x-daterange-recommended"></a>3.2.1 Apple HLS met Adobe Primetime EXT-X-DATERANGE (aanbevolen)

Met de specificatie Apple HTTP Live Streaming [RFC8216] kunnen berichten van [SCTE-35] worden gesignaleerd. De berichten worden ingevoegd in de segment-afspeel lijst in een EXT-X-DATERANGE-label per [RFC8216]-sectie met de titel ' mapping SCTE-35 in EXT-X-DATERANGE '.  De laag van de client kan de M3U-afspeel lijst en de M3U-labels voor processen parseren of de gebeurtenissen via het Apple Player-Framework ontvangen.  

De **Aanbevolen** benadering van Azure Media Services (versie 3 API) is het volgen van [RFC8216] en het gebruik van de ext-X_DATERANGE-tag voor [SCTE35] AD beschik bare decoratie in het manifest.

## <a name="322-apple-hls-with-adobe-primetime-ext-x-cue-legacy"></a>3.2.2 Apple HLS met Adobe Primetime EXT-X-CUE (verouderd)

Er is ook een ' verouderde ' implementatie beschikbaar in Azure Media Services (versie 2 en 3 API) die gebruikmaakt van de EXT-X-CUE-tag zoals gedefinieerd in [Adobe-Primetime] "SCTE-35-modus". In deze modus worden met Azure Media Services met base64 gecodeerde [SCTE-35] splice_info_section () Inge sloten in de tag EXT-X-CUE.  

De "verouderde" EXT-X-CUE-tag wordt hieronder gedefinieerd en er kan ook normatieve worden verwezen in de [Adobe-Primetime]-specificatie. Dit mag alleen worden gebruikt voor verouderde SCTE35-Signa lering wanneer dat nodig is, anders wordt de aanbevolen tag gedefinieerd in [RFC8216] als EXT-X-DATERANGE. 

| **Kenmerk naam** | **Type**                      | **Vereist?**                             | **Beschrijving**                                                                                                                                                                                                                                                                      |
|--------------------|-------------------------------|-------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| WMS                | teken reeks tussen aanhalings tekens                 | Vereist                                  | Het bericht dat is gecodeerd als een base64-gecodeerde teken reeks, zoals beschreven in [RFC4648]. Voor [SCTE-35] berichten is dit de met base64 gecodeerde splice_info_section ().                                                                                                |
| TYPE               | teken reeks tussen aanhalings tekens                 | Vereist                                  | Een URN of URL die het bericht schema aangeeft. Voor [SCTE-35]-berichten neemt het type de speciale waarde ' scte35 '.                                                                                                                                |
| id                 | teken reeks tussen aanhalings tekens                 | Vereist                                  | Een unieke id voor de gebeurtenis. Als de ID niet is opgegeven wanneer het bericht wordt opgenomen, wordt door Azure Media Services een unieke id gegenereerd.                                                                                                                                          |
| HEBBEN           | decimaal getal met drijvende komma | Vereist                                  | De duur van de gebeurtenis. Als dit onbekend is, **moet** de waarde 0 zijn. Eenheden zijn factional seconden.                                                                                                                                                                                           |
| VERSTREKEN            | decimaal getal met drijvende komma | Optioneel, maar vereist voor sliding window | Wanneer het signaal wordt herhaald ter ondersteuning van een schuif presentatie venster, **moet** dit veld de hoeveelheid presentatie tijd zijn die is verstreken sinds de gebeurtenis is gestart. Eenheden zijn fractie seconden. Deze waarde kan groter zijn dan de oorspronkelijke opgegeven duur van de Splice of het segment. |
| TIME               | decimaal getal met drijvende komma | Vereist                                  | De presentatie tijd van de gebeurtenis. Eenheden zijn fractie seconden.                                                                                                                                                                                                                    |


In de toepassingslaag van de HLS-speler wordt het TYPE gebruikt om de indeling van het bericht te identificeren, het bericht te decoderen, de benodigde tijds conversies toe te passen en de gebeurtenis te verwerken.  De gebeurtenissen worden gesynchroniseerd in de segment-afspeel lijst van het bovenliggende spoor, op basis van de tijds tempel van de gebeurtenis.  Ze worden vóór het dichtstbijzijnde segment (#EXTINF tag) ingevoegd.

### <a name="323-hls-segment-playlist-example-using-legacy-adobe-primetime-ext-x-cue"></a>Voor beeld van een HLS-segment afspeel lijst van 3.2.3 met ' legacy ' Adobe Primetime EXT-X-CUE

In het volgende voor beeld ziet u de HLS-manifest decoratie met behulp van de tag Adobe Primetime EXT-X-CUE.  De para meter ' CUE ' bevat de volledige met base64 gecodeerde SCTE-35 splice_info Payload, waarmee wordt aangegeven dat dit signaal is ontvangen met behulp van RTMP in de Adobe SCTE-35-signaal modus, of dat het is opgenomen in de signaal modus via Smooth Streaming SCTE-35. 

~~~
#EXTM3U
#EXT-X-VERSION:4
#EXT-X-ALLOW-CACHE:NO
#EXT-X-MEDIA-SEQUENCE:346
#EXT-X-TARGETDURATION:6
#EXT-X-I-FRAMES-ONLY
#EXT-X-PROGRAM-DATE-TIME:2018-12-13T15:54:19.462Z
#EXTINF:4.000000,no-desc
KeyFrames(video_track=15447164594627600,format=m3u8-aapl)
#EXTINF:6.000000,no-desc
KeyFrames(video_track=15447164634627600,format=m3u8-aapl)
#EXT-X-CUE:ID="1026",TYPE="scte35",DURATION=30.000000,TIME=1544716520.022760,CUE="/DAlAAAAAAAAAP/wFAUAAAQCf+//KRjAfP4AKTLgAAAAAAAAVYsh2w=="
#EXTINF:6.000000,no-desc
KeyFrames(video_track=15447165474627600,format=m3u8-aapl)
~~~

### <a name="324-hls-message-handling-for-legacy-adobe-primetime-ext-x-cue"></a>3.2.4 HLS-bericht afhandeling voor "verouderde" Adobe Primetime EXT-X-CUE

Gebeurtenissen worden gesignaleerd in de segment-afspeel lijst van elk video-en audio spoor. De positie van het label EXT-X-CUE **moet** altijd direct vóór het eerste HLS-segment (voor het starten of het sluiten van het segment) of direct na het laatste HLS-segment (voor Splice in of segment einde) zijn waarnaar de kenmerken voor tijd en duur verwijzen, zoals vereist door [Adobe-Primetime].

Wanneer een verschuivende presentatie venster is ingeschakeld, **moet** de tag Ext-X-Cue vaak voldoende worden herhaald, waardoor de Splice of het segment altijd volledig wordt beschreven in de segment afspeel lijst en het verstreken kenmerk **moet** worden gebruikt om aan te geven hoe lang de Splice of segment is actief, zoals vereist door [Adobe-Primetime].

Wanneer een verschuivende presentatie venster is ingeschakeld, worden de Tags EXT-X-CUE verwijderd uit de segment-afspeel lijst wanneer de media tijd waarnaar ze verwijzen, is uitgerold uit het venster voor de schuif weergave.

## <a name="33-dash-manifest-decoration-mpd"></a>3,3-streep manifest decoratie (MPD)

[MPEGDASH] biedt drie manieren om gebeurtenissen te Signa leren:

1.  Gebeurtenissen die in het MPD-EventStream worden gesignaleerd
2.  In-band gesignaleerde gebeurtenissen met behulp van het gebeurtenis bericht venster (' emsg ')
3.  Een combi natie van zowel 1 als 2

Gebeurtenissen die zijn gesignaleerd in de MPD EventStream zijn handig voor VOD streaming, omdat clients toegang hebben tot alle gebeurtenissen, onmiddellijk wanneer de MPD wordt gedownload. Het is ook handig voor SSAI-Signa lering, waarbij de leverancier van de downstream SSAI de signalen moet parseren vanuit een MPD-manifest met meerdere Peri Oden en dynamische AD-inhoud kunt invoegen.  De oplossing ' in-band ' (' emsg ') is handig voor live streaming waarbij clients de MPD niet opnieuw hoeven te downloaden, of als er geen SSAI-manifest bewerking plaatsvindt tussen de client en de oorsprong. 

Azure Media Services standaard gedrag voor een streepje is om in het EventStream en in de band een signaal te geven met behulp van het gebeurtenis bericht box (' emsg ').

Cue-berichten die zijn opgenomen in [RTMP] of [MS-SSTR-opname], worden toegewezen aan streep gebeurtenissen, met behulp van de vakken in-band emsg en/of in-MPD EventStreams. 

In-band SCTE-35-Signa lering voor streepje volgt de definitie en vereisten die zijn gedefinieerd in [SCTE-214-3] en in [streepje-IF-ONTDUBBELINGS IOP] sectie 13.12.2 (' SCTE35 Events '). 

Voor het Trans Port in-band [SCTE-35] gebruikt het gebeurtenis bericht venster (' emsg ') de schemeId = "urn: SCTE: scte35:2013: bin". Voor de MPD-manifest decoratie maakt de EventStream schemeId gebruik van ' urn: scte: scte35:2014: XML + bin '.  Deze indeling is een XML-weer gave van de gebeurtenis, inclusief een binaire, met base64 gecodeerde uitvoer van het volledige SCTE-35-bericht dat bij opname is aangekomen. 

Normatieve referentie definities van het vervoer van [SCTE-35]-Hint berichten in het DASH zijn beschikbaar in [SCTE-214-1] SEC 6.7.4 (MPD) en [SCTE-214-3] SEC 7.3.2 (Trans Port van SCTE 35-Hint berichten).

### <a name="331-mpeg-dash-mpd-eventstream-signaling"></a>3.3.1 MPEG-streepje (MPD) EventStream-Signa lering

Manifest (MPD) van gebeurtenissen wordt in de MPD gesignaleerd met behulp van het EventStream-element, dat binnen het period-element wordt weer gegeven. De schemeId die wordt gebruikt is "urn: scte: scte35:2014: XML + bin".

> [!NOTE]
> Voor de boog doelen [SCTE-35] staat het gebruik toe van de sectie met base64-code ring in het element Signal. binary (in plaats van het element Signal. SpliceInfoSection) als alternatief voor het vervoer van een volledig geparseerd bericht van een hint.
> Azure Media Services maakt gebruik van deze ' XML + bin '-benadering voor Signa lering in het MPD-manifest.
> Dit is ook de aanbevolen methode die wordt gebruikt in de sectie [DASH-IF-ONTDUBBELINGS IOP]-Zie het gedeelte [' ad-invoeg gebeurtenis stromen ' van het streepje weer geven als ontdubbelings IOP richtlijn](https://dashif-documents.azurewebsites.net/DASH-IF-IOP/master/DASH-IF-IOP.html#ads-insertion-event-streams)
> 

Het EventStream-element heeft de volgende kenmerken:

| **Kenmerk naam** | **Type**                | **Vereist?** | **Beschrijving**                                                                                                                                                                                                                                                                                   |
|--------------------|-------------------------|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| scheme_id_uri      | string                  | Vereist      | Hiermee wordt het schema van het bericht aangegeven. Het schema wordt ingesteld op de waarde van het kenmerk schema in het vak Live server-manifest. De waarde **moet** een urn of URL zijn die het bericht schema aangeeft. De ondersteunde uitvoer schemeId moet "urn: scte: scte35:2014: XML + bin" per [SCTE-214-1] SEC 6.7.4 (MPD) zijn, aangezien de service op dit moment alleen de XML + bin ondersteunt voor het boog gebruik in de MPD.  |
| value              | string                  | Optioneel      | Een extra teken reeks waarde die door de eigen aren van het schema wordt gebruikt om de semantiek van het bericht aan te passen. Als u meerdere gebeurtenis stromen wilt onderscheiden met hetzelfde schema, **moet** de waarde worden ingesteld op de naam van de gebeurtenis stroom (Tracknaam voor [MS-SSTR-ingestie] of AMF-bericht naam voor een [RTMP]-opname). |
| Timescale          | 32-bits geheel getal zonder teken | Vereist      | De tijd schaal, in ticks per seconde.                                                                                                                                                                                                       |


### <a name="332-example-mpeg-dash-manifest-mpd-signaling-of-scte-35-using-eventstream"></a>3.3.2, voor beeld van MPEG DASH manifest (MPD)-Signa lering van SCTE-35 met EventStream

~~~ xml
<!-- Example MPD using xml+bin style signaling per [SCTE-214-1] -->
  <EventStream schemeIdUri="urn:scte:scte35:2014:xml+bin" value="scte35_track_001_000" timescale="10000000">
        <Event presentationTime="15447165200227600" duration="300000000" id="1026">
            <scte35:Signal>
                <scte35:Binary>
                    /DAlAAAAAAAAAP/wFAUAAAQDf+//KaeGwP4AKTLgAAAAAAAAn75a3g==
                </scte35:Binary>
            </scte35:Signal>
        </Event>
        <Event presentationTime="15447166250227600" duration="300000000" id="1027">
           <scte35:Signal>
                <scte35:Binary>
                    /DAlAAAAAAAAAP/wFAUAAAQDf+//KaeGwP4AKTLgAAAAAAAAn75a3g==
                </scte35:Binary>
            </scte35:Signal>
        </Event>
    </EventStream>
~~~

> [!IMPORTANT]
> Houd er rekening mee dat presentationTime de presentatie tijd van de gebeurtenis [SCTE-35] is die relatief is ten opzichte van de begin tijd van de periode, niet de aankomst tijd van het bericht.
> [MPEGDASH] definieert de Event@presentationTime as "bepaalt de tijd van de presentatie van de gebeurtenis ten opzichte van het begin van de periode.
> De waarde van de presentatie tijd in seconden is de deling van de waarde van dit kenmerk en de waarde van het EventStream@timescale kenmerk.
> Als deze niet aanwezig is, is de waarde van de tijd van de presentatie 0.


### <a name="333-mpeg-dash-in-band-event-message-box-signaling"></a>3.3.3 MPEG-DASH in-band gebeurtenis bericht Box signalering

Voor een in-band-gebeurtenis stroom moet de mpd een InbandEventStream-element hebben op het niveau van de aanpassings.  Dit element heeft een verplicht schemeIdUri-kenmerk en een optioneel tijdschaal kenmerk dat ook wordt weer gegeven in het gebeurtenis bericht venster (' emsg ').  Gebeurtenis bericht vakken met schema-id's die niet in de MPD zijn gedefinieerd, **mogen** niet aanwezig zijn.

Voor het Trans Port in-band [SCTE-35] **moeten** signalen de schemeId = "urn: SCTE: scte35:2013: bin" gebruiken.
Normatieve definities van het vervoer van [SCTE-35] in-band berichten worden gedefinieerd in [SCTE-214-3] SEC 7.3.2 (Trans Port van SCTE 35 Cue-berichten).

In de volgende details worden de specifieke waarden beschreven die de client in overeenstemming met [SCTE-214-3] verwacht in het ' emsg ':

| **Veld naam**          | **Veld type**          | **Vereist?** | **Beschrijving**                                                                                                                                                                                                                                                                                                                                                    |
|-------------------------|-------------------------|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| scheme_id_uri           | string                  | Vereist      | Hiermee wordt het schema van het bericht aangegeven. Het schema wordt ingesteld op de waarde van het kenmerk schema in het vak Live server-manifest. De waarde **moet** een urn zijn waarmee het bericht schema wordt aangeduid. Voor [SCTE-35] berichten **moet** dit ' urn: SCTE: scte35:2013: bin ' zijn in overeenstemming met [SCTE-214-3] |
| Value                   | string                  | Vereist      | Een extra teken reeks waarde die door de eigen aren van het schema wordt gebruikt om de semantiek van het bericht aan te passen. Als u meerdere gebeurtenis stromen wilt onderscheiden met hetzelfde schema, wordt de waarde ingesteld op de naam van de gebeurtenis stroom (Tracknaam voor Smooth-opname of AMF-bericht naam voor RTMP-opname).                                                                  |
| Timescale               | 32-bits geheel getal zonder teken | Vereist      | De tijd schaal, in maten per seconde, van de velden tijden en duur in het vak ' emsg '.                                                                                                                                                                                                                                                                        |
| Presentation_time_delta | 32-bits geheel getal zonder teken | Vereist      | De tijd van de media presentatie verschilt van de presentatie tijd van de gebeurtenis en de eerste presentatie tijd in dit segment. De tijd en duur van de presentatie **moeten** worden uitgelijnd met Stream Access points (SAP) van het type 1 of 2, zoals gedefinieerd in [ISO-14496-12] bijlage I.                                                                                            |
| event_duration          | 32-bits geheel getal zonder teken | Vereist      | De duur van de gebeurtenis, of 0xFFFFFFFF, om een onbekende duur aan te geven.                                                                                                                                                                                                                                                                                          |
| ID                      | 32-bits geheel getal zonder teken | Vereist      | Identificeert dit exemplaar van het bericht. Berichten met een gelijkwaardige semantiek moeten dezelfde waarde hebben. Als de ID niet is opgegeven wanneer het bericht wordt opgenomen, wordt door Azure Media Services een unieke id gegenereerd.                                                                                                                                                    |
| Message_data            | bytematrix              | Vereist      | Het gebeurtenis bericht. Voor [SCTE-35] berichten zijn de bericht gegevens de binaire splice_info_section () in overeenstemming met [SCTE-214-3] |

### <a name="334-dash-message-handling"></a>Verwerking van 3.3.4-streepje-berichten

Gebeurtenissen worden in het vak ' emsg ' in een gesignaleerd bereik weer gegeven voor video-en audio-tracks.  Het signaal wordt weer gegeven voor alle segment aanvragen waarvoor de presentation_time_delta 15 seconden of minder is. 

Wanneer een verschuivende presentatie venster is ingeschakeld, worden gebeurtenis berichten uit de MPD verwijderd wanneer de som van de tijd en duur van het gebeurtenis bericht kleiner is dan de tijd van de media gegevens in het manifest.  Met andere woorden, de gebeurtenis berichten worden uit het manifest verwijderd wanneer de media tijd waarnaar ze verwijzen, uit het schuif venster wordt Gerolt.

## <a name="4-scte-35-ingest-implementation-guidance-for-encoder-vendors"></a>4. SCTE-35-implementatie richtlijnen voor encoder leveranciers

De volgende richt lijnen zijn veelvoorkomende problemen die van invloed kunnen zijn op de implementatie van de leverancier van het coderings programma van deze specificatie.  De onderstaande richt lijnen zijn verzameld op basis van de feedback van de echte wereld partner, zodat het eenvoudiger is om deze specificatie voor anderen te implementeren. 

[SCTE-35] berichten worden opgenomen in binaire indeling met het schema **"urn: SCTE: scte35:2013: bin"** voor [MS-SSTR-opname] en het type **"scte35"** voor [RTMP] opnemen. Voor het vergemakkelijken van de conversie van de timing [SCTE-35], die is gebaseerd op een MPEG-2-transport stroom (PTS), wordt een toewijzing tussen PTS (pts_time + pts_adjustment van de splice_time ()) en de tijd lijn van de media verzorgd door de gebeurtenis presentatie ( het veld fragment_absolute_time voor Smooth-opname en het tijd veld voor RTMP-opname). De toewijzing is nood zakelijk omdat de 33-bits waarde PTS ongeveer elke 26,5 uur rolt.

Smooth Streaming opname [MS-SSTR-ingestie] vereist dat de media Data Box (' mdat ') de **splice_info_section ()** bevatten die is gedefinieerd in [SCTE-35]. 

Voor RTMP-opname wordt het kenmerk Cue van het AMF-bericht ingesteld op de met base64 gecodeerde **splice_info_section ()** die is gedefinieerd in [SCTE-35].  

Wanneer de berichten de indeling hebben die hierboven is beschreven, worden ze verzonden naar HLS-, Smooth-en DASH-clients zoals hierboven is gedefinieerd.  

Wanneer u uw implementatie met het Azure Media Services-platform test, moet u eerst testen met een ' Pass-through ' LiveEvent voordat u doorgaat met het testen van een coderings LiveEvent.

---

## <a name="next-steps"></a>Volgende stappen
Media Services leer trajecten weer geven.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
