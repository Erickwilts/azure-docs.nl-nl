---
title: Functies van Azure Resource Manager-sjablonen - tekenreeks | Microsoft Docs
description: Beschrijft de functies in een Azure Resource Manager-sjabloon gebruiken om te werken met tekenreeksen.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.assetid: ''
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/08/2019
ms.author: tomfitz
ms.openlocfilehash: 82b9403a3d5a5b6938f5b95bbfce888d1e70e451
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66431223"
---
# <a name="string-functions-for-azure-resource-manager-templates"></a>Tekenreeksfuncties voor Azure Resource Manager-sjablonen

Resource Manager biedt de volgende functies voor het werken met tekenreeksen:

* [base64](#base64)
* [base64ToJson](#base64tojson)
* [base64ToString](#base64tostring)
* [concat](#concat)
* [contains](#contains)
* [dataUri](#datauri)
* [dataUriToString](#datauritostring)
* [empty](#empty)
* [endsWith](#endswith)
* [first](#first)
* [Indeling](#format)
* [GUID](#guid)
* [indexOf](#indexof)
* [laatste](#last)
* [lastIndexOf](#lastindexof)
* [Lengte](#length)
* [newGuid](#newguid)
* [padLeft](#padleft)
* [vervangen](#replace)
* [skip](#skip)
* [split](#split)
* [startsWith](#startswith)
* [Tekenreeks](#string)
* [de subtekenreeks](#substring)
* [take](#take)
* [toLower](#tolower)
* [toUpper](#toupper)
* [trim](#trim)
* [uniqueString](#uniquestring)
* [uri](#uri)
* [uriComponent](#uricomponent)
* [uriComponentToString](#uricomponenttostring)
* [utcNow](#utcnow)

## <a name="base64"></a>base64

`base64(inputString)`

Retourneert de Base 64-weergave van de ingevoerde tekenreeks.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| inputString |Ja |string |De waarde dat wordt geretourneerd als een Base 64-indeling. |

### <a name="return-value"></a>Retourwaarde

Een tekenreeks met Base 64-indeling.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/base64.json) laat zien hoe u de met base64-functie te gebruiken.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringData": {
            "type": "string",
            "defaultValue": "one, two, three"
        },
        "jsonFormattedData": {
            "type": "string",
            "defaultValue": "{'one': 'a', 'two': 'b'}"
        }
    },
    "variables": {
        "base64String": "[base64(parameters('stringData'))]",
        "base64Object": "[base64(parameters('jsonFormattedData'))]"
    },
    "resources": [
    ],
    "outputs": {
        "base64Output": {
            "type": "string",
            "value": "[variables('base64String')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[base64ToString(variables('base64String'))]"
        },
        "toJsonOutput": {
            "type": "object",
            "value": "[base64ToJson(variables('base64Object'))]"
        }
    }
}
```

De uitvoer uit het vorige voorbeeld met de standaardwaarden is:

| Name | Type | Value |
| ---- | ---- | ----- |
| base64Output | String | b25lLCB0d28sIHRocmVl |
| toStringOutput | String | Een twee drie |
| toJsonOutput | Object | {"een": "a", "2": "b"} |

## <a name="base64tojson"></a>base64ToJson

`base64tojson`

Converteert een Base 64-indeling naar een JSON-object.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| base64Value |Ja |string |De Base 64-indeling converteren naar een JSON-object. |

### <a name="return-value"></a>Retourwaarde

Een JSON-object.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/base64.json) maakt gebruik van de functie base64ToJson om een met base64-waarde te converteren:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringData": {
            "type": "string",
            "defaultValue": "one, two, three"
        },
        "jsonFormattedData": {
            "type": "string",
            "defaultValue": "{'one': 'a', 'two': 'b'}"
        }
    },
    "variables": {
        "base64String": "[base64(parameters('stringData'))]",
        "base64Object": "[base64(parameters('jsonFormattedData'))]"
    },
    "resources": [
    ],
    "outputs": {
        "base64Output": {
            "type": "string",
            "value": "[variables('base64String')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[base64ToString(variables('base64String'))]"
        },
        "toJsonOutput": {
            "type": "object",
            "value": "[base64ToJson(variables('base64Object'))]"
        }
    }
}
```

De uitvoer uit het vorige voorbeeld met de standaardwaarden is:

| Name | Type | Value |
| ---- | ---- | ----- |
| base64Output | String | b25lLCB0d28sIHRocmVl |
| toStringOutput | String | Een twee drie |
| toJsonOutput | Object | {"een": "a", "2": "b"} |

## <a name="base64tostring"></a>base64ToString

`base64ToString(base64Value)`

Converteert een Base 64-indeling naar een tekenreeks.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| base64Value |Ja |string |De Base 64-indeling converteren naar een tekenreeks. |

### <a name="return-value"></a>Retourwaarde

Een tekenreeks van de geconverteerde base64-waarde.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/base64.json) maakt gebruik van de functie base64ToString om een met base64-waarde te converteren:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringData": {
            "type": "string",
            "defaultValue": "one, two, three"
        },
        "jsonFormattedData": {
            "type": "string",
            "defaultValue": "{'one': 'a', 'two': 'b'}"
        }
    },
    "variables": {
        "base64String": "[base64(parameters('stringData'))]",
        "base64Object": "[base64(parameters('jsonFormattedData'))]"
    },
    "resources": [
    ],
    "outputs": {
        "base64Output": {
            "type": "string",
            "value": "[variables('base64String')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[base64ToString(variables('base64String'))]"
        },
        "toJsonOutput": {
            "type": "object",
            "value": "[base64ToJson(variables('base64Object'))]"
        }
    }
}
```

De uitvoer uit het vorige voorbeeld met de standaardwaarden is:

| Name | Type | Value |
| ---- | ---- | ----- |
| base64Output | String | b25lLCB0d28sIHRocmVl |
| toStringOutput | String | Een twee drie |
| toJsonOutput | Object | {"een": "a", "2": "b"} |

## <a name="concat"></a>concat

`concat (arg1, arg2, arg3, ...)`

Combineert meerdere tekenreekswaarden en retourneert de samengevoegde tekenreeks of combineert meerdere matrices en retourneert de samengevoegde matrix.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |tekenreeks of matrix |De eerste waarde voor samenvoeging. |
| aanvullende argumenten |Nee |string |Aanvullende waarden in opeenvolgende volgorde voor samenvoeging. |

### <a name="return-value"></a>Retourwaarde
Een tekenreeks of matrix met samengevoegde waarden.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/concat-string.json) laat zien hoe u het combineren van twee tekenreekswaarden en een samengevoegde tekenreeks te retourneren.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "prefix": {
            "type": "string",
            "defaultValue": "prefix"
        }
    },
    "resources": [],
    "outputs": {
        "concatOutput": {
            "value": "[concat(parameters('prefix'), '-', uniqueString(resourceGroup().id))]",
            "type" : "string"
        }
    }
}
```

De uitvoer uit het vorige voorbeeld met de standaardwaarden is:

| Name | Type | Value |
| ---- | ---- | ----- |
| concatOutput | String | prefix-5yj4yjf5mbg72 |

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/concat-array.json) laat zien hoe u twee matrices combineren.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": { 
        "firstArray": { 
            "type": "array", 
            "defaultValue": [ 
                "1-1", 
                "1-2", 
                "1-3" 
            ] 
        },
        "secondArray": {
            "type": "array", 
            "defaultValue": [ 
                "2-1", 
                "2-2",
                "2-3" 
            ] 
        }
    },
    "resources": [
    ],
    "outputs": {
        "return": {
            "type": "array",
            "value": "[concat(parameters('firstArray'), parameters('secondArray'))]"
        }
    }
}
```

De uitvoer uit het vorige voorbeeld met de standaardwaarden is:

| Name | Type | Value |
| ---- | ---- | ----- |
| terug | Matrix | ["1-1", "1-2", "1-3", "2-1", "2-2", "2-3"] |

## <a name="contains"></a>bevat

`contains (container, itemToFind)`

Controleert of een matrix een waarde bevat, een object een sleutel bevat of een tekenreeks een subtekenreeks. De vergelijking van gegevensreeksen is hoofdlettergevoelig. De vergelijking is echter niet-hoofdlettergevoelige bij het testen van als een object een sleutel bevat.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| container |Ja |matrix of object tekenreeks |De waarde met de waarde om te zoeken. |
| itemToFind |Ja |tekenreeks of int |De waarde om te zoeken. |

### <a name="return-value"></a>Retourwaarde

**De waarde True** als het item gevonden, anders is, **False**.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/contains.json) toont hoe u met verschillende typen bevat:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringToTest": {
            "type": "string",
            "defaultValue": "OneTwoThree"
        },
        "objectToTest": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "b", "three": "c"}
        },
        "arrayToTest": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "stringTrue": {
            "type": "bool",
            "value": "[contains(parameters('stringToTest'), 'e')]"
        },
        "stringFalse": {
            "type": "bool",
            "value": "[contains(parameters('stringToTest'), 'z')]"
        },
        "objectTrue": {
            "type": "bool",
            "value": "[contains(parameters('objectToTest'), 'one')]"
        },
        "objectFalse": {
            "type": "bool",
            "value": "[contains(parameters('objectToTest'), 'a')]"
        },
        "arrayTrue": {
            "type": "bool",
            "value": "[contains(parameters('arrayToTest'), 'three')]"
        },
        "arrayFalse": {
            "type": "bool",
            "value": "[contains(parameters('arrayToTest'), 'four')]"
        }
    }
}
```

De uitvoer uit het vorige voorbeeld met de standaardwaarden is:

| Name | Type | Value |
| ---- | ---- | ----- |
| stringTrue | Bool | True |
| stringFalse | Bool | False |
| objectTrue | Bool | True |
| objectFalse | Bool | False |
| arrayTrue | Bool | True |
| arrayFalse | Bool | False |

## <a name="datauri"></a>dataUri

`dataUri(stringToConvert)`

Converteert een waarde naar een gegevens-URI.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| stringToConvert |Ja |string |De waarde moet worden geconverteerd naar een gegevens-URI. |

### <a name="return-value"></a>Retourwaarde

Een tekenreeks is opgemaakt als een gegevens-URI.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/datauri.json) converteert een waarde naar een gegevens-URI en een gegevens-URI converteert naar een tekenreeks:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringToTest": {
            "type": "string",
            "defaultValue": "Hello"
        },
        "dataFormattedString": {
            "type": "string",
            "defaultValue": "data:;base64,SGVsbG8sIFdvcmxkIQ=="
        }
    },
    "resources": [],
    "outputs": {
        "dataUriOutput": {
            "value": "[dataUri(parameters('stringToTest'))]",
            "type" : "string"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[dataUriToString(parameters('dataFormattedString'))]"
        }
    }
}
```

De uitvoer uit het vorige voorbeeld met de standaardwaarden is:

| Name | Type | Value |
| ---- | ---- | ----- |
| dataUriOutput | String | data:text/plain;charset=utf8;base64,SGVsbG8= |
| toStringOutput | String | Hello, World! |

## <a name="datauritostring"></a>dataUriToString

`dataUriToString(dataUriToConvert)`

Een gegevens-URI converteert opgemaakt waarde naar een tekenreeks.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| dataUriToConvert |Ja |string |De gegevens-URI-waarde te converteren. |

### <a name="return-value"></a>Retourwaarde

Een tekenreeks met de geconverteerde waarde.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/datauri.json) converteert een waarde naar een gegevens-URI en een gegevens-URI converteert naar een tekenreeks:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringToTest": {
            "type": "string",
            "defaultValue": "Hello"
        },
        "dataFormattedString": {
            "type": "string",
            "defaultValue": "data:;base64,SGVsbG8sIFdvcmxkIQ=="
        }
    },
    "resources": [],
    "outputs": {
        "dataUriOutput": {
            "value": "[dataUri(parameters('stringToTest'))]",
            "type" : "string"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[dataUriToString(parameters('dataFormattedString'))]"
        }
    }
}
```

De uitvoer uit het vorige voorbeeld met de standaardwaarden is:

| Name | Type | Value |
| ---- | ---- | ----- |
| dataUriOutput | String | data:text/plain;charset=utf8;base64,SGVsbG8= |
| toStringOutput | String | Hello, World! |

## <a name="empty"></a>leeg zijn

`empty(itemToTest)`

Bepaalt of een matrix, een object of een tekenreeks leeg is.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| itemToTest |Ja |matrix of object tekenreeks |De waarde moet worden gecontroleerd als dit leeg zijn. |

### <a name="return-value"></a>Retourwaarde

Retourneert **waar** als de waarde leeg, anders is, **False**.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/empty.json) controleert of een matrix, het object en de tekenreeks leeg zijn.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testArray": {
            "type": "array",
            "defaultValue": []
        },
        "testObject": {
            "type": "object",
            "defaultValue": {}
        },
        "testString": {
            "type": "string",
            "defaultValue": ""
        }
    },
    "resources": [
    ],
    "outputs": {
        "arrayEmpty": {
            "type": "bool",
            "value": "[empty(parameters('testArray'))]"
        },
        "objectEmpty": {
            "type": "bool",
            "value": "[empty(parameters('testObject'))]"
        },
        "stringEmpty": {
            "type": "bool",
            "value": "[empty(parameters('testString'))]"
        }
    }
}
```

De uitvoer uit het vorige voorbeeld met de standaardwaarden is:

| Name | Type | Value |
| ---- | ---- | ----- |
| arrayEmpty | Bool | True |
| objectEmpty | Bool | True |
| stringEmpty | Bool | True |

## <a name="endswith"></a>endsWith

`endsWith(stringToSearch, stringToFind)`

Bepaalt of een tekenreeks met een waarde eindigt. De vergelijking is niet hoofdlettergevoelig.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| stringToSearch |Ja |string |De waarde die bevat het artikel om te zoeken. |
| stringToFind |Ja |string |De waarde om te zoeken. |

### <a name="return-value"></a>Retourwaarde

**De waarde True** als het laatste teken of van de tekenreeks overeen met de waarde; anders **False**.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/startsendswith.json) ziet u hoe u de functies startsWith en endsWith:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "startsTrue": {
            "value": "[startsWith('abcdef', 'ab')]",
            "type" : "bool"
        },
        "startsCapTrue": {
            "value": "[startsWith('abcdef', 'A')]",
            "type" : "bool"
        },
        "startsFalse": {
            "value": "[startsWith('abcdef', 'e')]",
            "type" : "bool"
        },
        "endsTrue": {
            "value": "[endsWith('abcdef', 'ef')]",
            "type" : "bool"
        },
        "endsCapTrue": {
            "value": "[endsWith('abcdef', 'F')]",
            "type" : "bool"
        },
        "endsFalse": {
            "value": "[endsWith('abcdef', 'e')]",
            "type" : "bool"
        }
    }
}
```

De uitvoer uit het vorige voorbeeld met de standaardwaarden is:

| Name | Type | Value |
| ---- | ---- | ----- |
| startsTrue | Bool | True |
| startsCapTrue | Bool | True |
| startsFalse | Bool | False |
| endsTrue | Bool | True |
| endsCapTrue | Bool | True |
| endsFalse | Bool | False |

## <a name="first"></a>eerste

`first(arg1)`

Retourneert het eerste teken van de tekenreeks of het eerste element van de matrix.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |matrix of tekenreeks |De waarde om op te halen van het eerste element of het teken. |

### <a name="return-value"></a>Retourwaarde

Een tekenreeks van het eerste teken of het type (string, int, matrix of object) van het eerste element in een matrix.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/first.json) ziet u hoe u de eerste functie met een matrix en een tekenreeks.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "arrayOutput": {
            "type": "string",
            "value": "[first(parameters('arrayToTest'))]"
        },
        "stringOutput": {
            "type": "string",
            "value": "[first('One Two Three')]"
        }
    }
}
```

De uitvoer uit het vorige voorbeeld met de standaardwaarden is:

| Name | Type | Value |
| ---- | ---- | ----- |
| arrayOutput | String | één |
| stringOutput | String | O |

## <a name="format"></a>format

`format(formatString, arg1, arg2, ...)`

Maakt een opgemaakte tekenreeks van de invoerwaarden.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| formatString | Ja | string | De samengestelde tekenreeks. |
| arg1 | Ja | tekenreeks, geheel getal of Booleaanse waarde | De waarde in de opgemaakte tekenreeks wilt opnemen. |
| aanvullende argumenten | Nee | tekenreeks, geheel getal of Booleaanse waarde | Aanvullende waarden in de opgemaakte tekenreeks wilt opnemen. |

### <a name="remarks"></a>Opmerkingen

Deze functie gebruiken om de opmaak van een tekenreeks in de sjabloon. Hierbij de dezelfde opties als de [System.String.Format](/dotnet/api/system.string.format) methode in .NET.

### <a name="examples"></a>Voorbeelden

De voorbeeldsjabloon van het volgende ziet u hoe u de functie format.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "greeting": {
            "type": "string",
            "defaultValue": "Hello"
        },
        "name": {
            "type": "string",
            "defaultValue": "User"
        },
        "numberToFormat": {
            "type": "int",
            "defaultValue": 8175133
        }
    },
    "resources": [
    ],
    "outputs": {
        "formatTest": {
            "type": "string",
            "value": "[format('{0}, {1}. Formatted number: {2:N0}', parameters('greeting'), parameters('name'), parameters('numberToFormat'))]"
        }
    }
}
```

De uitvoer uit het vorige voorbeeld met de standaardwaarden is:

| Name | Type | Value |
| ---- | ---- | ----- |
| formatTest | String | Hallo, gebruiker. Opgemaakte nummer: 8,175,133 |

## <a name="guid"></a>GUID

`guid(baseString, ...)`

Hiermee maakt een waarde in de indeling van een globally unique identifier op basis van de waarden geleverd als parameters.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| baseString |Ja |string |De waarde die wordt gebruikt in de hash-functie voor het maken van de GUID. |
| aanvullende parameters indien nodig |Nee |string |U kunt zoveel tekenreeksen als nodig voor het maken van de waarde die het niveau van de uniciteit bepaalt toevoegen. |

### <a name="remarks"></a>Opmerkingen

Deze functie is handig als u wilt maken van een waarde in de indeling van een globally unique identifier. U opgeven parameterwaarden die beperken van het bereik van de uniciteit voor het resultaat. U kunt opgeven of de naam van de unieke omlaag naar het abonnement, resourcegroep of implementatie.

De geretourneerde waarde is niet een willekeurige tekenreeks, maar in plaats van het resultaat van een hash-functie van de parameters. De geretourneerde waarde is 36 tekens lang zijn. Het is niet uniek. Voor het maken van een nieuwe GUID die niet is gebaseerd op deze hashwaarde van de parameters gebruikt de [newGuid](#newguid) functie.

De volgende voorbeelden laten zien hoe u guid om te maken van een unieke waarde voor de meest gebruikte niveaus.

Uniek binnen het bereik van abonnement

```json
"[guid(subscription().subscriptionId)]"
```

Uniek binnen het bereik van resourcegroep

```json
"[guid(resourceGroup().id)]"
```

Uniek binnen het bereik van de implementatie voor een resourcegroep

```json
"[guid(resourceGroup().id, deployment().name)]"
```

### <a name="return-value"></a>Retourwaarde

Een tekenreeks met 36 tekens in de indeling van een globally unique identifier.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/guid.json) resultaten uit guid:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "variables": {},
    "resources": [],
    "outputs": {
        "guidPerSubscription": {
            "value": "[guid(subscription().subscriptionId)]",
            "type": "string"
        },
        "guidPerResourceGroup": {
            "value": "[guid(resourceGroup().id)]",
            "type": "string"
        },
        "guidPerDeployment": {
            "value": "[guid(resourceGroup().id, deployment().name)]",
            "type": "string"
        }
    }
}
```

## <a name="indexof"></a>indexOf

`indexOf(stringToSearch, stringToFind)`

Retourneert de eerste positie van een waarde van een tekenreeks. De vergelijking is niet hoofdlettergevoelig.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| stringToSearch |Ja |string |De waarde die bevat het artikel om te zoeken. |
| stringToFind |Ja |string |De waarde om te zoeken. |

### <a name="return-value"></a>Retourwaarde

Een geheel getal dat staat voor de positie van het item te vinden. De waarde is nul. Als het item is niet gevonden, wordt -1 geretourneerd.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/indexof.json) ziet u hoe u de functies indexOf en lastIndexOf:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "firstT": {
            "value": "[indexOf('test', 't')]",
            "type" : "int"
        },
        "lastT": {
            "value": "[lastIndexOf('test', 't')]",
            "type" : "int"
        },
        "firstString": {
            "value": "[indexOf('abcdef', 'CD')]",
            "type" : "int"
        },
        "lastString": {
            "value": "[lastIndexOf('abcdef', 'AB')]",
            "type" : "int"
        },
        "notFound": {
            "value": "[indexOf('abcdef', 'z')]",
            "type" : "int"
        }
    }
}
```

De uitvoer uit het vorige voorbeeld met de standaardwaarden is:

| Name | Type | Value |
| ---- | ---- | ----- |
| firstT | Int | 0 |
| lastT | Int | 3 |
| firstString | Int | 2 |
| lastString | Int | 0 |
| notFound | Int | -1 |

## <a name="last"></a>laatste

`last (arg1)`

Retourneert de laatste teken van de tekenreeks of het laatste element van de matrix.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |matrix of tekenreeks |De waarde om op te halen van het laatste element of het teken. |

### <a name="return-value"></a>Retourwaarde

Een tekenreeks van het laatste teken of het type (string, int, matrix of object) van het laatste element in een matrix.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/last.json) ziet u hoe u de laatste functie met een matrix en een tekenreeks.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "arrayOutput": {
            "type": "string",
            "value": "[last(parameters('arrayToTest'))]"
        },
        "stringOutput": {
            "type": "string",
            "value": "[last('One Two Three')]"
        }
    }
}
```

De uitvoer uit het vorige voorbeeld met de standaardwaarden is:

| Name | Type | Value |
| ---- | ---- | ----- |
| arrayOutput | String | drie |
| stringOutput | String | e |

## <a name="lastindexof"></a>lastIndexOf

`lastIndexOf(stringToSearch, stringToFind)`

Retourneert de laatste positie van een waarde van een tekenreeks. De vergelijking is niet hoofdlettergevoelig.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| stringToSearch |Ja |string |De waarde die bevat het artikel om te zoeken. |
| stringToFind |Ja |string |De waarde om te zoeken. |

### <a name="return-value"></a>Retourwaarde

Een geheel getal dat staat voor de laatste positie van het item te vinden. De waarde is nul. Als het item is niet gevonden, wordt -1 geretourneerd.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/indexof.json) ziet u hoe u de functies indexOf en lastIndexOf:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "firstT": {
            "value": "[indexOf('test', 't')]",
            "type" : "int"
        },
        "lastT": {
            "value": "[lastIndexOf('test', 't')]",
            "type" : "int"
        },
        "firstString": {
            "value": "[indexOf('abcdef', 'CD')]",
            "type" : "int"
        },
        "lastString": {
            "value": "[lastIndexOf('abcdef', 'AB')]",
            "type" : "int"
        },
        "notFound": {
            "value": "[indexOf('abcdef', 'z')]",
            "type" : "int"
        }
    }
}
```

De uitvoer uit het vorige voorbeeld met de standaardwaarden is:

| Name | Type | Value |
| ---- | ---- | ----- |
| firstT | Int | 0 |
| lastT | Int | 3 |
| firstString | Int | 2 |
| lastString | Int | 0 |
| notFound | Int | -1 |

## <a name="length"></a>length

`length(string)`

Retourneert het aantal tekens in een tekenreeks of elementen in een matrix.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |matrix of tekenreeks |De matrix te gebruiken voor het ophalen van het aantal elementen, of een tekenreeks te gebruiken voor het ophalen van het aantal tekens. |

### <a name="return-value"></a>Retourwaarde

Een int. 

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/length.json) ziet u hoe u de lengte van een matrix en een tekenreeks:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": [
                "one",
                "two",
                "three"
            ]
        },
        "stringToTest": {
            "type": "string",
            "defaultValue": "One Two Three"
        }
    },
    "resources": [],
    "outputs": {
        "arrayLength": {
            "type": "int",
            "value": "[length(parameters('arrayToTest'))]"
        },
        "stringLength": {
            "type": "int",
            "value": "[length(parameters('stringToTest'))]"
        }
    }
}
```

De uitvoer uit het vorige voorbeeld met de standaardwaarden is:

| Name | Type | Value |
| ---- | ---- | ----- |
| arrayLength | Int | 3 |
| stringLength | Int | 13 |

## <a name="newguid"></a>newGuid

`newGuid()`

Retourneert een waarde in de indeling van een globally unique identifier. **Deze functie kan alleen worden gebruikt in de standaardwaarde voor een parameter.**

### <a name="remarks"></a>Opmerkingen

U kunt deze functie in een expressie alleen gebruiken voor de standaardwaarde van een parameter. Met deze functie ergens anders in een sjabloon wordt een fout geretourneerd. De functie is niet toegestaan in andere onderdelen van de sjabloon, omdat deze een andere waarde retourneert telkens wanneer die deze wordt aangeroepen. Implementatie van dezelfde sjabloon met dezelfde parameters, wouldn't op betrouwbare wijze de dezelfde resultaten opleveren.

De functie newGuid wijkt af van de [guid](#guid) omdat deze parameters niet uitvoeren. Wanneer u de guid met dezelfde parameter aanroept, wordt dezelfde id telkens wanneer. Guid gebruiken wanneer u nodig hebt voor het genereren van betrouwbaar dezelfde GUID voor een specifieke omgeving. NewGuid gebruiken wanneer u een andere id telkens moet, zoals het implementeren van resources in een testomgeving.

Als u de [optie voor het implementeren van een eerder geslaagde implementatie](resource-group-template-deploy-rest.md#redeploy-when-deployment-fails), en de eerdere implementatie omvat een parameter die gebruikmaakt van newGuid, de parameter is niet opnieuw geëvalueerd. De waarde van parameter uit de eerdere implementatie is in plaats daarvan automatisch opnieuw gebruikt in de implementatie ongedaan maken.

In een testomgeving moet u mogelijk resources die alleen live voor korte tijd herhaaldelijk te implementeren. In plaats van unieke namen maken, kunt u newGuid met [uniqueString](#uniquestring) unieke namen moeten worden gemaakt.

Wees voorzichtig een sjabloon die is gebaseerd op de functie newGuid voor een standaardwaarde opnieuw wilt implementeren. Wanneer u opnieuw implementeren en niet een waarde voor de parameter opgeven, wordt de functie opnieuw geëvalueerd. Als u wilt bijwerken van een bestaande resource in plaats van een nieuwe maken, worden uit de eerdere implementatie in de waarde van parameter doorgeven.

### <a name="return-value"></a>Retourwaarde

Een tekenreeks met 36 tekens in de indeling van een globally unique identifier.

### <a name="examples"></a>Voorbeelden

De voorbeeldsjabloon van het volgende ziet u een parameter met een nieuwe id.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "guidValue": {
            "type": "string",
            "defaultValue": "[newGuid()]"
        }
    },
    "resources": [
    ],
    "outputs": {
        "guidOutput": {
            "type": "string",
            "value": "[parameters('guidValue')]"
        }
    }
}
```

De uitvoer van het vorige voorbeeld varieert voor elke implementatie, maar is vergelijkbaar met:

| Name | Type | Value |
| ---- | ---- | ----- |
| guidOutput | string | b76a51fc-bd72-4a77-b9a2-3c29e7d2e551 |

Het volgende voorbeeld gebruikt de functie newGuid om een unieke naam voor een opslagaccount maken. Deze sjabloon werkt mogelijk voor testomgeving waar het opslagaccount bestaat voor een korte periode en wordt niet geïmplementeerd.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "guidValue": {
            "type": "string",
            "defaultValue": "[newGuid()]"
        }
    },
    "variables": {
        "storageName": "[concat('storage', uniqueString(parameters('guidValue')))]"
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('storageName')]",
            "location": "West US",
            "apiVersion": "2018-07-01",
            "sku":{
                "name": "Standard_LRS"
            },
            "kind": "StorageV2",
            "properties": {}
        }
    ],
    "outputs": {
        "nameOutput": {
            "type": "string",
            "value": "[variables('storageName')]"
        }
    }
}
```

De uitvoer van het vorige voorbeeld varieert voor elke implementatie, maar is vergelijkbaar met:

| Name | Type | Value |
| ---- | ---- | ----- |
| nameOutput | string | storagenziwvyru7uxie |


## <a name="padleft"></a>padLeft

`padLeft(valueToPad, totalLength, paddingCharacter)`

Retourneert een rechts uitgelijnde tekenreeks met tekens toe te voegen aan de linkerkant tot het bereiken van de totale lengte van de opgegeven.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| valueToPad |Ja |tekenreeks of int |De waarde rechts uitlijnen. |
| totalLength |Ja |int |Het totale aantal tekens in de geretourneerde tekenreeks. |
| paddingCharacter |Nee |willekeurig teken |Het teken dat moet worden gebruikt voor links-opvulling totdat de totale lengte is bereikt. De standaardwaarde is een spatie. |

Als de oorspronkelijke reeks langer dan het aantal tekens dat moet worden opvullen met het teken is, worden geen tekens toegevoegd.

### <a name="return-value"></a>Retourwaarde

Een tekenreeks met ten minste het aantal tekens opgegeven.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/padleft.json) laat zien hoe u opvullen met het teken van de gebruiker opgegeven parameterwaarde door toe te voegen van het teken nul totdat het totale aantal tekens is bereikt. 

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "123"
        }
    },
    "resources": [],
    "outputs": {
        "stringOutput": {
            "type": "string",
            "value": "[padLeft(parameters('testString'),10,'0')]"
        }
    }
}
```

