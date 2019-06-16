---
title: Azure Resource Manager-modus voor volledig verwijderen op resourcetype
description: Laat zien hoe de verwijdering van de modus voor volledig in Azure Resource Manager-sjablonen voor het verwerken van resourcetypen.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 04/24/2019
ms.author: tomfitz
ms.openlocfilehash: 21b3972a96c1601b15c403275474d58873753b08
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64712990"
---
# <a name="deletion-of-azure-resources-for-complete-mode-deployments"></a>Verwijderen van de Azure-resources voor volledige-implementaties
Dit artikel wordt beschreven hoe resourcetypen verwijderen als deze niet in een sjabloon die is geïmplementeerd in de modus voor volledig verwerken.

De resourcetypen die zijn gemarkeerd met `Yes` worden verwijderd als het type is niet in de sjabloon geïmplementeerd met volledige-modus. 

De resourcetypen die zijn gemarkeerd met `No` worden niet automatisch verwijderd als deze niet in de sjabloon; ze echter worden verwijderd als de bovenliggende resource wordt verwijderd. Zie voor een volledige beschrijving van het gedrag van [Azure Resource Manager-implementatiemodi](deployment-modes.md).

Als u dezelfde gegevens als een bestand met door komma's gescheiden waarden, download [voltooien modus deletion.csv](https://github.com/tfitzmac/resource-capabilities/blob/master/complete-mode-deletion.csv).

## <a name="microsoftaad"></a>Microsoft.AAD
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| DomainServices | Ja | 
| DomainServices/oucontainer | Nee | 

## <a name="microsoftaadiam"></a>microsoft.aadiam
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| diagnosticSettings | Nee | 
| diagnosticSettingsCategories | Nee | 

## <a name="microsoftaddons"></a>Microsoft.Addons
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| supportProviders | Nee | 

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| aadsupportcases | Nee | 
| addsservices | Nee | 
| Agents | Nee | 
| anonymousapiusers | Nee | 
| configuratie | Nee | 
| logs | Nee | 
| rapporten | Nee | 
| services | Nee | 

## <a name="microsoftadvisor"></a>Microsoft.Advisor
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| Configuraties | Nee | 
| generateRecommendations | Nee | 
| Aanbevelingen | Nee | 
| suppressions | Nee | 

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| actionRules | Nee | 
| waarschuwingen | Nee | 
| alertsList | Nee | 
| alertsSummary | Nee | 
| alertsSummaryList | Nee | 
| smartDetectorAlertRules | Nee | 
| smartDetectorRuntimeEnvironments | Nee | 
| smartGroups | Nee | 

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| servers | Ja | 

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| reportFeedback | Nee | 
| service | Ja | 
| validateServiceName | Nee | 

## <a name="microsoftattestation"></a>Microsoft.Attestation
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| attestationProviders | Nee | 

## <a name="microsoftauthorization"></a>Microsoft.Authorization
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| classicAdministrators | Nee | 
| denyAssignments | Nee | 
| elevateAccess | Nee | 
| Hiermee vergrendelt u | Nee | 
| machtigingen | Nee | 
| policyAssignments | Nee | 
| policyDefinitions | Nee | 
| policySetDefinitions | Nee | 
| providerOperations | Nee | 
| roleAssignments | Nee | 
| roleDefinitions | Nee | 

## <a name="microsoftautomation"></a>Microsoft.Automation
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| automationAccounts | Ja | 
| automationAccounts/configurations | Ja | 
| automationAccounts/jobs | Nee | 
| automationAccounts/runbooks | Ja | 
| automationAccounts/softwareUpdateConfigurations | Nee | 
| automationAccounts/webhooks | Nee | 

## <a name="microsoftazuregeneva"></a>Microsoft.Azure.Geneva
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| Omgevingen | Nee | 
| omgevingen/accounts | Nee | 
| accounts-omgevingen-naamruimten | Nee | 
| omgevingen/accounts/naamruimten/configuraties | Nee | 

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| b2cDirectories | Ja | 

## <a name="microsoftazurestack"></a>Microsoft.AzureStack
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| registraties | Ja | 
| registrations/customerSubscriptions | Nee | 
| registraties/producten | Nee | 

## <a name="microsoftbatch"></a>Microsoft.Batch
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| batchAccounts | Ja | 

