---
title: 'Quickstart: De Text Analytics-service met behulp van de Go SDK aanroepen'
titleSuffix: Azure Cognitive Services
description: Get-informatie en codevoorbeelden om u te helpen snel aan de slag met behulp van de Tekstanalyse-API in Microsoft Cognitive Services.
services: cognitive-services
author: laramume
manager: assafi
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 05/23/2019
ms.author: aahi
ms.openlocfilehash: 44def29292bc882fdaa08ff76667742756f178b8
ms.sourcegitcommit: 8c49df11910a8ed8259f377217a9ffcd892ae0ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/29/2019
ms.locfileid: "66299609"
---
# <a name="quickstart-call-the-text-analytics-service-using-the-go-sdk"></a>Quickstart: De Text Analytics-service met behulp van de Go SDK aanroepen 
<a name="HOLTop"></a>

Gebruik deze Quick Start om te beginnen met het analyseren van de taal met de Text Analytics-SDK voor Go. Dit artikel ziet u hoe u de taal detecteren, sentiment analyseren, sleuteltermen extraheren en gekoppelde entiteiten identificeren. De REST-API is compatibel met de meeste moderne programmeertalen, biedt de SDK een eenvoudige manier om de service te integreren in uw toepassingen. De broncode voor dit voorbeeld is te vinden op [GitHub](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/cognitiveservices).

## <a name="prerequisites"></a>Vereisten

* De Text Analytics [SDK voor Go](https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices/v2.1/textanalytics)

[!INCLUDE [cognitive-services-text-analytics-signup-requirements](../../../../includes/cognitive-services-text-analytics-signup-requirements.md)]

U moet ook de [eindpunt- en toegangssleutel](../How-tos/text-analytics-how-to-access-key.md) hebben die voor u is gegenereerd tijden de registratie.

## <a name="set-up-a-new-project"></a>Instellen van een nieuw Project

Maak een nieuwe Go-project in uw favoriete code-editor of IDE. De volgende importinstructie toevoegen aan de Go-bestand.

```golang
import (
    "context"
    "encoding/json"
    "fmt"
    "github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics"
    "github.com/Azure/go-autorest/autorest"
)
```

Toevoegen aan uw project de volgende functies zoals de meeste van de parameters en de eigenschappen voor deze Quick Start tekenreeks en bool aanwijzers verwacht.

```golang
// returns a pointer to the string value passed in.
func StringPointer(v string) *string {
    return &v
}

// returns a pointer to the bool value passed in.
func BoolPointer(v bool) *bool {
    return &v
}
```

## <a name="create-text-analytics-client-and-authenticate-credentials"></a>Text Analytics-Client maken en verifiëren van referenties

In de main-functie van uw project, maakt u een nieuw `TextAnalytics` object. Gebruik de juiste Azure-regio voor uw Text Analytics-abonnement. Bijvoorbeeld: `https://eastus.api.cognitive.microsoft.com`. Als u een proefversie sleutel gebruikt, moet u niet bijwerken van de locatie.

```golang
//Replace 'eastus' with the correct region for your Text Analytics subscription
textAnalyticsClient := textanalytics.New("https://eastus.api.cognitive.microsoft.com")
```

Maak een variabele voor uw sleutel en deze doorgeven aan de functie `autorest.NewCognitiveServicesAuthorizer` die vervolgens worden doorgegeven aan van de client `authorizer` eigenschap.

```golang
subscriptionKey := "<<subscriptionKey>>"
textAnalyticsClient.Authorizer = autorest.NewCognitiveServicesAuthorizer(subscriptionKey)
```

## <a name="sentiment-analysis"></a>Stemmingsanalyse

Maak een nieuwe functie met de naam `SentimentAnalysis()` waarmee de client eerder hebt gemaakt. Maak een lijst van `MultiLanguageInput` objecten, met de documenten die u wilt analyseren. Elk object bevat een `id`, `Language` en een `text` kenmerk. De `text` slaat de tekst die moet worden geanalyseerd, kenmerk `language` is de taal van het document en de `id` is een waarde. 