De uitvoer uit het vorige voorbeeld met de standaardwaarden is:

| Name | Type | Value |
| ---- | ---- | ----- |
| stringOutput | String | 0000000123 |

## <a name="replace"></a>vervangen

`replace(originalString, oldString, newString)`

Retourneert een nieuwe tekenreeks met alle exemplaren van een tekenreeks vervangen door een andere tekenreeks.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| originalString |Ja |string |De waarde waarvoor alle exemplaren van een tekenreeks vervangen door een andere tekenreeks. |
| oldString |Ja |string |De tekenreeks die moet worden verwijderd uit de oorspronkelijke reeks. |
| newString |Ja |string |De tekenreeks om toe te voegen in plaats van de verwijderde tekenreeks. |

### <a name="return-value"></a>Retourwaarde

Een tekenreeks met de tekens die vervangen.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/replace.json) leest hoe alle streepjes verwijderen uit de gebruiker opgegeven tekenreeks en Vervang een deel van de tekenreeks met een andere tekenreeks.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "123-123-1234"
        }
    },
    "resources": [],
    "outputs": {
        "firstOutput": {
            "type": "string",
            "value": "[replace(parameters('testString'),'-', '')]"
        },
        "secondOutput": {
            "type": "string",
            "value": "[replace(parameters('testString'),'1234', 'xxxx')]"
        }
    }
}
```

De uitvoer uit het vorige voorbeeld met de standaardwaarden is:

| Name | Type | Value |
| ---- | ---- | ----- |
| firstOutput | String | 1231231234 |
| secondOutput | String | 123-123-xxxx |

## <a name="skip"></a>Overslaan

`skip(originalValue, numberToSkip)`

Retourneert een tekenreeks waarbij alle tekens na het opgegeven aantal tekens of een matrix met alle elementen na het opgegeven aantal elementen.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| originalValue |Ja |matrix of tekenreeks |De ingevoerde matrix of tekenreeks die moet worden gebruikt voor het overslaan. |
| numberToSkip |Ja |int |Het aantal elementen of tekens om over te slaan. Als deze waarde 0 of minder is, worden alle elementen of tekens in de waarde geretourneerd. Als dit groter is dan de lengte van de matrix of tekenreeks, wordt een lege matrix of tekenreeks geretourneerd. |

### <a name="return-value"></a>Retourwaarde

Een matrix of tekenreeks.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/skip.json) slaat het opgegeven aantal elementen in de matrix, en het opgegeven aantal tekens in een tekenreeks.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testArray": {
            "type": "array",
            "defaultValue": [
                "one",
                "two",
                "three"
            ]
        },
        "elementsToSkip": {
            "type": "int",
            "defaultValue": 2
        },
        "testString": {
            "type": "string",
            "defaultValue": "one two three"
        },
        "charactersToSkip": {
            "type": "int",
            "defaultValue": 4
        }
    },
    "resources": [],
    "outputs": {
        "arrayOutput": {
            "type": "array",
            "value": "[skip(parameters('testArray'),parameters('elementsToSkip'))]"
        },
        "stringOutput": {
            "type": "string",
            "value": "[skip(parameters('testString'),parameters('charactersToSkip'))]"
        }
    }
}
```