## <a name="microsoftbilling"></a>Microsoft.Billing
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| billingAccounts | Nee | 
| billingAccounts/billingProfiles | Nee | 
| billingAccounts/billingProfiles/billingSubscriptions | Nee | 
| billingAccounts/billingProfiles/invoices | Nee | 
| billingAccounts/billingProfiles/invoices/pricesheet | Nee | 
| billingAccounts/billingProfiles/operationStatus | Nee | 
| billingAccounts/billingProfiles/paymentMethods | Nee | 
| billingAccounts/billingProfiles/policies | Nee | 
| billingAccounts/billingProfiles/pricesheet | Nee | 
| billingAccounts/billingProfiles/products | Nee | 
| billingAccounts/billingProfiles/transactions | Nee | 
| billingAccounts/billingSubscriptions | Nee | 
| billingAccounts/departments | Nee | 
| billingAccounts/eligibleOffers | Nee | 
| billingAccounts/enrollmentAccounts | Nee | 
| billingAccounts/invoices | Nee | 
| billingAccounts/invoiceSections | Nee | 
| billingAccounts/invoiceSections/billingSubscriptions | Nee | 
| billingAccounts/invoiceSections/billingSubscriptions/transfer | Nee | 
| billingAccounts/invoiceSections/importRequests | Nee | 
| billingAccounts/invoiceSections/initiateImportRequest | Nee | 
| billingAccounts/invoiceSections/initiateTransfer | Nee | 
| billingAccounts/invoiceSections/operationStatus | Nee | 
| billingAccounts/invoiceSections/products | Nee | 
| billingAccounts/invoiceSections/transfers | Nee | 
| billingAccounts/products | Nee | 
| billingAccounts/projects | Nee | 
| billingAccounts/projects/billingSubscriptions | Nee | 
| billingAccounts/projects/importRequests | Nee | 
| billingAccounts/projects/initiateImportRequest | Nee | 
| billingAccounts/projects/operationStatus | Nee | 
| billingAccounts/projects/products | Nee | 
| billingAccounts/transactions | Nee | 
| billingPeriods | Nee | 
| BillingPermissions | Nee | 
| billingProperty | Nee | 
| BillingRoleAssignments | Nee | 
| BillingRoleDefinitions | Nee | 
| CreateBillingRoleAssignment | Nee | 
| afdelingen | Nee | 
| enrollmentAccounts | Nee | 
| importRequests | Nee | 
| importRequests/acceptImportRequest | Nee | 
| importRequests/declineImportRequest | Nee | 
| Facturen | Nee | 
| Overdrachten | Nee | 
| transfers/acceptTransfer | Nee | 
| overdrachten/declineTransfer | Nee | 
| transfers/operationStatus | Nee | 
| usagePlans | Nee | 

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| mapApis | Ja | 
| updateCommunicationPreference | Nee | 

## <a name="microsoftbiztalkservices"></a>Microsoft.BizTalkServices
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| BizTalk | Ja | 

## <a name="microsoftblueprint"></a>Microsoft.Blueprint
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| blueprintAssignments | Nee | 
| blueprintAssignments/assignmentOperations | Nee | 
| blueprintAssignments/operations | Nee | 
| blauwdrukken | Nee | 
| blauwdrukken/artefacten | Nee | 
| blauwdrukken /-versies | Nee | 
| versies-blauwdrukken-artefacten | Nee | 

## <a name="microsoftbotservice"></a>Microsoft.BotService
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| botServices | Ja | 
| botServices/channels | Nee | 
| botServices/connections | Nee | 

## <a name="microsoftcache"></a>Microsoft.Cache
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| Redis | Ja | 
| RedisConfigDefinition | Nee | 

## <a name="microsoftcapacity"></a>Microsoft.Capacity
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| appliedReservations | Nee | 
| calculatePrice | Nee | 
| catalogi | Nee | 
| commercialReservationOrders | Nee | 
| reservationOrders | Nee | 
| reservationOrders/calculateRefund | Nee | 
| reservationOrders/merge | Nee | 
| reservationOrders/reserveringen | Nee | 
| reservationOrders/reserveringen/revisies | Nee | 
| reservationOrders of retourneren | Nee | 
| reservationOrders/splitsen | Nee | 
| reservationOrders/wisselen | Nee | 
| reserveringen | Nee | 
| resources | Nee | 
| validateReservationOrder | Nee | 

## <a name="microsoftcdn"></a>Microsoft.Cdn
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| edgenodes | Nee | 
| Profielen | Ja | 
| profielen/eindpunten | Ja | 
| profiles/endpoints/customdomains | Nee | 
| profiles/endpoints/origins | Nee | 
| validateProbe | Nee | 

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| certificateOrders | Ja | 
| certificateOrders/certificaten | Nee | 
| validateCertificateRegistrationInformation | Nee | 

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| Mogelijkheden | Nee | 
| domainNames | Nee | 
| domainNames/mogelijkheden | Nee | 
| domainNames/internalLoadBalancers | Nee | 
| domainNames/serviceCertificates | Nee | 
| domainNames/sleuven | Nee | 
| sleuven-domainNames-rollen | Nee | 
| moveSubscriptionResources | Nee | 
| operatingSystemFamilies | Nee | 
| operatingSystems | Nee | 
| quotas | Nee | 
| resourceTypes | Nee | 
| validateSubscriptionMoveAvailability | Nee | 
| virtualMachines | Nee | 
| virtualMachines/diagnosticSettings | Nee | 

## <a name="microsoftclassicinfrastructuremigrate"></a>Microsoft.ClassicInfrastructureMigrate
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| classicInfrastructureResources | Nee | 

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| Mogelijkheden | Nee | 
| expressRouteCrossConnections | Nee | 
| expressRouteCrossConnections/peerings | Nee | 
| gatewaySupportedDevices | Nee | 
| networkSecurityGroups | Nee | 
| quotas | Nee | 
| reservedIps | Nee | 
| virtualNetworks | Nee | 
| virtualNetworks/remoteVirtualNetworkPeeringProxies | Nee | 
| virtualNetworks/virtualNetworkPeerings | Nee | 

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| Mogelijkheden | Nee | 
| Schijven | Nee | 
| images | Nee | 
| osImages | Nee | 
| osPlatformImages | Nee | 
| publicImages | Nee | 
| quotas | Nee | 
| storageAccounts | Nee | 
| storageAccounts/services | Nee | 
| storageAccounts/services/diagnosticSettings | Nee | 
| storageAccounts/vmImages | Nee | 
| vmImages | Nee | 

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| accounts | Ja | 

## <a name="microsoftcommerce"></a>Microsoft.Commerce
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| RateCard | Nee | 
| UsageAggregates | Nee | 

## <a name="microsoftcompute"></a>Microsoft.Compute
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| availabilitySets | Ja | 
| Schijven | Ja | 
| images | Ja | 
| restorePointCollections | Ja | 
| restorePointCollections/restorePoints | Nee | 
| sharedVMImages | Ja | 
| sharedVMImages /-versies | Ja | 
| Momentopnamen | Ja | 
| virtualMachines | Ja | 
| virtualMachines/diagnosticSettings | Nee | 
| virtualMachines/extensions | Ja | 
| virtualMachineScaleSets | Ja | 
| virtualMachineScaleSets/extensions | Nee | 
| virtualMachineScaleSets/networkInterfaces | Nee | 
| virtualMachineScaleSets/publicIPAddresses | Nee | 
| virtualMachineScaleSets/virtualMachines | Nee | 
| virtualMachineScaleSets/virtualMachines/networkInterfaces | Nee | 

## <a name="microsoftconsumption"></a>Microsoft.Consumption
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| AggregatedCost | Nee | 
| Tegoeden | Nee | 
| Budgetten | Nee | 
| Kosten in rekening gebracht | Nee | 
| CostTags | Nee | 
| credits | Nee | 
| events | Nee | 
| Prognoses | Nee | 
| veel | Nee | 
| Marktplaatsen | Nee | 
| Pricesheets | Nee | 
| Producten | Nee | 
| ReservationDetails | Nee | 
| ReservationRecommendations | Nee | 
| ReservationSummaries | Nee | 
| ReservationTransactions | Nee | 
| Tags | Nee | 
| Voorwaarden | Nee | 
| UsageDetails | Nee | 

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| containerGroups | Ja | 
| serviceAssociationLinks | Nee | 

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| registers | Ja | 
| registers/builds | Nee | 
| registers/builds/annuleren | Nee | 
| registries/builds/getLogLink | Nee | 
| registers/buildTasks | Ja | 
| registers/buildTasks/stappen | Nee | 
| registries/eventGridFilters | Nee | 
| registries/getBuildSourceUploadUrl | Nee | 
| registries/GetCredentials | Nee | 
| registries/importImage | Nee | 
| registries/queueBuild | Nee | 
| registries/regenerateCredential | Nee | 
| registries/regenerateCredentials | Nee | 
| registers/replicaties | Ja | 
| registers/wordt uitgevoerd | Nee | 
| registers/uitvoeringen/annuleren | Nee | 
| registries/scheduleRun | Nee | 
| registers/taken | Ja | 
| registries/updatePolicies | Nee | 
| registers/webhooks | Ja | 
| registries/webhooks/getCallbackConfig | Nee | 
| webhooks-registers-ping | Nee | 

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| containerServices | Ja | 
| managedClusters | Ja | 

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| toepassingen | Ja | 
| updateCommunicationPreference | Nee | 

## <a name="microsoftcortanaanalytics"></a>Microsoft.CortanaAnalytics
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| accounts | Ja | 

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| Waarschuwingen | Nee | 
| BillingAccounts | Nee | 
| Connectors | Ja | 
| afdelingen | Nee | 
| Dimensies | Nee | 
| EnrollmentAccounts | Nee | 
| Query’s uitvoeren | Nee | 
| Registreren | Nee | 
| Reportconfigs | Nee | 
| Rapporten | Nee | 

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| hubs | Ja | 
| hubs/authorizationPolicies | Nee | 
| hubs/connectors | Nee | 
| connectors-hubs-toewijzingen | Nee | 
| hubs/interacties | Nee | 
| hubs/kpi | Nee | 
| hubs/links | Nee | 
| hubs/profielen | Nee | 
| hubs/roleAssignments | Nee | 
| hubs/rollen | Nee | 
| hubs/suggestTypeSchema | Nee | 
| hubs/weergaven | Nee | 
| hubs/widgetTypes | Nee | 