```golang
func SentimentAnalysis(textAnalyticsclient textanalytics.BaseClient) {

    ctx := context.Background()
    inputDocuments := []textanalytics.MultiLanguageInput {
        textanalytics.MultiLanguageInput {
            Language: StringPointer("en"),
            ID:StringPointer("0"),
            Text:StringPointer("I had the best day of my life."),
        },
        textanalytics.MultiLanguageInput {
            Language: StringPointer("en"),
            ID:StringPointer("1"),
            Text:StringPointer("This was a waste of my time. The speaker put me to sleep."),
        },
        textanalytics.MultiLanguageInput {
            Language: StringPointer("es"),
            ID:StringPointer("2"),
            Text:StringPointer("No tengo dinero ni nada que dar..."),
        },
        textanalytics.MultiLanguageInput {
            Language: StringPointer("it"),
            ID:StringPointer("3"),
            Text:StringPointer("L'hotel veneziano era meraviglioso. È un bellissimo pezzo di architettura."),
        },
    }

    batchInput := textanalytics.MultiLanguageBatchInput{Documents:&inputDocuments}
}
```

In de dezelfde functie aanroepen `textAnalyticsclient.Sentiment()` en het resultaat. Vervolgens doorlopen en de resultaten en afdrukken van elk document-ID en gevoelsscore. Een score dichter bij 0 geeft aan dat een negatief gevoel, terwijl een score dichter bij 1 geeft aan een positieve stemming dat.

```golang
result, _ := textAnalyticsclient.Sentiment(ctx, BoolPointer(false), &batchInput)
batchResult := textanalytics.SentimentBatchResult{}
jsonString, _ := json.Marshal(result.Value)
json.Unmarshal(jsonString, &batchResult)

// Printing sentiment results
for _,document := range *batchResult.Documents {
    fmt.Printf("Document ID: %s " , *document.ID)
    fmt.Printf("Sentiment Score: %f\n",*document.Score)
}

// Printing document errors
fmt.Println("Document Errors")
for _,error := range *batchResult.Errors {
    fmt.Printf("Document ID: %s Message : %s\n" ,*error.ID, *error.Message)
}
```

In de main-functie van uw project, roept u `SentimentAnalysis()`.

### <a name="output"></a>Uitvoer

```console
Document ID: 1 , Sentiment Score: 0.87
Document ID: 2 , Sentiment Score: 0.11
Document ID: 3 , Sentiment Score: 0.44
Document ID: 4 , Sentiment Score: 1.00
```

## <a name="language-detection"></a>Taaldetectie

Maak een nieuwe functie met de naam `LanguageDetection()` waarmee de client eerder hebt gemaakt. Maak een lijst van `LanguageInput` objecten, met de documenten die u wilt analyseren. Elk object bevat een `id` en een `text` kenmerk. De `text` slaat de tekst die moet worden geanalyseerd, kenmerk en de `id` is een waarde. 

```golang
func LanguageDetection(textAnalyticsclient textanalytics.BaseClient) {

    ctx := context.Background()
    inputDocuments := []textanalytics.LanguageInput {
        textanalytics.LanguageInput {
            ID:StringPointer("0"),
            Text:StringPointer("This is a document written in English."),
        },
        textanalytics.LanguageInput {
            ID:StringPointer("1"),
            Text:StringPointer("Este es un document escrito en Español."),
        },
        textanalytics.LanguageInput {
            ID:StringPointer("2"),
            Text:StringPointer("这是一个用中文写的文件"),
        },
    }

    batchInput := textanalytics.LanguageBatchInput{Documents:&inputDocuments}
}
```

In de dezelfde functie aanroepen `textAnalyticsclient.DetectLanguage()` en het resultaat. Vervolgens doorlopen en de resultaten en afdrukken van elk document-ID en gedetecteerde taal.