De uitvoer uit het vorige voorbeeld met de standaardwaarden is:

| Name | Type | Value |
| ---- | ---- | ----- |
| arrayOutput | Matrix | ["3"] |
| stringOutput | String | twee drie |

## <a name="split"></a>split

`split(inputString, delimiter)`

Retourneert een matrix met tekenreeksen die de subtekenreeksen van de ingevoerde tekenreeks die worden gescheiden door de opgegeven scheidingstekens bevat.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| inputString |Ja |string |De tekenreeks te splitsen. |
| scheidingsteken |Ja |tekenreeks of een matrix met tekenreeksen |Het scheidingsteken moet worden gebruikt voor het splitsen van de tekenreeks. |

### <a name="return-value"></a>Retourwaarde

Een matrix met tekenreeksen.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/split.json) splitst de ingevoerde tekenreeks met een door komma's, en met een komma of puntkomma's.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstString": {
            "type": "string",
            "defaultValue": "one,two,three"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "one;two,three"
        }
    },
    "variables": {
        "delimiters": [ ",", ";" ]
    },
    "resources": [],
    "outputs": {
        "firstOutput": {
            "type": "array",
            "value": "[split(parameters('firstString'),',')]"
        },
        "secondOutput": {
            "type": "array",
            "value": "[split(parameters('secondString'),variables('delimiters'))]"
        }
    }
}
```

De uitvoer uit het vorige voorbeeld met de standaardwaarden is:

| Name | Type | Value |
| ---- | ---- | ----- |
| firstOutput | Matrix | ["een", "twee", "drie"] |
| secondOutput | Matrix | ["een", "twee", "drie"] |

## <a name="startswith"></a>startsWith

`startsWith(stringToSearch, stringToFind)`

Bepaalt of een tekenreeks met een waarde begint. De vergelijking is niet hoofdlettergevoelig.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| stringToSearch |Ja |string |De waarde die bevat het artikel om te zoeken. |
| stringToFind |Ja |string |De waarde om te zoeken. |

### <a name="return-value"></a>Retourwaarde

**De waarde True** als het eerste teken of van de tekenreeks overeen met de waarde; anders **False**.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/startsendswith.json) ziet u hoe u de functies startsWith en endsWith:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "startsTrue": {
            "value": "[startsWith('abcdef', 'ab')]",
            "type" : "bool"
        },
        "startsCapTrue": {
            "value": "[startsWith('abcdef', 'A')]",
            "type" : "bool"
        },
        "startsFalse": {
            "value": "[startsWith('abcdef', 'e')]",
            "type" : "bool"
        },
        "endsTrue": {
            "value": "[endsWith('abcdef', 'ef')]",
            "type" : "bool"
        },
        "endsCapTrue": {
            "value": "[endsWith('abcdef', 'F')]",
            "type" : "bool"
        },
        "endsFalse": {
            "value": "[endsWith('abcdef', 'e')]",
            "type" : "bool"
        }
    }
}
```