## <a name="microsoftdatabox"></a>Microsoft.DataBox
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| jobs | Ja | 

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| DataBoxEdgeDevices | Ja | 

## <a name="microsoftdatabricks"></a>Microsoft.Databricks
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| workspaces | Ja | 
| workspaces/virtualNetworkPeerings | Nee | 

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| catalogi | Ja | 

## <a name="microsoftdataconnect"></a>Microsoft.DataConnect
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| connectionManagers | Ja | 

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| dataFactories | Ja | 
| dataFactories/diagnosticSettings | Nee | 
| dataFactorySchema | Nee | 
| factory 's | Ja | 
| factories/integrationRuntimes | Nee | 

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| accounts | Ja | 
| accounts/dataLakeStoreAccounts | Nee | 
| accounts/storageAccounts | Nee | 
| accounts/storageAccounts/containers | Nee | 

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| accounts | Ja | 
| accounts/eventGridFilters | Nee | 
| accounts/firewallRules | Nee | 

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| services | Ja | 
| Services-projecten | Ja | 

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| servers | Ja | 
| servers/recoverableServers | Nee | 
| servers/virtualNetworkRules | Nee | 

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| servers | Ja | 
| servers/recoverableServers | Nee | 
| servers/virtualNetworkRules | Nee | 

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| servers | Ja | 
| servers/advisors | Nee | 
| servers/queryTexts | Nee | 
| servers/recoverableServers | Nee | 
| servers/topQueryStatistics | Nee | 
| servers/virtualNetworkRules | Nee | 
| servers/waitStatistics | Nee | 

## <a name="microsoftdevices"></a>Microsoft.Devices
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| IotHubs | Ja | 
| IotHubs/eventGridFilters | Nee | 
| ProvisioningServices | Ja | 
| Het gebruik van | Nee | 

## <a name="microsoftdevspaces"></a>Microsoft.DevSpaces
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| Domeincontrollers | Ja | 

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| Labs | Ja | 
| Labs/serviceRunners | Ja | 
| labs/virtualMachines | Ja | 
| Schema 's | Ja | 

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| databaseAccountNames | Nee | 
| databaseAccounts | Ja | 

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| domeinen | Ja | 
| domains/domainOwnershipIdentifiers | Nee | 
| generateSsoRequest | Nee | 
| topLevelDomains | Nee | 
| validateDomainRegistrationInformation | Nee | 

## <a name="microsoftdynamicslcs"></a>Microsoft.DynamicsLcs
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| lcsprojects | Nee | 
| lcsprojects/clouddeployments | Nee | 
| lcsprojects/connectors | Nee | 

## <a name="microsofteventgrid"></a>Microsoft.EventGrid
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| domeinen | Ja | 
| domeinen/onderwerpen | Nee | 
| eventSubscriptions | Nee | 
| extensionTopics | Nee | 
| onderwerpen | Ja | 
| topicTypes | Nee | 

## <a name="microsofteventhub"></a>Microsoft.EventHub
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| Clusters | Ja | 
| Naamruimten | Ja | 
| naamruimten/authorizationrules | Nee | 
| naamruimten/disasterrecoveryconfigs | Nee | 
| namespaces/eventhubs | Nee | 
| Event hubs-naamruimten/authorizationrules | Nee | 
| Event hubs-naamruimten/consumergroups | Nee | 

## <a name="microsoftfeatures"></a>Microsoft.Features
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| database | Nee | 
| Providers | Nee | 

## <a name="microsoftgallery"></a>Microsoft.Gallery
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| registreren | Nee | 
| galleryitems | Nee | 
| generateartifactaccessuri | Nee | 
| myareas | Nee | 
| myareas/areas | Nee | 
| myareas/areas/areas | Nee | 
| myareas/areas/areas/galleryitems | Nee | 
| myareas/areas/galleryitems | Nee | 
| myareas/galleryitems | Nee | 
| Registreren | Nee | 
| resources | Nee | 
| retrieveresourcesbyid | Nee | 

## <a name="microsoftguestconfiguration"></a>Microsoft.GuestConfiguration
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| guestConfigurationAssignments | Nee | 
| software | Nee | 

## <a name="microsofthanaonazure"></a>Microsoft.HanaOnAzure
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| hanaInstances | Ja | 

## <a name="microsofthdinsight"></a>Microsoft.HDInsight
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| Clusters | Ja | 
| clusters/toepassingen | Nee | 

## <a name="microsoftimportexport"></a>Microsoft.ImportExport
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| jobs | Ja | 

## <a name="microsoftinformationprotection"></a>Microsoft.InformationProtection
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| labelGroups | Nee | 
| labelGroups/labels | Nee | 
| labelGroups/labels/conditions | Nee | 
| labelGroups/labels/subLabels | Nee | 
| labelGroups/labels/subLabels/conditions | Nee | 

