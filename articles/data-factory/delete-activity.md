---
title: Activiteit in Azure Data Factory verwijderen | Microsoft Docs
description: Meer informatie over het verwijderen van bestanden in verschillende bestands archieven met de activiteit verwijderen in Azure Data Factory.
services: data-factory
documentationcenter: ''
author: dearandyxu
ms.author: yexu
ms.reviewer: douglasl
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/25/2019
ms.openlocfilehash: 606cab09debf760d1b101390b2a19a1a090bb4c3
ms.sourcegitcommit: b2db98f55785ff920140f117bfc01f1177c7f7e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/16/2019
ms.locfileid: "68234567"
---
# <a name="delete-activity-in-azure-data-factory"></a>Activiteit in Azure Data Factory verwijderen

U kunt de activiteit verwijderen in Azure Data Factory gebruiken om bestanden of mappen te verwijderen uit on-premises opslag archieven of opslag ruimten in de Cloud. Gebruik deze activiteit om bestanden op te schonen of te archiveren wanneer deze niet meer nodig zijn.

> [!WARNING]
> Verwijderde bestanden of mappen kunnen niet worden hersteld. Wees voorzichtig wanneer u de Delete-activiteit gebruikt voor het verwijderen van bestanden of mappen.

## <a name="best-practices"></a>Aanbevolen procedures

Hier volgen enkele aanbevelingen voor het gebruik van de activiteit verwijderen:

-   Maak een back-up van uw bestanden voordat u ze verwijdert met de Delete-activiteit voor het geval u ze in de toekomst moet herstellen.

-   Zorg ervoor dat Data Factory schrijf machtigingen heeft om mappen of bestanden uit het opslag archief te verwijderen.

-   Zorg ervoor dat u geen bestanden verwijdert die tegelijkertijd worden geschreven. 

-   Als u bestanden of mappen van een on-premises systeem wilt verwijderen, moet u ervoor zorgen dat u een zelf-hostende Integration runtime gebruikt met een versie die hoger is dan 3,14.

## <a name="supported-data-stores"></a>Ondersteunde gegevens archieven

-   [Azure Blob Storage](connector-azure-blob-storage.md)
-   [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md)
-   [Azure Data Lake Storage Gen2](connector-azure-data-lake-storage.md)

### <a name="file-system-data-stores"></a>Gegevens archieven van bestands systeem

-   [Bestandssysteem](connector-file-system.md)
-   [FTP](connector-ftp.md)
-   [SFTP](connector-sftp.md)
-   [Amazon S3](connector-amazon-simple-storage-service.md)

## <a name="syntax"></a>Syntaxis

```json
{
    "name": "DeleteActivity",
    "type": "Delete",
    "typeProperties": {
        "dataset": {
            "referenceName": "<dataset name>",
            "type": "DatasetReference"
        },
        "recursive": true/false,
        "maxConcurrentConnections": <number>,
        "enableLogging": true/false,
        "logStorageSettings": {
            "linkedServiceName": {
                "referenceName": "<name of linked service>",
                "type": "LinkedServiceReference"
            },
            "path": "<path to save log file>"
        }
    }
}
```

## <a name="type-properties"></a>Type-eigenschappen