De uitvoer uit het vorige voorbeeld met de standaardwaarden is:

| Name | Type | Value |
| ---- | ---- | ----- |
| startsTrue | Bool | True |
| startsCapTrue | Bool | True |
| startsFalse | Bool | False |
| endsTrue | Bool | True |
| endsCapTrue | Bool | True |
| endsFalse | Bool | False |

## <a name="string"></a>string

`string(valueToConvert)`

De opgegeven waarde omgezet in een tekenreeks.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| valueToConvert |Ja | Alle |De waarde te converteren naar een tekenreeks. Elk type waarde kan worden geconverteerd, met inbegrip van objecten en -matrices. |

### <a name="return-value"></a>Retourwaarde

Een tekenreeks van de geconverteerde waarde.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/string.json) laat zien hoe u verschillende soorten waarden converteren naar tekenreeksen:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testObject": {
            "type": "object",
            "defaultValue": {
                "valueA": 10,
                "valueB": "Example Text"
            }
        },
        "testArray": {
            "type": "array",
            "defaultValue": [
                "a",
                "b",
                "c"
            ]
        },
        "testInt": {
            "type": "int",
            "defaultValue": 5
        }
    },
    "resources": [],
    "outputs": {
        "objectOutput": {
            "type": "string",
            "value": "[string(parameters('testObject'))]"
        },
        "arrayOutput": {
            "type": "string",
            "value": "[string(parameters('testArray'))]"
        },
        "intOutput": {
            "type": "string",
            "value": "[string(parameters('testInt'))]"
        }
    }
}
```

De uitvoer uit het vorige voorbeeld met de standaardwaarden is:

| Name | Type | Value |
| ---- | ---- | ----- |
| objectOutput | String | {"valueA":10,"valueB":"Example Text"} |
| arrayOutput | String | ["a","b","c"] |
| intOutput | String | 5 |

## <a name="substring"></a>de subtekenreeks

`substring(stringToParse, startIndex, length)`

Retourneert een subtekenreeks die begint op de positie van het opgegeven teken en het opgegeven aantal tekens bevat.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| stringToParse |Ja |string |De oorspronkelijke tekenreeks waaruit de subtekenreeks wordt opgehaald. |
| startIndex |Nee |int |De op nul gebaseerde beginpositie voor de subtekenreeks. |
| length |Nee |int |Het aantal tekens in voor de subtekenreeks. Moet verwijzen naar een locatie in de tekenreeks. Moet nul of groter zijn. |

### <a name="return-value"></a>Retourwaarde

De subtekenreeks. Of een lege tekenreeks als de lengte nul is.

### <a name="remarks"></a>Opmerkingen

De functie mislukt wanneer de subtekenreeks valt buiten het einde van de tekenreeks, of wanneer de lengte is kleiner dan nul zijn. Het volgende voorbeeld is mislukt met de fout "de index en lengte moeten verwijzen naar een locatie in de tekenreeks. De Indexparameter: '0', de lengteparameter: '11', de lengte van de tekenreeksparameter: '10'.".

```json
"parameters": {
    "inputString": { "type": "string", "value": "1234567890" }
},
"variables": { 
    "prefix": "[substring(parameters('inputString'), 0, 11)]"
}
```

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/substring.json) een subtekenreeks uit een parameter.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "one two three"
        }
    },
    "resources": [],
    "outputs": {
        "substringOutput": {
            "value": "[substring(parameters('testString'), 4, 3)]",
            "type": "string"
        }
    }
}
```

