---
title: HDInsight-clusters maken met Azure Data Lake Storage Gen1 als standaardopslag met behulp van PowerShell | Microsoft-Docs
description: Azure PowerShell gebruiken om te maken en gebruiken van HDInsight-clusters met Azure Data Lake Storage Gen1
services: data-lake-store,hdinsight
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.assetid: 8917af15-8e37-46cf-87ad-4e6d5d67ecdb
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: c57a45145d9abc43d0ca79839ea297dfc025db9b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66161420"
---
# <a name="create-hdinsight-clusters-with-azure-data-lake-storage-gen1-as-default-storage-by-using-powershell"></a>HDInsight-clusters maken met Azure Data Lake Storage Gen1 als standaardopslag met behulp van PowerShell

> [!div class="op_single_selector"]
> * [Azure Portal gebruiken](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [PowerShell gebruiken (voor standaardopslag)](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [PowerShell gebruiken (voor extra opslag)](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [Gebruik Resource Manager](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)

Leer hoe u Azure PowerShell gebruiken voor het configureren van Azure HDInsight-clusters met Azure Data Lake Storage Gen1, als standaardopslag. Zie voor instructies over het maken van een HDInsight-cluster met Data Lake Storage Gen1 als extra opslag [een HDInsight-cluster maken met Data Lake Storage Gen1 als extra opslag](data-lake-store-hdinsight-hadoop-use-powershell.md).

Hier volgen enkele belangrijke overwegingen voor het gebruik van HDInsight met Data Lake Storage Gen1:

* De optie voor het maken van HDInsight-clusters met toegang tot Data Lake Storage Gen1 als standaardopslag is beschikbaar voor HDInsight versie 3.5 en 3.6.

* De optie voor het maken van HDInsight-clusters met toegang op Data Lake Storage Gen1 als standaardopslag is *niet beschikbaar* voor HDInsight Premium-clusters.

Volg de instructies in de volgende vijf secties voor het configureren van HDInsight om te werken met Data Lake Storage Gen1 met behulp van PowerShell.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Voordat u deze zelfstudie begint, zorg ervoor dat u voldoet aan de volgende vereisten:

* **Een Azure-abonnement**: Ga naar [gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).
* **Azure PowerShell 1.0 of hoger**: Zie [installeren en configureren van PowerShell](/powershell/azure/overview).
* **Windows Software Development Kit (SDK)** : Voor het installeren van Windows-SDK, gaat u naar [Downloads en hulpprogramma's voor Windows 10](https://dev.windows.com/downloads). De SDK wordt gebruikt voor het maken van een security-certificaat.
* **Service-principal voor Azure Active Directory**: In deze zelfstudie wordt beschreven hoe u een service-principal in Azure Active Directory (Azure AD) maken. Voor het maken van een service-principal, moet u echter een Azure AD-beheerder zijn. Als u een beheerder bent, kunt u deze vereiste overslaan en doorgaan met de zelfstudie.

    >[!NOTE]
    >U kunt een service-principal maken alleen als u een Azure AD-beheerder. Uw Azure AD-beheerder moet een service-principal maken voordat u een HDInsight-cluster met Data Lake Storage Gen1 kunt maken. De service-principal moet worden gemaakt met een certificaat, zoals beschreven in [een service-principal maken met certificaat](../active-directory/develop/howto-authenticate-service-principal-powershell.md#create-service-principal-with-certificate-from-certificate-authority).
    >

## <a name="create-a-data-lake-storage-gen1-account"></a>Een Data Lake Storage Gen1-account maken

Voor het maken van een Data Lake Storage Gen1-account, het volgende doen:

1. Vanaf het bureaublad, open een PowerShell-venster en voer vervolgens de codefragmenten hieronder. Wanneer u zich aanmeldt, meld u aan als een van de abonnementbeheerders of eigenaren wordt gevraagd. 

        # Sign in to your Azure account
        Connect-AzAccount

        # List all the subscriptions associated to your account
        Get-AzSubscription

        # Select a subscription
        Set-AzContext -SubscriptionId <subscription ID>

        # Register for Data Lake Storage Gen1
        Register-AzResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

    > [!NOTE]
    > Als u de Data Lake Storage Gen1 resourceprovider registreren en een foutbericht weergegeven die vergelijkbaar is met `Register-AzResourceProvider : InvalidResourceNamespace: The resource namespace 'Microsoft.DataLakeStore' is invalid`, uw abonnement mogelijk niet toegelaten voor Data Lake Storage Gen1. Volg de instructies in zodat uw Azure-abonnement voor Data Lake Storage Gen1 [aan de slag met Azure Data Lake Storage Gen1 met behulp van de Azure-portal](data-lake-store-get-started-portal.md).
    >

2. Een Data Lake Storage Gen1-account is gekoppeld aan een Azure-resourcegroep. Beginnen met het maken van een resourcegroep.

        $resourceGroupName = "<your new resource group name>"
        New-AzResourceGroup -Name $resourceGroupName -Location "East US 2"

    Hier ziet u uitvoer als volgt:

        ResourceGroupName : hdiadlgrp
        Location          : eastus2
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/<subscription-id>/resourceGroups/hdiadlgrp

3. Maak een Data Lake Storage Gen1-account. De accountnaam die u opgeeft moet alleen kleine letters en cijfers bevatten.

        $dataLakeStorageGen1Name = "<your new Data Lake Storage Gen1 name>"
        New-AzDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStorageGen1Name -Location "East US 2"

    Als het goed is, wordt ongeveer de volgende uitvoer weergegeven:

        ...
        ProvisioningState           : Succeeded
        State                       : Active
        CreationTime                : 5/5/2017 10:53:56 PM
        EncryptionState             : Enabled
        ...
        LastModifiedTime            : 5/5/2017 10:53:56 PM
        Endpoint                    : hdiadlstore.azuredatalakestore.net
        DefaultGroup                :
        Id                          : /subscriptions/<subscription-id>/resourceGroups/hdiadlgrp/providers/Microsoft.DataLakeStore/accounts/hdiadlstore
        Name                        : hdiadlstore
        Type                        : Microsoft.DataLakeStore/accounts
        Location                    : East US 2
        Tags                        : {}

4. Met behulp van Data Lake Storage Gen1 als standaardopslag, moet u om op te geven van een pad naar de hoofdmap op waarnaar de cluster-specifieke bestanden tijdens het maken van een cluster zijn gekopieerd. Maken van een pad naar hoofdmap is **/clusters/hdiadlcluster** in het codefragment maakt gebruik van de volgende cmdlets:

        $myrootdir = "/"
        New-AzDataLakeStoreItem -Folder -AccountName $dataLakeStorageGen1Name -Path $myrootdir/clusters/hdiadlcluster


## <a name="set-up-authentication-for-role-based-access-to-data-lake-storage-gen1"></a>Verificatie instellen voor op rollen gebaseerde toegang tot Data Lake Storage Gen1
Elk Azure-abonnement is gekoppeld aan een Azure AD-entiteit. Gebruikers en services die toegang hebben tot resources van abonnement met behulp van de Azure-portal of de Azure Resource Manager-API moeten eerst worden geverifieerd met Azure AD. Toegang te krijgen tot Azure-abonnementen en services door toe te wijzen ze de juiste rol op een Azure-resource. Voor services, een service-principal aangegeven van de service in Azure AD.

In deze sectie ziet u het verlenen van een toepassingsservice, zoals HDInsight, toegang tot een Azure-resource (het Gen1 van Data Lake Storage-account dat u eerder hebt gemaakt). Dit doet u door het maken van een service principal voor de toepassing en het toewijzen van rollen toe via PowerShell.

Als u Active Directory-verificatie voor Data Lake Storage Gen1 instelt, moet u de taken uitvoeren in de volgende twee secties.

### <a name="create-a-self-signed-certificate"></a>Een zelfondertekend certificaat maken
Zorg ervoor dat u hebt [Windows SDK](https://dev.windows.com/en-us/downloads) geïnstalleerd voordat u doorgaat met de stappen in deze sectie. U moet hebben ook een map gemaakt, zoals *C:\mycertdir*, waar u het certificaat maken.

1. Ga naar de locatie waar u de Windows-SDK geïnstalleerd in het PowerShell-venster (normaal gesproken *C:\Program Files (x86) \Windows Kits\10\bin\x86*) en gebruik de [MakeCert] [ makecert] hulpprogramma voor het maken van een zelfondertekend certificaat en een persoonlijke sleutel. Gebruik de volgende opdrachten:

        $certificateFileDir = "<my certificate directory>"
        cd $certificateFileDir

        makecert -sv mykey.pvk -n "cn=HDI-ADL-SP" CertFile.cer -r -len 2048

    U wordt gevraagd het wachtwoord voor persoonlijke sleutel invoeren. Nadat de opdracht is uitgevoerd, ziet u **CertFile.cer** en **mykey.pvk** in de certificaat-map die u hebt opgegeven.
2. Gebruik de [Pvk2Pfx] [ pvk2pfx] hulpprogramma voor het converteren van de PVK en de .cer-bestanden die MakeCert gemaakt naar een pfx-bestand. Voer de volgende opdracht uit:

        pvk2pfx -pvk mykey.pvk -spc CertFile.cer -pfx CertFile.pfx -po <password>

    Wanneer u wordt gevraagd, typt u het wachtwoord voor persoonlijke sleutel die u eerder hebt opgegeven. De waarde die u opgeeft voor de **-IO** parameter is het wachtwoord dat is gekoppeld aan het pfx-bestand. Nadat de opdracht is voltooid, moet er een **CertFile.pfx** in de certificaat-map die u hebt opgegeven.

### <a name="create-an-azure-ad-and-a-service-principal"></a>Een Azure AD maken en een service-principal
In deze sectie maakt u een service-principal voor een Azure AD-toepassing maken, een rol toewijzen aan de service-principal en verifiëren als de service-principal door te geven van een certificaat. Voor het maken van een toepassing in Azure AD, voer de volgende opdrachten:

1. Plak de volgende cmdlets in de PowerShell-consolevenster. Zorg ervoor dat de waarde die u opgeeft voor de **- DisplayName** eigenschap uniek is. De waarden voor **- startpagina** en **- IdentiferUris** tijdelijke aanduiding voor waarden zijn en worden niet geverifieerd.

        $certificateFilePath = "$certificateFileDir\CertFile.pfx"

        $password = Read-Host -Prompt "Enter the password" # This is the password you specified for the .pfx file

        $certificatePFX = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($certificateFilePath, $password)

        $rawCertificateData = $certificatePFX.GetRawCertData()

        $credential = [System.Convert]::ToBase64String($rawCertificateData)

        $application = New-AzADApplication `
            -DisplayName "HDIADL" `
            -HomePage "https://contoso.com" `
            -IdentifierUris "https://mycontoso.com" `
            -CertValue $credential  `
            -StartDate $certificatePFX.NotBefore  `
            -EndDate $certificatePFX.NotAfter

        $applicationId = $application.ApplicationId
2. Een service-principal maken met behulp van de toepassings-ID.

        $servicePrincipal = New-AzADServicePrincipal -ApplicationId $applicationId

        $objectId = $servicePrincipal.Id
3. De service principal toegang verlenen tot de hoofdmap van de Data Lake Storage Gen1 en alle mappen in het hoofdpad die u eerder hebt opgegeven. Gebruik de volgende cmdlets:

        Set-AzDataLakeStoreItemAclEntry -AccountName $dataLakeStorageGen1Name -Path / -AceType User -Id $objectId -Permissions All
        Set-AzDataLakeStoreItemAclEntry -AccountName $dataLakeStorageGen1Name -Path /clusters -AceType User -Id $objectId -Permissions All
        Set-AzDataLakeStoreItemAclEntry -AccountName $dataLakeStorageGen1Name -Path /clusters/hdiadlcluster -AceType User -Id $objectId -Permissions All

## <a name="create-an-hdinsight-linux-cluster-with-data-lake-storage-gen1-as-the-default-storage"></a>Een HDInsight Linux-cluster maken met Data Lake Storage Gen1 als standaardopslag

In deze sectie maakt u een HDInsight Hadoop Linux-cluster met Data Lake Storage Gen1 als de standaardopslag. Voor deze release de HDInsight-cluster en Data Lake Storage Gen1 moeten op dezelfde locatie.

1. De tenant-ID van het abonnement ophalen en opslaan voor later gebruik.

        $tenantID = (Get-AzContext).Tenant.TenantId

2. De HDInsight-cluster maken met behulp van de volgende cmdlets:

        # Set these variables

        $location = "East US 2"
        $storageAccountName = $dataLakeStorageGen1Name                         # Data Lake Storage Gen1 account name
        $storageRootPath = "<Storage root path you specified earlier>" # E.g. /clusters/hdiadlcluster
        $clusterName = "<unique cluster name>"
        $clusterNodes = <ClusterSizeInNodes>            # The number of nodes in the HDInsight cluster
        $httpCredentials = Get-Credential
        $sshCredentials = Get-Credential

        New-AzHDInsightCluster `
               -ClusterType Hadoop `
               -OSType Linux `
               -ClusterSizeInNodes $clusterNodes `
               -ResourceGroupName $resourceGroupName `
               -ClusterName $clusterName `
               -HttpCredential $httpCredentials `
               -Location $location `
               -DefaultStorageAccountType AzureDataLakeStore `
               -DefaultStorageAccountName "$storageAccountName.azuredatalakestore.net" `
               -DefaultStorageRootPath $storageRootPath `
               -Version "3.6" `
               -SshCredential $sshCredentials `
               -AadTenantId $tenantId `
               -ObjectId $objectId `
               -CertificateFilePath $certificateFilePath `
               -CertificatePassword $password

    Nadat de cmdlet is voltooid, ziet u uitvoer die hiermee de clusterdetails worden.

## <a name="run-test-jobs-on-the-hdinsight-cluster-to-use-data-lake-storage-gen1"></a>Testtaken uitvoeren op het HDInsight-cluster te gebruiken van Data Lake Storage Gen1
Nadat u een HDInsight-cluster hebt geconfigureerd, kunt u testtaken kunt uitvoeren om ervoor te zorgen dat deze toegang heeft tot Data Lake Storage Gen1. Om dit te doen, voer een voorbeeld-Hive-taak voor het maken van een tabel die gebruikmaakt van de voorbeeldgegevens die al beschikbaar is in Data Lake Storage Gen1 op  *\<cluster hoofdmap > /example/data/sample.log*.

In deze sectie maakt u een Secure Shell (SSH)-verbinding naar de HDInsight Linux-cluster die u hebt gemaakt en voert u een voorbeeld-Hive-query.

* Als u een Windows-client gebruikt een SSH-verbinding in het cluster te maken, raadpleegt u [SSH gebruiken met Linux gebaseerde Hadoop op HDInsight vanaf Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).
* Als u een Linux-client gebruikt een SSH-verbinding in het cluster te maken, raadpleegt u [SSH gebruiken met Linux gebaseerde Hadoop op HDInsight vanaf Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).

1. Als u de verbinding hebt gemaakt, start u de Hive-opdrachtregelinterface (CLI) met behulp van de volgende opdracht uit:

        hive
2. De CLI gebruiken voor het invoeren van de volgende instructies voor het maken van een nieuwe tabel met de naam **voertuigen** met behulp van de voorbeeldgegevens in Data Lake Storage Gen1:

        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'adl:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    U ziet de queryuitvoer op de SSH-console.

    >[!NOTE]
    >Het pad naar de voorbeeldgegevens in de voorgaande opdracht voor CREATE TABLE is `adl:///example/data/`, waarbij `adl:///` is de hoofdmap van het cluster. Na het voorbeeld van de hoofdmap van het cluster dat is opgegeven in deze zelfstudie is van de opdracht `adl://hdiadlstore.azuredatalakestore.net/clusters/hdiadlcluster`. U kunt de kortere alternatieve gebruiken of geef het volledige pad naar de hoofdmap van het cluster.
    >

## <a name="access-data-lake-storage-gen1-by-using-hdfs-commands"></a>Toegang tot Data Lake Storage Gen1 met behulp van HDFS-opdrachten
Nadat u het HDInsight-cluster voor het gebruik van Data Lake Storage Gen1 hebt geconfigureerd, kunt u Hadoop Distributed File System (HDFS) shell-opdrachten kunt gebruiken voor toegang tot de store.

In deze sectie maakt u een SSH-verbinding in de HDInsight Linux-cluster die u hebt gemaakt en voert u de HDFS-opdrachten.

* Als u een Windows-client gebruikt een SSH-verbinding in het cluster te maken, raadpleegt u [SSH gebruiken met Linux gebaseerde Hadoop op HDInsight vanaf Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).
* Als u een Linux-client gebruikt een SSH-verbinding in het cluster te maken, raadpleegt u [SSH gebruiken met Linux gebaseerde Hadoop op HDInsight vanaf Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).

Nadat u de verbinding hebt gemaakt, moet u de bestanden in Data Lake Storage Gen1 weergeven met behulp van de volgende opdracht voor het systeem HDFS-bestand.

    hdfs dfs -ls adl:///

U kunt ook de `hdfs dfs -put` opdracht voor sommige bestanden uploaden naar Data Lake Storage Gen1 en gebruik vervolgens `hdfs dfs -ls` om te controleren of de bestanden zijn geüpload.

## <a name="see-also"></a>Zie ook
* [Data Lake Storage Gen1 met Azure HDInsight-clusters gebruiken](../hdinsight/hdinsight-hadoop-use-data-lake-store.md)
* [Azure-portal: Een HDInsight-cluster voor het gebruik van Data Lake Storage Gen1 maken](data-lake-store-hdinsight-hadoop-use-portal.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx
