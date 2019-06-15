---
title: Azure FileUpload UI-element | Microsoft Docs
description: Beschrijft de Microsoft.Common.FileUpload UI-element voor Azure-portal.
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: managed-applications
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/05/2018
ms.author: tomfitz
ms.openlocfilehash: 92a5f7c058904015cb22a239b7e7c4938ae1fdae
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61044639"
---
# <a name="microsoftcommonfileupload-ui-element"></a>Microsoft.Common.FileUpload UI element
Een besturingselement waarmee een gebruiker om op te geven van een of meer bestanden te uploaden.

## <a name="ui-sample"></a>Voorbeeld van de gebruikersinterface
![Microsoft.Common.FileUpload](./media/managed-application-elements/microsoft.common.fileupload.png)

## <a name="schema"></a>Schema
```json
{
  "name": "element1",
  "type": "Microsoft.Common.FileUpload",
  "label": "Some file upload",
  "toolTip": "",
  "constraints": {
    "required": true,
    "accept": ".doc,.docx,.xml,application/msword"
  },
  "options": {
    "multiple": false,
    "uploadMode": "file",
    "openMode": "text",
    "encoding": "UTF-8"
  },
  "visible": true
}
```

## <a name="remarks"></a>Opmerkingen
- `constraints.accept` Hiermee geeft u de typen bestanden die worden weergegeven in het dialoogvenster voor het bestand van de browser. Zie de [HTML5-specificatie](https://www.w3.org/TR/html5/forms.html#attr-input-accept) voor toegestane waarden. De standaardwaarde is **null**.
- Als `options.multiple` is ingesteld op **waar**, de gebruiker meer dan één bestand selecteren in het dialoogvenster voor het bestand van de browser is toegestaan. De standaardwaarde is **false**.
- Dit element biedt ondersteuning voor bestanden die worden geüpload in twee modi op basis van de waarde van `options.uploadMode`. Als **bestand** is opgegeven, wordt de uitvoer heeft de inhoud van het bestand op als een blob. Als **url** is opgegeven, wordt het bestand is geüpload naar een tijdelijke locatie en de uitvoer de URL van de blob bevat. Tijdelijke blobs opgeschoond na 24 uur. De standaardwaarde is **bestand**.
- Een geüploade bestand is beveiligd. De uitvoer-URL bevat een [SAS-token](../storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) voor toegang tot het bestand tijdens de implementatie.
- De waarde van `options.openMode` bepaalt hoe het bestand wordt gelezen. Als het bestand is naar verwachting uit tekst zonder opmaak, geef **tekst**; anders is, geef **binaire**. De standaardwaarde is **tekst**.
- Als `options.uploadMode` is ingesteld op **bestand** en `options.openMode` is ingesteld op **binaire**, de uitvoer is base64-gecodeerd.
- `options.encoding` Hiermee geeft u de codering als u wilt gebruiken bij het lezen van het bestand. De standaardwaarde is **UTF-8**, en wordt gebruikt alleen wanneer `options.openMode` is ingesteld op **tekst**.

## <a name="sample-output"></a>Voorbeelduitvoer
Als options.multiple ingesteld op false is en options.uploadMode bestand is, heeft de uitvoer de inhoud van het bestand als een JSON-tekenreeks:

```json
"Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua."
```

Als options.multiple ingesteld op true is and'options.uploadMode bestand is, wordt de uitvoer heeft de inhoud van de bestanden als een JSON-matrix:

```json
[
  "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.",
  "Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.",
  "Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.",
  "Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum."
]
```

Als options.multiple ingesteld op false is en options.uploadMode url is, heeft de uitvoer een URL als een JSON-tekenreeks:

```json
"https://myaccount.blob.core.windows.net/pictures/profile.jpg?sv=2013-08-15&st=2013-08-16&se=2013-08-17&sr=c&sp=r&rscd=file;%20attachment&rsct=binary &sig=YWJjZGVmZw%3d%3d&sig=a39%2BYozJhGp6miujGymjRpN8tsrQfLo9Z3i8IRyIpnQ%3d"
```

Als options.multiple ingesteld op true is en options.uploadMode url is, heeft de uitvoer een lijst met URL's als een JSON-matrix:
```json
[
  "https://myaccount.blob.core.windows.net/pictures/profile1.jpg?sv=2013-08-15&st=2013-08-16&se=2013-08-17&sr=c&sp=r&rscd=file;%20attachment&rsct=binary &sig=YWJjZGVmZw%3d%3d&sig=a39%2BYozJhGp6miujGymjRpN8tsrQfLo9Z3i8IRyIpnQ%3d",
  "https://myaccount.blob.core.windows.net/pictures/profile2.jpg?sv=2013-08-15&st=2013-08-16&se=2013-08-17&sr=c&sp=r&rscd=file;%20attachment&rsct=binary &sig=YWJjZGVmZw%3d%3d&sig=a39%2BYozJhGp6miujGymjRpN8tsrQfLo9Z3i8IRyIpnQ%3d",
  "https://myaccount.blob.core.windows.net/pictures/profile3.jpg?sv=2013-08-15&st=2013-08-16&se=2013-08-17&sr=c&sp=r&rscd=file;%20attachment&rsct=binary &sig=YWJjZGVmZw%3d%3d&sig=a39%2BYozJhGp6miujGymjRpN8tsrQfLo9Z3i8IRyIpnQ%3d"
]
```

Bij het testen van een CreateUiDefinition afkappen sommige browsers (zoals Google Chrome) URL's die worden gegenereerd door het element Microsoft.Common.FileUpload in de browserconsole. U moet mogelijk met de rechtermuisknop op afzonderlijke koppelingen als u wilt kopiëren van de volledige URL's.


## <a name="next-steps"></a>Volgende stappen
* Zie voor een inleiding tot het maken van definities van de gebruikersinterface, [aan de slag met CreateUiDefinition](create-uidefinition-overview.md).
* Zie voor een beschrijving van de algemene eigenschappen in de UI-elementen, [CreateUiDefinition elementen](create-uidefinition-elements.md).