De uitvoer uit het vorige voorbeeld met de standaardwaarden is:

| Name | Type | Value |
| ---- | ---- | ----- |
| substringOutput | String | twee |

## <a name="take"></a>toets maken

`take(originalValue, numberToTake)`

Retourneert een tekenreeks zijn met het opgegeven aantal tekens vanaf het begin van de tekenreeks of een matrix met het opgegeven aantal elementen vanaf het begin van de matrix.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| originalValue |Ja |matrix of tekenreeks |De ingevoerde matrix of tekenreeks waaruit de elementen uit. |
| numberToTake |Ja |int |Het aantal elementen of tekens op te nemen. Als deze waarde 0 of minder is, wordt een lege matrix of tekenreeks geretourneerd. Als dit groter is dan de lengte van de opgegeven matrix of tekenreeks, worden alle elementen in de matrix of tekenreeks worden geretourneerd. |

### <a name="return-value"></a>Retourwaarde

Een matrix of tekenreeks.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/take.json) neemt van het opgegeven aantal elementen van de matrix en tekens uit een tekenreeks.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testArray": {
            "type": "array",
            "defaultValue": [
                "one",
                "two",
                "three"
            ]
        },
        "elementsToTake": {
            "type": "int",
            "defaultValue": 2
        },
        "testString": {
            "type": "string",
            "defaultValue": "one two three"
        },
        "charactersToTake": {
            "type": "int",
            "defaultValue": 2
        }
    },
    "resources": [],
    "outputs": {
        "arrayOutput": {
            "type": "array",
            "value": "[take(parameters('testArray'),parameters('elementsToTake'))]"
        },
        "stringOutput": {
            "type": "string",
            "value": "[take(parameters('testString'),parameters('charactersToTake'))]"
        }
    }
}
```

De uitvoer uit het vorige voorbeeld met de standaardwaarden is:

| Name | Type | Value |
| ---- | ---- | ----- |
| arrayOutput | Matrix | ["een", "twee"] |
| stringOutput | String | op |

## <a name="tolower"></a>toLower

`toLower(stringToChange)`

De opgegeven tekenreeks die wordt omgezet in kleine letters.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| stringToChange |Ja |string |De waarde moet worden geconverteerd naar kleine letters. |

### <a name="return-value"></a>Retourwaarde

De tekenreeks wordt geconverteerd naar kleine letters.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/tolower.json) een parameterwaarde wordt geconverteerd naar kleine letters en hoofdletters.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "One Two Three"
        }
    },
    "resources": [],
    "outputs": {
        "toLowerOutput": {
            "value": "[toLower(parameters('testString'))]",
            "type": "string"
        },
        "toUpperOutput": {
            "type": "string",
            "value": "[toUpper(parameters('testString'))]"
        }
    }
}
```

