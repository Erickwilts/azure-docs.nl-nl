---
title: Voorbeeldgegevens in Azure blob storage - Team Data Science Process
description: Steekproef nemen voor gegevens die zijn opgeslagen in Azure blob-opslag programmatisch downloaden en vervolgens sampling met behulp van de procedures die zijn geschreven in Python.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/13/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 1c455106e5faa4aa20ec56f37788e0b8c324fee1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61042905"
---
# <a name="heading"></a>Voorbeeldgegevens in Azure blob-opslag

In dit artikel bevat informatie over steekproef nemen voor gegevens die zijn opgeslagen in Azure blob-opslag programmatisch downloaden en vervolgens sampling met behulp van de procedures die zijn geschreven in Python.

**Waarom sample van uw gegevens?**
Als de gegevensset die u van plan bent om te analyseren groot is, is het doorgaans een goed idee om down-sampling van de gegevens om deze aan de grootte van een kleiner, maar representatieve en gemakkelijker. Dit vereenvoudigt het begrijpen van gegevens, verkennen en feature-engineering. De rol in het Cortana Analytics-proces is om in te schakelen, snel ontwikkelen van prototypen van de functies voor het verwerken van gegevens en machine learning-modellen.

Deze taak steekproeven is een stap in de [Team Data Science Process (TDSP)](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/).

## <a name="download-and-down-sample-data"></a>Download en down-sampling van gegevens
1. De gegevens uit Azure blob-opslag met behulp van de blobservice van de volgende voorbeeldcode voor Python downloaden: 
   
        from azure.storage.blob import BlobService
        import tables
   
        STORAGEACCOUNTNAME= <storage_account_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        LOCALFILENAME= <local_file_name>        
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>
   
        #download from blob
        t1=time.time()
        blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
        blob_service.get_blob_to_path(CONTAINERNAME,BLOBNAME,LOCALFILENAME)
        t2=time.time()
        print(("It takes %s seconds to download "+blobname) % (t2 - t1))

2. Lezen van gegevens in een Pandas gegevensframe uit het bestand gedownload hierboven.
   
        import pandas as pd
   
        #directly ready from file on disk
        dataframe_blobdata = pd.read_csv(LOCALFILE)

3. Down-sampling van de gegevens met de `numpy`van `random.choice` als volgt:
   
        # A 1 percent sample
        sample_ratio = 0.01 
        sample_size = np.round(dataframe_blobdata.shape[0] * sample_ratio)
        sample_rows = np.random.choice(dataframe_blobdata.index.values, sample_size)
        dataframe_blobdata_sample = dataframe_blobdata.ix[sample_rows]

Nu kunt u werken met de bovenstaande gegevensframe met het voorbeeld 1 procent voor verdere exploratie en genereren van de functie.

## <a name="heading"></a>Gegevens uploaden en te lezen in Azure Machine Learning
U kunt de volgende voorbeeldcode down-sampling van de gegevens en deze rechtstreeks in Azure Machine Learning te gebruiken:

1. Het gegevensframe schrijven naar een lokaal bestand
   
        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. Het lokale bestand uploaden naar een Azure-blob met behulp van de volgende voorbeeldcode:
   
        from azure.storage.blob import BlobService
        import tables
   
        STORAGEACCOUNTNAME= <storage_account_name>
        LOCALFILENAME= <local_file_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>
   
        output_blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)    
        localfileprocessed = os.path.join(os.getcwd(),LOCALFILENAME) #assuming file is in current working directory
   
        try:
   
        #perform upload
        output_blob_service.put_block_blob_from_path(CONTAINERNAME,BLOBNAME,localfileprocessed)
   
        except:            
            print ("Something went wrong with uploading to the blob:"+ BLOBNAME)

3. De gegevens lezen van de Azure-blob met Azure Machine Learning [importgegevens](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) zoals wordt weergegeven in de onderstaande afbeelding:

![lezer-blob](./media/sample-data-blob/reader_blob.png)