## <a name="microsoftinsights"></a>microsoft.insights
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| actiongroups | Ja | 
| activityLogAlerts | Ja | 
| alertrules | Ja | 
| automatedExportSettings | Nee | 
| autoscalesettings | Ja | 
| Basislijn | Nee | 
| calculatebaseline | Nee | 
| Onderdelen | Ja | 
| onderdelen/gebeurtenissen | Nee | 
| onderdelen/pricingPlans | Nee | 
| onderdelen/query | Nee | 
| diagnosticSettings | Nee | 
| diagnosticSettingsCategories | Nee | 
| eventCategories | Nee | 
| eigenschap EventTypes | Nee | 
| extendedDiagnosticSettings | Nee | 
| logDefinitions | Nee | 
| logprofiles | Nee | 
| logs | Nee | 
| metricAlerts | Ja |
| migrateToNewPricingModel | Nee | 
| myWorkbooks | Nee | 
| query's | Nee | 
| rollbackToLegacyPricingModel | Nee | 
| scheduledqueryrules | Ja | 
| vmInsightsOnboardingStatuses | Nee | 
| webtests | Ja | 
| werkmappen | Ja | 

## <a name="microsoftintune"></a>Microsoft.Intune
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| diagnosticSettings | Nee | 
| diagnosticSettingsCategories | Nee | 

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| IoTApps | Ja | 

## <a name="microsoftiotspaces"></a>Microsoft.IoTSpaces
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| Graph | Ja | 

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| deletedVaults | Nee | 
| Kluizen | Ja | 
| vaults/accessPolicies | Nee | 
| Kluizen/geheimen | Nee | 

## <a name="microsoftkusto"></a>Microsoft.Kusto
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| Clusters | Ja | 
| clusters/databases | Nee | 
| clusters/databases/met dataconnections | Nee | 
| clusters/databases/eventhubconnections | Nee | 

## <a name="microsoftlabservices"></a>Microsoft.LabServices
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| labaccounts | Ja | 
| gebruikers | Nee | 

## <a name="microsoftlocationbasedservices"></a>Microsoft.LocationBasedServices
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| accounts | Ja | 

## <a name="microsoftlocationservices"></a>Microsoft.LocationServices
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| accounts | Ja | 

## <a name="microsoftloganalytics"></a>Microsoft.LogAnalytics
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| logs | Nee | 

## <a name="microsoftlogic"></a>Microsoft.Logic
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| integrationAccounts | Ja | 
| Werkstromen | Ja | 

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| commitmentPlans | Ja | 
| webServices | Ja | 
| Workspaces | Ja | 

## <a name="microsoftmachinelearningexperimentation"></a>Microsoft.MachineLearningExperimentation
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| accounts | Ja | 
| accounts/werkruimten | Ja | 
| accounts/workspaces/projects | Ja | 
| teamAccounts | Ja | 
| teamAccounts/workspaces | Ja | 
| teamAccounts/workspaces/projects | Ja | 

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| accounts | Ja | 

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| workspaces | Ja | 
| workspaces/computes | Nee | 

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| Identiteiten | Nee | 
| userAssignedIdentities | Ja | 

## <a name="microsoftmanagement"></a>Microsoft.Management
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| getEntities | Nee | 
| managementGroups | Nee | 
| resources | Nee | 
| startTenantBackfill | Nee | 
| tenantBackfillStatus | Nee | 

## <a name="microsoftmaps"></a>Microsoft.Maps
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| accounts | Ja | 
| accounts/eventGridFilters | Nee | 

## <a name="microsoftmarketplace"></a>Microsoft.Marketplace
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| Aanbiedingen | Nee | 
| offerTypes | Nee | 
| offerTypes/publishers | Nee | 
| offerTypes/publishers/offers | Nee | 
| offerTypes/publishers/offers/plans | Nee | 
| offerTypes/publishers/offers/plans/agreements | Nee | 
| offerTypes/publishers/offers/plans/configs | Nee | 
| offerTypes/publishers/offers/plans/configs/importImage | Nee | 
| privategalleryitems | Nee | 
| Producten | Nee | 

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| classicDevServices | Ja | 
| updateCommunicationPreference | Nee | 

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceOrdering
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| Overeenkomsten | Nee | 
| offertypes | Nee | 

## <a name="microsoftmedia"></a>Microsoft.Media
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| mediaservices | Ja | 
| mediaservices/accountFilters | Nee | 
| mediaservices/activa | Nee | 
| mediaservices/assets/assetFilters | Nee | 
| mediaservices/contentKeyPolicies | Nee | 
| mediaservices/eventGridFilters | Nee | 
| mediaservices/liveEventOperations | Nee | 
| mediaservices/liveEvents | Ja | 
| mediaservices/liveEvents/liveOutputs | Nee | 
| mediaservices/liveOutputOperations | Nee | 
| mediaservices/streamingEndpointOperations | Nee | 
| mediaservices/streamingEndpoints | Ja | 
| mediaservices/streamingLocators | Nee | 
| mediaservices/streamingPolicies | Nee | 
| mediaservices/transformaties | Nee | 
| transformaties-mediaservices-taken | Nee | 

