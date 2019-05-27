---
author: rothja
ms.service: virtual-machines-sql
ms.topic: include
ms.date: 10/26/2018
ms.author: jroth
ms.openlocfilehash: 22f16a7382cb0fe1f3fe2a6ef5e7c00a6989623c
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66165315"
---
## <a name="next-steps"></a>Volgende stappen

Nadat Azure Key Vault-integratie is ingeschakeld, kunt u SQL Server-versleuteling inschakelen op de SQL-VM. Eerst moet u een asymmetrische sleutel in uw key vault en een symmetrische sleutel in SQL Server op de virtuele machine maken. Vervolgens kunt u zich voor het uitvoeren van T-SQL-instructies gebruikt om versleuteling voor uw databases en de back-ups te schakelen.

Er zijn verschillende vormen die u van profiteren kunt versleuteling:

* [TDE (Transparent Data Encryption)](https://msdn.microsoft.com/library/bb934049.aspx)
* [Versleutelde back-ups](https://msdn.microsoft.com/library/dn449489.aspx)
* [Versleuteling op kolom (wissen)](https://msdn.microsoft.com/library/ms173744.aspx)

De volgende Transact-SQL-scripts vindt u voorbeelden voor elk van deze gebieden.

### <a name="prerequisites-for-examples"></a>Vereisten voor voorbeelden

Elk voorbeeld is gebaseerd op de twee vereisten: een asymmetrische sleutel van uw key vault met de naam **CONTOSO_KEY** en een referentie die zijn gemaakt door de functie Azure Sleutelkluis-integratie met de naam **Azure_EKM_TDE_cred**. De volgende Transact-SQL-opdrachten deze vereisten voor het uitvoeren van de voorbeelden voor installatie.

``` sql
USE master;
GO

--create credential
--The <<SECRET>> here requires the <Application ID> (without hyphens) and <Secret> to be passed together without a space between them.
CREATE CREDENTIAL sysadmin_ekm_cred
    WITH IDENTITY = 'keytestvault', --keyvault
    SECRET = '<<SECRET>>'
FOR CRYPTOGRAPHIC PROVIDER AzureKeyVault_EKM_Prov;


--Map the credential to a SQL login that has sysadmin permissions. This allows the SQL login to access the key vault when creating the asymmetric key in the next step.
ALTER LOGIN [SQL_Login]
ADD CREDENTIAL sysadmin_ekm_cred;


CREATE ASYMMETRIC KEY CONTOSO_KEY
FROM PROVIDER [AzureKeyVault_EKM_Prov]
WITH PROVIDER_KEY_NAME = 'KeyName_in_KeyVault',  --The key name here requires the key we created in the key vault
CREATION_DISPOSITION = OPEN_EXISTING;
```

### <a name="transparent-data-encryption-tde"></a>Transparent Data Encryption (TDE)

1. Maak een SQL Server-aanmelding moet worden gebruikt door de Database-Engine voor TDE, en vervolgens de referentie aan toe te voegen.

   ``` sql
   USE master;
   -- Create a SQL Server login associated with the asymmetric key
   -- for the Database engine to use when it loads a database
   -- encrypted by TDE.
   CREATE LOGIN EKM_Login
   FROM ASYMMETRIC KEY CONTOSO_KEY;
   GO

   -- Alter the TDE Login to add the credential for use by the
   -- Database Engine to access the key vault
   ALTER LOGIN EKM_Login
   ADD CREDENTIAL Azure_EKM_cred;
   GO
   ```

1. Maak de database-versleutelingssleutel die wordt gebruikt voor TDE.

   ``` sql
   USE ContosoDatabase;
   GO

   CREATE DATABASE ENCRYPTION KEY 
   WITH ALGORITHM = AES_128 
   ENCRYPTION BY SERVER ASYMMETRIC KEY CONTOSO_KEY;
   GO

   -- Alter the database to enable transparent data encryption.
   ALTER DATABASE ContosoDatabase
   SET ENCRYPTION ON;
   GO
   ```

### <a name="encrypted-backups"></a>Versleutelde back-ups

1. Een SQL Server-aanmelding moet worden gebruikt door de Database-Engine voor het versleutelen van back-ups maken en de referentie aan toe te voegen.

   ``` sql
   USE master;
   -- Create a SQL Server login associated with the asymmetric key
   -- for the Database engine to use when it is encrypting the backup.
   CREATE LOGIN EKM_Login
   FROM ASYMMETRIC KEY CONTOSO_KEY;
   GO

   -- Alter the Encrypted Backup Login to add the credential for use by
   -- the Database Engine to access the key vault
   ALTER LOGIN EKM_Login
   ADD CREDENTIAL Azure_EKM_cred ;
   GO
   ```

1. Back-up van de versleuteling van de database opgeven met de asymmetrische sleutel opgeslagen in de key vault.

   ``` sql
   USE master;
   BACKUP DATABASE [DATABASE_TO_BACKUP]
   TO DISK = N'[PATH TO BACKUP FILE]'
   WITH FORMAT, INIT, SKIP, NOREWIND, NOUNLOAD,
   ENCRYPTION(ALGORITHM = AES_256, SERVER ASYMMETRIC KEY = [CONTOSO_KEY]);
   GO
   ```

### <a name="column-level-encryption-cle"></a>Versleuteling op kolom (wissen)

Met dit script maakt een symmetrische sleutel die beveiligd is door de asymmetrische sleutel in de key vault en gebruikt vervolgens de symmetrische sleutel voor het versleutelen van gegevens in de database.

``` sql
CREATE SYMMETRIC KEY DATA_ENCRYPTION_KEY
WITH ALGORITHM=AES_256
ENCRYPTION BY ASYMMETRIC KEY CONTOSO_KEY;

DECLARE @DATA VARBINARY(MAX);

--Open the symmetric key for use in this session
OPEN SYMMETRIC KEY DATA_ENCRYPTION_KEY
DECRYPTION BY ASYMMETRIC KEY CONTOSO_KEY;

--Encrypt syntax
SELECT @DATA = ENCRYPTBYKEY(KEY_GUID('DATA_ENCRYPTION_KEY'), CONVERT(VARBINARY,'Plain text data to encrypt'));

-- Decrypt syntax
SELECT CONVERT(VARCHAR, DECRYPTBYKEY(@DATA));

--Close the symmetric key
CLOSE SYMMETRIC KEY DATA_ENCRYPTION_KEY;
```

## <a name="additional-resources"></a>Aanvullende resources

Zie voor meer informatie over het gebruik van deze versleutelingsfuncties [EKM gebruiken met SQL Server-versleutelingsfuncties](https://msdn.microsoft.com/library/dn198405.aspx#UsesOfEKM).

Merk op dat de stappen in dit artikel wordt ervan uitgegaan dat u al hebt uitgevoerd op een virtuele machine van Azure SQL-Server. Als dit niet het geval is, Zie [een SQL Server-machine inrichten in Azure](../articles/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision.md). Zie voor andere instructies over het uitvoeren van SQL Server op Azure Virtual machines [SQL Server op Azure Virtual Machines overzicht](../articles/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md).