De uitvoer uit het vorige voorbeeld met de standaardwaarden is:

| Name | Type | Value |
| ---- | ---- | ----- |
| toLowerOutput | String | Een twee drie |
| toUpperOutput | String | EEN TWEE DRIE |

## <a name="toupper"></a>toUpper

`toUpper(stringToChange)`

De opgegeven tekenreeks die wordt omgezet in hoofdletters.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| stringToChange |Ja |string |De waarde moet worden geconverteerd naar hoofdletters. |

### <a name="return-value"></a>Retourwaarde

De tekenreeks wordt geconverteerd naar hoofdletters.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/tolower.json) een parameterwaarde wordt geconverteerd naar kleine letters en hoofdletters.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "One Two Three"
        }
    },
    "resources": [],
    "outputs": {
        "toLowerOutput": {
            "value": "[toLower(parameters('testString'))]",
            "type": "string"
        },
        "toUpperOutput": {
            "type": "string",
            "value": "[toUpper(parameters('testString'))]"
        }
    }
}
```

De uitvoer uit het vorige voorbeeld met de standaardwaarden is:

| Name | Type | Value |
| ---- | ---- | ----- |
| toLowerOutput | String | Een twee drie |
| toUpperOutput | String | EEN TWEE DRIE |

## <a name="trim"></a>trim

`trim (stringToTrim)`

Verwijdert alle voorloop- en volgspaties spatietekens bestaan uit de opgegeven tekenreeks.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| stringToTrim |Ja |string |De waarde naar trim. |

### <a name="return-value"></a>Retourwaarde

De tekenreeks zonder voorloopspaties en afsluitende spatietekens bevatten.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/trim.json) verwijdert de spatietekens bevatten van de parameter.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "    one two three   "
        }
    },
    "resources": [],
    "outputs": {
        "return": {
            "type": "string",
            "value": "[trim(parameters('testString'))]"
        }
    }
}
```

