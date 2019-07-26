---
title: On-premises Apache Hadoop clusters migreren naar Azure HDInsight-beveiligings-en DevOps best practices
description: Leer de best practices voor beveiliging en DevOps voor het migreren van on-premises Hadoop-clusters naar Azure HDInsight.
author: hrasheed-msft
ms.reviewer: ashishth
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 10/25/2018
ms.author: hrasheed
ms.openlocfilehash: 0e7db970bee44d40831c05e8911b72841d027211
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68442094"
---
# <a name="migrate-on-premises-apache-hadoop-clusters-to-azure-hdinsight---security-and-devops-best-practices"></a>On-premises Apache Hadoop clusters migreren naar Azure HDInsight-beveiligings-en DevOps best practices

Dit artikel bevat aanbevelingen voor beveiliging en DevOps in azure HDInsight-systemen. Het is onderdeel van een serie die aanbevolen procedures biedt voor het migreren van on-premises Apache Hadoop systemen naar Azure HDInsight.

## <a name="secure-and-govern-cluster-with-enterprise-security-package"></a>Cluster beveiligen en beheren met Enterprise Security Package

De Enterprise Security Package (ESP) ondersteunt op Active Directory gebaseerde verificatie, ondersteuning voor meerdere gebruikers en toegangs beheer op basis van rollen. Als de ESP-optie is gekozen, wordt HDInsight-cluster gekoppeld aan het Active Directory domein en kan de ondernemings beheerder op rollen gebaseerd toegangs beheer (RBAC) configureren voor Apache Hive beveiliging met behulp van Apache zwerver. De beheerder kan ook de gegevens toegang door werk nemers en alle wijzigingen die worden uitgevoerd voor toegangs beheer beleid controleren.

ESP is beschikbaar op de volgende cluster typen: Apache Hadoop, Apache Spark, Apache HBase, Apache Kafka en Interactive query (Hive LLAP). 

Gebruik de volgende stappen om het HDInsight-cluster dat is gekoppeld aan het domein te implementeren:

- Implementeer Azure Active Directory (AAD) door de domein naam door te geven.
- Azure Active Directory Domain Services implementeren (AAD DS).
- Maak de vereiste Virtual Network en het subnet.
- Implementeer een virtuele machine in de Virtual Network om AAD DS te beheren.
- Voeg de virtuele machine toe aan het domein.
- Installeer AD-en DNS-hulpprogram ma's.
- Laat de AAD DS-beheerder een organisatie-eenheid (OE) maken.
- LDAPS inschakelen voor AAD DS.
- Maak een service account in Azure Active Directory met gedelegeerde Lees & machtiging voor het schrijven van beheerders voor de organisatie-eenheid, zodat het kan. Dit service account kan vervolgens worden toegevoegd aan het domein en computer-principals in de organisatie-eenheid worden geplaatst. Het kan ook service-principals maken binnen de OE die u opgeeft tijdens het maken van het cluster.


    > [!Note]
    > Het service account hoeft geen AD-domein beheerders account te zijn.


- Implementeer het HDInsight ESP-cluster door de volgende para meters in te stellen:
    - **Domein naam**: De domein naam die is gekoppeld aan Azure AD DS.
    - **Domein gebruikers naam**: Het service account in het door Azure AD DS DC beheerde domein dat u in de vorige sectie hebt gemaakt, bijvoorbeeld: `hdiadmin@contoso.onmicrosoft.com`. Deze domein gebruiker is de beheerder van dit HDInsight-cluster.
    - **Domein wachtwoord**: Het wacht woord van het service account.
    - **Organisatie-eenheid**: De DN-naam van de organisatie-eenheid die u wilt gebruiken met het HDInsight-cluster, `OU=HDInsightOU,DC=contoso,DC=onmicrosoft,DC=com`bijvoorbeeld:. Als deze OE niet bestaat, probeert het HDInsight-cluster de OE te maken met behulp van de bevoegdheden van het service account.
    - **URL van LDAPS**: bijvoorbeeld `ldaps://contoso.onmicrosoft.com:636`.
    - **Gebruikers groep voor toegang**: De beveiligings groepen waarvan u de gebruikers wilt synchroniseren met het cluster, bijvoorbeeld: `HiveUsers`. Als u meerdere gebruikers groepen wilt opgeven, scheidt u deze met punt komma '; '. De groep (en) moeten aanwezig zijn in de map voordat het ESP-cluster wordt gemaakt.

Raadpleeg voor meer informatie de volgende artikelen:

- [Een inleiding tot Apache Hadoop beveiliging met HDInsight-clusters die zijn toegevoegd aan een domein](../domain-joined/hdinsight-security-overview.md)

