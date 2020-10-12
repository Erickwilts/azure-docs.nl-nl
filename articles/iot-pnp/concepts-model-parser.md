---
title: Meer informatie over de parser Digital Apparaatdubbels model | Microsoft Docs
description: Meer informatie over het gebruik van de DTDL-parser voor het valideren van modellen
author: rido-min
ms.author: rmpablos
ms.date: 04/29/2020
ms.topic: conceptual
ms.custom: mvc
ms.service: iot-pnp
services: iot-pnp
manager: peterpr
ms.openlocfilehash: 20c4452a32c791f33e08c883d8cec89a345ab188
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "87352285"
---
# <a name="understand-the-digital-twins-model-parser"></a>Meer informatie over de Digital Twins Model Parser

De Digital Apparaatdubbels Definition Language (DTDL) wordt beschreven in de [DTDL-specificatie](https://github.com/Azure/opendigitaltwins-dtdl). Gebruikers kunnen het NuGet-pakket van het _Digital apparaatdubbels-model_ gebruiken om een model dat in meerdere bestanden is gedefinieerd, te valideren en op te vragen.

## <a name="install-the-dtdl-model-parser"></a>De DTDL-model-parser installeren

De parser is beschikbaar in NuGet.org met de ID: [micro soft. Azure. DigitalTwins. parser](https://www.nuget.org/packages/Microsoft.Azure.DigitalTwins.Parser). Als u de parser wilt installeren, gebruikt u een compatibele NuGet-pakket manager, zoals de-versie in Visual Studio of in de `dotnet` cli.

```bash
dotnet add package Microsoft.Azure.DigitalTwins.Parser
```

## <a name="use-the-parser-to-validate-a-model"></a>De parser gebruiken om een model te valideren

Het model dat u wilt valideren, bestaat mogelijk uit een of meer interfaces die worden beschreven in JSON-bestanden. U kunt de parser gebruiken om alle bestanden in een bepaalde map te laden en de parser te gebruiken om alle bestanden als geheel te valideren, inclusief eventuele verwijzingen tussen de bestanden:

1. Een maken `IEnumerable<string>` met een lijst met alle model inhoud:

    ```csharp
    using System.IO;

    string folder = @"c:\myModels\";
    string filespec = "*.json";

    List<string> modelJson = new List<string>();
    foreach (string filename in Directory.GetFiles(folder, filespec))
    {
        using StreamReader modelReader = new StreamReader(filename);
        modelJson.Add(modelReader.ReadToEnd());
    }
    ```

1. Instantiëren `ModelParser` en bellen `ParseAsync` :

    ```csharp
    using Microsoft.Azure.DigitalTwins.Parser;

    ModelParser modelParser = new ModelParser();
    IReadOnlyDictionary<Dtmi, DTEntityInfo> parseResult = await modelParser.ParseAsync(modelJson);
    ```

1. Controleren op validatie fouten. Als de parser fouten vindt, wordt er een `AggregateException` met een lijst met gedetailleerde fout berichten weer geven:

    ```csharp
    try
    {
        IReadOnlyDictionary<Dtmi, DTEntityInfo> parseResult = await modelParser.ParseAsync(modelJson);
    }
    catch (AggregateException ae)
    {
        foreach (var e in ae.InnerExceptions)
        {
            Console.WriteLine(e.Message);
        }
    }
    ```

1. Inspecteer de `Model` . Als de validatie is geslaagd, kunt u de model parser-API gebruiken om het model te controleren. Het volgende code fragment laat zien hoe u door alle geparseerde modellen herhaalt en de bestaande eigenschappen weergeeft:

    ```csharp
    foreach (var m in parseResult)
    {
        Console.WriteLine(m.Key);
        foreach (var item in m.Value.AsEnumerable<DTEntityInfo>())
        {
            var p = item as DTInterfaceInfo;
            if (p!=null)
            {
                Console.WriteLine($"\t{p.Id}");
                Console.WriteLine($"\t{p.Description.FirstOrDefault()}");
            }
            Console.WriteLine("--------------");
        }
    }
    ```

## <a name="next-steps"></a>Volgende stappen

Met de API voor model parsers die in dit artikel wordt gecontroleerd, kunnen veel scenario's worden geautomatiseerd of gevalideerd die afhankelijk zijn van DTDL modellen. U kunt bijvoorbeeld een gebruikers interface dynamisch maken op basis van de gegevens in het model.