De uitvoer uit het vorige voorbeeld met de standaardwaarden is:

| Name | Type | Value |
| ---- | ---- | ----- |
| terug | String | Een twee drie |

## <a name="uniquestring"></a>uniqueString

`uniqueString (baseString, ...)`

Hiermee maakt u een deterministische hash-tekenreeks op basis van de waarden geleverd als parameters. 

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| baseString |Ja |string |De waarde die wordt gebruikt in de hash-functie voor het maken van een unieke tekenreeks. |
| aanvullende parameters indien nodig |Nee |string |U kunt zoveel tekenreeksen als nodig voor het maken van de waarde die het niveau van de uniciteit bepaalt toevoegen. |

### <a name="remarks"></a>Opmerkingen

Deze functie is handig wanneer u moet een unieke naam voor een resource maken. U opgeven parameterwaarden die beperken van het bereik van de uniciteit voor het resultaat. U kunt opgeven of de naam van de unieke omlaag naar het abonnement, resourcegroep of implementatie. 

De geretourneerde waarde is niet een willekeurige tekenreeks, maar in plaats van het resultaat van een hash-functie. De geretourneerde waarde is 13 tekens bevatten. Het is niet uniek. U kunt de waarde worden gecombineerd met een voorvoegsel van de naamconventie voor het maken van een naam die zinvol is. Het volgende voorbeeld ziet de indeling van de geretourneerde waarde. De werkelijke waarde is afhankelijk van de opgegeven parameters.

    tcvhiyu5h2o5o

De volgende voorbeelden laten zien hoe uniqueString gebruiken voor het maken van een unieke waarde voor de meest gebruikte niveaus.

Uniek binnen het bereik van abonnement

```json
"[uniqueString(subscription().subscriptionId)]"
```

Uniek binnen het bereik van resourcegroep

```json
"[uniqueString(resourceGroup().id)]"
```

Uniek binnen het bereik van de implementatie voor een resourcegroep

```json
"[uniqueString(resourceGroup().id, deployment().name)]"
```

Het volgende voorbeeld ziet hoe u een unieke naam voor een opslagaccount op basis van de resourcegroep te maken. De naam is niet binnen de resourcegroep, unieke als op dezelfde manier samengesteld.

```json
"resources": [{ 
    "name": "[concat('storage', uniqueString(resourceGroup().id))]", 
    "type": "Microsoft.Storage/storageAccounts", 
    ...
```

Als u wilt maken van een nieuwe unieke naam telkens wanneer u een sjabloon implementeren, en niet van plan bent om bij te werken van de bron, kunt u de [utcNow](#utcnow) functie met uniqueString. U kunt deze aanpak gebruiken in een testomgeving. Zie voor een voorbeeld [utcNow](#utcnow).

### <a name="return-value"></a>Retourwaarde

Een tekenreeks met 13 tekens.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/uniquestring.json) resultaten uit uniquestring:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "uniqueRG": {
            "value": "[uniqueString(resourceGroup().id)]",
            "type" : "string"
        },
        "uniqueDeploy": {
            "value": "[uniqueString(resourceGroup().id, deployment().name)]",
            "type" : "string"
        }
    }
}
```

## <a name="uri"></a>URI

`uri (baseUri, relativeUri)`

Hiermee maakt u een absolute URI zijn die door de baseUri en de tekenreeks relativeUri te combineren.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| baseUri |Ja |string |De basis-uri-tekenreeks. |
| relativeUri |Ja |string |De relatieve uri-tekenreeks om toe te voegen aan de basis-uri-tekenreeks. |

De waarde voor de **baseUri** parameter kan een specifiek bestand bevatten, maar alleen het basispad wordt gebruikt bij het maken van de URI. Bijvoorbeeld, doorgeven `http://contoso.com/resources/azuredeploy.json` als de resultaten van de parameter baseUri in een basis-URI van `http://contoso.com/resources/`.

### <a name="return-value"></a>Retourwaarde

Een tekenreeks die de absolute URI zijn die voor de basis- en relatieve waarden vertegenwoordigt.

### <a name="examples"></a>Voorbeelden

Het volgende voorbeeld ziet hoe u een koppeling naar een geneste sjabloon op basis van de waarde van de bovenliggende sjabloon.

```json
"templateLink": "[uri(deployment().properties.templateLink.uri, 'nested/azuredeploy.json')]"
```

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/uri.json) ziet u hoe u de uri en uriComponent uriComponentToString:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
        "uriFormat": "[uri('http://contoso.com/resources/', 'nested/azuredeploy.json')]",
        "uriEncoded": "[uriComponent(variables('uriFormat'))]" 
    },
    "resources": [
    ],
    "outputs": {
        "uriOutput": {
            "type": "string",
            "value": "[variables('uriFormat')]"
        },
        "componentOutput": {
            "type": "string",
            "value": "[variables('uriEncoded')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[uriComponentToString(variables('uriEncoded'))]"
        }
    }
}
```

De uitvoer uit het vorige voorbeeld met de standaardwaarden is:

| Name | Type | Value |
| ---- | ---- | ----- |
| uriOutput | String | http://contoso.com/resources/nested/azuredeploy.json |
| componentOutput | String | http%3A%2F%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.json |
| toStringOutput | String | http://contoso.com/resources/nested/azuredeploy.json |

## <a name="uricomponent"></a>uriComponent

`uricomponent(stringToEncode)`

Codeert een URI.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| stringToEncode |Ja |string |De waarde moet worden gecodeerd. |

### <a name="return-value"></a>Retourwaarde

Een tekenreeks van de URI gecodeerde waarde.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/uri.json) ziet u hoe u de uri en uriComponent uriComponentToString:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
        "uriFormat": "[uri('http://contoso.com/resources/', 'nested/azuredeploy.json')]",
        "uriEncoded": "[uriComponent(variables('uriFormat'))]" 
    },
    "resources": [
    ],
    "outputs": {
        "uriOutput": {
            "type": "string",
            "value": "[variables('uriFormat')]"
        },
        "componentOutput": {
            "type": "string",
            "value": "[variables('uriEncoded')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[uriComponentToString(variables('uriEncoded'))]"
        }
    }
}
```