| Eigenschap | Description | Verplicht |
| --- | --- | --- |
| sets | Bevat de verwijzing naar de gegevensset om te bepalen welke bestanden of mappen moeten worden verwijderd | Ja |
| recursive | Hiermee wordt aangegeven of de bestanden recursief uit de submappen of alleen uit de opgegeven map worden verwijderd.  | Nee. De standaardwaarde is `false`. |
| maxConcurrentConnections | Het aantal verbindingen dat gelijktijdig verbinding maakt met opslag Archief voor het verwijderen van mappen of bestanden.   |  Nee. De standaardwaarde is `1`. |
| enablelogging | Hiermee wordt aangegeven of u de naam van de map of het bestand dat u hebt verwijderd, moet vastleggen. Als dit het geval is, moet u een opslag account verder opgeven om het logboek bestand op te slaan, zodat u het gedrag van de activiteit verwijderen kunt volgen door het logboek bestand te lezen. | Nee |
| logStorageSettings | Alleen van toepassing als EnableLogging = True.<br/><br/>Een groep opslag eigenschappen die kunnen worden opgegeven waar u het logboek bestand wilt opslaan met de map of de bestands namen die zijn verwijderd door de Delete-activiteit. | Nee |
| linkedServiceName | Alleen van toepassing als EnableLogging = True.<br/><br/>De gekoppelde service van [Azure Storage](connector-azure-blob-storage.md#linked-service-properties), [Azure data Lake Storage gen1](connector-azure-data-lake-store.md#linked-service-properties)of [Azure data Lake Storage Gen2](connector-azure-data-lake-storage.md#linked-service-properties) voor het opslaan van het logboek bestand met de map-of bestands namen die zijn verwijderd door de Delete-activiteit. | Nee |
| path | Alleen van toepassing als EnableLogging = True.<br/><br/>Het pad voor het opslaan van het logboek bestand in uw opslag account. Als u geen pad opgeeft, maakt de service een container voor u. | Nee |

## <a name="monitoring"></a>Bewaking

Er zijn twee locaties waar u de resultaten van de Delete-activiteit kunt zien en bewaken: 
-   Uit de uitvoer van de Delete-activiteit.
-   Vanuit het logboek bestand.

### <a name="sample-output-of-the-delete-activity"></a>Voorbeeld uitvoer van de Delete-activiteit

```json
{ 
  "datasetName": "AmazonS3",
  "type": "AmazonS3Object",
  "prefix": "test",
  "bucketName": "adf",
  "recursive": true,
  "isWildcardUsed": false,
  "maxConcurrentConnections": 2,  
  "filesDeleted": 4,
  "logPath": "https://sample.blob.core.windows.net/mycontainer/5c698705-a6e2-40bf-911e-e0a927de3f07",
  "effectiveIntegrationRuntime": "MyAzureIR (West Central US)",
  "executionDuration": 650
}
```

### <a name="sample-log-file-of-the-delete-activity"></a>Voorbeeld logboek bestand van de activiteit verwijderen

| Name | Categorie | Status | Fout |
|:--- |:--- |:--- |:--- |
| test1/yyy.json | File | Verwijderen |  |
| test2/hello789.txt | File | Verwijderen |  |
| test2/test3/hello000.txt | File | Verwijderen |  |
| test2/test3/zzz.json | File | Verwijderen |  |

## <a name="examples-of-using-the-delete-activity"></a>Voor beelden van het gebruik van de activiteit verwijderen

### <a name="delete-specific-folders-or-files"></a>Specifieke mappen of bestanden verwijderen

Het archief heeft de volgende mapstructuur:

Basis<br/>&nbsp;&nbsp;&nbsp;&nbsp;Folder_A_1/<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;Folder_A_2/<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;5.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Folder_B_1/<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;6.txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;7.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Folder_B_2/<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;8.txt

Nu gebruikt u de activiteit verwijderen om map of bestanden te verwijderen met de combi natie van verschillende eigenschaps waarden uit de gegevensset en de activiteit verwijderen:

| folderPath (uit gegevensset) | Bestands naam (van gegevensset) | recursief (vanuit de activiteit verwijderen) | Output |
|:--- |:--- |:--- |:--- |
| Hoofdmap-Folder_A_2 | NULL | False | Basis<br/>&nbsp;&nbsp;&nbsp;&nbsp;Folder_A_1/<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;Folder_A_2/<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>4.txt</strike><br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>5.csv</strike><br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Folder_B_1/<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;6.txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;7.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Folder_B_2/<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;8.txt |
| Hoofdmap-Folder_A_2 | NULL | Waar | Basis<br/>&nbsp;&nbsp;&nbsp;&nbsp;Folder_A_1/<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;<strike>Folder_A_2/</strike><br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>4.txt</strike><br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>5.csv</strike><br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>Folder_B_1/</strike><br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>6.txt</strike><br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>7.csv</strike><br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>Folder_B_2/</strike><br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>8.txt</strike> |
| Hoofdmap-Folder_A_2 | *. txt | False | Basis<br/>&nbsp;&nbsp;&nbsp;&nbsp;Folder_A_1/<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;Folder_A_2/<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>4.txt</strike><br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;5.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Folder_B_1/<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;6.txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;7.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Folder_B_2/<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;8.txt |
| Hoofdmap-Folder_A_2 | *. txt | Waar | Basis<br/>&nbsp;&nbsp;&nbsp;&nbsp;Folder_A_1/<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;Folder_A_2/<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>4.txt</strike><br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;5.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Folder_B_1/<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>6.txt</strike><br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;7.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Folder_B_2/<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>8.txt</strike> |

### <a name="periodically-clean-up-the-time-partitioned-folder-or-files"></a>Regel matig de gepartitioneerde map of bestanden opschonen

U kunt een pijp lijn maken voor het periodiek opschonen van de gepartitioneerde map of bestanden.  De mapstructuur is bijvoorbeeld vergelijkbaar met: `/mycontainer/2018/12/14/*.csv`.  U kunt de systeem variabele ADF van schema gebruiken om te bepalen welke map of bestanden moeten worden verwijderd in elke pijplijn uitvoering. 

#### <a name="sample-pipeline"></a>Voorbeeld pijplijn

```json
{
    "name": "cleanup_time_partitioned_folder",
    "properties": {
        "activities": [
            {
                "name": "DeleteOneFolder",
                "type": "Delete",
                "policy": {
                    "timeout": "7.00:00:00",
                    "retry": 0,
                    "retryIntervalInSeconds": 30,
                    "secureOutput": false,
                    "secureInput": false
                },
                "typeProperties": {
                    "dataset": {
                        "referenceName": "PartitionedFolder",
                        "type": "DatasetReference",
                        "parameters": {
                            "TriggerTime": {
                                "value": "@formatDateTime(pipeline().parameters.TriggerTime, 'yyyy/MM/dd')",
                                "type": "Expression"
                            }
                        }
                    },
                    "recursive": true,
                    "logStorageSettings": {
                        "linkedServiceName": {
                            "referenceName": "BloblinkedService",
                            "type": "LinkedServiceReference"
                        },
                        "path": "mycontainer/log"
                    },
                    "enableLogging": true
                }
            }
        ],
        "parameters": {
            "TriggerTime": {
                "type": "String"
            }
        }
    },
    "type": "Microsoft.DataFactory/factories/pipelines"
}
```

#### <a name="sample-dataset"></a>Voor beeld-gegevensset

```json
{
    "name": "PartitionedFolder",
    "properties": {
        "linkedServiceName": {
            "referenceName": "BloblinkedService",
            "type": "LinkedServiceReference"
        },
        "parameters": {
            "TriggerTime": {
                "type": "String"
            }
        },
        "type": "AzureBlob",
        "typeProperties": {
            "folderPath": {
                "value": "@concat('mycontainer/',dataset().TriggerTime)",
                "type": "Expression"
            }
        }
    },
    "type": "Microsoft.DataFactory/factories/datasets"
}
```

#### <a name="sample-trigger"></a>Voorbeeld trigger

```json
{
    "name": "DailyTrigger",
    "properties": {
        "runtimeState": "Started",
        "pipelines": [
            {
                "pipelineReference": {
                    "referenceName": "cleanup_time_partitioned_folder",
                    "type": "PipelineReference"
                },
                "parameters": {
                    "TriggerTime": "@trigger().scheduledTime"
                }
            }
        ],
        "type": "ScheduleTrigger",
        "typeProperties": {
            "recurrence": {
                "frequency": "Day",
                "interval": 1,
                "startTime": "2018-12-13T00:00:00.000Z",
                "timeZone": "UTC",
                "schedule": {
                    "minutes": [
                        59
                    ],
                    "hours": [
                        23
                    ]
                }
            }
        }
    }
}
```

### <a name="clean-up-the-expired-files-that-were-last-modified-before-201811"></a>De verlopen bestanden opschonen die voor het laatst zijn gewijzigd voordat 2018.1.1

U kunt een pijp lijn maken voor het opschonen van de oude of verlopen bestanden met behulp van bestands kenmerk filter: ' LastModified ' in DataSet.  

#### <a name="sample-pipeline"></a>Voorbeeld pijplijn

```json
{
    "name": "CleanupExpiredFiles",
    "properties": {
        "activities": [
            {
                "name": "DeleteFilebyLastModified",
                "type": "Delete",
                "policy": {
                    "timeout": "7.00:00:00",
                    "retry": 0,
                    "retryIntervalInSeconds": 30,
                    "secureOutput": false,
                    "secureInput": false
                },
                "typeProperties": {
                    "dataset": {
                        "referenceName": "BlobFilesLastModifiedBefore201811",
                        "type": "DatasetReference"
                    },
                    "recursive": true,
                    "logStorageSettings": {
                        "linkedServiceName": {
                            "referenceName": "BloblinkedService",
                            "type": "LinkedServiceReference"
                        },
                        "path": "mycontainer/log"
                    },
                    "enableLogging": true
                }
            }
        ]
    }
}
```

#### <a name="sample-dataset"></a>Voor beeld-gegevensset

```json
{
    "name": "BlobFilesLastModifiedBefore201811",
    "properties": {
        "linkedServiceName": {
            "referenceName": "BloblinkedService",
            "type": "LinkedServiceReference"
        },
        "type": "AzureBlob",
        "typeProperties": {
            "fileName": "*",
            "folderPath": "mycontainer",
            "modifiedDatetimeEnd": "2018-01-01T00:00:00.000Z"
        }
    }
}
```

### <a name="move-files-by-chaining-the-copy-activity-and-the-delete-activity"></a>Verplaats bestanden door de Kopieer activiteit en de activiteit verwijderen te koppelen

U kunt een bestand verplaatsen met behulp van een Kopieer activiteit om een bestand te kopiëren en vervolgens een Verwijder activiteit om een bestand in een pijp lijn te verwijderen.  Als u meerdere bestanden wilt verplaatsen, kunt u de activiteit GetMetadata activiteit + activiteit + foreach activity + Copy activiteit + Delete gebruiken, zoals in het volgende voor beeld:

> [!NOTE]
> Als u de volledige map wilt verplaatsen door een gegevensset met alleen een mappad te definiëren en vervolgens een Kopieer activiteit en een delete-activiteit wilt gebruiken om te verwijzen naar dezelfde gegevensset die een map vertegenwoordigt, moet u zeer voorzichtig zijn. Dit komt omdat u ervoor moet zorgen dat er geen nieuwe bestanden in de map arriveren tussen het kopiëren en het verwijderen van de bewerking.  Als er nieuwe bestanden in de map arriveren op het moment dat de Kopieer activiteit de Kopieer taak heeft voltooid, maar de Delete-activiteit niet is gesterd, is het mogelijk dat het nieuwe binnenkomende bestand dat niet is gekopieerd naar de destinati wordt verwijderd met de Delete-activiteit. Als u de volledige map wilt verwijderen. 

#### <a name="sample-pipeline"></a>Voorbeeld pijplijn

```json
{
    "name": "MoveFiles",
    "properties": {
        "activities": [
            {
                "name": "GetFileList",
                "type": "GetMetadata",
                "typeProperties": {
                    "dataset": {
                        "referenceName": "OneSourceFolder",
                        "type": "DatasetReference"
                    },
                    "fieldList": [
                        "childItems"
                    ]
                }
            },
            {
                "name": "FilterFiles",
                "type": "Filter",
                "dependsOn": [
                    {
                        "activity": "GetFileList",
                        "dependencyConditions": [
                            "Succeeded"
                        ]
                    }
                ],
                "typeProperties": {
                    "items": {
                        "value": "@activity('GetFileList').output.childItems",
                        "type": "Expression"
                    },
                    "condition": {
                        "value": "@equals(item().type, 'File')",
                        "type": "Expression"
                    }
                }
            },
            {
                "name": "ForEachFile",
                "type": "ForEach",
                "dependsOn": [
                    {
                        "activity": "FilterFiles",
                        "dependencyConditions": [
                            "Succeeded"
                        ]
                    }
                ],
                "typeProperties": {
                    "items": {
                        "value": "@activity('FilterFiles').output.value",
                        "type": "Expression"
                    },
                    "batchCount": 20,
                    "activities": [
                        {
                            "name": "CopyAFile",
                            "type": "Copy",
                            "policy": {
                                "timeout": "7.00:00:00",
                                "retry": 0,
                                "retryIntervalInSeconds": 30,
                                "secureOutput": false,
                                "secureInput": false
                            },
                            "typeProperties": {
                                "source": {
                                    "type": "BlobSource",
                                    "recursive": false
                                },
                                "sink": {
                                    "type": "BlobSink"
                                },
                                "enableStaging": false,
                                "dataIntegrationUnits": 0
                            },
                            "inputs": [
                                {
                                    "referenceName": "OneSourceFile",
                                    "type": "DatasetReference",
                                    "parameters": {
                                        "path": "myFolder",
                                        "filename": {
                                            "value": "@item().name",
                                            "type": "Expression"
                                        }
                                    }
                                }
                            ],
                            "outputs": [
                                {
                                    "referenceName": "OneDestinationFile",
                                    "type": "DatasetReference",
                                    "parameters": {
                                        "DestinationFileName": {
                                            "value": "@item().name",
                                            "type": "Expression"
                                        }
                                    }
                                }
                            ]
                        },
                        {
                            "name": "DeleteAFile",
                            "type": "Delete",
                            "dependsOn": [
                                {
                                    "activity": "CopyAFile",
                                    "dependencyConditions": [
                                        "Succeeded"
                                    ]
                                }
                            ],
                            "policy": {
                                "timeout": "7.00:00:00",
                                "retry": 0,
                                "retryIntervalInSeconds": 30,
                                "secureOutput": false,
                                "secureInput": false
                            },
                            "typeProperties": {
                                "dataset": {
                                    "referenceName": "OneSourceFile",
                                    "type": "DatasetReference",
                                    "parameters": {
                                        "path": "myFolder",
                                        "filename": {
                                            "value": "@item().name",
                                            "type": "Expression"
                                        }
                                    }
                                },
                                "logStorageSettings": {
                                    "linkedServiceName": {
                                        "referenceName": "BloblinkedService",
                                        "type": "LinkedServiceReference"
                                    },
                                    "path": "Container/log"
                                },
                                "enableLogging": true
                            }
                        }
                    ]
                }
            }
        ]
    }
}
```

#### <a name="sample-datasets"></a>Voorbeeldgegevenssets

De gegevensset die wordt gebruikt door de GetMetadata-activiteit voor het inventariseren van de bestanden lijst.

```json
{
    "name": "OneSourceFolder",
    "properties": {
        "linkedServiceName": {
            "referenceName": "AzureStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "type": "AzureBlob",
        "typeProperties": {
            "fileName": "",
            "folderPath": "myFolder"
        }
    }
}
```

Gegevensset voor gegevens bron die wordt gebruikt door de Kopieer activiteit en de Delete-activiteit.

```json
{
    "name": "OneSourceFile",
    "properties": {
        "linkedServiceName": {
            "referenceName": "AzureStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "parameters": {
            "path": {
                "type": "String"
            },
            "filename": {
                "type": "String"
            }
        },
        "type": "AzureBlob",
        "typeProperties": {
            "fileName": {
                "value": "@dataset().filename",
                "type": "Expression"
            },
            "folderPath": {
                "value": "@{dataset().path}",
                "type": "Expression"
            }
        }
    }
}
```

Gegevensset voor de gegevens bestemming die wordt gebruikt door de Kopieer activiteit.

```json
{
    "name": "OneDestinationFile",
    "properties": {
        "linkedServiceName": {
            "referenceName": "AzureStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "parameters": {
            "DestinationFileName": {
                "type": "String"
            }
        },
        "type": "AzureBlob",
        "typeProperties": {
            "fileName": {
                "value": "@dataset().DestinationFileName",
                "type": "Expression"
            },
            "folderPath": "mycontainer/dest"
        }
    }
}
```

U kunt ook de sjabloon ophalen om bestanden van [hier](solution-template-move-files.md)te verplaatsen.

## <a name="known-limitation"></a>Bekende beperking

-   Delete-activiteit biedt geen ondersteuning voor het verwijderen van een lijst met mappen die worden beschreven door een Joker teken.

-   Wanneer u bestands kenmerk filter: modifiedDatetimeStart en modifiedDatetimeEnd gebruikt om bestanden te selecteren die moeten worden verwijderd, moet u "bestands naam": "*" in gegevensset instellen.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het verplaatsen van bestanden in Azure Data Factory.

-   [Hulpprogramma voor gegevens kopiëren in Azure Data Factory](copy-data-tool.md)