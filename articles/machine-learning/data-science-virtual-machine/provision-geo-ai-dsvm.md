---
title: Inrichten van een Geo kunstmatige intelligentie virtuele Machine op Azure - Azure | Microsoft Docs
description: Informatie over het maken en configureren van de Geo AI Data Science Virtual Machine. De Geo AI Data Science Virtual Machine biedt u de hulpmiddelen om AI en ML-oplossingen met behulp van geografische gegevens te maken.
keywords: deep learning, AI, hulpprogramma's voor data science, virtuele machine voor datatechnologie, georuimtelijke analyses
services: machine-learning
documentationcenter: ''
author: vijetajo
manager: cgronlun
ms.custom: seodec18
ms.assetid: ''
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 03/05/2018
ms.author: vijetaj
ms.openlocfilehash: 4772bf8341196485a91b3df30801b9714a4a64a8
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68591868"
---
# <a name="provision-a-geo-artificial-intelligence-virtual-machine-on-azure"></a>Een geografisch kunstmatige intelligentie-Machine in Azure inrichten 

De Geo AI Data Science Virtual Machine (Geo-DSVM) is een uitbreiding van de populaire [Azure Data Science Virtual Machine](https://aka.ms/dsvm) dat speciaal is geconfigureerd voor het combineren van AI en georuimtelijke analyses. De georuimtelijke analyses in de virtuele machine worden aangestuurd door [ArcGIS Pro](https://www.arcgis.com/features/index.html). De Data Science VM kan de snelle training van machine learning-modellen en zelfs van deep learning-modellen, op gegevens die is uitgebreid met geografische gegevens. Het wordt alleen ondersteund op Windows 2016 DSVM. 

De Geo-DSVM bevat verschillende hulpprogramma's voor AI met inbegrip van:

- GPU-versies van frameworks voor deep learning populaire, zoals Microsoft Cognitive Toolkit, TensorFlow, Keras, Caffe2, Chainer; 
- hulpprogramma's aan te schaffen en voorverwerking afbeelding tekstgegevens, 
- hulpprogramma's voor ontwikkelingsactiviteiten, zoals Microsoft R Server Developer Edition, Anaconda Python, Jupyter-notebooks voor Python en R IDE's voor Python / R, SQL databases
- Van ESRI ArcGIS Pro desktopsoftware samen met Python / R interfaces waarmee de georuimtelijke gegevens uit uw AI-toepassingen kunt werken. 
 

## <a name="create-your-geo-ai-data-science-vm"></a>Uw Geo AI Data Science VM maken

Hier volgt de procedure voor het maken van een exemplaar van de Geo AI Data Science VM: 


1. Navigeer naar de virtuele machine weergeven op [Azure-portal](https://ms.portal.azure.com/#create/microsoft-ads.geodsvmwindows).
2. Selecteer de **maken** onder in de wizard een moeten worden genomen.
![create-geo-ai-dsvm](./media/provision-geo-ai-dsvm/Create-Geo-AI.png)
3. De wizard die wordt gebruikt voor het maken van de Geo-DSVM vereist **invoer** voor elk van de **vier stappen** geïnventariseerd aan de rechterkant van de volgende afbeelding. Hier volgen de invoer die nodig zijn voor elk van deze stappen configureren:



   - **Basisinstellingen**

      1. **Naam**: De naam van de data Science-server die u maakt.

      2. **Gebruikersnaam**: Aanmeldings-id van beheerders account.

      3. **Wachtwoord**: Wacht woord voor beheerders account.

      4. **Abonnement**: Als u meer dan één abonnement hebt, selecteert u een waar de machine zich moet worden gemaakt en worden kosten in rekening gebracht.

      5. **Resourcegroep**: U kunt een nieuw item maken of een **lege** bestaande Azure-resource groep in uw abonnement gebruiken.

      6. **Locatie**: Selecteer het Data Center dat het meest geschikt is. Dit is meestal het datacenter dat de meeste van uw gegevens of zich het dichtst bij uw fysieke locatie voor de snelste toegang tot het netwerk. Als u doen, deep learning op GPU wilt, moet u een van de locaties in Azure met de NC-serie GPU VM-exemplaren. Momenteel zijn de locaties met GPU-Vm's: VS- **Oost, Noord-Centraal VS, Zuid-Centraal VS, VS-West 2, Europa-Noord, Europa-West**. Voor de meest recente lijst, Controleer de [Azure-producten per regio pagina](https://azure.microsoft.com/regions/services/) en zoek naar **NC-serie** onder **Compute**. 


   - **Instellingen voor**: Selecteer een van de grootte van de virtuele machine van de NC-Series GPU als u een diepe training wilt uitvoeren op GPU op uw Geo-DSVM. Anders kunt u een van de CPU op basis van exemplaar.  Maak een opslagaccount voor uw virtuele machine. 
   
   - **Samenvatting**: Controleer of alle informatie die u hebt ingevoerd juist is.

   - **Kopen**: Klik op **kopen** om het inrichten te starten. Een koppeling wordt met de voorwaarden van de service geleverd. De virtuele machine heeft geen eventuele extra kosten buiten de rekenkracht voor de servergrootte van de die u hebt gekozen in de **grootte** stap. 
 
>[!NOTE]
> De inrichting duurt ongeveer 20-30 minuten. De status van de inrichting wordt weergegeven op de Azure-portal.

 
## <a name="how-to-access-the-geo-ai-data-science-virtual-machine"></a>Toegang tot de Geo AI Data Science Virtual Machine

 Als uw virtuele machine is gemaakt, bent u klaar om te beginnen met behulp van de hulpprogramma's die zijn geïnstalleerd en geconfigureerd op het. Zijn er tegels van het menu start en pictogrammen op het bureaublad voor veel van de hulpprogramma's. U kunt Extern bureaublad in met behulp van de accountreferenties van de beheerder die u hebt geconfigureerd in de voorgaande **basisbeginselen** sectie. 

 
## <a name="using-arcgis-pro-installed-in-the-vm"></a>In de virtuele machine met behulp van ArcGIS Pro geïnstalleerd

De Geo-DSVM al ArcGIS Pro desktop vooraf zijn geïnstalleerd en de omgeving die vooraf geconfigureerd voor samenwerking met alle hulpmiddelen die in de DSVM. Bij het starten van ArcGIS wordt u gevraagd een aanmelding bij uw account ArcGIS. Als u al een ArcGIS-account hebt en licenties voor de software hebt, kunt u uw bestaande referenties gebruiken.  

![Boog-GIS-aanmelding](./media/provision-geo-ai-dsvm/ArcGISLogon.png)

Anders kunt u zich aanmelden voor nieuwe ArcGIS-account en -licentie of vraag een [gratis proefversie](https://www.arcgis.com/features/free-trial.html). 

![ArcGIS-Free-Trial](./media/provision-geo-ai-dsvm/ArcGIS-Free-Trial.png)

Nadat u zich hebt aangemeld voor een betaalde of gratis proef versie van ArcGIS, kunt u ArcGIS Pro voor uw account autoriseren door de instructies in aan de slag [met ArcGIS Pro](https://www.esri.com/library/brochures/getting-started-with-arcgis-pro.pdf)te volgen. 

Nadat u zich aanmeldt bij ArcGIS Pro desktop met uw account ArcGIS, bent u klaar om te beginnen met de data science-hulpprogramma's die zijn geïnstalleerd en geconfigureerd op de virtuele machine voor uw georuimtelijke analyses en machine learning-projecten.

## <a name="next-steps"></a>Volgende stappen

Aan de slag met behulp van de Geo AI Data Science VM met behulp van de volgende onderwerpen:

* [De Geo AI Data Science VM gebruiken](use-geo-ai-dsvm.md)