De uitvoer uit het vorige voorbeeld met de standaardwaarden is:

| Name | Type | Value |
| ---- | ---- | ----- |
| uriOutput | String | http://contoso.com/resources/nested/azuredeploy.json |
| componentOutput | String | http%3A%2F%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.json |
| toStringOutput | String | http://contoso.com/resources/nested/azuredeploy.json |

## <a name="uricomponenttostring"></a>uriComponentToString

`uriComponentToString(uriEncodedString)`

Retourneert dat een tekenreeks van een URI-gecodeerde waarde.

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| uriEncodedString |Ja |string |Waarde moet worden geconverteerd naar een tekenreeks met de URI-codering. |

### <a name="return-value"></a>Retourwaarde

Een gedecodeerde tekenreeks met URI gecodeerde waarde.

### <a name="examples"></a>Voorbeelden

De volgende [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/uri.json) ziet u hoe u de uri en uriComponent uriComponentToString:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
        "uriFormat": "[uri('http://contoso.com/resources/', 'nested/azuredeploy.json')]",
        "uriEncoded": "[uriComponent(variables('uriFormat'))]" 
    },
    "resources": [
    ],
    "outputs": {
        "uriOutput": {
            "type": "string",
            "value": "[variables('uriFormat')]"
        },
        "componentOutput": {
            "type": "string",
            "value": "[variables('uriEncoded')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[uriComponentToString(variables('uriEncoded'))]"
        }
    }
}
```

De uitvoer uit het vorige voorbeeld met de standaardwaarden is:

| Name | Type | Value |
| ---- | ---- | ----- |
| uriOutput | String | http://contoso.com/resources/nested/azuredeploy.json |
| componentOutput | String | http%3A%2F%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.json |
| toStringOutput | String | http://contoso.com/resources/nested/azuredeploy.json |

## <a name="utcnow"></a>utcNow

`utcNow(format)`

Retourneert de huidige datum/tijd-waarde van (UTC) in de indeling die is opgegeven. Als er geen indeling is opgegeven, wordt de ISO 8601 (JJJJMMDDTuummssZ)-indeling wordt gebruikt. **Deze functie kan alleen worden gebruikt in de standaardwaarde voor een parameter.**

### <a name="parameters"></a>Parameters

| Parameter | Vereist | Type | Description |
|:--- |:--- |:--- |:--- |
| format |Nee |string |Waarde moet worden geconverteerd naar een tekenreeks met de URI-codering. Gebruik een [standard opmaaktekenreeksen](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) of [tekenreeksen met aangepaste notatie](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). |

### <a name="remarks"></a>Opmerkingen

U kunt deze functie in een expressie alleen gebruiken voor de standaardwaarde van een parameter. Met deze functie ergens anders in een sjabloon wordt een fout geretourneerd. De functie is niet toegestaan in andere onderdelen van de sjabloon, omdat deze een andere waarde retourneert telkens wanneer die deze wordt aangeroepen. Implementatie van dezelfde sjabloon met dezelfde parameters, wouldn't op betrouwbare wijze de dezelfde resultaten opleveren.

Als u de [optie voor het implementeren van een eerder geslaagde implementatie](resource-group-template-deploy-rest.md#redeploy-when-deployment-fails), en de eerdere implementatie omvat een parameter die gebruikmaakt van utcNow, de parameter is niet opnieuw geëvalueerd. De waarde van parameter uit de eerdere implementatie is in plaats daarvan automatisch opnieuw gebruikt in de implementatie ongedaan maken.

Wees voorzichtig een sjabloon die is gebaseerd op de functie utcNow voor een standaardwaarde opnieuw wilt implementeren. Wanneer u opnieuw implementeren en niet een waarde voor de parameter opgeven, wordt de functie opnieuw geëvalueerd. Als u wilt bijwerken van een bestaande resource in plaats van een nieuwe maken, worden uit de eerdere implementatie in de waarde van parameter doorgeven.

### <a name="return-value"></a>Retourwaarde

De huidige UTC datum/tijd-waarde.

### <a name="examples"></a>Voorbeelden

De volgende voorbeeldsjabloon bevat verschillende indelingen voor de datum / tijdwaarde.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "utcValue": {
            "type": "string",
            "defaultValue": "[utcNow()]"
        },
        "utcShortValue": {
            "type": "string",
            "defaultValue": "[utcNow('d')]"
        },
        "utcCustomValue": {
            "type": "string",
            "defaultValue": "[utcNow('M d')]"
        }
    },
    "resources": [
    ],
    "outputs": {
        "utcOutput": {
            "type": "string",
            "value": "[parameters('utcValue')]"
        },
        "utcShortOutput": {
            "type": "string",
            "value": "[parameters('utcShortValue')]"
        },
        "utcCustomOutput": {
            "type": "string",
            "value": "[parameters('utcCustomValue')]"
        }
    }
}
```

De uitvoer van het vorige voorbeeld varieert voor elke implementatie, maar is vergelijkbaar met:

| Name | Type | Value |
| ---- | ---- | ----- |
| utcOutput | string | 20190305T175318Z |
| utcShortOutput | string | 03/05/2019 |
| utcCustomOutput | string | 3 5 |

Het volgende voorbeeld ziet u hoe u een waarde van de functie wordt gebruikt bij het instellen van een tagwaarde.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "utcShort": {
            "type": "string",
            "defaultValue": "[utcNow('d')]"
        },
        "rgName": {
            "type": "string"
        }
    },
    "resources": [
        {
            "type": "Microsoft.Resources/resourceGroups",
            "apiVersion": "2018-05-01",
            "name": "[parameters('rgName')]",
            "location": "westeurope",
            "tags":{
                "createdDate": "[parameters('utcShort')]"
            },
            "properties":{}
        }
    ],
    "outputs": {
        "utcShort": {
            "type": "string",
            "value": "[parameters('utcShort')]"
        }
    }
}
```

## <a name="next-steps"></a>Volgende stappen
* Zie voor een beschrijving van de secties in een Azure Resource Manager-sjabloon, [Authoring Azure Resource Manager-sjablonen](resource-group-authoring-templates.md).
* U kunt meerdere sjablonen samenvoegen, Zie [gekoppelde sjablonen gebruiken met Azure Resource Manager](resource-group-linked-templates.md).
* Op een opgegeven aantal keren herhalen bij het maken van een type resource, Zie [meerdere exemplaren van resources maken in Azure Resource Manager](resource-group-create-multiple.md).
* Zie voor meer informatie over het implementeren van de sjabloon die u hebt gemaakt, [een toepassing implementeren met Azure Resource Manager-sjabloon](resource-group-template-deploy.md).

