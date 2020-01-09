---
title: De activiteit van de pijp lijn uitvoeren in Azure Data Factory
description: Meer informatie over hoe u de activiteit pijp lijn uitvoeren kunt gebruiken om een Data Factory pijp lijn vanuit een andere Data Factory pijp lijn aan te roepen.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/10/2018
ms.openlocfilehash: 26dd7c4f33360030b13ddbfc1516396436724c40
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/25/2019
ms.locfileid: "75440446"
---
# <a name="execute-pipeline-activity-in-azure-data-factory"></a>De activiteit van de pijp lijn uitvoeren in Azure Data Factory
Met de activiteit voor het uitvoeren van de pijp lijn kan een Data Factory pijp lijn een andere pijp lijn aanroepen.

## <a name="syntax"></a>Syntaxis

```json
{
    "name": "MyPipeline",
    "properties": {
        "activities": [
            {
                "name": "ExecutePipelineActivity",
                "type": "ExecutePipeline",
                "typeProperties": {
                    "parameters": {                        
                        "mySourceDatasetFolderPath": {
                            "value": "@pipeline().parameters.mySourceDatasetFolderPath",
                            "type": "Expression"
                        }
                    },
                    "pipeline": {
                        "referenceName": "<InvokedPipelineName>",
                        "type": "PipelineReference"
                    },
                    "waitOnCompletion": true
                 }
            }
        ],
        "parameters": [
            {
                "mySourceDatasetFolderPath": {
                    "type": "String"
                }
            }
        ]
    }
}
```

## <a name="type-properties"></a>Type-eigenschappen

Eigenschap | Beschrijving | Toegestane waarden | Verplicht
-------- | ----------- | -------------- | --------
name | De naam van de activiteit voor het uitvoeren van de pijp lijn. | Tekenreeks | Ja
type | Moet worden ingesteld op: **ExecutePipeline**. | Tekenreeks | Ja
pijp lijn | Pijplijn verwijzing naar de afhankelijke pijp lijn die deze pijp lijn aanroept. Een pijplijn referentie object heeft twee eigenschappen: **referentie** naam en **type**. De eigenschap refernaam geeft de naam van de referentie pijplijn aan. De eigenschap type moet worden ingesteld op PipelineReference. | PipelineReference | Ja
parameters | De para meters die moeten worden door gegeven aan de aangeroepen pijp lijn | Een JSON-object waarmee parameter namen worden toegewezen aan argument waarden | Nee
waitOnCompletion | Hiermee definieert u of de uitvoering van de activiteit wacht totdat de afhankelijke uitvoering van de pijp lijn is voltooid. De standaardinstelling is onwaar. | Booleaans | Nee

## <a name="sample"></a>Voorbeeld
Dit scenario heeft twee pijp lijnen:

- **Hoofd pijplijn** : deze pijp lijn heeft één activiteit voor het uitvoeren van een pijp lijn die de aangeroepen pijp lijn aanroept. De hoofd pijplijn heeft twee para meters: `masterSourceBlobContainer`, `masterSinkBlobContainer`.
- **Aangeroepen pijp lijn** : deze pijp lijn heeft één Kopieer activiteit die gegevens kopieert van een Azure Blob-bron naar Azure Blob-sink. De aangeroepen pijp lijn heeft twee para meters: `sourceBlobContainer`, `sinkBlobContainer`.

### <a name="master-pipeline-definition"></a>Model pijplijn definitie

```json
{
  "name": "masterPipeline",
  "properties": {
    "activities": [
      {
        "type": "ExecutePipeline",
        "typeProperties": {
          "pipeline": {
            "referenceName": "invokedPipeline",
            "type": "PipelineReference"
          },
          "parameters": {
            "sourceBlobContainer": {
              "value": "@pipeline().parameters.masterSourceBlobContainer",
              "type": "Expression"
            },
            "sinkBlobContainer": {
              "value": "@pipeline().parameters.masterSinkBlobContainer",
              "type": "Expression"
            }
          },
          "waitOnCompletion": true
        },
        "name": "MyExecutePipelineActivity"
      }
    ],
    "parameters": {
      "masterSourceBlobContainer": {
        "type": "String"
      },
      "masterSinkBlobContainer": {
        "type": "String"
      }
    }
  }
}

```

### <a name="invoked-pipeline-definition"></a>De definitie van de pijp lijn is aangeroepen

```json
{
  "name": "invokedPipeline",
  "properties": {
    "activities": [
      {
        "type": "Copy",
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "BlobSink"
          }
        },
        "name": "CopyBlobtoBlob",
        "inputs": [
          {
            "referenceName": "SourceBlobDataset",
            "type": "DatasetReference"
          }
        ],
        "outputs": [
          {
            "referenceName": "sinkBlobDataset",
            "type": "DatasetReference"
          }
        ]
      }
    ],
    "parameters": {
      "sourceBlobContainer": {
        "type": "String"
      },
      "sinkBlobContainer": {
        "type": "String"
      }
    }
  }
}

```

**Gekoppelde service**

```json
{
    "name": "BlobStorageLinkedService",
    "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=*****;AccountKey=*****"
    }
  }
}
```

**Bron gegevensset**
```json
{
    "name": "SourceBlobDataset",
    "properties": {
    "type": "AzureBlob",
    "typeProperties": {
      "folderPath": {
        "value": "@pipeline().parameters.sourceBlobContainer",
        "type": "Expression"
      },
      "fileName": "salesforce.txt"
    },
    "linkedServiceName": {
      "referenceName": "BlobStorageLinkedService",
      "type": "LinkedServiceReference"
    }
  }
}
```

**Sink-gegevensset**
```json
{
    "name": "sinkBlobDataset",
    "properties": {
    "type": "AzureBlob",
    "typeProperties": {
      "folderPath": {
        "value": "@pipeline().parameters.sinkBlobContainer",
        "type": "Expression"
      }
    },
    "linkedServiceName": {
      "referenceName": "BlobStorageLinkedService",
      "type": "LinkedServiceReference"
    }
  }
}
```

### <a name="running-the-pipeline"></a>De pijp lijn uitvoeren

Als u de hoofd pijplijn in dit voor beeld wilt uitvoeren, worden de volgende waarden door gegeven voor de para meters masterSourceBlobContainer en masterSinkBlobContainer: 

```json
{
  "masterSourceBlobContainer": "executetest",
  "masterSinkBlobContainer": "executesink"
}
```

De hoofd pijplijn stuurt deze waarden naar de aangeroepen pijp lijn, zoals wordt weer gegeven in het volgende voor beeld: 

```json
{
    "type": "ExecutePipeline",
    "typeProperties": {
      "pipeline": {
        "referenceName": "invokedPipeline",
        "type": "PipelineReference"
      },
      "parameters": {
        "sourceBlobContainer": {
          "value": "@pipeline().parameters.masterSourceBlobContainer",
          "type": "Expression"
        },
        "sinkBlobContainer": {
          "value": "@pipeline().parameters.masterSinkBlobContainer",
          "type": "Expression"
        }
      },

      ....
}

```
## <a name="next-steps"></a>Volgende stappen
Zie andere controle stroom activiteiten die door Data Factory worden ondersteund: 

- [Voor elke activiteit](control-flow-for-each-activity.md)
- [Get Metadata Activity](control-flow-get-metadata-activity.md)
- [Lookup Activity](control-flow-lookup-activity.md)
- [Webactiviteit](control-flow-web-activity.md)
