---
title: Azure Resource Manager 依資源類型的完整模式刪除
description: 顯示資源類型如何處理 Azure Resource Manager 範本中的完整模式刪除。
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 02/13/2019
ms.author: tomfitz
ms.openlocfilehash: b38b1af8c72b814813804a49642668c28f3898d6
ms.sourcegitcommit: f863ed1ba25ef3ec32bd188c28153044124cacbc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/15/2019
ms.locfileid: "56302620"
---
# <a name="deletion-of-azure-resources-for-complete-mode-deployments"></a>完整模式部署的 Azure 資源刪除
此文章說明當以完整模式部署的範本中沒有資源類型時，資源類型會如何處理刪除。

當類型不存在於使用完整模式部署的範本中時，會刪除以 `Yes` 標示的資源類型。 

以 `No` 標示的資源類型若沒有在範本中，則不會被自動刪除，但如果其父資源被刪除，則也會被刪除。 如需行為的完整描述，請參閱 [Azure Resource Manager 部署模式](deployment-modes.md)。

若要以逗點分隔值檔案的形式取得相同資料，請下載 [complete-mode-deletion.csv](https://github.com/tfitzmac/resource-capabilities/blob/master/complete-mode-deletion.csv)。

## <a name="microsoftaad"></a>Microsoft.AAD
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| DomainServices | yes | 
| DomainServices/oucontainer | 否 | 

## <a name="microsoftaadiam"></a>microsoft.aadiam
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| diagnosticSettings | 否 | 
| diagnosticSettingsCategories | 否 | 

## <a name="microsoftaddons"></a>Microsoft.Addons
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| supportProviders | 否 | 

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| aadsupportcases | 否 | 
| addsservices | 否 | 
| agents | 否 | 
| anonymousapiusers | 否 | 
| 組態 | 否 | 
| 記錄 | 否 | 
| reports | 否 | 
| 服務 | 否 | 

## <a name="microsoftadvisor"></a>Microsoft.Advisor
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| 組態 | 否 | 
| generateRecommendations | 否 | 
| Mahout | 否 | 
| suppressions | 否 | 

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| actionRules | 否 | 
| alerts | 否 | 
| alertsList | 否 | 
| alertsSummary | 否 | 
| alertsSummaryList | 否 | 
| smartDetectorAlertRules | 否 | 
| smartDetectorRuntimeEnvironments | 否 | 
| smartGroups | 否 | 

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| servers | yes | 

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| reportFeedback | 否 | 
| service | yes | 
| validateServiceName | 否 | 

## <a name="microsoftattestation"></a>Microsoft.Attestation
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| attestationProviders | 否 | 

## <a name="microsoftauthorization"></a>Microsoft.Authorization
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| classicAdministrators | 否 | 
| denyAssignments | 否 | 
| elevateAccess | 否 | 
| locks | 否 | 
| 權限 | 否 | 
| policyAssignments | 否 | 
| policyDefinitions | 否 | 
| policySetDefinitions | 否 | 
| providerOperations | 否 | 
| roleAssignments | 否 | 
| roleDefinitions | 否 | 

## <a name="microsoftautomation"></a>Microsoft.Automation
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| automationAccounts | yes | 
| automationAccounts/configurations | yes | 
| automationAccounts/jobs | 否 | 
| automationAccounts/runbooks | yes | 
| automationAccounts/softwareUpdateConfigurations | 否 | 
| automationAccounts/webhooks | 否 | 

## <a name="microsoftazuregeneva"></a>Microsoft.Azure.Geneva
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| environments | 否 | 
| environments/accounts | 否 | 
| environments/accounts/namespaces | 否 | 
| environments/accounts/namespaces/configurations | 否 | 

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| b2cDirectories | yes | 

## <a name="microsoftazurestack"></a>Microsoft.AzureStack
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| registrations | yes | 
| registrations/customerSubscriptions | 否 | 
| registrations/products | 否 | 

## <a name="microsoftbatch"></a>Microsoft.Batch
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| batchAccounts | yes | 

## <a name="microsoftbilling"></a>Microsoft.Billing
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| billingAccounts | 否 | 
| billingAccounts/billingProfiles | 否 | 
| billingAccounts/billingProfiles/billingSubscriptions | 否 | 
| billingAccounts/billingProfiles/invoices | 否 | 
| billingAccounts/billingProfiles/invoices/pricesheet | 否 | 
| billingAccounts/billingProfiles/operationStatus | 否 | 
| billingAccounts/billingProfiles/paymentMethods | 否 | 
| billingAccounts/billingProfiles/policies | 否 | 
| billingAccounts/billingProfiles/pricesheet | 否 | 
| billingAccounts/billingProfiles/products | 否 | 
| billingAccounts/billingProfiles/transactions | 否 | 
| billingAccounts/billingSubscriptions | 否 | 
| billingAccounts/departments | 否 | 
| billingAccounts/eligibleOffers | 否 | 
| billingAccounts/enrollmentAccounts | 否 | 
| billingAccounts/invoices | 否 | 
| billingAccounts/invoiceSections | 否 | 
| billingAccounts/invoiceSections/billingSubscriptions | 否 | 
| billingAccounts/invoiceSections/billingSubscriptions/transfer | 否 | 
| billingAccounts/invoiceSections/importRequests | 否 | 
| billingAccounts/invoiceSections/initiateImportRequest | 否 | 
| billingAccounts/invoiceSections/initiateTransfer | 否 | 
| billingAccounts/invoiceSections/operationStatus | 否 | 
| billingAccounts/invoiceSections/products | 否 | 
| billingAccounts/invoiceSections/transfers | 否 | 
| billingAccounts/products | 否 | 
| billingAccounts/projects | 否 | 
| billingAccounts/projects/billingSubscriptions | 否 | 
| billingAccounts/projects/importRequests | 否 | 
| billingAccounts/projects/initiateImportRequest | 否 | 
| billingAccounts/projects/operationStatus | 否 | 
| billingAccounts/projects/products | 否 | 
| billingAccounts/transactions | 否 | 
| billingPeriods | 否 | 
| BillingPermissions | 否 | 
| billingProperty | 否 | 
| BillingRoleAssignments | 否 | 
| BillingRoleDefinitions | 否 | 
| CreateBillingRoleAssignment | 否 | 
| departments | 否 | 
| enrollmentAccounts | 否 | 
| importRequests | 否 | 
| importRequests/acceptImportRequest | 否 | 
| importRequests/declineImportRequest | 否 | 
| invoices | 否 | 
| transfers | 否 | 
| transfers/acceptTransfer | 否 | 
| transfers/declineTransfer | 否 | 
| transfers/operationStatus | 否 | 
| usagePlans | 否 | 

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| mapApis | yes | 
| updateCommunicationPreference | 否 | 

## <a name="microsoftbiztalkservices"></a>Microsoft.BizTalkServices
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| BizTalk | yes | 

## <a name="microsoftblueprint"></a>Microsoft.Blueprint
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| blueprintAssignments | 否 | 
| blueprintAssignments/assignmentOperations | 否 | 
| blueprintAssignments/operations | 否 | 
| blueprints | 否 | 
| blueprints/artifacts | 否 | 
| blueprints/versions | 否 | 
| blueprints/versions/artifacts | 否 | 

## <a name="microsoftbotservice"></a>Microsoft.BotService
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| botServices | yes | 
| botServices/channels | 否 | 
| botServices/connections | 否 | 

## <a name="microsoftcache"></a>Microsoft.Cache
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| Redis | yes | 
| RedisConfigDefinition | 否 | 

## <a name="microsoftcapacity"></a>Microsoft.Capacity
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| appliedReservations | 否 | 
| calculatePrice | 否 | 
| catalogs | 否 | 
| commercialReservationOrders | 否 | 
| reservationOrders | 否 | 
| reservationOrders/calculateRefund | 否 | 
| reservationOrders/merge | 否 | 
| reservationOrders/reservations | 否 | 
| reservationOrders/reservations/revisions | 否 | 
| reservationOrders/return | 否 | 
| reservationOrders/split | 否 | 
| reservationOrders/swap | 否 | 
| reservations | 否 | 
| resources | 否 | 
| validateReservationOrder | 否 | 

## <a name="microsoftcdn"></a>Microsoft.Cdn
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| edgenodes | 否 | 
| 設定檔 | yes | 
| profiles/endpoints | yes | 
| profiles/endpoints/customdomains | 否 | 
| profiles/endpoints/origins | 否 | 
| validateProbe | 否 | 

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| certificateOrders | yes | 
| certificateOrders/certificates | 否 | 
| validateCertificateRegistrationInformation | 否 | 

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| capabilities | 否 | 
| domainNames | 否 | 
| domainNames/capabilities | 否 | 
| domainNames/internalLoadBalancers | 否 | 
| domainNames/serviceCertificates | 否 | 
| domainNames/slots | 否 | 
| domainNames/slots/roles | 否 | 
| moveSubscriptionResources | 否 | 
| operatingSystemFamilies | 否 | 
| operatingSystems | 否 | 
| quotas | 否 | 
| resourceTypes | 否 | 
| validateSubscriptionMoveAvailability | 否 | 
| virtualMachines | 否 | 
| virtualMachines/diagnosticSettings | 否 | 

## <a name="microsoftclassicinfrastructuremigrate"></a>Microsoft.ClassicInfrastructureMigrate
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| classicInfrastructureResources | 否 | 

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| capabilities | 否 | 
| expressRouteCrossConnections | 否 | 
| expressRouteCrossConnections/peerings | 否 | 
| gatewaySupportedDevices | 否 | 
| networkSecurityGroups | 否 | 
| quotas | 否 | 
| reservedIps | 否 | 
| virtualNetworks | 否 | 
| virtualNetworks/remoteVirtualNetworkPeeringProxies | 否 | 
| virtualNetworks/virtualNetworkPeerings | 否 | 

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| capabilities | 否 | 
| disks | 否 | 
| images | 否 | 
| osImages | 否 | 
| osPlatformImages | 否 | 
| publicImages | 否 | 
| quotas | 否 | 
| storageAccounts | 否 | 
| storageAccounts/services | 否 | 
| storageAccounts/services/diagnosticSettings | 否 | 
| storageAccounts/vmImages | 否 | 
| vmImages | 否 | 

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| accounts | yes | 

## <a name="microsoftcommerce"></a>Microsoft.Commerce
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| RateCard | 否 | 
| UsageAggregates | 否 | 

## <a name="microsoftcompute"></a>Microsoft.Compute
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| availabilitySets | yes | 
| disks | yes | 
| images | yes | 
| restorePointCollections | yes | 
| restorePointCollections/restorePoints | 否 | 
| sharedVMImages | yes | 
| sharedVMImages/versions | yes | 
| snapshots | yes | 
| virtualMachines | yes | 
| virtualMachines/diagnosticSettings | 否 | 
| virtualMachines/extensions | yes | 
| virtualMachineScaleSets | yes | 
| virtualMachineScaleSets/extensions | 否 | 
| virtualMachineScaleSets/networkInterfaces | 否 | 
| virtualMachineScaleSets/publicIPAddresses | 否 | 
| virtualMachineScaleSets/virtualMachines | 否 | 
| virtualMachineScaleSets/virtualMachines/networkInterfaces | 否 | 

## <a name="microsoftconsumption"></a>Microsoft.Consumption
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| AggregatedCost | 否 | 
| 餘額 | 否 | 
| 預算 | 否 | 
| Charges | 否 | 
| CostTags | 否 | 
| credits | 否 | 
| 活動 | 否 | 
| 預測 | 否 | 
| lots | 否 | 
| Marketplace | 否 | 
| Pricesheets | 否 | 
| products | 否 | 
| ReservationDetails | 否 | 
| ReservationRecommendations | 否 | 
| ReservationSummaries | 否 | 
| ReservationTransactions | 否 | 
| 標記 | 否 | 
| 詞彙 | 否 | 
| UsageDetails | 否 | 

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| containerGroups | yes | 
| serviceAssociationLinks | 否 | 

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| registries | yes | 
| registries/builds | 否 | 
| registries/builds/cancel | 否 | 
| registries/builds/getLogLink | 否 | 
| registries/buildTasks | yes | 
| registries/buildTasks/steps | 否 | 
| registries/eventGridFilters | 否 | 
| registries/getBuildSourceUploadUrl | 否 | 
| registries/GetCredentials | 否 | 
| registries/importImage | 否 | 
| registries/queueBuild | 否 | 
| registries/regenerateCredential | 否 | 
| registries/regenerateCredentials | 否 | 
| registries/replications | yes | 
| registries/runs | 否 | 
| registries/runs/cancel | 否 | 
| registries/scheduleRun | 否 | 
| registries/tasks | yes | 
| registries/updatePolicies | 否 | 
| registries/webhooks | yes | 
| registries/webhooks/getCallbackConfig | 否 | 
| registries/webhooks/ping | 否 | 

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| containerServices | yes | 
| managedClusters | yes | 

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| 應用程式所需 | yes | 
| updateCommunicationPreference | 否 | 

## <a name="microsoftcortanaanalytics"></a>Microsoft.CortanaAnalytics
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| accounts | yes | 

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| 警示 | 否 | 
| BillingAccounts | 否 | 
| 連接器 | yes | 
| 部門 | 否 | 
| 維度 | 否 | 
| EnrollmentAccounts | 否 | 
| 查詢 | 否 | 
| 註冊 | 否 | 
| Reportconfigs | 否 | 
| 報告 | 否 | 

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| hubs | yes | 
| hubs/authorizationPolicies | 否 | 
| hubs/connectors | 否 | 
| hubs/connectors/mappings | 否 | 
| hubs/interactions | 否 | 
| hubs/kpi | 否 | 
| hubs/links | 否 | 
| hubs/profiles | 否 | 
| hubs/roleAssignments | 否 | 
| hubs/roles | 否 | 
| hubs/suggestTypeSchema | 否 | 
| hubs/views | 否 | 
| hubs/widgetTypes | 否 | 

## <a name="microsoftdatabox"></a>Microsoft.DataBox
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| jobs | yes | 

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| DataBoxEdgeDevices | yes | 

## <a name="microsoftdatabricks"></a>Microsoft.Databricks
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| workspaces | yes | 
| workspaces/virtualNetworkPeerings | 否 | 

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| catalogs | yes | 

## <a name="microsoftdataconnect"></a>Microsoft.DataConnect
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| connectionManagers | yes | 

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| dataFactories | yes | 
| dataFactories/diagnosticSettings | 否 | 
| dataFactorySchema | 否 | 
| factories | yes | 
| factories/integrationRuntimes | 否 | 

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| accounts | yes | 
| accounts/dataLakeStoreAccounts | 否 | 
| accounts/storageAccounts | 否 | 
| accounts/storageAccounts/containers | 否 | 

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| accounts | yes | 
| accounts/eventGridFilters | 否 | 
| accounts/firewallRules | 否 | 

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| 服務 | yes | 
| services/projects | yes | 

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| servers | yes | 
| servers/recoverableServers | 否 | 
| servers/virtualNetworkRules | 否 | 

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| servers | yes | 
| servers/recoverableServers | 否 | 
| servers/virtualNetworkRules | 否 | 

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| servers | yes | 
| servers/advisors | 否 | 
| servers/queryTexts | 否 | 
| servers/recoverableServers | 否 | 
| servers/topQueryStatistics | 否 | 
| servers/virtualNetworkRules | 否 | 
| servers/waitStatistics | 否 | 

## <a name="microsoftdevices"></a>Microsoft.Devices
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| IotHubs | yes | 
| IotHubs/eventGridFilters | 否 | 
| ProvisioningServices | yes | 
| usages | 否 | 

## <a name="microsoftdevspaces"></a>Microsoft.DevSpaces
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| controllers | yes | 

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| labs | yes | 
| labs/serviceRunners | yes | 
| labs/virtualMachines | yes | 
| schedules | yes | 

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| databaseAccountNames | 否 | 
| databaseAccounts | yes | 

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| domains | yes | 
| domains/domainOwnershipIdentifiers | 否 | 
| generateSsoRequest | 否 | 
| topLevelDomains | 否 | 
| validateDomainRegistrationInformation | 否 | 

## <a name="microsoftdynamicslcs"></a>Microsoft.DynamicsLcs
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| lcsprojects | 否 | 
| lcsprojects/clouddeployments | 否 | 
| lcsprojects/connectors | 否 | 

## <a name="microsofteventgrid"></a>Microsoft.EventGrid
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| domains | yes | 
| domains/topics | 否 | 
| eventSubscriptions | 否 | 
| extensionTopics | 否 | 
| topics | yes | 
| topicTypes | 否 | 

## <a name="microsofteventhub"></a>Microsoft.EventHub
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| clusters | yes | 
| namespaces | yes | 
| namespaces/authorizationrules | 否 | 
| namespaces/disasterrecoveryconfigs | 否 | 
| namespaces/eventhubs | 否 | 
| namespaces/eventhubs/authorizationrules | 否 | 
| namespaces/eventhubs/consumergroups | 否 | 

## <a name="microsoftfeatures"></a>Microsoft.Features
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| 特性 | 否 | 
| 提供者 | 否 | 

## <a name="microsoftgallery"></a>Microsoft.Gallery
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| enroll | 否 | 
| galleryitems | 否 | 
| generateartifactaccessuri | 否 | 
| myareas | 否 | 
| myareas/areas | 否 | 
| myareas/areas/areas | 否 | 
| myareas/areas/areas/galleryitems | 否 | 
| myareas/areas/galleryitems | 否 | 
| myareas/galleryitems | 否 | 
| 註冊 | 否 | 
| resources | 否 | 
| retrieveresourcesbyid | 否 | 

## <a name="microsoftguestconfiguration"></a>Microsoft.GuestConfiguration
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| guestConfigurationAssignments | 否 | 
| software | 否 | 

## <a name="microsofthanaonazure"></a>Microsoft.HanaOnAzure
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| hanaInstances | yes | 

## <a name="microsofthdinsight"></a>Microsoft.HDInsight
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| clusters | yes | 
| clusters/applications | 否 | 

## <a name="microsoftimportexport"></a>Microsoft.ImportExport
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| jobs | yes | 

## <a name="microsoftinformationprotection"></a>Microsoft.InformationProtection
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| labelGroups | 否 | 
| labelGroups/labels | 否 | 
| labelGroups/labels/conditions | 否 | 
| labelGroups/labels/subLabels | 否 | 
| labelGroups/labels/subLabels/conditions | 否 | 

## <a name="microsoftinsights"></a>microsoft.insights
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| actiongroups | yes | 
| activityLogAlerts | yes | 
| alertrules | yes | 
| automatedExportSettings | 否 | 
| autoscalesettings | yes | 
| baseline | 否 | 
| calculatebaseline | 否 | 
| components | yes | 
| components/events | 否 | 
| components/pricingPlans | 否 | 
| components/query | 否 | 
| diagnosticSettings | 否 | 
| diagnosticSettingsCategories | 否 | 
| eventCategories | 否 | 
| eventtypes | 否 | 
| extendedDiagnosticSettings | 否 | 
| logDefinitions | 否 | 
| logprofiles | 否 | 
| 記錄 | 否 | 
| metricAlerts | yes |
| migrateToNewPricingModel | 否 | 
| myWorkbooks | 否 | 
| 查詢 | 否 | 
| rollbackToLegacyPricingModel | 否 | 
| scheduledqueryrules | yes | 
| vmInsightsOnboardingStatuses | 否 | 
| webtests | yes | 
| workbooks | yes | 

## <a name="microsoftintune"></a>Microsoft.Intune
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| diagnosticSettings | 否 | 
| diagnosticSettingsCategories | 否 | 

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| IoTApps | yes | 

## <a name="microsoftiotspaces"></a>Microsoft.IoTSpaces
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| 圖形 | yes | 

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| deletedVaults | 否 | 
| vaults | yes | 
| vaults/accessPolicies | 否 | 
| vaults/secrets | 否 | 

## <a name="microsoftkusto"></a>Microsoft.Kusto
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| clusters | yes | 
| clusters/databases | 否 | 
| clusters/databases/dataconnections | 否 | 
| clusters/databases/eventhubconnections | 否 | 

## <a name="microsoftlabservices"></a>Microsoft.LabServices
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| labaccounts | yes | 
| users | 否 | 

## <a name="microsoftlocationbasedservices"></a>Microsoft.LocationBasedServices
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| accounts | yes | 

## <a name="microsoftlocationservices"></a>Microsoft.LocationServices
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| accounts | yes | 

## <a name="microsoftloganalytics"></a>Microsoft.LogAnalytics
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| 記錄 | 否 | 

## <a name="microsoftlogic"></a>Microsoft.Logic
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| integrationAccounts | yes | 
| workflows | yes | 

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| commitmentPlans | yes | 
| webServices | yes | 
| 工作區 | yes | 

## <a name="microsoftmachinelearningexperimentation"></a>Microsoft.MachineLearningExperimentation
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| accounts | yes | 
| accounts/workspaces | yes | 
| accounts/workspaces/projects | yes | 
| teamAccounts | yes | 
| teamAccounts/workspaces | yes | 
| teamAccounts/workspaces/projects | yes | 

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| accounts | yes | 

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| workspaces | yes | 
| workspaces/computes | 否 | 

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| Identities | 否 | 
| userAssignedIdentities | yes | 

## <a name="microsoftmanagement"></a>Microsoft.Management
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| getEntities | 否 | 
| managementGroups | 否 | 
| resources | 否 | 
| startTenantBackfill | 否 | 
| tenantBackfillStatus | 否 | 

## <a name="microsoftmaps"></a>Microsoft.Maps
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| accounts | yes | 
| accounts/eventGridFilters | 否 | 

## <a name="microsoftmarketplace"></a>Microsoft.Marketplace
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| offers | 否 | 
| offerTypes | 否 | 
| offerTypes/publishers | 否 | 
| offerTypes/publishers/offers | 否 | 
| offerTypes/publishers/offers/plans | 否 | 
| offerTypes/publishers/offers/plans/agreements | 否 | 
| offerTypes/publishers/offers/plans/configs | 否 | 
| offerTypes/publishers/offers/plans/configs/importImage | 否 | 
| privategalleryitems | 否 | 
| products | 否 | 

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| classicDevServices | yes | 
| updateCommunicationPreference | 否 | 

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceOrdering
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| agreements | 否 | 
| offertypes | 否 | 

## <a name="microsoftmedia"></a>Microsoft.Media
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| mediaservices | yes | 
| mediaservices/accountFilters | 否 | 
| mediaservices/assets | 否 | 
| mediaservices/assets/assetFilters | 否 | 
| mediaservices/contentKeyPolicies | 否 | 
| mediaservices/eventGridFilters | 否 | 
| mediaservices/liveEventOperations | 否 | 
| mediaservices/liveEvents | yes | 
| mediaservices/liveEvents/liveOutputs | 否 | 
| mediaservices/liveOutputOperations | 否 | 
| mediaservices/streamingEndpointOperations | 否 | 
| mediaservices/streamingEndpoints | yes | 
| mediaservices/streamingLocators | 否 | 
| mediaservices/streamingPolicies | 否 | 
| mediaservices/transforms | 否 | 
| mediaservices/transforms/jobs | 否 | 

## <a name="microsoftmigrate"></a>Microsoft.Migrate
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| projects | yes | 

## <a name="microsoftnetwork"></a>Microsoft.Network
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| applicationGateways | yes | 
| applicationSecurityGroups | yes | 
| azureFirewallFqdnTags | 否 | 
| azureFirewalls | yes | 
| bgpServiceCommunities | 否 | 
| connections | yes | 
| ddosCustomPolicies | yes | 
| ddosProtectionPlans | yes | 
| dnsOperationStatuses | 否 | 
| dnszones | yes | 
| dnszones/A | 否 | 
| dnszones/AAAA | 否 | 
| dnszones/all | 否 | 
| dnszones/CAA | 否 | 
| dnszones/CNAME | 否 | 
| dnszones/MX | 否 | 
| dnszones/NS | 否 | 
| dnszones/PTR | 否 | 
| dnszones/recordsets | 否 | 
| dnszones/SOA | 否 | 
| dnszones/SRV | 否 | 
| dnszones/TXT | 否 | 
| expressRouteCircuits | yes | 
| expressRouteServiceProviders | 否 | 
| frontdoors | yes | 
| frontdoorWebApplicationFirewallPolicies | yes | 
| getDnsResourceReference | 否 | 
| interfaceEndpoints | yes | 
| internalNotify | 否 | 
| loadBalancers | yes | 
| localNetworkGateways | yes | 
| natGateways | yes | 
| networkIntentPolicies | yes | 
| networkInterfaces | yes | 
| networkProfiles | yes | 
| networkSecurityGroups | yes | 
| networkWatchers | yes | 
| networkWatchers/connectionMonitors | yes | 
| networkWatchers/lenses | yes | 
| networkWatchers/pingMeshes | yes | 
| privateLinkServices | yes | 
| publicIPAddresses | yes | 
| publicIPPrefixes | yes | 
| routeFilters | yes | 
| routeTables | yes | 
| serviceEndpointPolicies | yes | 
| trafficManagerGeographicHierarchies | 否 | 
| trafficmanagerprofiles | yes | 
| trafficmanagerprofiles/heatMaps | 否 | 
| virtualHubs | yes | 
| virtualNetworkGateways | yes | 
| virtualNetworks | yes | 
| virtualNetworkTaps | yes | 
| virtualWans | yes | 
| vpnGateways | yes | 
| vpnSites | yes | 
| webApplicationFirewallPolicies | yes | 

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| namespaces | yes | 
| namespaces/notificationHubs | yes | 

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| devices | 否 | 
| linkTargets | 否 | 
| storageInsightConfigs | 否 | 
| workspaces | yes | 
| workspaces/dataSources | 否 | 
| workspaces/linkedServices | 否 | 
| workspaces/query | 否 | 

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| managementassociations | 否 | 
| managementconfigurations | yes | 
| solutions | yes | 
| 檢視 | yes | 

## <a name="microsoftpolicyinsights"></a>Microsoft.PolicyInsights
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| policyEvents | 否 | 
| policyStates | 否 | 
| policyTrackedResources | 否 | 
| remediations | 否 | 

## <a name="microsoftportal"></a>Microsoft.Portal
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| consoles | 否 | 
| dashboards | yes | 
| userSettings | 否 | 

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| workspaceCollections | yes | 

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| capacities | yes | 

## <a name="microsoftprojectoxford"></a>Microsoft.ProjectOxford
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| accounts | yes | 

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| backupProtectedItems | 否 | 
| vaults | yes | 

## <a name="microsoftrelay"></a>Microsoft.Relay
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| namespaces | yes | 
| namespaces/authorizationrules | 否 | 
| namespaces/hybridconnections | 否 | 
| namespaces/hybridconnections/authorizationrules | 否 | 
| namespaces/wcfrelays | 否 | 
| namespaces/wcfrelays/authorizationrules | 否 | 

## <a name="microsoftresourcegraph"></a>Microsoft.ResourceGraph
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| resources | 否 | 
| subscriptionsStatus | 否 | 

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| availabilityStatuses | 否 | 
| childAvailabilityStatuses | 否 | 
| childResources | 否 | 
| 活動 | 否 | 
| impactedResources | 否 | 
| 通知 | 否 | 

## <a name="microsoftresources"></a>Microsoft.Resources
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| deployments | 否 | 
| deployments/operations | 否 | 
| 連結 | 否 | 
| notifyResourceJobs | 否 | 
| 提供者 | 否 | 
| resourceGroups | 否 | 
| resources | 否 | 
| subscriptions | 否 | 
| subscriptions/providers | 否 | 
| subscriptions/resourceGroups | 否 | 
| subscriptions/resourcegroups/resources | 否 | 
| subscriptions/resources | 否 | 
| subscriptions/tagnames | 否 | 
| subscriptions/tagNames/tagValues | 否 | 
| tenants | 否 | 

## <a name="microsoftsaas"></a>Microsoft.SaaS
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| 應用程式所需 | yes | 
| saasresources | 否 | 

## <a name="microsoftscheduler"></a>Microsoft.Scheduler
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| flows | yes | 
| jobcollections | yes | 

## <a name="microsoftsearch"></a>Microsoft.Search
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| resourceHealthMetadata | 否 | 
| searchServices | yes | 

## <a name="microsoftsecurity"></a>Microsoft.Security
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| advancedThreatProtectionSettings | 否 | 
| alerts | 否 | 
| allowedConnections | 否 | 
| appliances | 否 | 
| applicationWhitelistings | 否 | 
| AutoProvisioningSettings | 否 | 
| Compliances | 否 | 
| dataCollectionAgents | 否 | 
| discoveredSecuritySolutions | 否 | 
| externalSecuritySolutions | 否 | 
| InformationProtectionPolicies | 否 | 
| jitNetworkAccessPolicies | 否 | 
| monitoring | 否 | 
| monitoring/antimalware | 否 | 
| monitoring/baseline | 否 | 
| monitoring/patch | 否 | 
| 原則 | 否 | 
| pricings | 否 | 
| securityContacts | 否 | 
| securitySolutions | 否 | 
| securitySolutionsReferenceData | 否 | 
| securityStatus | 否 | 
| securityStatus/endpoints | 否 | 
| securityStatus/subnets | 否 | 
| securityStatus/virtualMachines | 否 | 
| securityStatuses | 否 | 
| securityStatusesSummaries | 否 | 
| settings | 否 | 
| 工作 | 否 | 
| topologies | 否 | 
| workspaceSettings | 否 | 

## <a name="microsoftsecuritygraph"></a>Microsoft.SecurityGraph
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| diagnosticSettings | 否 | 
| diagnosticSettingsCategories | 否 | 

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| namespaces | yes | 
| namespaces/authorizationrules | 否 | 
| namespaces/disasterrecoveryconfigs | 否 | 
| namespaces/eventgridfilters | 否 | 
| namespaces/queues | 否 | 
| namespaces/queues/authorizationrules | 否 | 
| namespaces/topics | 否 | 
| namespaces/topics/authorizationrules | 否 | 
| namespaces/topics/subscriptions | 否 | 
| namespaces/topics/subscriptions/rules | 否 | 
| premiumMessagingRegions | 否 | 

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| clusters | yes | 
| clusters/applications | 否 | 

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| 應用程式所需 | yes | 
| gateways | yes | 
| networks | yes | 
| 密碼 | yes | 
| 磁碟區 | yes | 

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| SignalR | yes | 

## <a name="microsoftsolutions"></a>Microsoft.Solutions
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| applianceDefinitions | yes | 
| appliances | yes | 
| applicationDefinitions | yes | 
| 應用程式所需 | yes | 
| jitRequests | yes | 

## <a name="microsoftsql"></a>Microsoft.SQL
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| managedInstances | yes |
| managedInstances/databases | 是 (請參閱下方注意事項) |
| managedInstances/databases/backupShortTermRetentionPolicies | 否 |
| managedInstances/databases/schemas/tables/columns/sensitivityLabels | 否 |
| managedInstances/databases/vulnerabilityAssessments | 否 |
| managedInstances/databases/vulnerabilityAssessments/rules/baselines | 否 |
| managedInstances/encryptionProtector | 否 |
| managedInstances/keys | 否 |
| managedInstances/restorableDroppedDatabases/backupShortTermRetentionPolicies | 否 |
| managedInstances/vulnerabilityAssessments | 否 |
| servers | yes | 
| servers/administrators | 否 | 
| servers/communicationLinks | 否 | 
| servers/databases | 是 (請參閱下方注意事項) | 
| servers/encryptionProtector | 否 | 
| servers/firewallRules | 否 | 
| servers/keys | 否 | 
| servers/restorableDroppedDatabases | 否 | 
| servers/serviceobjectives | 否 | 
| servers/tdeCertificates | 否 | 

> [!NOTE]
> Master 資料庫不支援標籤，但其他資料庫 (包括「資料倉儲」資料庫) 則可支援標籤。


## <a name="microsoftsqlvirtualmachine"></a>Microsoft.SqlVirtualMachine
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| SqlVirtualMachineGroups | yes | 
| SqlVirtualMachineGroups/AvailabilityGroupListeners | 否 | 
| SqlVirtualMachines | yes | 

## <a name="microsoftstorage"></a>Microsoft.Storage
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| storageAccounts | yes | 
| storageAccounts/blobServices | 否 | 
| storageAccounts/fileServices | 否 | 
| storageAccounts/queueServices | 否 | 
| storageAccounts/services | 否 | 
| storageAccounts/tableServices | 否 | 
| usages | 否 | 

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| storageSyncServices | yes | 
| storageSyncServices/registeredServers | 否 | 
| storageSyncServices/syncGroups | 否 | 
| storageSyncServices/syncGroups/cloudEndpoints | 否 | 
| storageSyncServices/syncGroups/serverEndpoints | 否 | 
| storageSyncServices/workflows | 否 | 

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| managers | yes | 

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| streamingjobs | 是 (請參閱下方注意事項) | 
| streamingjobs/diagnosticSettings | 否 | 

> [!NOTE]
> 您無法在 streamingjobs 執行時新增標記。 阻止資源新增標記。

## <a name="microsoftsubscription"></a>Microsoft.Subscription
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| CreateSubscription | 否 | 
| SubscriptionDefinitions | 否 | 
| SubscriptionOperations | 否 | 

## <a name="microsoftsupport"></a>microsoft.support
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| supporttickets | 否 | 

## <a name="microsoftterraformoss"></a>Microsoft.TerraformOSS
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| providerRegistrations | yes | 
| resources | yes | 

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| environments | yes | 
| environments/accessPolicies | 否 | 
| environments/eventsources | yes | 
| environments/referenceDataSets | yes | 

## <a name="microsoftvisualstudio"></a>microsoft.visualstudio
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| 帳戶 | yes | 
| account/extension | yes | 
| account/project | yes | 

## <a name="microsoftweb"></a>Microsoft.Web
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| apiManagementAccounts | 否 | 
| apiManagementAccounts/apiAcls | 否 | 
| apiManagementAccounts/apis | 否 | 
| apiManagementAccounts/apis/apiAcls | 否 | 
| apiManagementAccounts/apis/connectionAcls | 否 | 
| apiManagementAccounts/apis/connections | 否 | 
| apiManagementAccounts/apis/connections/connectionAcls | 否 | 
| apiManagementAccounts/apis/localizedDefinitions | 否 | 
| apiManagementAccounts/connectionAcls | 否 | 
| apiManagementAccounts/connections | 否 | 
| billingMeters | 否 | 
| certificates | yes | 
| connectionGateways | yes | 
| connections | yes | 
| customApis | yes | 
| deletedSites | 否 | 
| functions | 否 | 
| hostingEnvironments | yes | 
| hostingEnvironments/multiRolePools | 否 | 
| hostingEnvironments/multiRolePools/instances | 否 | 
| hostingEnvironments/workerPools | 否 | 
| hostingEnvironments/workerPools/instances | 否 | 
| publishingUsers | 否 | 
| Mahout | 否 | 
| resourceHealthMetadata | 否 | 
| runtimes | 否 | 
| serverFarms | yes | 
| serverFarms/workers | 否 | 
| sites | yes | 
| sites/domainOwnershipIdentifiers | 否 | 
| sites/hostNameBindings | 否 | 
| sites/instances | 否 | 
| sites/instances/extensions | 否 | 
| sites/premieraddons | yes | 
| sites/recommendations | 否 | 
| sites/resourceHealthMetadata | 否 | 
| sites/slots | yes | 
| sites/slots/hostNameBindings | 否 | 
| sites/slots/instances | 否 | 
| sites/slots/instances/extensions | 否 | 
| sourceControls | 否 | 
| validate | 否 | 
| verifyHostingEnvironmentVnet | 否 | 

## <a name="microsoftwindowsdefenderatp"></a>Microsoft.WindowsDefenderATP
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| diagnosticSettings | 否 | 
| diagnosticSettingsCategories | 否 | 

## <a name="microsoftwindowsiot"></a>Microsoft.WindowsIoT
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| DeviceServices | yes | 

## <a name="microsoftworkloadmonitor"></a>Microsoft.WorkloadMonitor
| 資源類型 | 完整模式刪除 |
| ------------- | ----------- |
| components | 否 | 
| componentsSummary | 否 | 
| monitorInstances | 否 | 
| monitorInstancesSummary | 否 | 
| monitors | 否 | 
| notificationSettings | 否 | 

## <a name="next-steps"></a>後續步驟
若要了解如何將標記套用至資源，請參閱[使用標記來組織您的 Azure 資源](resource-group-using-tags.md)。
