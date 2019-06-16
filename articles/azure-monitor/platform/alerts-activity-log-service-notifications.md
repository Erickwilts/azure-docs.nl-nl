---
title: Waarschuwingen voor activiteitenlogboeken van Azure-service-meldingen ontvangen
description: Ontvang een melding via SMS, e-mail of webhook als Azure-service optreedt.
author: shawntabrizi
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/09/2018
ms.author: shtabriz
ms.openlocfilehash: 3157cef6952f01d689d3cb4a30c9d7c843095809
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67071701"
---
# <a name="create-activity-log-alerts-on-service-notifications"></a>Waarschuwingen voor activiteitenlogboek maken voor servicemeldingen
## <a name="overview"></a>Overzicht
In dit artikel leest u hoe u waarschuwingen voor activiteitenlogboeken voor servicestatusmeldingen instelt met behulp van de Azure-portal.  

U kunt een waarschuwing ontvangt als Azure service health meldingen naar uw Azure-abonnement verzendt. U kunt de waarschuwing op basis van configureren:

- De klasse van de melding van de health service (serviceproblemen, gepland onderhoud, statusadvies).
- Het abonnement dat is beïnvloed.
- De (s) dat is beïnvloed.
- De regio('s) beïnvloed.

> [!NOTE]
> Servicestatusmeldingen verzendt een waarschuwing met betrekking tot resource health-gebeurtenissen.

Ook kunt u configureren die de waarschuwing moet worden verzonden naar:

- Selecteer een bestaande actiegroep.
- Maak een nieuwe actiegroep (die kan worden gebruikt voor toekomstige waarschuwingen).

Raadpleeg [Actiegroepen maken en beheren](../../azure-monitor/platform/action-groups.md) voor meer informatie over actiegroepen.

Zie voor meer informatie over het configureren van de statusmeldingen notification service met behulp van Azure Resource Manager-sjablonen [Resource Manager-sjablonen](alerts-activity-log.md).

### <a name="watch-a-video-on-setting-up-your-first-azure-service-health-alert"></a>Bekijk een video over het instellen van uw eerste Azure Service Health-waarschuwing

>[!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2OaXt]

## <a name="alert-and-new-action-group-using-azure-portal"></a>Waarschuwing en nieuwe actiegroep met behulp van Azure portal
1. In de [portal](https://portal.azure.com), selecteer **servicestatus**.

    ![De "Service" statusservice](media/alerts-activity-log-service-notifications/home-servicehealth.png)

1. In de **waarschuwingen** sectie, selecteer **statusmeldingen**.

    ![Het tabblad 'Waarschuwingen voor Health'](media/alerts-activity-log-service-notifications/alerts-blades-sh.png)

1. Selecteer **statuswaarschuwing voor de service maken** en vul de velden in.

    ![De opdracht 'Service-statusmelding maken'](media/alerts-activity-log-service-notifications/service-health-alert.png)

1. Selecteer de **abonnement**, **Services**, en **regio's** u wilt worden gewaarschuwd voor.

    ![In het dialoogvenster 'Waarschuwing voor activiteitenlogboek toevoegen'](media/alerts-activity-log-service-notifications/activity-log-alert-new-ux.png)

> [!NOTE]
> Dit abonnement wordt gebruikt voor het opslaan van de waarschuwing voor activiteitenlogboek. De waarschuwing resource wordt geïmplementeerd voor dit abonnement en gebeurtenissen in het activiteitenlogboek voor bewaakt.

1. Kies de **gebeurtenistypen** u wilt worden gewaarschuwd voor: *Service-probleem*, *gepland onderhoud*, en *statusadvies* 

1. De Waarschuwingsdetails definiëren door te voeren een **naam waarschuwingsregel** en **beschrijving**.

1. Selecteer de **resourcegroep** waar u de waarschuwing moet worden opgeslagen.

1. Een nieuwe actiegroep maken door te selecteren **nieuwe actiegroep**. Voer een naam in de **naam van de actiegroep** in en voert u een naam in de **afkorting** vak. De korte naam wordt verwezen in de meldingen die worden verzonden wanneer deze waarschuwing wordt geactiveerd.

    ![Een nieuwe actiegroep maken](media/alerts-activity-log-service-notifications/action-group-creation.png)

1. Een lijst met ontvangers definiëren door te geven van de ontvanger:

    a. **Naam**: Voer de naam, alias of id van de ontvanger.

    b. **Actietype**: Selecteer SMS, e-mail, webhook, Azure-app en meer.

    c. **Details**: Op basis van het actietype is gekozen, voer een telefoonnummer, e-mailadres, webhook URI, enzovoort.

1. Selecteer **OK** de actiegroep maken en vervolgens **waarschuwingsregel maken** om uit te voeren van de waarschuwing.

Binnen een paar minuten, de waarschuwing is actief en begint met activeren op basis van de voorwaarden die u hebt opgegeven tijdens het maken van.

Meer informatie over het [configureren van de webhook-meldingen voor bestaande beheersystemen voor probleem](../../service-health/service-health-alert-webhook-guide.md). Zie voor meer informatie over de webhook-schema voor waarschuwingen voor activiteitenlogboeken [Webhooks voor Azure-activiteit waarschuwingen voor activiteitenlogboeken](../../azure-monitor/platform/activity-log-alerts-webhook.md).

>[!NOTE]
>De actiegroep die is gedefinieerd in deze stappen is herbruikbare als een bestaande actiegroep voor alle toekomstige definities van de waarschuwing.
>
>

## <a name="alert-with-existing-action-group-using-azure-portal"></a>Waarschuwing met bestaande actiegroep met behulp van Azure portal

1. Volg de stappen 1 t/m 6 in de vorige sectie om te maken van de melding van de health service. 

1. Onder **actiegroep definiëren**, klikt u op de **actie selecteren groep** knop. Selecteer de juiste actie.

1. Selecteer **toevoegen** om toe te voegen van de actiegroep en vervolgens **waarschuwingsregel maken** om uit te voeren van de waarschuwing.

Binnen een paar minuten, de waarschuwing is actief en begint met activeren op basis van de voorwaarden die u hebt opgegeven tijdens het maken van.

## <a name="alert-and-new-action-group-using-the-azure-resource-manager-templates"></a>Waarschuwing en nieuwe actiegroep met behulp van de Azure Resource Manager-sjablonen

Hier volgt een voorbeeld waarin een actiegroep die u maakt met een e-doel en kan alle servicestatusmeldingen voor het doelabonnement.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "actionGroups_name": {
            "defaultValue": "SubHealth",
            "type": "String"
        },
        "activityLogAlerts_name": {
            "defaultValue": "ServiceHealthActivityLogAlert",
            "type": "String"
        },
        "emailAddress":{
            "type":"string"
        }
    },
    "variables": {
        "alertScope":"[concat('/','subscriptions','/',subscription().subscriptionId)]"
    },
    "resources": [
        {
            "comments": "Action Group",
            "type": "microsoft.insights/actionGroups",
            "name": "[parameters('actionGroups_name')]",
            "apiVersion": "2017-04-01",
            "location": "Global",
            "tags": {},
            "scale": null,
            "properties": {
                "groupShortName": "[parameters('actionGroups_name')]",
                "enabled": true,
                "emailReceivers": [
                    {
                        "name": "[parameters('actionGroups_name')]",
                        "emailAddress": "[parameters('emailAddress')]"
                    }
                ],
                "smsReceivers": [],
                "webhookReceivers": []
            },
            "dependsOn": []
        },
        {
            "comments": "Service Health Activity Log Alert",
            "type": "microsoft.insights/activityLogAlerts",
            "name": "[parameters('activityLogAlerts_name')]",
            "apiVersion": "2017-04-01",
            "location": "Global",
            "tags": {},
            "scale": null,
            "properties": {
                "scopes": [
                    "[variables('alertScope')]"
                ],
                "condition": {
                    "allOf": [
                        {
                            "field": "category",
                            "equals": "ServiceHealth"
                        },
                        {
                            "field": "properties.incidentType",
                            "equals": "Incident"
                        }
                    ]
                },
                "actions": {
                    "actionGroups": [
                        {
                            "actionGroupId": "[resourceId('microsoft.insights/actionGroups', parameters('actionGroups_name'))]",
                            "webhookProperties": {}
                        }
                    ]
                },
                "enabled": true,
                "description": ""
            },
            "dependsOn": [
                "[resourceId('microsoft.insights/actionGroups', parameters('actionGroups_name'))]"
            ]
        }
    ]
}
```

## <a name="manage-your-alerts"></a>De waarschuwingen beheren

Nadat u een waarschuwing gemaakt, wordt het weergegeven in de **waarschuwingen** sectie van **Monitor**. Selecteer de waarschuwing die u wilt beheren:

* Deze bewerken.
* Het verwijderen.
* \- Of uitschakelen, als u wilt tijdelijk stoppen of doorgaan met het ontvangen van meldingen voor de waarschuwing.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [aanbevolen procedures voor het instellen van waarschuwingen van Azure Service Health](https://www.microsoft.com/en-us/videoplayer/embed/RE2OtUa).
- Meer informatie over het [mobiele pushmeldingen te verzenden voor Azure Service Health instellen](https://www.microsoft.com/en-us/videoplayer/embed/RE2OtUw).
- Meer informatie over het [configureren van de webhook-meldingen voor bestaande beheersystemen voor probleem](../../service-health/service-health-alert-webhook-guide.md).
- Meer informatie over [health servicemeldingen](../../azure-monitor/platform/service-notifications.md).
- Meer informatie over [melding gelden enkele beperkingen](../../azure-monitor/platform/alerts-rate-limiting.md).
- Controleer de [activiteit log waarschuwing webhook-schema](../../azure-monitor/platform/activity-log-alerts-webhook.md).
- Krijgen een [overzicht van waarschuwingen voor activiteitenlogboeken](../../azure-monitor/platform/alerts-overview.md), en leer hoe u waarschuwingen ontvangt. 
- Meer informatie over [actiegroepen](../../azure-monitor/platform/action-groups.md).