## <a name="microsoftmigrate"></a>Microsoft.Migrate
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| projecten | Ja | 

## <a name="microsoftnetwork"></a>Microsoft.Network
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| applicationGateways | Ja | 
| applicationSecurityGroups | Ja | 
| azureFirewallFqdnTags | Nee | 
| azureFirewalls | Ja | 
| bgpServiceCommunities | Nee | 
| Verbindingen | Ja | 
| ddosCustomPolicies | Ja | 
| ddosProtectionPlans | Ja | 
| dnsOperationStatuses | Nee | 
| dnszones | Ja | 
| dnszones/A | Nee | 
| dnszones/AAAA | Nee | 
| dnszones/all | Nee | 
| dnszones/CAA | Nee | 
| dnszones/CNAME | Nee | 
| dnszones/MX | Nee | 
| dnszones/NS | Nee | 
| dnszones/PTR | Nee | 
| dnszones/recordsets | Nee | 
| dnszones/SOA | Nee | 
| dnszones/SRV | Nee | 
| dnszones/TXT | Nee | 
| expressRouteCircuits | Ja | 
| expressRouteServiceProviders | Nee | 
| frontdoors | Ja | 
| frontdoorWebApplicationFirewallPolicies | Ja | 
| getDnsResourceReference | Nee | 
| interfaceEndpoints | Ja | 
| internalNotify | Nee | 
| loadBalancers | Ja | 
| localNetworkGateways | Ja | 
| natGateways | Ja | 
| networkIntentPolicies | Ja | 
| networkInterfaces | Ja | 
| networkProfiles | Ja | 
| networkSecurityGroups | Ja | 
| networkWatchers | Ja | 
| networkWatchers/connectionMonitors | Ja | 
| networkWatchers/lenses | Ja | 
| networkWatchers/pingMeshes | Ja | 
| privateLinkServices | Ja | 
| publicIPAddresses | Ja | 
| publicIPPrefixes | Ja | 
| routeFilters | Ja | 
| routeTables | Ja | 
| serviceEndpointPolicies | Ja | 
| trafficManagerGeographicHierarchies | Nee | 
| trafficmanagerprofiles | Ja | 
| trafficmanagerprofiles/heatMaps | Nee | 
| virtualHubs | Ja | 
| virtualNetworkGateways | Ja | 
| virtualNetworks | Ja | 
| virtualNetworkTaps | Ja | 
| virtualWans | Ja | 
| vpnGateways | Ja | 
| vpnSites | Ja | 
| webApplicationFirewallPolicies | Ja | 

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| Naamruimten | Ja | 
| namespaces/notificationHubs | Ja | 

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| apparaten | Nee | 
| linkTargets | Nee | 
| storageInsightConfigs | Nee | 
| workspaces | Ja | 
| workspaces/dataSources | Nee | 
| workspaces/linkedServices | Nee | 
| workspaces/query | Nee | 

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| managementassociations | Nee | 
| managementconfigurations | Ja | 
| oplossingen | Ja | 
| Weergaven | Ja | 

## <a name="microsoftpolicyinsights"></a>Microsoft.PolicyInsights
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| policyEvents | Nee | 
| policyStates | Nee | 
| policyTrackedResources | Nee | 
| herstelbewerkingen | Nee | 

## <a name="microsoftportal"></a>Microsoft.Portal
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| consoles | Nee | 
| dashboards | Ja | 
| userSettings | Nee | 

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| workspaceCollections | Ja | 

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| Capaciteit | Ja | 

## <a name="microsoftprojectoxford"></a>Microsoft.ProjectOxford
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| accounts | Ja | 

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| backupProtectedItems | Nee | 
| Kluizen | Ja | 

## <a name="microsoftrelay"></a>Microsoft.Relay
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| Naamruimten | Ja | 
| naamruimten/authorizationrules | Nee | 
| namespaces/hybridconnections | Nee | 
| namespaces/hybridconnections/authorizationrules | Nee | 
| naamruimten/wcfrelays | Nee | 
| namespaces/wcfrelays/authorizationrules | Nee | 

## <a name="microsoftresourcegraph"></a>Microsoft.ResourceGraph
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| resources | Nee | 
| subscriptionsStatus | Nee | 

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| availabilityStatuses | Nee | 
| childAvailabilityStatuses | Nee | 
| childResources | Nee | 
| events | Nee | 
| impactedResources | Nee | 
| Meldingen | Nee | 