```golang
result, _ := textAnalyticsclient.DetectLanguage(ctx, BoolPointer(false), &batchInput)

// Printing language detection results
for _,document := range *result.Documents {
    fmt.Printf("Document ID: %s " , *document.ID)
    fmt.Printf("Detected Languages with Score: ")
    for _,language := range *document.DetectedLanguages{
        fmt.Printf("%s %f,",*language.Name, *language.Score)
    }
    fmt.Println()
}

// Printing document errors
fmt.Println("Document Errors")
for _,error := range *result.Errors {
    fmt.Printf("Document ID: %s Message : %s\n" ,*error.ID, *error.Message)
}
```

In de main-functie van uw project, roept u `LanguageDetection()`.

### <a name="output"></a>Uitvoer

```console
Document ID: 0 Detected Languages with Score: English 1.000000,
Document ID: 1 Detected Languages with Score: Spanish 1.000000,
Document ID: 2 Detected Languages with Score: Chinese_Simplified 1.000000,
```

## <a name="entity-recognition"></a>Herkenning van entiteit

Maak een nieuwe functie met de naam `ExtractEntities()` waarmee de client eerder hebt gemaakt. Maak een lijst van `MultiLanguageInput` objecten, met de documenten die u wilt analyseren. Elk object bevat een `id`, `language`, en een `text` kenmerk. De `text` slaat de tekst die moet worden geanalyseerd, kenmerk `language` is de taal van het document en de `id` is een waarde. 

```golang
func ExtractKeyPhrases(textAnalyticsclient textanalytics.BaseClient) {

    ctx := context.Background()
    inputDocuments := []textanalytics.MultiLanguageInput {
        textanalytics.MultiLanguageInput {
            Language: StringPointer("en"),
            ID:StringPointer("0"),
            Text:StringPointer("Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, to develop and sell BASIC interpreters for the Altair 8800."),
        },
        textanalytics.MultiLanguageInput {
            Language: StringPointer("es"),
            ID:StringPointer("1"),
            Text:StringPointer("La sede principal de Microsoft se encuentra en la ciudad de Redmond, a 21 kilómetros de Seattle."),
        },
    }

    batchInput := textanalytics.MultiLanguageBatchInput{Documents:&inputDocuments}
}
```

In de dezelfde functie `call textAnalyticsclient.Entities()` en het resultaat. Vervolgens doorlopen en de resultaten en elk document-ID van het en uitgepakt entiteiten te beoordelen.

```golang
    result, _ := textAnalyticsclient.Entities(ctx, BoolPointer(false), &batchInput)

    // Printing extracted entities results
    for _,document := range *result.Documents {
        fmt.Printf("Document ID: %s\n" , *document.ID)
        fmt.Printf("\tExtracted Entities:\n")
        for _,entity := range *document.Entities{
            fmt.Printf("\t\tName: %s\tType: %s",*entity.Name, *entity.Type)
            if entity.SubType != nil{
                fmt.Printf("\tSub-Type: %s\n", *entity.SubType)
            }
            fmt.Println()
            for _,match := range *entity.Matches{
                fmt.Printf("\t\t\tOffset: %v\tLength: %v\tScore: %f\n", *match.Offset, *match.Length, *match.EntityTypeScore)
            }
        }
        fmt.Println()
    }

    // Printing document errors
    fmt.Println("Document Errors")
    for _,error := range *result.Errors {
        fmt.Printf("Document ID: %s Message : %s\n" ,*error.ID, *error.Message)
    }
```

In de main-functie van uw project, roept u `ExtractEntities()`.

### <a name="output"></a>Uitvoer

