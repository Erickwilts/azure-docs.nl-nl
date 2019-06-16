---
title: Naslaggids voor Azure Import/Export-hulpprogramma import-taak opdrachten | Microsoft Docs
description: Naslaginformatie van Azure Import/Export-hulpprogramma voor importeren van veelgebruikte taak opdrachten.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
ms.subservice: common
ms.openlocfilehash: 7d21ab42188d5b8d6cb8c179a28d1b5bee822f05
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61478416"
---
# <a name="quick-reference-for-frequently-used-commands-for-import-jobs"></a>Snelzoekgids voor veelgebruikte opdrachten voor importtaken

In dit artikel biedt dat een snelle referentie voor een aantal veelgebruikte opdrachten. Zie voor gedetailleerde informatie over gebruik, [harde schijven voorbereiden voor een importtaak](../storage-import-export-tool-preparing-hard-drives-import.md).

## <a name="first-session"></a>Eerste sessie

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#1 /sk:************* /InitialDriveSet:driveset-1.csv /DataSet:dataset-1.csv /logdir:F:\logs
```

## <a name="second-session"></a>Tweede sessie

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2 /DataSet:dataset-2.csv
```

## <a name="abort-latest-session"></a>De meest recente sessie afbreken

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2 /AbortSession
```

## <a name="resume-latest-interrupted-session"></a>Meest recente onderbroken sessie hervatten

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#3 /ResumeSession
```

## <a name="add-drives-to-latest-session"></a>Stations toevoegen aan de meest recente sessie

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#3 /AdditionalDriveSet:driveset-2.csv
```

## <a name="next-steps"></a>Volgende stappen

* [Voorbeeldwerkstroom voor het voorbereiden van harde schijven voor een importtaak](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow.md)
