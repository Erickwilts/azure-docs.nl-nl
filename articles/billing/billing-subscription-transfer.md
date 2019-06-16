---
title: Azure-abonnement overdragen aan een ander account | Microsoft Docs
description: Hierin wordt beschreven hoe u een Azure-abonnement overdragen naar een andere gebruiker, en enkele veelgestelde vragen (FAQ) over het proces
keywords: overdracht van abonnement voor azure-abonnement, azure-overdracht, azure-abonnement verplaatsen naar een andere eigenaar van het wijzigen abonnement-account, azure, azure-abonnement overdragen naar ander account
author: bandersmsft
manager: amberb
tags: billing,top-support-issue
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/03/2019
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 91880e43382662b5d55f112455ee8f4c92ad01c5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66471597"
---
# <a name="transfer-ownership-of-an-azure-subscription-to-another-account"></a>Eigendom van een Azure-abonnement naar een ander account overdragen

Overdracht van uw abonnement aan een andere gebruiker in het Accountcentrum te wijzigen van de accountbeheerder hand via abonnement eigendom. Zie voor informatie over het wijzigen van uw abonnement naar een andere aanbieding [uw Azure-abonnement overstappen naar een andere aanbieding](billing-how-to-switch-azure-offer.md).

> [!IMPORTANT]
>
> Als u overdraagt van een abonnement op een nieuwe Azure AD-tenant, worden alle roltoewijzingen in [op rollen gebaseerd toegangsbeheer (RBAC)](../role-based-access-control/overview.md) permanent worden verwijderd uit de bron-tenant en worden niet gemigreerd naar de doel-tenant. U moet ook handmatig opnieuw maken van beheerde identiteiten voor Azure-resources. Zie voor meer informatie, [Veelgestelde vragen en bekende problemen met beheerde identiteiten](../active-directory/managed-identities-azure-resources/known-issues.md).

## <a name="transfer-ownership-of-an-azure-subscription"></a>Eigendom van een Azure-abonnement overdragen

> [!VIDEO https://channel9.msdn.com/Series/Microsoft-Azure-Tutorials/Transfer-an-Azure-subscription/player]


1. Meld u aan bij [Azure-Accountcentrum](https://account.windowsazure.com/Subscriptions) als de accountbeheerder. Als u wilt weten wie de accountbeheerder van het abonnement is, Zie [Veelgestelde vragen over](#faq).

1. Selecteer het abonnement om over te dragen.

1. Controleer of uw abonnement die in aanmerking komen voor de overdracht van self-service voor stroomactiviteitvoortgang door het controleren van de **bieden** en **aanbiedings-ID** met de [lijst met ondersteunde aanbiedingen](#supported).

   ![Controleer of de aanbiedings-ID van abonnement in het Accountcentrum](./media/billing-subscription-transfer/image0.png)
1. Klik op **Abonnement overdragen**.

   ![Tabblad voor abonnementen van Azure-account](./media/billing-subscription-transfer/image1.png)
1. Geef de ontvanger op.

   > [!IMPORTANT]
   >
   > Als u overdraagt van een abonnement op een nieuwe Azure AD-tenant, worden alle roltoewijzingen in [op rollen gebaseerd toegangsbeheer (RBAC)](../role-based-access-control/overview.md) permanent worden verwijderd uit de bron-tenant en worden niet gemigreerd naar de doel-tenant. U moet ook handmatig opnieuw maken van beheerde identiteiten voor Azure-resources. Zie voor meer informatie, [Veelgestelde vragen en bekende problemen met beheerde identiteiten](../active-directory/managed-identities-azure-resources/known-issues.md).

   ![In het dialoogvenster voor abonnement overdragen](./media/billing-subscription-transfer/image2.PNG)

1. De ontvanger ontvangt automatisch een e-mail met een acceptatielink.

   ![E-mail van abonnement overbrengen naar ontvanger](./media/billing-subscription-transfer/image3.png)
1. De ontvanger klikt op de link en volgt de instructies, waaronder het invoeren van zijn of haar betaalgegevens.

   ![Eerste webpagina voor overdracht van abonnement](./media/billing-subscription-transfer/image4.png)

   ![Tweede webpagina voor overdracht van abonnement](./media/billing-subscription-transfer/image5.png)
1. Gelukt! Nu wordt het abonnement overgedragen.

<a id="EA"></a>

## <a name="transfer-subscription-ownership-for-ea-customers"></a>Het abonnement overdragen voor EA-klanten

De Enterprise-beheerder kan het eigendom van abonnementen binnen een inschrijving overdragen. Als u wilt beginnen, Zie [accounteigendom Transfer](https://ea.azure.com/helpdocs/changeAccountOwnerForASubscription) in de EA-portal.

## <a name="next-steps-after-accepting-ownership"></a>Volgende stappen na de eigendom accepteren

1. U bent nu de accountbeheerder. Controleren en bijwerken van de servicebeheerder, Co-beheerders en andere RBAC-rollen. Zie voor meer informatie, [toevoegen of wijzigen Azure-abonnementbeheerders](billing-add-change-azure-subscription-administrator.md) en [toegang met RBAC en de Azure-portal beheren](../role-based-access-control/role-assignments-portal.md).
1. Referenties van de update die is gekoppeld aan services van dit abonnement met inbegrip van:
   1. Van beheercertificaten die aan de gebruiker beheerdersrechten voor resources-abonnement. Zie voor meer informatie, [maken en uploaden van een certificaat voor Azure](../cloud-services/cloud-services-certs-create.md)
   1. Toegangssleutels voor services zoals opslag. Zie voor meer informatie, [over Azure storage-accounts](../storage/common/storage-create-storage-account.md)
   1. Referenties voor externe toegang voor services zoals Azure Virtual Machines.
1. Als u met een partner werkt, kunt u overwegen de partner-ID voor dit abonnement wordt bijgewerkt. U kunt bijwerken de partner-ID in de [Azure-portal](https://portal.azure.com).

<a id="supported"></a>

## <a name="supported-offers"></a>Ondersteunde aanbiedingen

Self-service voor stroomactiviteitvoortgang abonnementsoverdracht is beschikbaar voor de aanbiedingen of abonnementstypen die worden vermeld in de volgende tabel. U kunt een gratis proefversie op dit moment niet overdragen of [Azure in Open (AIO)](https://azure.microsoft.com/offers/ms-azr-0111p/) abonnementen. Zie voor een tijdelijke oplossing [resources verplaatsen naar een nieuwe resourcegroep of abonnement](../azure-resource-manager/resource-group-move-resources.md). Om over te dragen van andere abonnementen, zoals [Sponsorship](https://azure.microsoft.com/offers/ms-azr-0036p/) of ondersteuningsabonnementen, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

| Naam van aanbieding:                                                                             | Nummer van de aanbieding |
|----------------------------------------------------------------------------------------|--------------|
| [Enterprise Agreement (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/)\*|MS-AZR-0017P        |
| [Microsoft Partner Network](https://azure.microsoft.com/offers/ms-azr-0025p/)          | MS-AZR-0025P        |
| [MSDN Platforms](https://azure.microsoft.com/offers/ms-azr-0062p/)                     | MS-AZR-0062P        |
| [Pay-As-You-Go](https://azure.microsoft.com/offers/ms-azr-0003p/)                      | MS-AZR-0003P        |
| [Pay-As-You-Go Dev/Test](https://azure.microsoft.com/offers/ms-azr-0023p/)             | MS-AZR-0023P        |
| [Visual Studio Enterprise](https://azure.microsoft.com/offers/ms-azr-0063p/)           | MS-AZR-0063P        |
| [Visual Studio Enterprise: BizSpark](https://azure.microsoft.com/offers/ms-azr-0064p/) | MS-AZR-0064P        |
| [Visual Studio Professional](https://azure.microsoft.com/offers/ms-azr-0059p/)         | MS-AZR-0059P        |
| [Visual Studio Test Professional](https://azure.microsoft.com/offers/ms-azr-0060p/)    | MS-AZR-0060P        |

\* [Via de EA-portal](#EA)

<a id="faq"></a>

## <a name="frequently-asked-questions-faq"></a>Veelgestelde vragen

### <a name="whoisaa"></a> Wie de accountbeheerder voor het abonnement is?

De accountbeheerder is de persoon die zich heeft aangemeld of het Azure-abonnement hebt gekocht. Ze zijn gemachtigd voor toegang tot de [Accountcentrum](https://account.azure.com/Subscriptions) en uitvoeren van verschillende beheertaken, zoals het maken van abonnementen, abonnementen annuleren, wijzigen van de facturering voor een abonnement of wijzigen van de Service-beheerder. Zie voor meer informatie over de informatie over beheerdersrollen en machtigingen, [rol beheerdersmachtigingen in Azure Active Directory](../active-directory/users-groups-roles/directory-assign-admin-roles.md)

Als u niet zeker weet wie de accountbeheerder is voor een abonnement, gebruikt u de volgende stappen om erachter te komen.

1. Ga naar de [pagina Abonnementen in de Azure-portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Selecteer het abonnement dat u wilt controleren, en ga vervolgens naar **instellingen**.
1. Selecteer **eigenschappen**. De accountbeheerder voor het abonnement wordt weergegeven in de **accountbeheerder** vak.

### <a name="does-everything-transfer-including-resource-groups-vms-disks-and-other-running-services"></a>Alles wat overbrengen? Met inbegrip van resourcegroepen, virtuele machines, schijven en andere actieve services?

Al uw resources, zoals virtuele machines, schijven en websites worden overgedragen naar de nieuwe eigenaar. Echter, een [beheerdersrollen](billing-add-change-azure-subscription-administrator.md) en [rollen gebaseerd toegangsbeheer (RBAC)](../role-based-access-control/role-assignments-portal.md) beleidsregels die u hebt ingesteld in verschillende mappen niet overgedragen. Bovendien [app-registraties](../active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad.md) en andere tenant-specifieke services worden niet overgedragen aan.

### <a id="no-button"></a> Waarom zie ik niet de knop 'Abonnement overdragen'?

Self-service voor stroomactiviteitvoortgang abonnementsoverdracht is helaas niet beschikbaar voor uw aanbieding. Bekijk de lijst met ondersteunde aanbiedingen in de [ondersteund aanbiedingen](#supported-offers) sectie van dit artikel. Bovendien wordt niet geblokkeerd door de overdracht van het abonnement voor alle landen. Echter, cross-land overdracht wordt niet ondersteund. Om over te dragen van uw abonnement cross-land, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). 


### <a name="can-i-transfer-ownership-to-an-account-in-another-country"></a>Kan ik overdragen aan een account in een ander land?

Azure, kunnen helaas geen cross land overdracht. Om over te dragen van uw abonnement cross-land, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).


### <a name="does-a-subscription-transfer-result-in-any-service-downtime"></a>Resulteert overdracht van het abonnement in een service-uitvaltijd?

Er zijn geen gevolgen voor de service. Het abonnement overdraagt, wordt het abonnement dat onder het huidige Account beheerder geannuleerd en maakt u een abonnement van de ontvanger het account. Het nieuwe abonnement is gekoppeld aan de onderliggende Azure-services. De abonnements-ID blijft hetzelfde.

### <a name="how-do-i-use-this-process-to-change-the-directory-for-subscription"></a>Hoe gebruik ik dit proces te wijzigen van de map voor het abonnement?

Een Azure-abonnement wordt gemaakt in de map die de accountbeheerder behoort. Als u de map, moet u het abonnement overbrengen naar een gebruikersaccount in de doelmap. Als die gebruiker de stappen voor het accepteren van de overdracht voltooit, wordt het abonnement automatisch verplaatst naar de doelmap.

### <a name="if-i-take-over-billing-ownership-of-a-subscription-from-another-organization-do-they-continue-to-have-access-to-my-resources"></a>Als ik eigendom van een abonnement van een andere organisatie overnemen, ze toegang hebben tot mijn resources blijven?

Als het abonnement wordt overgedragen naar een andere tenant, de gebruikers die zijn gekoppeld aan de vorige tenant geen toegang meer tot het abonnement. Zelfs als een gebruiker niet een servicebeheerder is of Co-beheerder zich voordoet, ze nog steeds toegang tot het abonnement via andere beveiligingsmechanismen hebben mogelijk, met inbegrip van:

* Van beheercertificaten die aan de gebruiker beheerdersrechten voor resources-abonnement. Zie voor meer informatie, [maken en uploaden van een Beheercertificaat voor Azure](../cloud-services/cloud-services-certs-create.md).
* Toegangssleutels voor services zoals opslag. Zie [Over Azure-opslagaccounts](../storage/common/storage-create-storage-account.md) voor meer informatie.
* Referenties voor externe toegang voor services zoals Azure Virtual Machines.

Als de ontvanger moet toegang tot hun resources te beperken, moeten ze rekening houden met alle geheimen die zijn gekoppeld aan de service wordt bijgewerkt. De meeste resources kunnen worden bijgewerkt met behulp van de volgende stappen uit:

  1. Ga naar de [Azure Portal](https://portal.azure.com).
  2. Selecteer in het menu Hub **alle resources**.
  3. Selecteer de resource.
  4. Klik in de resource-blade op **instellingen**. Hier kunt u bekijken en bijwerken van bestaande geheimen.

### <a name="if-i-transfer-the-subscription-in-the-middle-of-the-billing-cycle-does-the-recipient-pay-for-the-entire-billing-cycle"></a>Als ik het abonnement in het midden van de factureringscyclus overdraagt, de ontvanger betalen voor de hele facturering-cyclus?

De afzender is verantwoordelijk voor de betaling voor het gebruik die is gerapporteerd tot het tijdstip dat de overdracht is voltooid. De ontvanger is verantwoordelijk voor het gebruik gerapporteerd na de overdracht en hoger. Mogelijk zijn er enkele gebruik die heeft plaatsgevonden vóór overdracht, maar daarna is gemeld. Het gebruik is opgenomen in de factuur van de ontvanger.

### <a name="does-the-recipient-have-access-to-usage-and-billing-history"></a>Beschikt over de ontvanger toegang tot de gebruiks- en factureringsgeschiedenis?

  De enige informatie die beschikbaar zijn voor de ontvanger het bedrag van de laatste factuur is of als het abonnement is overgedragen voordat de eerste factuur is gegenereerd, het huidige saldo. De rest van het gebruik en de factureringsgeschiedenis draagt aan het abonnement.

### <a name="can-the-offer-be-changed-during-a-transfer"></a>Kan de aanbieding worden gewijzigd tijdens een overdracht?

De aanbieding moet hetzelfde blijven. Als u wilt wijzigen van uw aanbieding, Zie [uw Azure-abonnement overstappen naar een andere aanbieding](billing-how-to-switch-azure-offer.md).

### <a name="can-i-transfer-a-subscription-to-a-user-account-in-another-countryregion"></a>Kan ik een abonnement overdragen aan een gebruikersaccount in een ander land/regio?

Nee, een abonnement overdragen aan een gebruikersaccount in een ander land/de regio wordt niet ondersteund. Van de geadresseerde gebruikersaccount moet zich in hetzelfde land/regio.

### <a name="can-the-recipient-use-a-different-payment-method"></a>Kan de ontvanger van een andere betalingswijze gebruiken?

Ja. Maar de factureringsgeschiedenis van het abonnement is verdeeld over twee accounts.  

### <a name="is-the-payment-method-impacted-after-i-transferred-an-azure-subscription"></a>Is de betalingswijze beïnvloed nadat ik een Azure-abonnement overgezet?

Voor het accepteren van de overdracht van een abonnement, moet een creditcard of vergelijkbare betalingswijze wordt gebruikt om te betalen voor het abonnement worden opgegeven. Als Bob een abonnement naar ANS draagt en ANS de overdracht accepteert, moet ANS een betalingswijze wordt gebruikt om te betalen voor het abonnement opgeven. Nadat de overdracht voltooid is, wordt voor het abonnement niet Bob ANS gefactureerd.

### <a name="how-do-i-migrate-data-and-services-for-my-azure-subscription-to-new-subscription"></a>Hoe Migreer ik gegevens en services voor mijn Azure-abonnement naar een nieuw abonnement?

Als u niet kan het abonnement overdragen, kunt u handmatig uw resources migreren. Zie [resources verplaatsen naar een nieuwe resourcegroep of abonnement](../azure-resource-manager/resource-group-move-resources.md).

## <a name="need-help-contact-us"></a>Hulp nodig? Neem contact met ons op.

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Volgende stappen

- Controleren en bijwerken van de servicebeheerder, Co-beheerders en andere RBAC-rollen. Zie voor meer informatie, [toevoegen of wijzigen Azure-abonnementbeheerders](billing-add-change-azure-subscription-administrator.md) en [toegang met RBAC en de Azure-portal beheren](../role-based-access-control/role-assignments-portal.md).
