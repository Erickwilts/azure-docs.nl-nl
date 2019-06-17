---
title: Phoenix-queryserver REST SDK - Azure HDInsight
description: Installeren en gebruiken van de REST-SDK voor de Phoenix-queryserver in Azure HDInsight.
ms.service: hdinsight
author: ashishthaps
ms.author: ashishth
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 12/04/2017
ms.openlocfilehash: 1f468cac29579d8748f61a47b548a67d36ff8279
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64695955"
---
# <a name="apache-phoenix-query-server-rest-sdk"></a>Apache Phoenix-queryserver REST SDK

[Apache Phoenix](https://phoenix.apache.org/) is een open source, uiterst parallelle relationele databaselaag boven [Apache HBase](apache-hbase-overview.md). Phoenix kunt u SQL-achtige query's gebruiken met HBase met de SSH-hulpprogramma's zoals [SQLLine](apache-hbase-phoenix-squirrel-linux.md). Phoenix bevat ook een HTTP-server met de naam Phoenix Query Server (PQS), een thin client die ondersteuning biedt voor twee transportmechanismen voor clientcommunicatie: JSON en Protocol Buffers. Protocol Buffers is van het standaardmechanisme voor, en biedt meer efficiënte communicatie dan JSON.

In dit artikel wordt beschreven hoe u de PQS REST SDK gebruiken voor het maken van tabellen, upsert rijen afzonderlijk en bulksgewijs en selecteert u gegevens met behulp van SQL-instructies. De voorbeelden gebruiken de [Microsoft .NET-stuurprogramma voor Apache Phoenix-queryserver](https://www.nuget.org/packages/Microsoft.Phoenix.Client). Deze SDK is gebaseerd op [van Apache calciet Avatica](https://calcite.apache.org/avatica/) API's, die uitsluitend gebruik van Protocol Buffers voor de serialisatie-indeling.

Zie voor meer informatie, [Apache calciet Avatica Protocol Buffers verwijzing](https://calcite.apache.org/avatica/docs/protobuf_reference.html).

## <a name="install-the-sdk"></a>De SDK installeren

Microsoft .NET-stuurprogramma voor Apache Phoenix-queryserver wordt geleverd als een NuGet-pakket, die kan worden geïnstalleerd vanuit de Visual Studio **NuGet Package Manager Console** met de volgende opdracht:

    Install-Package Microsoft.Phoenix.Client

## <a name="instantiate-new-phoenixclient-object"></a>Exemplaar maken van nieuwe PhoenixClient-object

Om te beginnen met behulp van de bibliotheek, exemplaar maken van een nieuwe `PhoenixClient` -object doorgeven in een `ClusterCredentials` met de `Uri` aan uw cluster en de Apache Hadoop-gebruikersnaam en wachtwoord van het cluster.

```csharp
var credentials = new ClusterCredentials(new Uri("https://CLUSTERNAME.azurehdinsight.net/"), "USERNAME", "PASSWORD");
client = new PhoenixClient(credentials);
```

CLUSTERNAME vervangen door de naam van HDInsight HBase-cluster, en de gebruikersnaam en wachtwoord met de Hadoop-referenties opgegeven voor het maken van clusters. De standaardnaam van de Hadoop-gebruiker is **admin**.

## <a name="generate-unique-connection-identifier"></a>De unieke verbindings-id genereren

Voor het verzenden van een of meer aanvragen naar PQS, moet u een unieke verbindings-id voor het koppelen van de aanvragen met de verbinding.

```csharp
string connId = Guid.NewGuid().ToString();
```

Elk voorbeeld wordt eerst een aanroep naar de `OpenConnectionRequestAsync` methode, waarbij in de unieke verbindings-id wordt doorgegeven. Vervolgens definieert `ConnectionProperties` en `RequestOptions`, doorgeven die objecten en de id van de gegenereerde verbinding naar de `ConnectionSyncRequestAsync` methode. De PQS `ConnectionSyncRequest` object zorgt ervoor dat de client en de server een consistente weergave van de database-eigenschappen hebben.

## <a name="connectionsyncrequest-and-its-connectionproperties"></a>ConnectionSyncRequest en de ConnectionProperties

Om aan te roepen `ConnectionSyncRequestAsync`, doorgeven een `ConnectionProperties` object.

```csharp
ConnectionProperties connProperties = new ConnectionProperties
{
    HasAutoCommit = true,
    AutoCommit = true,
    HasReadOnly = true,
    ReadOnly = false,
    TransactionIsolation = 0,
    Catalog = "",
    Schema = "",
    IsDirty = true
};
await client.ConnectionSyncRequestAsync(connId, connProperties, options);
```

Hier volgen enkele eigenschappen van belang:

| Eigenschap | Description |
| -- | -- |
| Committed | Een Booleaanse waarde die aangeeft of `autoCommit` is ingeschakeld voor Phoenix-transacties. |
| ReadOnly | Een Booleaanse waarde die aangeeft of de verbinding alleen-lezen is. |
| TransactionIsolation | Een geheel getal die aangeeft van het niveau van de transactie-isolatieniveau volgens de specificatie JDBC - Zie de volgende tabel.|
| Catalogus | De naam van de catalogus te gebruiken tijdens het ophalen van eigenschappen van de verbinding. |
| Schema | De naam van het schema moet worden gebruikt tijdens het ophalen van eigenschappen van de verbinding. |
| IsDirty | Een Booleaanse waarde die aangeeft of de eigenschappen zijn gewijzigd. |

Hier volgen de `TransactionIsolation` waarden:

| Isolatie-waarde | Description |
| -- | -- |
| 0 | Transacties worden niet ondersteund. |
| 1 | Dirty leesbewerkingen, niet kan worden herhaald leesbewerkingen en dummy leesbewerkingen kunnen zich voordoen. |
| 2 | Dirty leesbewerkingen worden voorkomen, maar niet kan worden herhaald lees- en dummy leesbewerkingen kunnen zich voordoen. |
| 4 | Dirty lees- en niet kan worden herhaald leesbewerkingen worden voorkomen, maar dummy leesbewerkingen optreden. |
| 8 | Alle worden vervuild leesbewerkingen, niet kan worden herhaald leesbewerkingen en dummy leesbewerkingen voorkomen. |

## <a name="create-a-new-table"></a>Een nieuwe tabel maken

HBase, net als elke andere RDBMS van TOPKLASSE, worden gegevens opgeslagen in tabellen. Phoenix gebruikt standaard SQL-query's om nieuwe tabellen maken tijdens het definiëren van de primaire sleutel en in de kolom typen.

In dit voorbeeld en alle volgende voorbeelden gebruiken de geïnstantieerde `PhoenixClient` zoals gedefinieerd in object [exemplaar maken van een nieuw PhoenixClient object](#instantiate-new-phoenixclient-object).

```csharp
string connId = Guid.NewGuid().ToString();
RequestOptions options = RequestOptions.GetGatewayDefaultOptions();

// You can set certain request options, such as timeout in milliseconds:
options.TimeoutMillis = 300000;

// In gateway mode, PQS requests will be https://<cluster dns name>.azurehdinsight.net/hbasephoenix<N>/
// Requests sent to hbasephoenix0/ will be forwarded to PQS on workernode0
options.AlternativeEndpoint = "hbasephoenix0/";
CreateStatementResponse createStatementResponse = null;
OpenConnectionResponse openConnResponse = null;

try
{
    // Opening connection
    var info = new pbc::MapField<string, string>();
    openConnResponse = await client.OpenConnectionRequestAsync(connId, info, options);
    
    // Syncing connection
    ConnectionProperties connProperties = new ConnectionProperties
    {
        HasAutoCommit = true,
        AutoCommit = true,
        HasReadOnly = true,
        ReadOnly = false,
        TransactionIsolation = 0,
        Catalog = "",
        Schema = "",
        IsDirty = true
    };
    await client.ConnectionSyncRequestAsync(connId, connProperties, options);

    // Create the statement
    createStatementResponse = client.CreateStatementRequestAsync(connId, options).Result;
    
    // Create the table if it does not exist
    string sql = "CREATE TABLE IF NOT EXISTS Customers (Id varchar(20) PRIMARY KEY, FirstName varchar(50), " +
        "LastName varchar(100), StateProvince char(2), Email varchar(255), Phone varchar(15))";
    await client.PrepareAndExecuteRequestAsync(connId, sql, createStatementResponse.StatementId, long.MaxValue, int.MaxValue, options);

    Console.WriteLine($"Table \"Customers\" created.");
}
catch (Exception e)
{
    Console.WriteLine(e);
    throw;
}
finally
{
    if (createStatementResponse != null)
    {
        client.CloseStatementRequestAsync(connId, createStatementResponse.StatementId, options).Wait();
        createStatementResponse = null;
    }

    if (openConnResponse != null)
    {
        client.CloseConnectionRequestAsync(connId, options).Wait();
        openConnResponse = null;
    }
}
```

Het vorige voorbeeld maakt een nieuwe tabel met de naam `Customers` met behulp van de `IF NOT EXISTS` optie. De `CreateStatementRequestAsync` aanroep maakt u een nieuwe instructie in de Avitica (PQS)-server. De `finally` blok wordt de geretourneerde gesloten `CreateStatementResponse` en de `OpenConnectionResponse` objecten.

## <a name="insert-data-individually"></a>Afzonderlijk gegevens invoegen

In dit voorbeeld ziet u een afzonderlijke gegevens invoegen, die verwijst naar een `List<string>` verzameling van de Amerikaanse staat en gebied afkortingen:

```csharp
var states = new List<string> { "AL", "AK", "AS", "AZ", "AR", "CA", "CO", "CT", "DE", "DC", "FM", "FL", "GA", "GU", "HI", "ID", "IL", "IN", "IA", "KS", "KY", "LA", "ME", "MH", "MD", "MA", "MI", "MN", "MS", "MO", "MT", "NE", "NV", "NH", "NJ", "NM", "NY", "NC", "ND", "MP", "OH", "OK", "OR", "PW", "PA", "PR", "RI", "SC", "SD", "TN", "TX", "UT", "VT", "VI", "VA", "WA", "WV", "WI", "WY" };
```

Van de tabel `StateProvince` waarde in de kolom wordt gebruikt in een latere bewerking select.

```csharp
string connId = Guid.NewGuid().ToString();
RequestOptions options = RequestOptions.GetGatewayDefaultOptions();
options.TimeoutMillis = 300000;

// In gateway mode, PQS requests will be https://<cluster dns name>.azurehdinsight.net/hbasephoenix<N>/
// Requests sent to hbasephoenix0/ will be forwarded to PQS on workernode0
options.AlternativeEndpoint = "hbasephoenix0/";
OpenConnectionResponse openConnResponse = null;
StatementHandle statementHandle = null;
try
{
    // Opening connection
    pbc::MapField<string, string> info = new pbc::MapField<string, string>();
    openConnResponse = await client.OpenConnectionRequestAsync(connId, info, options);
    // Syncing connection
    ConnectionProperties connProperties = new ConnectionProperties
    {
        HasAutoCommit = true,
        AutoCommit = true,
        HasReadOnly = true,
        ReadOnly = false,
        TransactionIsolation = 0,
        Catalog = "",
        Schema = "",
        IsDirty = true
    };
    await client.ConnectionSyncRequestAsync(connId, connProperties, options);

    string sql = "UPSERT INTO Customers VALUES (?,?,?,?,?,?)";
    PrepareResponse prepareResponse = await client.PrepareRequestAsync(connId, sql, 100, options);
    statementHandle = prepareResponse.Statement;
    
    var r = new Random();

    // Insert 300 rows
    for (int i = 0; i < 300; i++)
    {
        var list = new pbc.RepeatedField<TypedValue>();
        var id = new TypedValue
        {
            StringValue = "id" + i,
            Type = Rep.String
        };
        var firstName = new TypedValue
        {
            StringValue = "first" + i,
            Type = Rep.String
        };
        var lastName = new TypedValue
        {
            StringValue = "last" + i,
            Type = Rep.String
        };
        var state = new TypedValue
        {
            StringValue = states.ElementAt(r.Next(0, 49)),
            Type = Rep.String
        };
        var email = new TypedValue
        {
            StringValue = $"email{1}@junkemail.com",
            Type = Rep.String
        };
        var phone = new TypedValue
        {
            StringValue = $"555-229-341{i.ToString().Substring(0,1)}",
            Type = Rep.String
        };
        list.Add(id);
        list.Add(firstName);
        list.Add(lastName);
        list.Add(state);
        list.Add(email);
        list.Add(phone);

        Console.WriteLine("Inserting customer " + i);

        await client.ExecuteRequestAsync(statementHandle, list, long.MaxValue, true, options);
    }

    await client.CommitRequestAsync(connId, options);

    Console.WriteLine("Upserted customer data");

}
catch (Exception ex)
{

}
finally
{
    if (statementHandle != null)
    {
        await client.CloseStatementRequestAsync(connId, statementHandle.Id, options);
        statementHandle = null;
    }
    if (openConnResponse != null)
    {
        await client.CloseConnectionRequestAsync(connId, options);
        openConnResponse = null;
    }
}
```

De structuur voor het uitvoeren van een instructie insert is vergelijkbaar met het maken van een nieuwe tabel. Houd er rekening mee dat aan het einde van de `try` blok, de transactie is expliciet doorgevoerd. In dit voorbeeld wordt een insert-transactie 300 keren herhaald. Het volgende voorbeeld ziet u een efficiëntere batch insert-proces.

## <a name="batch-insert-data"></a>Batch insert-gegevens

De volgende code is bijna identiek aan de code voor het invoegen van gegevens afzonderlijk. In dit voorbeeld wordt de `UpdateBatch` -object in een aanroep van `ExecuteBatchRequestAsync`, in plaats van herhaaldelijk aanroept `ExecuteRequestAsync` met een voorbereide instructie.

```csharp
string connId = Guid.NewGuid().ToString();
RequestOptions options = RequestOptions.GetGatewayDefaultOptions();
options.TimeoutMillis = 300000;

// In gateway mode, PQS requests will be https://<cluster dns name>.azurehdinsight.net/hbasephoenix<N>/
// Requests sent to hbasephoenix0/ will be forwarded to PQS on workernode0
options.AlternativeEndpoint = "hbasephoenix0/";
OpenConnectionResponse openConnResponse = null;
CreateStatementResponse createStatementResponse = null;
try
{
    // Opening connection
    pbc::MapField<string, string> info = new pbc::MapField<string, string>();
    openConnResponse = await client.OpenConnectionRequestAsync(connId, info, options);
    // Syncing connection
    ConnectionProperties connProperties = new ConnectionProperties
    {
        HasAutoCommit = true,
        AutoCommit = true,
        HasReadOnly = true,
        ReadOnly = false,
        TransactionIsolation = 0,
        Catalog = "",
        Schema = "",
        IsDirty = true
    };
    await client.ConnectionSyncRequestAsync(connId, connProperties, options);

    // Creating statement
    createStatementResponse = await client.CreateStatementRequestAsync(connId, options);

    string sql = "UPSERT INTO Customers VALUES (?,?,?,?,?,?)";
    PrepareResponse prepareResponse = client.PrepareRequestAsync(connId, sql, long.MaxValue, options).Result;
    var statementHandle = prepareResponse.Statement;
    var updates = new pbc.RepeatedField<UpdateBatch>();

    var r = new Random();

    // Insert 300 rows
    for (int i = 300; i < 600; i++)
    {
        var list = new pbc.RepeatedField<TypedValue>();
        var id = new TypedValue
        {
            StringValue = "id" + i,
            Type = Rep.String
        };
        var firstName = new TypedValue
        {
            StringValue = "first" + i,
            Type = Rep.String
        };
        var lastName = new TypedValue
        {
            StringValue = "last" + i,
            Type = Rep.String
        };
        var state = new TypedValue
        {
            StringValue = states.ElementAt(r.Next(0, 49)),
            Type = Rep.String
        };
        var email = new TypedValue
        {
            StringValue = $"email{1}@junkemail.com",
            Type = Rep.String
        };
        var phone = new TypedValue
        {
            StringValue = $"555-229-341{i.ToString().Substring(0, 1)}",
            Type = Rep.String
        };
        list.Add(id);
        list.Add(firstName);
        list.Add(lastName);
        list.Add(state);
        list.Add(email);
        list.Add(phone);

        var batch = new UpdateBatch
        {
            ParameterValues = list
        };
        updates.Add(batch);

        Console.WriteLine($"Added customer {i} to batch");
    }

    var executeBatchResponse = await client.ExecuteBatchRequestAsync(connId, statementHandle.Id, updates, options);

    Console.WriteLine("Batch upserted customer data");

}
catch (Exception ex)
{

}
finally
{
    if (openConnResponse != null)
    {
        await client.CloseConnectionRequestAsync(connId, options);
        openConnResponse = null;
    }
}
```

In een testomgeving duurde afzonderlijk invoegen 300 nieuwe records bijna twee minuten. Invoegen van 300 records als batch vereist daarentegen alleen 6 seconden.

## <a name="select-data"></a>Gegevens selecteren

In dit voorbeeld laat zien hoe een verbinding voor het uitvoeren van meerdere query's opnieuw gebruiken:

1. Alle records selecteren en vervolgens resterende records ophalen nadat het maximumaantal van 100 worden geretourneerd.
2. Een rij Totaal aantal select-instructie gebruiken om op te halen van de scalaire resultaat.
3. Voer een select-instructie die als resultaat het totale aantal klanten per staat of provincie geeft.

```csharp
string connId = Guid.NewGuid().ToString();
RequestOptions options = RequestOptions.GetGatewayDefaultOptions();

// In gateway mode, PQS requests will be https://<cluster dns name>.azurehdinsight.net/hbasephoenix<N>/
// Requests sent to hbasephoenix0/ will be forwarded to PQS on workernode0
options.AlternativeEndpoint = "hbasephoenix0/";
OpenConnectionResponse openConnResponse = null;
StatementHandle statementHandle = null;

try
{
    // Opening connection
    pbc::MapField<string, string> info = new pbc::MapField<string, string>();
    openConnResponse = await client.OpenConnectionRequestAsync(connId, info, options);
    // Syncing connection
    ConnectionProperties connProperties = new ConnectionProperties
    {
        HasAutoCommit = true,
        AutoCommit = true,
        HasReadOnly = true,
        ReadOnly = false,
        TransactionIsolation = 0,
        Catalog = "",
        Schema = "",
        IsDirty = true
    };
    await client.ConnectionSyncRequestAsync(connId, connProperties, options);
    var createStatementResponse = await client.CreateStatementRequestAsync(connId, options);

    string sql = "SELECT * FROM Customers";
    ExecuteResponse executeResponse = await client.PrepareAndExecuteRequestAsync(connId, sql, createStatementResponse.StatementId, long.MaxValue, int.MaxValue, options);

    pbc::RepeatedField<Row> rows = executeResponse.Results[0].FirstFrame.Rows;
    // Loop through all of the returned rows and display the first two columns
    for (int i = 0; i < rows.Count; i++)
    {
        Row row = rows[i];
        Console.WriteLine(row.Value[0].ScalarValue.StringValue + " " + row.Value[1].ScalarValue.StringValue);
    }
    
    // 100 is hard-coded on the server side as the default firstframe size
    // FetchRequestAsync is called to get any remaining rows
    Console.WriteLine("");
    Console.WriteLine($"Number of rows: {rows.Count}");

    // Fetch remaining rows, offset is not used, simply set to 0
    // When FetchResponse.Frame.Done is true, all rows were fetched
    FetchResponse fetchResponse = await client.FetchRequestAsync(connId, createStatementResponse.StatementId, 0, int.MaxValue, options);
    Console.WriteLine($"Frame row count: {fetchResponse.Frame.Rows.Count}");
    Console.WriteLine($"Fetch response is done: {fetchResponse.Frame.Done}");
    Console.WriteLine("");

    // Running query 2
    string sql2 = "select count(*) from Customers";
    ExecuteResponse countResponse = await client.PrepareAndExecuteRequestAsync(connId, sql2, createStatementResponse.StatementId, long.MaxValue, int.MaxValue, options);
    long count = countResponse.Results[0].FirstFrame.Rows[0].Value[0].ScalarValue.NumberValue;

    Console.WriteLine($"Total customer records: {count}");
    Console.WriteLine("");

    // Running query 3
    string sql3 = "select StateProvince, count(*) as Number from Customers group by StateProvince order by Number desc";
    ExecuteResponse groupByResponse = await client.PrepareAndExecuteRequestAsync(connId, sql3, createStatementResponse.StatementId, long.MaxValue, int.MaxValue, options);

    pbc::RepeatedField<Row> stateRows = groupByResponse.Results[0].FirstFrame.Rows;
    for (int i = 0; i < stateRows.Count; i++)
    {
        Row row = stateRows[i];
        Console.WriteLine(row.Value[0].ScalarValue.StringValue + ": " + row.Value[1].ScalarValue.NumberValue);
    }
}
catch (Exception ex)
{

}
finally
{
    if (statementHandle != null)
    {
        await client.CloseStatementRequestAsync(connId, statementHandle.Id, options);
        statementHandle = null;
    }
    if (openConnResponse != null)
    {
        await client.CloseConnectionRequestAsync(connId, options);
        openConnResponse = null;
    }
}
```

De uitvoer van de `select` instructies moet het volgende resultaat:

```
id0 first0
id1 first1
id10 first10
id100 first100
id101 first101
id102 first102
. . .
id185 first185
id186 first186
id187 first187
id188 first188

Number of rows: 100
Frame row count: 500
Fetch response is done: True

Total customer records: 600

NJ: 21
CA: 19
GU: 17
NC: 16
IN: 16
MA: 16
AZ: 16
ME: 16
IL: 15
OR: 15
. . . 
MO: 10
HI: 10
GA: 10
DC: 9
NM: 9
MD: 9
MP: 9
SC: 7
AR: 7
MH: 6
FM: 5
```

## <a name="next-steps"></a>Volgende stappen 

* [Apache Phoenix in HDInsight](../hdinsight-phoenix-in-hdinsight.md)
* [Met behulp van de Apache HBase REST SDK](apache-hbase-rest-sdk.md)