- [Apache Hadoop clusters die zijn toegevoegd aan het Azure-domein plannen in HDInsight](../domain-joined/apache-domain-joined-architecture.md)
- [Een HDInsight-cluster dat is gekoppeld aan een domein configureren met behulp van Azure Active Directory Domain Services](../domain-joined/apache-domain-joined-configure-using-azure-adds.md)
- [Azure Active Directory gebruikers synchroniseren met een HDInsight-cluster](../hdinsight-sync-aad-users-to-cluster.md)
- [Apache Hive-beleid configureren in HDInsight die lid is van een domein](../domain-joined/apache-domain-joined-run-hive.md)
- [Apache Oozie uitvoeren in HDInsight Hadoop-clusters die zijn toegevoegd aan een domein](../domain-joined/hdinsight-use-oozie-domain-joined-clusters.md)

## <a name="implement-end-to-end-enterprise-security"></a>End-to-end Enter prise Security implementeren

End-to-end Bedrijfs beveiliging kan worden bereikt met de volgende besturings elementen:

- **Persoonlijke en beveiligde gegevens pijplijn (beveiliging op perimeter niveau)**
    - Beveiliging op perimeter niveau kan worden bereikt via virtuele netwerken van Azure, netwerk beveiligings groepen en Gateway Service.

- **Verificatie en autorisatie voor gegevens toegang**
    - Maak een HDInsight-cluster dat is gekoppeld aan het domein met behulp van Azure Active Directory Domain Services. (Enterprise Security Package).
    - Gebruik Ambari om op rollen gebaseerde toegang tot cluster bronnen voor AD-gebruikers te bieden.
    - Gebruik Apache zwerver om toegangs beheer beleid in te stellen voor Hive op het niveau van tabel/kolom/rij.
    - SSH-toegang tot het cluster kan alleen worden beperkt tot de beheerder.

- **Controle**
    - Alle toegang tot de bronnen en gegevens van het HDInsight-cluster weer geven en rapporteren.
    - Alle wijzigingen in het toegangs beheer beleid weer geven en rapporteren.

- **Versleuteling**
    - Transparante versleuteling aan de server zijde met door micro soft beheerde sleutels of door de klant beheerde sleutels.
    - In transit versleuteling met versleuteling aan client zijde met behulp van HTTPS en TLS.

Raadpleeg voor meer informatie de volgende artikelen:

- [Overzicht van Azure Virtual Networks](../../virtual-network/virtual-networks-overview.md)
- [Overzicht van Azure-netwerk beveiligings groepen](../../virtual-network/security-overview.md)
- [Azure Virtual Network peering](../../virtual-network/virtual-network-peering-overview.md)
- [Beveiligings handleiding voor Azure Storage](../../storage/common/storage-security-guide.md)
- [Versleuteling van Azure Storage-service bij rest](../../storage/common/storage-service-encryption.md)

## <a name="use-monitoring--alerting"></a>Bewakings & waarschuwingen gebruiken

Zie voor meer informatie het artikel:

[Overzicht van Azure Monitor](../../azure-monitor/overview.md)

## <a name="upgrade-clusters"></a>Upgrade clusters

Voer regel matig een upgrade uit naar de nieuwste HDInsight-versie om te profiteren van de nieuwste functies. De volgende stappen kunnen worden gebruikt om het cluster bij te werken naar de meest recente versie:

1. Maak een nieuw HDInsight-TESTLAB-cluster met de meest recente versie van HDInsight.
1. Test op het nieuwe cluster om te controleren of de taken en werk belastingen naar verwachting werken.
1. Wijzig taken of toepassingen of werk belastingen zoals vereist.
1. Maak een back-up van tijdelijke gegevens die lokaal zijn opgeslagen op de cluster knooppunten.
1. Verwijder het bestaande cluster.
1. Maak een cluster van de meest recente HDInsight-versie in hetzelfde VNET-subnet, met dezelfde standaard gegevens en hetzelfde meta archief als het vorige cluster.
1. Importeer alle tijdelijke gegevens waarvan een back-up is gemaakt.
1. Start taken/Ga door met de verwerking met het nieuwe cluster.

Zie voor meer informatie het artikel: [Upgrade HDInsight-cluster naar een nieuwe versie](../hdinsight-upgrade-cluster.md).

## <a name="patch-cluster-operating-systems"></a>Patch voor cluster besturingssystemen

Als beheerde Hadoop-service zorgt HDInsight voor het patchen van het besturings systeem van de virtuele machines die worden gebruikt door HDInsight-clusters.

Zie voor meer informatie het artikel: [Patches voor het besturings systeem voor HDInsight](../hdinsight-os-patching.md).

## <a name="post-migration"></a>Na de migratie

1. **Toepassingen herstellen** -iteratieve de benodigde wijzigingen aanbrengen in de taken, processen en scripts.
2. **Voer tests uit** om functionele en prestatie tests van iteratief uit te voeren.
3. **Optimaliseer** : Los alle prestatie problemen op op basis van de bovenstaande test resultaten en voer vervolgens opnieuw een test uit om de prestatie verbeteringen te bevestigen.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [HDInsight 4,0](https://docs.microsoft.com/azure/hdinsight/hadoop/apache-hadoop-introduction).
