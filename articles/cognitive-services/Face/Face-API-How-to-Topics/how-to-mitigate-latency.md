---
title: Latentie beperken bij het gebruik van de face-service
titleSuffix: Azure Cognitive Services
description: Meer informatie over het beperken van de latentie bij het gebruik van de face-service.
services: cognitive-services
author: v-jaswel
manager: chrhoder
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 1/5/2021
ms.author: v-jawe
ms.openlocfilehash: 07c9bd12664a94c64a0d0b37d638b5668cc7f61e
ms.sourcegitcommit: fc401c220eaa40f6b3c8344db84b801aa9ff7185
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/20/2021
ms.locfileid: "98605502"
---
# <a name="how-to-mitigate-latency-when-using-the-face-service"></a>Procedure: latentie beperken bij het gebruik van de face-service

Er kan een latentie optreden bij het gebruik van de service face. Latentie verwijst naar een wille keurige vertraging die optreedt bij communicatie via een netwerk. Over het algemeen zijn mogelijke oorzaken van latentie:
- De fysieke afstand van elk pakket moet worden getransporteerd van de bron naar het doel.
- Problemen met het verzend medium.
- Fouten in routers of switches langs het transmissie traject.
- De tijd die nodig is voor antivirus toepassingen, firewalls en andere beveiligings mechanismen voor het inspecteren van pakketten.
- Storingen in client-of server toepassingen.

In dit onderwerp vindt u informatie over mogelijke oorzaken van een latentie die specifiek is voor het gebruik van de Azure-Cognitive Services en hoe u deze oorzaken kunt verhelpen.

> [!NOTE]
> Azure Cognitive Services bieden geen enkele Service Level Agreement (SLA) over latentie.

## <a name="possible-causes-of-latency"></a>Mogelijke oorzaken van een latentie

### <a name="slow-connection-between-the-cognitive-service-and-a-remote-url"></a>Trage verbinding tussen de cognitieve service en een externe URL

Sommige Azure-Cognitive Services bieden methoden die gegevens ophalen van een externe URL die u opgeeft. Wanneer u bijvoorbeeld de [methode DetectWithUrlAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceoperationsextensions.detectwithurlasync?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Vision_Face_FaceOperationsExtensions_DetectWithUrlAsync_Microsoft_Azure_CognitiveServices_Vision_Face_IFaceOperations_System_String_System_Nullable_System_Boolean__System_Nullable_System_Boolean__System_Collections_Generic_IList_System_Nullable_Microsoft_Azure_CognitiveServices_Vision_Face_Models_FaceAttributeType___System_String_System_Nullable_System_Boolean__System_String_System_Threading_CancellationToken_) van de face-service aanroept, kunt u de URL opgeven van een installatie kopie waarin de service probeert gezichten te detecteren.

```csharp
var faces = await client.Face.DetectWithUrlAsync("https://www.biography.com/.image/t_share/MTQ1MzAyNzYzOTgxNTE0NTEz/john-f-kennedy---mini-biography.jpg");
```

De face-service moet vervolgens de installatie kopie downloaden van de externe server. Als de verbinding van de face-service met de externe server traag is, heeft dit invloed op de reactie tijd van de detectie methode.