## <a name="microsoftresources"></a>Microsoft.Resources
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| Implementaties | Nee | 
| Implementaties/operations | Nee | 
| Koppelingen | Nee | 
| notifyResourceJobs | Nee | 
| Providers | Nee | 
| resourceGroups | Nee | 
| resources | Nee | 
| Abonnementen | Nee | 
| Abonnementen/providers | Nee | 
| abonnementen/resourcegroepen | Nee | 
| abonnementen/resourcegroups/resources | Nee | 
| Abonnementen/resources | Nee | 
| abonnementen/tagnames | Nee | 
| subscriptions/tagNames/tagValues | Nee | 
| Tenants | Nee | 

## <a name="microsoftsaas"></a>Microsoft.SaaS
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| toepassingen | Ja | 
| saasresources | Nee | 

## <a name="microsoftscheduler"></a>Microsoft.Scheduler
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| stromen | Ja | 
| taakverzamelingen | Ja | 

## <a name="microsoftsearch"></a>Microsoft.Search
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| resourceHealthMetadata | Nee | 
| searchServices | Ja | 

## <a name="microsoftsecurity"></a>Microsoft.Security
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| advancedThreatProtectionSettings | Nee | 
| waarschuwingen | Nee | 
| allowedConnections | Nee | 
| apparaten | Nee | 
| applicationWhitelistings | Nee | 
| AutoProvisioningSettings | Nee | 
| Conformiteit | Nee | 
| dataCollectionAgents | Nee | 
| discoveredSecuritySolutions | Nee | 
| externalSecuritySolutions | Nee | 
| InformationProtectionPolicies | Nee | 
| jitNetworkAccessPolicies | Nee | 
| Bewaking | Nee | 
| monitoring/antimalware | Nee | 
| bewaking/basislijn | Nee | 
| bewaking/patch | Nee | 
| Beleid | Nee | 
| prijzen | Nee | 
| securityContacts | Nee | 
| securitySolutions | Nee | 
| securitySolutionsReferenceData | Nee | 
| securityStatus | Nee | 
| securityStatus/eindpunten | Nee | 
| securityStatus/subnetten | Nee | 
| securityStatus/virtuele machines | Nee | 
| securityStatuses | Nee | 
| securityStatusesSummaries | Nee | 
| instellingen | Nee | 
| taken | Nee | 
| Topologieën | Nee | 
| workspaceSettings | Nee | 

## <a name="microsoftsecuritygraph"></a>Microsoft.SecurityGraph
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| diagnosticSettings | Nee | 
| diagnosticSettingsCategories | Nee | 

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| Naamruimten | Ja | 
| naamruimten/authorizationrules | Nee | 
| naamruimten/disasterrecoveryconfigs | Nee | 
| naamruimten/eventgridfilters | Nee | 
| namespaces/queues | Nee | 
| namespaces/queues/authorizationrules | Nee | 
| naamruimten/onderwerpen | Nee | 
| naamruimten/onderwerpen/authorizationrules | Nee | 
| naamruimten/onderwerpen/abonnementen | Nee | 
| naamruimten/onderwerpen/abonnementen/regels | Nee | 
| premiumMessagingRegions | Nee | 

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| Clusters | Ja | 
| clusters/toepassingen | Nee | 

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| toepassingen | Ja | 
| Gateways | Ja | 
| Netwerken | Ja | 
| geheimen | Ja | 
| volumes | Ja | 

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| SignalR | Ja | 

## <a name="microsoftsolutions"></a>Microsoft.Solutions
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| applianceDefinitions | Ja | 
| apparaten | Ja | 
| applicationDefinitions | Ja | 
| toepassingen | Ja | 
| jitRequests | Ja | 

## <a name="microsoftsql"></a>Microsoft.SQL
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| managedInstances | Ja |
| managedInstances/databases | Ja (Zie opmerking hieronder) |
| managedInstances/databases/backupShortTermRetentionPolicies | Nee |
| managedInstances/databases/schemas/tables/columns/sensitivityLabels | Nee |
| managedInstances/databases/vulnerabilityAssessments | Nee |
| managedInstances/databases/vulnerabilityAssessments/rules/baselines | Nee |
| managedInstances/encryptionProtector | Nee |
| managedInstances/sleutels | Nee |
| managedInstances/restorableDroppedDatabases/backupShortTermRetentionPolicies | Nee |
| managedInstances/vulnerabilityAssessments | Nee |
| servers | Ja | 
| servers/beheerders | Nee | 
| servers/communicationLinks | Nee | 
| servers/databases | Ja (Zie opmerking hieronder) | 
| servers/encryptionProtector | Nee | 
| servers/firewallRules | Nee | 
| servers/sleutels | Nee | 
| servers/restorableDroppedDatabases | Nee | 
| servers/serviceobjectives | Nee | 
| servers/tdeCertificates | Nee | 

> [!NOTE]
> De Master database biedt geen ondersteuning voor labels, maar andere databases, datawarehouse-databases, inclusief ondersteuning voor tags.


