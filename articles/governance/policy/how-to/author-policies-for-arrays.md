---
title: Beleid voor het schrijven van matrix eigenschappen voor bronnen
description: Meer informatie over het werken met matrix parameters en matrix-taal expressies, de alias [*] evalueren en elementen toevoegen met Azure Policy definitie regels.
ms.date: 10/22/2020
ms.topic: how-to
ms.openlocfilehash: 92339a6da4fd2061d66935cc8d04428c69822862
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2020
ms.locfileid: "93323231"
---
# <a name="author-policies-for-array-properties-on-azure-resources"></a>Beleid voor het schrijven van matrix eigenschappen op Azure-resources

Azure Resource Manager eigenschappen worden meestal gedefinieerd als teken reeksen en Booleaanse waarden. Als er sprake is van een een-op-veel-relatie, worden complexe eigenschappen in plaats daarvan gedefinieerd als matrices. In Azure Policy worden matrices op verschillende manieren gebruikt:

- Het type [definitie parameter](../concepts/definition-structure.md#parameters), om meerdere opties te bieden
- Onderdeel van een [beleids regel](../concepts/definition-structure.md#policy-rule) met de voor waarden **in** of **notIn**
- Onderdeel van een beleids regel waarmee de [ \[ \* \] alias](../concepts/definition-structure.md#understanding-the--alias) wordt geëvalueerd om te evalueren:
  - Scenario's zoals **geen** , **alle** of **alle**
  - Complexe scenario's met **aantal**
- In het [effect toevoegen](../concepts/effects.md#append) om te vervangen of toe te voegen aan een bestaande matrix

In dit artikel wordt elk gebruik door Azure Policy beschreven en worden verschillende voorbeeld definities geboden.

## <a name="parameter-arrays"></a>Parameter matrices

### <a name="define-a-parameter-array"></a>Een parameter matrix definiëren

Als u een para meter als een matrix definieert, kan het beleid flexibel zijn wanneer er meer dan één waarde nodig is.
Met deze beleids definitie kunt u één locatie voor de para meter **allowedLocations** en de standaard waarden voor _eastus2_ :

```json
"parameters": {
    "allowedLocations": {
        "type": "string",
        "metadata": {
            "description": "The list of allowed locations for resources.",
            "displayName": "Allowed locations",
            "strongType": "location"
        },
        "defaultValue": "eastus2"
    }
}
```

Als **type** is _teken reeks_ , kan slechts één waarde worden ingesteld wanneer het beleid wordt toegewezen. Als dit beleid is toegewezen, zijn resources in het bereik alleen toegestaan binnen één Azure-regio. De meeste beleids definities moeten een lijst met goedgekeurde opties toestaan, zoals het toestaan van _eastus2_ , _oostus_ en _westus2_.

Als u de beleids definitie wilt maken om meerdere opties toe te staan, gebruikt u het _matrix_ **type**. Hetzelfde beleid kan als volgt worden herschreven:

```json
"parameters": {
    "allowedLocations": {
        "type": "array",
        "metadata": {
            "description": "The list of allowed locations for resources.",
            "displayName": "Allowed locations",
            "strongType": "location"
        },
        "defaultValue": "eastus2",
        "allowedValues": [
            "eastus2",
            "eastus",
            "westus2"
        ]

    }
}
```

> [!NOTE]
> Zodra een beleids definitie is opgeslagen, kan de eigenschap **type** van een para meter niet worden gewijzigd.

Deze nieuwe parameter definitie heeft meer dan één waarde tijdens de beleids toewijzing. Met de matrix eigenschap **allowedValues** gedefinieerd, worden de waarden die beschikbaar zijn tijdens de toewijzing, verder beperkt tot de vooraf gedefinieerde lijst met opties. Het gebruik van **allowedValues** is optioneel.

### <a name="pass-values-to-a-parameter-array-during-assignment"></a>Waarden door geven aan een parameter matrix tijdens toewijzing

Wanneer u het beleid toewijst via de Azure Portal, wordt een para meter van het **type** _matrix_ weer gegeven als één tekstvak. De hint zegt ' use; om waarden te scheiden. (bijvoorbeeld Londen; New York) ". Gebruik de volgende teken reeks om de toegestane locatie waarden van _eastus2_ , _oostus_ en _westus2_ aan de para meter door te geven:

`eastus2;eastus;westus2`

De notatie voor de parameter waarde is anders wanneer u Azure CLI, Azure PowerShell of de REST API gebruikt. De waarden worden door gegeven via een JSON-teken reeks die ook de naam van de para meter bevat.

```json
{
    "allowedLocations": {
        "value": [
            "eastus2",
            "eastus",
            "westus2"
        ]
    }
}
```

Gebruik de volgende opdrachten om deze teken reeks te gebruiken voor elke SDK:

- Azure CLI: opdracht [AZ Policy Assignment Create](/cli/azure/policy/assignment#az-policy-assignment-create) with para meter **params**
- Azure PowerShell: cmdlet [New-AzPolicyAssignment](/powershell/module/az.resources/New-Azpolicyassignment) met para meter **PolicyParameter**
- REST API: in de _put_ -bewerking [maken](/rest/api/resources/policyassignments/create) als onderdeel van de aanvraag tekst als de waarde van de **Eigenschappen. para meters** -eigenschap

## <a name="array-conditions"></a>Matrix voorwaarden

De beleids regel [voorwaarden](../concepts/definition-structure.md#conditions) waarvoor het _matrix_ 
 **type** van de para meter mag worden gebruikt, zijn beperkt tot `in` en `notIn` . Neem de volgende beleids definitie met voor waarde `equals` als voor beeld:

```json
{
  "policyRule": {
    "if": {
      "not": {
        "field": "location",
        "equals": "[parameters('allowedLocations')]"
      }
    },
    "then": {
      "effect": "audit"
    }
  },
  "parameters": {
    "allowedLocations": {
      "type": "Array",
      "metadata": {
        "description": "The list of allowed locations for resources.",
        "displayName": "Allowed locations",
        "strongType": "location"
      }
    }
  }
}
```

Het maken van deze beleids definitie via de Azure Portal leidt tot een fout, zoals dit fout bericht:

- "Het beleid {GUID} kan niet worden geparametriseerde vanwege validatie fouten. Controleer of de beleids parameters juist zijn gedefinieerd. Het evaluatie resultaat van de interne uitzonde ring van de taal expressie [para meters (' allowedLocations ')] ' is van het type ' matrix ', het verwachte type is ' String '. '

Het verwachte **type** voor waarde `equals` is _teken reeks_. Omdat **allowedLocations** is gedefinieerd als **type** _matrix_ , evalueert de beleids engine de expressie van de taal en genereert de fout. Met de `in` `notIn` voor-voor waarde verwacht de beleids engine de **type** _matrix_ in de taal expressie. Als u dit fout bericht wilt oplossen, wijzigt `equals` u in `in` of `notIn` .

## <a name="referencing-array-resource-properties"></a>Verwijzen naar eigenschappen van matrix resources

In veel gevallen moet u werken met matrix eigenschappen in de geëvalueerde resource. In sommige scenario's moet worden verwezen naar een volledige matrix (bijvoorbeeld de lengte controleren). Anderen moeten een voor waarde Toep assen op elk afzonderlijk lid van de matrix (Controleer bijvoorbeeld of alle firewall regel toegang vanaf internet blok keren). Informatie over de verschillende manieren waarop Azure Policy kan verwijzen naar resource-eigenschappen, en hoe deze verwijzingen zich gedragen wanneer ze verwijzen naar matrix eigenschappen is de sleutel voor het schrijven van voor waarden die betrekking hebben op deze scenario's.

### <a name="referencing-resource-properties"></a>Verwijzen naar bron eigenschappen
Er kan naar resource-eigenschappen worden verwezen door Azure Policy met behulp van [aliassen](../concepts/definition-structure.md#aliases) er zijn twee manieren om te verwijzen naar de waarden van een bron eigenschap in azure Policy:

- Gebruik [veld](../concepts/definition-structure.md#fields) voorwaarde om te controleren of **alle** geselecteerde bron eigenschappen voldoen aan een voor waarde. Voorbeeld:

  ```json
  {
    "field" : "Microsoft.Test/resourceType/property",
    "equals": "value"
  }
  ```

- Gebruik `field()` de functie om toegang te krijgen tot de waarde van een eigenschap. Voorbeeld:

  ```json
  {
    "value": "[take(field('Microsoft.Test/resourceType/property'), 7)]",
    "equals": "prefix_"
  }
  ```

De veld voorwaarde heeft het impliciete gedrag ' alle '. Als de alias een verzameling waarden vertegenwoordigt, wordt gecontroleerd of alle afzonderlijke waarden voldoen aan de voor waarde. De `field()` functie retourneert de waarden die worden weer gegeven door de alias als-is, die vervolgens door andere sjabloon functies kan worden gemanipuleerd.

### <a name="referencing-array-fields"></a>Verwijzen naar matrix velden

Eigenschappen van matrix bronnen worden meestal vertegenwoordigd door twee verschillende typen aliassen. Een ' normale ' alias-en- [matrix aliassen](../concepts/definition-structure.md#understanding-the--alias) die `[*]` hieraan zijn gekoppeld:

- `Microsoft.Test/resourceType/stringArray`
- `Microsoft.Test/resourceType/stringArray[*]`

#### <a name="referencing-the-array"></a>Verwijzen naar de matrix

De eerste alias vertegenwoordigt één waarde, de waarde van de `stringArray` eigenschap van de inhoud van de aanvraag. Omdat de waarde van die eigenschap een matrix is, is het niet erg nuttig in beleids voorwaarden. Bijvoorbeeld:

```json
{
  "field": "Microsoft.Test/resourceType/stringArray",
  "equals": "..."
}
```

Met deze voor waarde wordt de gehele `stringArray` matrix vergeleken met één teken reeks waarde. De meeste voor waarden, waaronder `equals` , alleen teken reeks waarden accepteren, daarom is het niet veel meer om een matrix te vergelijken met een teken reeks. Het hoofd scenario waarin wordt verwezen naar de eigenschap matrix is handig wanneer wordt gecontroleerd of het bestaat:

```json
{
  "field": "Microsoft.Test/resourceType/stringArray",
  "exists": "true"
}
```

Met de `field()` functie is de geretourneerde waarde de matrix van de aanvraag inhoud, die vervolgens kan worden gebruikt met een van de [ondersteunde sjabloon functies](../concepts/definition-structure.md#policy-functions) die matrix argumenten accepteren. Met de volgende voor waarde wordt bijvoorbeeld gecontroleerd of de lengte `stringArray` groter is dan 0:

```json
{
  "value": "[length(field('Microsoft.Test/resourceType/stringArray'))]",
  "greater": 0
}
```

#### <a name="referencing-the-array-members-collection"></a>Verwijzen naar de verzameling matrix leden

Aliassen die gebruikmaken `[*]` van de syntaxis, vertegenwoordigen een **verzameling eigenschaps waarden die zijn geselecteerd uit een matrix eigenschap** , die anders is dan de matrix eigenschap zelf selecteren. In het geval van wordt `Microsoft.Test/resourceType/stringArray[*]` een verzameling met alle leden van weer gegeven `stringArray` . Zoals eerder vermeld, `field` controleert een voor waarde of alle geselecteerde bron eigenschappen voldoen aan de voor waarde, daarom is de volgende voor waarde alleen waar als **alle** leden van `stringArray` gelijk zijn aan ' "waarde" '.

```json
{
  "field": "Microsoft.Test/resourceType/stringArray[*]",
  "equals": "value"
}
```

Als de matrix objecten bevat, `[*]` kan een alias worden gebruikt om de waarde van een specifieke eigenschap van elk matrixlid te selecteren. Voorbeeld:

```json
{
  "field": "Microsoft.Test/resourceType/objectArray[*].property",
  "equals": "value"
}
```

Deze voor waarde is waar als de waarden van alle `property` Eigenschappen in `objectArray` zijn gelijk aan `"value"` .

Wanneer u de `field()` functie gebruikt om te verwijzen naar een matrix alias, is de geretourneerde waarde een matrix van alle geselecteerde waarden. Dit gedrag houdt in dat het algemene gebruik van de `field()` functie, de mogelijkheid om sjabloon functies toe te passen op eigenschaps waarden van resources, zeer beperkt is. De enige sjabloon functies die in dit geval kunnen worden gebruikt, zijn de opties die matrix argumenten accepteren. Het is bijvoorbeeld mogelijk om de lengte van de matrix met te verkrijgen `[length(field('Microsoft.Test/resourceType/objectArray[*].property'))]` . Complexere scenario's, zoals het Toep assen van sjabloon functie op elke matrix leden en het vergelijken ervan met een gewenste waarde, zijn alleen mogelijk wanneer u de `count` expressie gebruikt. Zie [aantal-expressies](#count-expressions)voor meer informatie.

Als u wilt samenvatten, raadpleegt u de volgende voorbeeld bron inhoud en de geselecteerde waarden die door verschillende aliassen worden geretourneerd:

```json
{
  "tags": {
    "env": "prod"
  },
  "properties":
  {
    "stringArray": [ "a", "b", "c" ],
    "objectArray": [
      {
        "property": "value1",
        "nestedArray": [ 1, 2 ]
      },
      {
        "property": "value2",
        "nestedArray": [ 3, 4 ]
      }
    ]
  }
}
```

Wanneer u de veld voorwaarde gebruikt op de voorbeeld bron inhoud, zijn de resultaten als volgt:

| Alias | Geselecteerde waarden |
|:--- |:---|
| `Microsoft.Test/resourceType/missingArray` | `null` |
| `Microsoft.Test/resourceType/missingArray[*]` | Een lege verzameling waarden. |
| `Microsoft.Test/resourceType/missingArray[*].property` | Een lege verzameling waarden. |
| `Microsoft.Test/resourceType/stringArray` | `["a", "b", "c"]` |
| `Microsoft.Test/resourceType/stringArray[*]` | `"a"`, `"b"`, `"c"` |
| `Microsoft.Test/resourceType/objectArray[*]` |  `{ "property": "value1", "nestedArray": [ 1, 2 ] }`,<br/>`{ "property": "value2", "nestedArray": [ 3, 4 ] }`|
| `Microsoft.Test/resourceType/objectArray[*].property` | `"value1"`, `"value2"` |
| `Microsoft.Test/resourceType/objectArray[*].nestedArray` | `[ 1, 2 ]`, `[ 3, 4 ]` |
| `Microsoft.Test/resourceType/objectArray[*].nestedArray[*]` | `1`, `2`, `3`, `4` |

Wanneer u de `field()` functie gebruikt op de voorbeeld bron inhoud, zijn de resultaten als volgt:

| Expression | Geretourneerde waarde |
|:--- |:---|
| `[field('Microsoft.Test/resourceType/missingArray')]` | `""` |
| `[field('Microsoft.Test/resourceType/missingArray[*]')]` | `[]` |
| `[field('Microsoft.Test/resourceType/missingArray[*].property')]` | `[]` |
| `[field('Microsoft.Test/resourceType/stringArray')]` | `["a", "b", "c"]` |
| `[field('Microsoft.Test/resourceType/stringArray[*]')]` | `["a", "b", "c"]` |
| `[field('Microsoft.Test/resourceType/objectArray[*]')]` |  `[{ "property": "value1", "nestedArray": [ 1, 2 ] }, { "property": "value2", "nestedArray": [ 3, 4 ] }]`|
| `[field('Microsoft.Test/resourceType/objectArray[*].property')]` | `["value1", "value2"]` |
| `[field('Microsoft.Test/resourceType/objectArray[*].nestedArray')]` | `[[ 1, 2 ], [ 3, 4 ]]` |
| `[field('Microsoft.Test/resourceType/objectArray[*].nestedArray[*]')]` | `[1, 2, 3, 4]` |

## <a name="count-expressions"></a>Expressies tellen

[Aantal](../concepts/definition-structure.md#count) expressies tellen hoeveel matrix leden aan een voor waarde voldoen en vergelijken het aantal met een doel waarde. `Count` is intuïtief en veelzijdig voor het evalueren van matrices vergeleken met `field` voor waarden. De syntaxis is:

```json
{
  "count": {
    "field": <[*] alias>,
    "where": <optional policy condition expression>
  },
  "equals|greater|less|any other operator": <target value>
}
```

Als u zonder een WHERE-voor waarde gebruikt, `count` retourneert eenvoudigweg de lengte van een matrix. Met de voorbeeld resource-inhoud uit de vorige sectie wordt de volgende `count` expressie geëvalueerd op `true` sinds `stringArray` drie leden:

```json
{
  "count": {
    "field": "Microsoft.Test/resourceType/stringArray[*]"
  },
  "equals": 3
}
```

Dit gedrag werkt ook met geneste matrices. Bijvoorbeeld, de volgende `count` expressie wordt geëvalueerd naar, `true` omdat er vier matrix leden in de matrices zijn `nestedArray` :

```json
{
  "count": {
    "field": "Microsoft.Test/resourceType/objectArray[*].nestedArray[*]"
  },
  "greaterOrEquals": 4
}
```

De kracht van `count` is in de `where` voor waarde. Wanneer deze is opgegeven, Azure Policy de matrix leden inventariseren en elke voor waarde evalueren, waarbij wordt geteld hoeveel matrix leden er zijn geëvalueerd `true` . In elk herhaling van de evaluatie van de `where` voor waarde, Azure Policy een lid van één matrix selecteren * **i** _ en de resource-inhoud evalueren `where` aan de voor waarde _*, alsof * *_i_*_ het enige lid van de array_ * is. Als er slechts één matrixlid in elke iteratie beschikbaar is, kunt u complexe voor waarden Toep assen op elk afzonderlijk lid van de matrix.

Voorbeeld:
```json
{
  "count": {
    "field": "Microsoft.Test/resourceType/stringArray[*]",
    "where": {
      "field": "Microsoft.Test/resourceType/stringArray[*]",
      "equals": "a"
    }
  },
  "equals": 1
}
```
Om de expressie te evalueren `count` , evalueert Azure Policy de `where` voor waarde drie keer en eenmaal voor elk lid van `stringArray` , geteld hoe vaak deze is geëvalueerd `true` . Wanneer de `where` voor waarde de `Microsoft.Test/resourceType/stringArray[*]` matrix leden bevat, worden er in plaats van alle leden te selecteren `stringArray` , een enkel lid van een matrix elke keer geselecteerd:

| Iteratie | Geselecteerde `Microsoft.Test/resourceType/stringArray[*]` waarden | `where` Resultaat van evaluatie |
|:---|:---|:---|
| 1 | `"a"` | `true` |
| 2 | `"b"` | `false` |
| 3 | `"c"` | `false` |

En daarom `count` wordt geretourneerd `1` .

Hier volgt een complexere expressie:
```json
{
  "count": {
    "field": "Microsoft.Test/resourceType/objectArray[*]",
    "where": {
      "allOf": [
        {
          "field": "Microsoft.Test/resourceType/objectArray[*].property",
          "equals": "value2"
        },
        {
          "field": "Microsoft.Test/resourceType/objectArray[*].nestedArray[*]",
          "greater": 2
        }
      ]
    }
  },
  "equals": 1
}
```

| Iteratie | Geselecteerde waarden | `where` Resultaat van evaluatie |
|:---|:---|:---|
| 1 | `Microsoft.Test/resourceType/objectArray[*].property` => `"value1"` </br> `Microsoft.Test/resourceType/objectArray[*].nestedArray[*]` => `1`, `2` | `false` |
| 2 | `Microsoft.Test/resourceType/objectArray[*].property` => `"value2"` </br> `Microsoft.Test/resourceType/objectArray[*].nestedArray[*]` => `3`, `4`| `true` |

En dus de `count` retour neren `1` .

Het feit dat de `where` expressie wordt geëvalueerd op basis van de **volledige** inhoud van de aanvraag (waarbij alleen wijzigingen worden aangebracht aan het matrixlid dat momenteel wordt geïnventariseerd) betekent dat de `where` voor waarde ook naar velden buiten de matrix kan verwijzen:
```json
{
  "count": {
    "field": "Microsoft.Test/resourceType/objectArray[*]",
    "where": {
      "field": "tags.env",
      "equals": "prod"
    }
  }
}
```

| Iteratie | Geselecteerde waarden | `where` Resultaat van evaluatie |
|:---|:---|:---|
| 1 | `tags.env` => `"prod"` | `true` |
| 2 | `tags.env` => `"prod"` | `true` |

Expressies voor genest aantal zijn ook toegestaan:
```json
{
  "count": {
    "field": "Microsoft.Test/resourceType/objectArray[*]",
    "where": {
      "allOf": [
        {
          "field": "Microsoft.Test/resourceType/objectArray[*].property",
          "equals": "value2"
        },
        {
          "count": {
            "field": "Microsoft.Test/resourceType/objectArray[*].nestedArray[*]",
            "where": {
              "field": "Microsoft.Test/resourceType/objectArray[*].nestedArray[*]",
              "equals": 3
            },
            "greater": 0
          }
        }
      ]
    }
  }
}
```
 
| Herhaling van de buitenste lus | Geselecteerde waarden | Iteratie van interne lus | Geselecteerde waarden |
|:---|:---|:---|:---|
| 1 | `Microsoft.Test/resourceType/objectArray[*].property` => `"value1`</br> `Microsoft.Test/resourceType/objectArray[*].nestedArray[*]` => `1`, `2` | 1 | `Microsoft.Test/resourceType/objectArray[*].nestedArray[*]` => `1` |
| 1 | `Microsoft.Test/resourceType/objectArray[*].property` => `"value1`</br> `Microsoft.Test/resourceType/objectArray[*].nestedArray[*]` => `1`, `2` | 2 | `Microsoft.Test/resourceType/objectArray[*].nestedArray[*]` => `2` |
| 2 | `Microsoft.Test/resourceType/objectArray[*].property` => `"value2`</br> `Microsoft.Test/resourceType/objectArray[*].nestedArray[*]` => `3`, `4` | 1 | `Microsoft.Test/resourceType/objectArray[*].nestedArray[*]` => `3` |
| 2 | `Microsoft.Test/resourceType/objectArray[*].property` => `"value2`</br> `Microsoft.Test/resourceType/objectArray[*].nestedArray[*]` => `3`, `4` | 2 | `Microsoft.Test/resourceType/objectArray[*].nestedArray[*]` => `4` |

### <a name="the-field-function-inside-where-conditions"></a>De `field()` functie in `where` voor waarden

De manier waarop `field()` functies zich gedragen wanneer binnen een `where` voor waarde is gebaseerd op de volgende concepten:
1. Matrix aliassen worden omgezet in een verzameling waarden die zijn geselecteerd in alle matrix leden.
1. `field()` functies die verwijzen naar matrix aliassen retour neren een matrix met de geselecteerde waarden.
1. Als u verwijst naar de alias van de getelde matrix binnen de `where` voor waarde, wordt een verzameling geretourneerd met een enkele waarde die is geselecteerd in het matrixlid dat in de huidige iteratie wordt geëvalueerd.

Dit gedrag betekent dat wanneer wordt verwezen naar het lid getelde matrix met een `field()` functie binnen de `where` voor waarde, **een matrix met één lid wordt geretourneerd**. Hoewel dit niet intuïtief is, is het consistent met het idee dat matrix aliassen altijd een verzameling geselecteerde eigenschappen retour neren. Hier volgt een voorbeeld:

```json
{
  "count": {
    "field": "Microsoft.Test/resourceType/stringArray[*]",
    "where": {
      "field": "Microsoft.Test/resourceType/stringArray[*]",
      "equals": "[field('Microsoft.Test/resourceType/stringArray[*]')]"
    }
  },
  "equals": 0
}
```

| Iteratie | Expressie waarden | `where` Resultaat van evaluatie |
|:---|:---|:---|
| 1 | `Microsoft.Test/resourceType/stringArray[*]` => `"a"` </br>  `[field('Microsoft.Test/resourceType/stringArray[*]')]` => `[ "a" ]` | `false` |
| 2 | `Microsoft.Test/resourceType/stringArray[*]` => `"b"` </br>  `[field('Microsoft.Test/resourceType/stringArray[*]')]` => `[ "b" ]` | `false` |
| 3 | `Microsoft.Test/resourceType/stringArray[*]` => `"c"` </br>  `[field('Microsoft.Test/resourceType/stringArray[*]')]` => `[ "c" ]` | `false` |

Daarom moet u, wanneer u de waarde van de alias van de getelde matrix met een `field()` functie, de manier waarop u dit wilt doen, de functie laten teruglopen met een `first()` sjabloon:

```json
{
  "count": {
    "field": "Microsoft.Test/resourceType/stringArray[*]",
    "where": {
      "field": "Microsoft.Test/resourceType/stringArray[*]",
      "equals": "[first(field('Microsoft.Test/resourceType/stringArray[*]'))]"
    }
  }
}
```

| Iteratie | Expressie waarden | `where` Resultaat van evaluatie |
|:---|:---|:---|
| 1 | `Microsoft.Test/resourceType/stringArray[*]` => `"a"` </br>  `[first(field('Microsoft.Test/resourceType/stringArray[*]'))]` => `"a"` | `true` |
| 2 | `Microsoft.Test/resourceType/stringArray[*]` => `"b"` </br>  `[first(field('Microsoft.Test/resourceType/stringArray[*]'))]` => `"b"` | `true` |
| 3 | `Microsoft.Test/resourceType/stringArray[*]` => `"c"` </br>  `[first(field('Microsoft.Test/resourceType/stringArray[*]'))]` => `"c"` | `true` |

Zie [aantal voor beelden](../concepts/definition-structure.md#count-examples)voor nuttige voor beelden.

## <a name="modifying-arrays"></a>Matrices wijzigen

De [toevoeg](../concepts/effects.md#append) -en [wijzigings](../concepts/effects.md#modify) eigenschappen voor een resource tijdens het maken of bijwerken. Wanneer u werkt met matrix eigenschappen, is het gedrag van deze effecten afhankelijk van of de bewerking probeert de alias te wijzigen  **\[\*\]** of niet:

> [!NOTE]
> Het gebruiken van het `modify` effect met aliassen is momenteel beschikbaar als **Preview-versie**.

|Alias |Effect | Resultaat |
|-|-|-|
| `Microsoft.Storage/storageAccounts/networkAcls.ipRules` | `append` | Azure Policy voegt de volledige matrix toe die is opgegeven in de effect Details als deze ontbreken. |
| `Microsoft.Storage/storageAccounts/networkAcls.ipRules` | `modify` met `add` bewerking | Azure Policy voegt de volledige matrix toe die is opgegeven in de effect Details als deze ontbreken. |
| `Microsoft.Storage/storageAccounts/networkAcls.ipRules` | `modify` met `addOrReplace` bewerking | Azure Policy voegt de volledige matrix die is opgegeven in de effect Details, toe als de bestaande matrix ontbreekt of wordt vervangen. |
| `Microsoft.Storage/storageAccounts/networkAcls.ipRules[*]` | `append` | Azure Policy voegt het matrixlid toe dat is opgegeven in de effect Details. |
| `Microsoft.Storage/storageAccounts/networkAcls.ipRules[*]` | `modify` met `add` bewerking | Azure Policy voegt het matrixlid toe dat is opgegeven in de effect Details. |
| `Microsoft.Storage/storageAccounts/networkAcls.ipRules[*]` | `modify` met `addOrReplace` bewerking | Azure Policy verwijdert alle bestaande matrix leden en voegt het matrixlid toe dat is opgegeven in de effect Details. |
| `Microsoft.Storage/storageAccounts/networkAcls.ipRules[*].action` | `append` | Azure Policy voegt een waarde toe aan de `action` eigenschap van elk matrixlid. |
| `Microsoft.Storage/storageAccounts/networkAcls.ipRules[*].action` | `modify` met `add` bewerking | Azure Policy voegt een waarde toe aan de `action` eigenschap van elk matrixlid. |
| `Microsoft.Storage/storageAccounts/networkAcls.ipRules[*].action` | `modify` met `addOrReplace` bewerking | Azure Policy voegt de bestaande `action` eigenschap van elk matrixlid of vervangt deze. |

Zie voor meer informatie de [Append-voor beelden](../concepts/effects.md#append-examples).

## <a name="next-steps"></a>Volgende stappen

- Bekijk voor beelden op [Azure Policy voor beelden](../samples/index.md).
- Lees over de [structuur van Azure Policy-definities](../concepts/definition-structure.md).
- Lees [Informatie over de effecten van het beleid](../concepts/effects.md).
- Meer informatie over het [programmatisch maken van beleids regels](programmatically-create.md).
- Ontdek hoe u [niet-compatibele resources kunt herstellen](remediate-resources.md).
- Bekijk wat een beheer groep is met [het organiseren van uw resources met Azure-beheer groepen](../../management-groups/overview.md).