Als u dit wilt verhelpen, kunt u overwegen om [de installatie kopie op te slaan in azure Premium Blob Storage](https://docs.microsoft.com/azure/storage/blobs/storage-upload-process-images?tabs=dotnet).

### <a name="large-upload-size"></a>Grote upload grootte

Sommige Azure-Cognitive Services bieden methoden die gegevens ophalen uit een bestand dat u uploadt. Wanneer u bijvoorbeeld de [methode DetectWithStreamAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceoperationsextensions.detectwithstreamasync?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Vision_Face_FaceOperationsExtensions_DetectWithStreamAsync_Microsoft_Azure_CognitiveServices_Vision_Face_IFaceOperations_System_IO_Stream_System_Nullable_System_Boolean__System_Nullable_System_Boolean__System_Collections_Generic_IList_System_Nullable_Microsoft_Azure_CognitiveServices_Vision_Face_Models_FaceAttributeType___System_String_System_Nullable_System_Boolean__System_String_System_Threading_CancellationToken_) van de face-service aanroept, kunt u een installatie kopie uploaden waarin de service wordt getracht gezichten te detecteren.

```csharp
using FileStream fs = File.OpenRead(@"C:\images\face.jpg");
System.Collections.Generic.IList<DetectedFace> faces = await client.Face.DetectWithStreamAsync(fs, detectionModel: DetectionModel.Detection02);
```

Als het te uploaden bestand groot is, is dat van invloed op de reactie tijd van de `DetectWithStreamAsync` methode, om de volgende redenen:
- Het duurt langer om het bestand te uploaden.
- Het duurt langer voordat de service het bestand verwerkt, in verhouding tot de bestands grootte.

Oplossingen
- Overweeg [de installatie kopie op te slaan in azure Premium Blob Storage](https://docs.microsoft.com/azure/storage/blobs/storage-upload-process-images?tabs=dotnet).
- Overweeg een kleiner bestand te uploaden.
    - Zie de richt lijnen met betrekking tot [invoer gegevens voor gezichts detectie](https://docs.microsoft.com/azure/cognitive-services/face/concepts/face-detection#input-data) en [invoer gegevens voor gezichts herkenning](https://docs.microsoft.com/azure/cognitive-services/face/concepts/face-recognition#input-data).
    - Bij gezichts detectie, wanneer detectie model wordt gebruikt `DetectionModel.Detection01` , wordt de verwerkings snelheid verhoogd wanneer de grootte van het afbeeldings bestand wordt gereduceerd. Wanneer u detectie model gebruikt `DetectionModel.Detection02` , wordt de verwerkings snelheid van de afbeeldings bestands grootte alleen verhoogd als het afbeeldings bestand kleiner is dan 1920.
    - Voor gezichts herkenning is het verminderen van de face-grootte naar 200x200 pixels niet van invloed op de nauw keurigheid van het herkennings model.
    - De prestaties van de- `DetectWithUrlAsync` en- `DetectWithStreamAsync` methoden zijn ook afhankelijk van het aantal gezichten dat zich in een afbeelding bevindt. De face-service kan tot 100 gezichten retour neren voor een installatie kopie. Gezichten worden gerangschikt op basis van de grootte van het gezichts kader van groot naar klein.
    - Als u meerdere service methoden moet aanroepen, kunt u overwegen deze parallel aan te roepen als uw toepassings ontwerp dit toestaat. Als u bijvoorbeeld gezichten in twee afbeeldingen moet detecteren om een gezichts vergelijking uit te voeren:
```csharp
var faces_1 = client.Face.DetectWithUrlAsync("https://www.biography.com/.image/t_share/MTQ1MzAyNzYzOTgxNTE0NTEz/john-f-kennedy---mini-biography.jpg");
var faces_2 = client.Face.DetectWithUrlAsync("https://www.biography.com/.image/t_share/MTQ1NDY3OTIxMzExNzM3NjE3/john-f-kennedy---debating-richard-nixon.jpg");
Task.WaitAll (new Task<IList<DetectedFace>>[] { faces_1, faces_2 });
IEnumerable<DetectedFace> results = faces_1.Result.Concat (faces_2.Result);
```

### <a name="slow-connection-between-your-compute-resource-and-the-face-service"></a>Trage verbinding tussen uw reken resource en de face-service

Als uw computer een trage verbinding heeft met de face-service, is dit van invloed op de reactie tijd van service methoden.

Oplossingen
- Wanneer u uw gezichts abonnement maakt, moet u ervoor zorgen dat u de regio kiest die het dichtst bij de host ligt waar uw toepassing wordt gehost.
- Als u meerdere service methoden moet aanroepen, kunt u overwegen deze parallel aan te roepen als uw toepassings ontwerp dit toestaat. Zie de vorige sectie voor een voor beeld.

## <a name="next-steps"></a>Volgende stappen

In deze hand leiding hebt u geleerd hoe u de latentie kunt beperken wanneer u de face-service gebruikt. Vervolgens leert u hoe u kunt opschalen van bestaande PersonGroup-en FaceList-objecten naar respectievelijk LargePersonGroup-en LargeFaceList-objecten.

> [!div class="nextstepaction"]
> [Voorbeeld: De functie Grootschalig gebruiken](how-to-use-large-scale.md)

## <a name="related-topics"></a>Verwante onderwerpen

- [Referentie documentatie (REST)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
- [Referentie documentatie (.NET SDK)](/dotnet/api/overview/azure/cognitiveservices/client/faceapi?view=azure-dotnet)