## <a name="microsoftsqlvirtualmachine"></a>Microsoft.SqlVirtualMachine
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| SqlVirtualMachineGroups | Ja | 
| SqlVirtualMachineGroups/AvailabilityGroupListeners | Nee | 
| SqlVirtualMachines | Ja | 

## <a name="microsoftstorage"></a>Microsoft.Storage
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| storageAccounts | Ja | 
| storageAccounts/blobServices | Nee | 
| storageAccounts/fileServices | Nee | 
| storageAccounts/queueServices | Nee | 
| storageAccounts/services | Nee | 
| storageAccounts/tableServices | Nee | 
| Het gebruik van | Nee | 

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| storageSyncServices | Ja | 
| storageSyncServices/registeredServers | Nee | 
| storageSyncServices/syncGroups | Nee | 
| storageSyncServices/syncGroups/cloudEndpoints | Nee | 
| storageSyncServices/syncGroups/serverEndpoints | Nee | 
| storageSyncServices/workflows | Nee | 

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| managers | Ja | 

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| streamingjobs | Ja (Zie opmerking hieronder) | 
| streamingjobs/diagnosticSettings | Nee | 

> [!NOTE]
> U kunt een label niet toevoegen als streamingjobs wordt uitgevoerd. Stop de resource als een tag wilt toevoegen.

## <a name="microsoftsubscription"></a>Microsoft.Subscription
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| CreateSubscription | Nee | 
| SubscriptionDefinitions | Nee | 
| SubscriptionOperations | Nee | 

## <a name="microsoftsupport"></a>microsoft.support
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| supporttickets | Nee | 

## <a name="microsoftterraformoss"></a>Microsoft.TerraformOSS
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| providerRegistrations | Ja | 
| resources | Ja | 

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| Omgevingen | Ja | 
| environments/accessPolicies | Nee | 
| omgevingen/eventsources | Ja | 
| omgevingen/referenceDataSets | Ja | 

## <a name="microsoftvisualstudio"></a>microsoft.visualstudio
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| account | Ja | 
| -/ accountextensie | Ja | 
| account/project | Ja | 

## <a name="microsoftweb"></a>Microsoft.Web
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| apiManagementAccounts | Nee | 
| apiManagementAccounts/apiAcls | Nee | 
| apiManagementAccounts/apis | Nee | 
| apiManagementAccounts/apis/apiAcls | Nee | 
| apiManagementAccounts/apis/connectionAcls | Nee | 
| apiManagementAccounts/apis/connections | Nee | 
| apiManagementAccounts/apis/connections/connectionAcls | Nee | 
| apiManagementAccounts/apis/localizedDefinitions | Nee | 
| apiManagementAccounts/connectionAcls | Nee | 
| apiManagementAccounts/connections | Nee | 
| billingMeters | Nee | 
| Certificaten | Ja | 
| connectionGateways | Ja | 
| Verbindingen | Ja | 
| customApis | Ja | 
| deletedSites | Nee | 
| functions | Nee | 
| hostingEnvironments | Ja | 
| hostingEnvironments/multiRolePools | Nee | 
| hostingEnvironments/multiRolePools/instances | Nee | 
| hostingEnvironments/workerPools | Nee | 
| hostingEnvironments/workerPools/instances | Nee | 
| publishingUsers | Nee | 
| Aanbevelingen | Nee | 
| resourceHealthMetadata | Nee | 
| runtimes | Nee | 
| serverFarms | Ja | 
| serverFarms/werkrollen | Nee | 
| sites | Ja | 
| sites/domainOwnershipIdentifiers | Nee | 
| sites/hostNameBindings | Nee | 
| sites/instanties | Nee | 
| exemplaren-websites-extensies | Nee | 
| sites/premieraddons | Ja | 
| sites/aanbevelingen | Nee | 
| sites/resourceHealthMetadata | Nee | 
| sites/slots | Ja | 
| sites/sleuven/hostNameBindings | Nee | 
| sleuven-sites-instanties | Nee | 
| sites/sleuven/exemplaren/extensies | Nee | 
| sourceControls | Nee | 
| Valideren | Nee | 
| verifyHostingEnvironmentVnet | Nee | 

## <a name="microsoftwindowsdefenderatp"></a>Microsoft.WindowsDefenderATP
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| diagnosticSettings | Nee | 
| diagnosticSettingsCategories | Nee | 

## <a name="microsoftwindowsiot"></a>Microsoft.WindowsIoT
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| DeviceServices | Ja | 

## <a name="microsoftworkloadmonitor"></a>Microsoft.WorkloadMonitor
| Resourcetype | Modus voor volledige verwijdering |
| ------------- | ----------- |
| Onderdelen | Nee | 
| componentsSummary | Nee | 
| monitorInstances | Nee | 
| monitorInstancesSummary | Nee | 
| Monitors | Nee | 
| notificationSettings | Nee | 

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over tags toepassen op resources, [tags gebruiken om uw Azure-resources te organiseren](resource-group-using-tags.md).
