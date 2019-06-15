---
title: Web service logboekregistratie - Azure Machine Learning Studio | Microsoft Docs
description: Leer hoe u logboekregistratie inschakelen voor Machine Learning Studio-webservices. Logboekregistratie bevat aanvullende informatie over het oplossen van de API's.
services: machine-learning
documentationcenter: ''
author: xiaoharper
ms.custom: seodec18
ms.author: amlstudiodocs
editor: cgronlun
ms.assetid: c54d41e1-0300-46ef-bbfc-d6f7dca85086
ms.service: machine-learning
ms.subservice: studio
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/15/2017
ms.openlocfilehash: 727379edb60756ca8cb3e5ebdc29cd38858945e4
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60345641"
---
# <a name="enable-logging-for-azure-machine-learning-studio-web-services"></a>Logboekregistratie inschakelen voor Azure Machine Learning Studio-webservices
Dit document bevat informatie over de logboekregistratie van webservices voor Machine Learning Studio. Logboekregistratie bevat aanvullende informatie, dan alleen een foutnummer en een bericht, die kan helpen bij het oplossen van de aanroepen van de API's voor Machine Learning Studio.  

## <a name="how-to-enable-logging-for-a-web-service"></a>Het inschakelen van logboekregistratie voor een webservice

Inschakelen van logboekregistratie van het [Azure Machine Learning Studio-webservices](https://services.azureml.net) portal. 

1. Meld u aan bij de portal van Azure Machine Learning Studio-webservices op [ https://services.azureml.net ](https://services.azureml.net). Een klassieke webservice, u kunt ook aanschaffen bij de portal door te klikken op **nieuwe Web Serviceservaring** op de pagina Machine Learning Studio-webservices in Machine Learning Studio.

   ![Nieuwe Web Services-ervaring-koppeling](./media/web-services-logging/new-web-services-experience-link.png)

2. Klik op de bovenste menubalk **webservices** voor een nieuwe webservice, of klik op **klassieke webservices** voor een klassieke webservice.

   ![Selecteer Nieuw of klassieke webservices](./media/web-services-logging/select-web-service.png)

3. Klik op de naam van de webservice voor een nieuwe webservice. Voor een klassieke webservice, klikt u op de naam van de webservice en klik vervolgens op de volgende pagina op het juiste eindpunt.

4. Klik op de bovenste menubalk **configureren**.

5. Stel de **logboekregistratie inschakelen** optie naar *fout* (alleen om fouten te registreren) of *alle* (voor volledige logboekregistratie).

   ![Selecteer het logboekregistratieniveau van](./media/web-services-logging/enable-logging.png)

6. Klik op **Opslaan**.

7. Voor klassieke webservices, maakt u de **ml-diagnostics** container.

   Alle logboeken van de web service worden bewaard in een blobcontainer met de naam **ml-diagnostics** in het opslagaccount dat is gekoppeld aan de webservice. Deze container wordt de eerste keer dat u toegang de webservice tot gemaakt voor nieuwe webservices. Voor klassieke webservices moet u de container maken als deze nog niet bestaat. 

   1. In de [Azure-portal](https://portal.azure.com), gaat u naar het opslagaccount dat is gekoppeld aan de webservice.

   2. Klik onder **Blob Service** op **Containers**.

   3. Als de container **ml-diagnostics** niet bestaat, klikt u op **+ Container**, geef de container de naam 'ml-diagnostics' en selecteer de **toegangstype** als 'Blob'. Klik op **OK**.

      ![Maak een nieuwe container voor het opslaan van uw logboeken met diagnostische gegevens](./media/web-services-logging/create-ml-diagnostics-container.png)

> [!TIP]
>
> De Web Services-Dashboard in Machine Learning Studio heeft ook een switch logboekregistratie inschakelen voor een klassieke webservice. Aangezien logboekregistratie wordt nu beheerd via het Web Services-portal, moet u Schakel logboekregistratie in via de portal zoals wordt beschreven in dit artikel. Als u al ingeschakeld logboekregistratie in Studio, in de webportal Services logboekregistratie uitschakelen en weer inschakelen.


## <a name="the-effects-of-enabling-logging"></a>De gevolgen van logboekregistratie inschakelen
Als logboekregistratie is ingeschakeld, de diagnostische gegevens en fouten van de webservice-eindpunt worden vastgelegd in de **ml-diagnostics** blob-container in Azure Storage-Account is gekoppeld met de werkruimte van de gebruiker. Deze container bevat de diagnostische gegevens voor alle de web service-eindpunten voor alle werkruimten die zijn gekoppeld aan dit storage-account.

De logboeken kunnen worden weergegeven met behulp van een van de verschillende hulpprogramma's die beschikbaar zijn voor het verkennen van een Azure Storage-Account. De eenvoudigste manier kan worden te navigeren naar het opslagaccount in Azure portal, klikt u op **Containers**, en klik vervolgens op de container **ml-diagnostics**.  

## <a name="log-blob-detail-information"></a>Logboekgegevens voor blob
Elke blob in de container bevat de diagnostische gegevens voor exact één van de volgende acties:

* Een uitvoering van de Batch Execution-methode  
* Een uitvoering van de Request Response-methode  
* De initialisatie van een Request Response-container

De naam van elke blob heeft een voorvoegsel van de volgende notatie: 


`{Workspace Id}-{Web service Id}-{Endpoint Id}/{Log type}`


Waar _Logboektype_ is een van de volgende waarden:  

* batch  
* score/aanvragen  
* score/init  

