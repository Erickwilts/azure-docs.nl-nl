---
title: DevOps Toep assen met GitHub-LUIS
titleSuffix: Azure Cognitive Services
description: Pas DevOps toe met Language Understanding (LUIS) en GitHub.
services: cognitive-services
author: andycw
manager: cmayo
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 06/5/2020
ms.author: anwigley
ms.openlocfilehash: 6df65040277ac61cca5fb4bf7fce5b5a7b2f3afe
ms.sourcegitcommit: 52d2f06ecec82977a1463d54a9000a68ff26b572
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/15/2020
ms.locfileid: "84783759"
---
# <a name="apply-devops-to-luis-app-development-using-github-actions"></a>DevOps Toep assen op het ontwikkelen van LUIS-apps met behulp van GitHub-acties

Ga naar de [Luis DevOps-sjabloon opslag plaats](https://github.com/Azure-Samples/LUIS-DevOps-Template) voor een volledige oplossing waarmee de aanbevolen procedures voor DevOps en software techniek voor Luis worden geïmplementeerd. U kunt deze sjabloon opslag plaats gebruiken om uw eigen opslag plaats te maken met ingebouwde ondersteuning voor CI/CD-werk stromen en procedures voor het inschakelen van [broncode](luis-concept-devops-sourcecontrol.md)beheer, [geautomatiseerde builds](luis-concept-devops-automation.md), [testen](luis-concept-devops-testing.md)en [release beheer](luis-concept-devops-automation.md#release-management) met Luis voor uw eigen project.

## <a name="the-luis-devops-template-repo"></a>De LUIS DevOps-sjabloon opslag plaats

Met de [Luis DevOps-sjabloon opslag plaats](https://github.com/Azure-Samples/LUIS-DevOps-Template) wordt uitgelegd hoe u:

* **Kloon de sjabloon opslag plaats** -Kopieer de sjabloon naar uw eigen github-opslag plaats.
* **Luis-resources configureren** : Maak de [Luis-ontwerp-en Voorspellings resources in azure](https://docs.microsoft.com/azure/cognitive-services/luis/luis-how-to-azure-subscription#create-resources-in-azure-cli) die worden gebruikt door de doorlopende integratie werk stromen.
* **Configureer de CI/cd-werk stromen** -Configureer para meters voor de CI/cd-werk stromen en sla ze op in [github geheimen](https://help.github.com/actions/configuring-and-managing-workflows/creating-and-storing-encrypted-secrets).
* **Door loop de [' dev Inner loop '](https://mitchdenny.com/the-inner-loop/) ** : de ontwikkelaar maakt tijdens het werken in een ontwikkelings vertakking updates voor een voor beeld van een Luis-app, test de updates en genereert vervolgens een pull-aanvraag om wijzigingen voor te stellen en de goed keuring te controleren.
* **CI/cd-werk stromen uitvoeren** : Voer [doorlopende integratie werk stromen uit om een Luis-app te bouwen en te testen](luis-concept-devops-automation.md) met github-acties.
* **Automatische tests uitvoeren** : Voer [geautomatiseerde batch tests uit voor een Luis-app](luis-concept-devops-testing.md) om de kwaliteit van de app te evalueren.
* **De Luis-app implementeren** : Voer een [taak voor continue levering (cd)](luis-concept-devops-automation.md#continuous-delivery-cd) uit om de Luis-app te publiceren.
* **Gebruik de opslag plaats met uw eigen project** : hierin wordt uitgelegd hoe u de opslag plaats gebruikt met uw eigen Luis-toepassing.

## <a name="next-steps"></a>Volgende stappen

* Gebruik het [Luis DevOps-sjabloon opslag plaats](https://github.com/Azure-Samples/LUIS-DevOps-Template) om DevOps toe te passen op uw eigen project.
* [Broncode beheer en vertakkings strategieën voor LUIS](luis-concept-devops-sourcecontrol.md)
* [Testen op LUIS DevOps](luis-concept-devops-testing.md)
* [Automation-werk stromen voor LUIS DevOps](luis-concept-devops-automation.md)