```console
Document ID: 0
    Extracted Entities:
        Name: Microsoft Type: Organization
            Offset: 0   Length: 9   Score: 1.000000
        Name: Bill Gates    Type: Person
            Offset: 25  Length: 10  Score: 0.999847
        Name: Paul Allen    Type: Person
            Offset: 40  Length: 10  Score: 0.998841
        Name: April 4   Type: Other
            Offset: 54  Length: 7   Score: 0.800000
        Name: April 4, 1975 Type: DateTime  Sub-Type: Date

            Offset: 54  Length: 13  Score: 0.800000
        Name: BASIC Type: Other
            Offset: 89  Length: 5   Score: 0.800000
        Name: Altair 8800   Type: Other
            Offset: 116 Length: 11  Score: 0.800000

Document ID: 1
    Extracted Entities:
        Name: Microsoft Type: Organization
            Offset: 21  Length: 9   Score: 0.999756
        Name: Redmond (Washington)  Type: Location
            Offset: 60  Length: 7   Score: 0.991128
        Name: 21 kilómetros Type: Quantity  Sub-Type: Dimension

            Offset: 71  Length: 13  Score: 0.800000
        Name: Seattle   Type: Location
            Offset: 88  Length: 7   Score: 0.999878
```

## <a name="key-phrase-extraction"></a>Sleuteltermextractie

Maak een nieuwe functie met de naam `ExtractKeyPhrases()` waarmee de client eerder hebt gemaakt. Maak een lijst van `MultiLanguageInput` objecten, met de documenten die u wilt analyseren. Elk object bevat een `id`, `language`, en een `text` kenmerk. De `text` slaat de tekst die moet worden geanalyseerd, kenmerk `language` is de taal van het document en de `id` is een waarde.

```golang
func ExtractKeyPhrases(textAnalyticsclient textanalytics.BaseClient) {

    ctx := context.Background()
    inputDocuments := []textanalytics.MultiLanguageInput {
        textanalytics.MultiLanguageInput {
            Language: StringPointer("ja"),
            ID:StringPointer("0"),
            Text:StringPointer("猫は幸せ"),
        },
        textanalytics.MultiLanguageInput {
            Language: StringPointer("de"),
            ID:StringPointer("1"),
            Text:StringPointer("Fahrt nach Stuttgart und dann zum Hotel zu Fu."),
        },
        textanalytics.MultiLanguageInput {
            Language: StringPointer("en"),
            ID:StringPointer("2"),
            Text:StringPointer("My cat might need to see a veterinarian."),
        },
        textanalytics.MultiLanguageInput {
            Language: StringPointer("es"),
            ID:StringPointer("3"),
            Text:StringPointer("A mi me encanta el fútbol!"),
        },
    }

    batchInput := textanalytics.MultiLanguageBatchInput{Documents:&inputDocuments}
}
```

In de dezelfde functie textAnalyticsclient.KeyPhrases() aanroepen en het resultaat. Vervolgens doorlopen en de resultaten en afdrukken van elk document-ID en uitgepakte sleuteltermen.

```golang
    result, _ := textAnalyticsclient.KeyPhrases(ctx, BoolPointer(false), &batchInput)

    // Printing extracted key phrases results
    for _,document := range *result.Documents {
        fmt.Printf("Document ID: %s\n" , *document.ID)
        fmt.Printf("\tExtracted Key Phrases:\n")
        for _,keyPhrase := range *document.KeyPhrases{
            fmt.Printf("\t\t%s\n",keyPhrase)
        }
        fmt.Println()
    }

    // Printing document errors
    fmt.Println("Document Errors")
    for _,error := range *result.Errors {
        fmt.Printf("Document ID: %s Message : %s\n" ,*error.ID, *error.Message)
    }
```

In de main-functie van uw project, roept u `ExtractKeyPhrases()`.

### <a name="output"></a>Uitvoer

```console
Document ID: 0
    Extracted Key Phrases:
        幸せ

Document ID: 1
    Extracted Key Phrases:
        Stuttgart
        Hotel
        Fahrt
        Fu

Document ID: 2
    Extracted Key Phrases:
        cat
        veterinarian

Document ID: 3
    Extracted Key Phrases:
        fútbol
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Text Analytics met Power BI](../tutorials/tutorial-power-bi-key-phrases.md)

## <a name="see-also"></a>Zie ook

 [Overzicht van Text Analytics](../overview.md) [Veelgestelde vragen](../text-analytics-resource-faq.md)
