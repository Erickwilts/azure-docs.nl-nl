---
title: Azure Virtual Machines-implementatie voor SAP NetWeaver | Microsoft Docs
description: Meer informatie over het implementeren van SAP-software op virtuele Linux-machines in Azure.
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: MSSedusch
manager: gwallace
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: 1c4f1951-3613-4a5a-a0af-36b85750c84e
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/26/2018
ms.author: sedusch
ms.openlocfilehash: 23f2d9add5838fc0c9ac5764921a2f3cc6ae7ab7
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/12/2019
ms.locfileid: "67709902"
---
# <a name="azure-virtual-machines-deployment-for-sap-netweaver"></a>Azure Virtual Machines-implementatie voor SAP NetWeaver

[767598]:https://launchpad.support.sap.com/#/notes/767598
[773830]:https://launchpad.support.sap.com/#/notes/773830
[826037]:https://launchpad.support.sap.com/#/notes/826037
[965908]:https://launchpad.support.sap.com/#/notes/965908
[1031096]: https://launchpad.support.sap.com/#/notes/1031096
[1139904]:https://launchpad.support.sap.com/#/notes/1139904
[1173395]:https://launchpad.support.sap.com/#/notes/1173395
[1245200]:https://launchpad.support.sap.com/#/notes/1245200
[1409604]: https://launchpad.support.sap.com/#/notes/1409604
[1558958]:https://launchpad.support.sap.com/#/notes/1558958
[1585981]:https://launchpad.support.sap.com/#/notes/1585981
[1588316]:https://launchpad.support.sap.com/#/notes/1588316
[1590719]:https://launchpad.support.sap.com/#/notes/1590719
[1597355]: https://launchpad.support.sap.com/#/notes/1597355
[1605680]:https://launchpad.support.sap.com/#/notes/1605680
[1619720]: https://launchpad.support.sap.com/#/notes/1619720
[1619726]:https://launchpad.support.sap.com/#/notes/1619726
[1619967]:https://launchpad.support.sap.com/#/notes/1619967
[1750510]:https://launchpad.support.sap.com/#/notes/1750510
[1752266]:https://launchpad.support.sap.com/#/notes/1752266
[1757924]:https://launchpad.support.sap.com/#/notes/1757924
[1757928]:https://launchpad.support.sap.com/#/notes/1757928
[1758182]:https://launchpad.support.sap.com/#/notes/1758182
[1758496]:https://launchpad.support.sap.com/#/notes/1758496
[1772688]:https://launchpad.support.sap.com/#/notes/1772688
[1814258]:https://launchpad.support.sap.com/#/notes/1814258
[1882376]:https://launchpad.support.sap.com/#/notes/1882376
[1909114]:https://launchpad.support.sap.com/#/notes/1909114
[1922555]:https://launchpad.support.sap.com/#/notes/1922555
[1928533]: https://launchpad.support.sap.com/#/notes/1928533
[1941500]:https://launchpad.support.sap.com/#/notes/1941500
[1956005]:https://launchpad.support.sap.com/#/notes/1956005
[1973241]:https://launchpad.support.sap.com/#/notes/1973241
[1984787]: https://launchpad.support.sap.com/#/notes/1984787
[1999351]: https://launchpad.support.sap.com/#/notes/1999351
[2002167]: https://launchpad.support.sap.com/#/notes/2002167
[2015553]: https://launchpad.support.sap.com/#/notes/2015553
[2039619]:https://launchpad.support.sap.com/#/notes/2039619
[2069760]: https://launchpad.support.sap.com/#/notes/2069760
[2121797]:https://launchpad.support.sap.com/#/notes/2121797
[2134316]:https://launchpad.support.sap.com/#/notes/2134316
[2178632]: https://launchpad.support.sap.com/#/notes/2178632
[2191498]: https://launchpad.support.sap.com/#/notes/2191498
[2233094]:https://launchpad.support.sap.com/#/notes/2233094
[2243692]: https://launchpad.support.sap.com/#/notes/2243692
[2367194]:https://launchpad.support.sap.com/#/notes/2367194

[azure-cli]:../../../cli-install-nodejs.md
[azure-cli-2]:https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest
[azure-portal]:https://portal.azure.com
[azure-ps]:/powershell/azureps-cmdlets-docs
[azure-quickstart-templates-github]:https://github.com/Azure/azure-quickstart-templates
[azure-script-ps]:https://go.microsoft.com/fwlink/p/?LinkID=395017
[azure-subscription-service-limits]:../../../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../../../azure-subscription-service-limits.md#subscription-limits

[dbms-guide]:dbms-guide.md (Azure Virtual Machines DBMS-implementatie voor SAP)
[dbms-guide-2.1]:dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f (Caching voor Vm's en Vhd's)
[dbms-guide-2.2]:dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91 (Software-RAID)
[dbms-guide-2.3]:dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152 (Microsoft Azure Storage)
[dbms-guide-2]:dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64 (Structuur van een RDBMS-implementatie)
[dbms-guide-3]:dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3 (Hoge Beschik baarheid en herstel na nood geval met virtuele Azure-machines)
[dbms-guide-5.5.1]:dbms-guide.md#0fef0e79-d3fe-4ae2-85af-73666a6f7268 (SQL Server 2012 SP1 CU4 en hoger)
[dbms-guide-5.5.2]:dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b (SQL Server 2012 SP1 CU3 en eerdere versies)
[dbms-guide-5.6]:dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8 (Een SQL Server-installatie kopie van Azure Marketplace gebruiken)
[dbms-guide-5.8]:dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30 (Algemene SQL Server voor SAP on Azure samen vatting)
[dbms-guide-5]:dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737 (Details SQL Server RDBMS)
[dbms-guide-8.4.1]:dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573 (Opslag configuratie)
[dbms-guide-8.4.2]:dbms-guide.md#23c78d3b-ca5a-4e72-8a24-645d141a3f5d (Back-ups maken en herstellen)
[dbms-guide-8.4.3]:dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c (Prestatie overwegingen voor back-up en herstel)
[dbms-guide-8.4.4]:dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818 (Daarenteg)
[dbms-guide-900-sap-cache-server-on-premises]:dbms-guide.md#642f746c-e4d4-489d-bf63-73e80177a0a8

[dbms-guide-figure-100]:media/virtual-machines-shared-sap-dbms-guide/100_storage_account_types.png
[dbms-guide-figure-200]:media/virtual-machines-shared-sap-dbms-guide/200-ha-set-for-dbms-ha.png
[dbms-guide-figure-300]:media/virtual-machines-shared-sap-dbms-guide/300-reference-config-iaas.png
[dbms-guide-figure-400]:media/virtual-machines-shared-sap-dbms-guide/400-sql-2012-backup-to-blob-storage.png
[dbms-guide-figure-500]:media/virtual-machines-shared-sap-dbms-guide/500-sql-2012-backup-to-blob-storage-different-containers.png
[dbms-guide-figure-600]:media/virtual-machines-shared-sap-dbms-guide/600-iaas-maxdb.png
[dbms-guide-figure-700]:media/virtual-machines-shared-sap-dbms-guide/700-livecach-prod.png
[dbms-guide-figure-800]:media/virtual-machines-shared-sap-dbms-guide/800-azure-vm-sap-content-server.png
[dbms-guide-figure-900]:media/virtual-machines-shared-sap-dbms-guide/900-sap-cache-server-on-premises.png

[deployment-guide]:deployment-guide.md (Azure Virtual Machines-implementatie voor SAP)
[deployment-guide-2.2]:deployment-guide.md#42ee2bdb-1efc-4ec7-ab31-fe4c22769b94 (SAP-bronnen)
[deployment-guide-3.1.2]:deployment-guide.md#3688666f-281f-425b-a312-a77e7db2dfab (Een virtuele machine implementeren met behulp van een aangepaste installatie kopie)
[deployment-guide-3.2]:deployment-guide.md#db477013-9060-4602-9ad4-b0316f8bb281 (Scenario 1: Een virtuele machine implementeren vanuit Azure Marketplace voor SAP)
[deployment-guide-3.3]:deployment-guide.md#54a1fc6d-24fd-4feb-9c57-ac588a55dff2 (Scenario 2: Een virtuele machine implementeren met een aangepaste installatie kopie voor SAP)
[deployment-guide-3.4]:deployment-guide.md#a9a60133-a763-4de8-8986-ac0fa33aa8c1 (Scenario 3: Een virtuele machine verplaatsen van on-premises met een niet-gegeneraliseerde virtuele harde schijf met SAP)
[deployment-guide-3]:deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e (Implementatie scenario's van Vm's voor SAP op Microsoft Azure)
[deployment-guide-4.1]:deployment-guide.md#604bcec2-8b6e-48d2-a944-61b0f5dee2f7 (Azure PowerShell-cmdlets implementeren)
[deployment-guide-4.2]:deployment-guide.md#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e (SAP-relevante Power shell-cmdlets downloaden en importeren)
[deployment-guide-4.3]:deployment-guide.md#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc (Een VM toevoegen aan een on-premises domein-alleen Windows)
[deployment-guide-4.4.2]:deployment-guide.md#6889ff12-eaaf-4f3c-97e1-7c9edc7f7542 (Linux)
[deployment-guide-4.4]:deployment-guide.md#c7cbb0dc-52a4-49db-8e03-83e7edc2927d (De Azure VM-agent downloaden, installeren en inschakelen)
[deployment-guide-4.5.1]:deployment-guide.md#987cf279-d713-4b4c-8143-6b11589bb9d4 (Azure PowerShell)
[deployment-guide-4.5.2]:deployment-guide.md#408f3779-f422-4413-82f8-c57a23b4fc2f (Azure CLI)
[deployment-guide-4.5]:deployment-guide.md#d98edcd3-f2a1-49f7-b26a-07448ceb60ca (De uitgebreide bewakings extensie van Azure configureren voor SAP)
[deployment-guide-5.1]:deployment-guide.md#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2 (Gereedheids controle voor uitgebreide Azure-bewaking voor SAP)
[deployment-guide-5.2]:deployment-guide.md#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1 (Status controle voor de infra structuur van Azure monitoring)
[deployment-guide-5.3]:deployment-guide.md#fe25a7da-4e4e-4388-8907-8abc2d33cfd8 (Problemen met Azure-bewaking voor SAP oplossen)

[deployment-guide-configure-monitoring-scenario-1]:deployment-guide.md#ec323ac3-1de9-4c3a-b770-4ff701def65b (Bewaking configureren)
[deployment-guide-configure-proxy]:deployment-guide.md#baccae00-6f79-4307-ade4-40292ce4e02d (De proxy configureren)
[deployment-guide-figure-100]:media/virtual-machines-shared-sap-deployment-guide/100-deploy-vm-image.png
[deployment-guide-figure-1000]:media/virtual-machines-shared-sap-deployment-guide/1000-service-properties.png
[deployment-guide-figure-11]:deployment-guide.md#figure-11
[deployment-guide-figure-1100]:media/virtual-machines-shared-sap-deployment-guide/1100-azperflib.png
[deployment-guide-figure-1200]:media/virtual-machines-shared-sap-deployment-guide/1200-cmd-test-login.png
[deployment-guide-figure-1300]:media/virtual-machines-shared-sap-deployment-guide/1300-cmd-test-executed.png
[deployment-guide-figure-14]:deployment-guide.md#figure-14
[deployment-guide-figure-1400]:media/virtual-machines-shared-sap-deployment-guide/1400-azperflib-error-servicenotstarted.png
[deployment-guide-figure-300]:media/virtual-machines-shared-sap-deployment-guide/300-deploy-private-image.png
[deployment-guide-figure-400]:media/virtual-machines-shared-sap-deployment-guide/400-deploy-using-disk.png
[deployment-guide-figure-5]:deployment-guide.md#figure-5
[deployment-guide-figure-50]:media/virtual-machines-shared-sap-deployment-guide/50-forced-tunneling-suse.png
[deployment-guide-figure-500]:media/virtual-machines-shared-sap-deployment-guide/500-install-powershell.png
[deployment-guide-figure-6]:deployment-guide.md#figure-6
[deployment-guide-figure-600]:media/virtual-machines-shared-sap-deployment-guide/600-powershell-version.png
[deployment-guide-figure-7]:deployment-guide.md#figure-7
[deployment-guide-figure-700]:media/virtual-machines-shared-sap-deployment-guide/700-install-powershell-installed.png
[deployment-guide-figure-760]:media/virtual-machines-shared-sap-deployment-guide/760-azure-cli-version.png
[deployment-guide-figure-900]:media/virtual-machines-shared-sap-deployment-guide/900-cmd-update-executed.png
[deployment-guide-figure-azure-cli-installed]:deployment-guide.md#402488e5-f9bb-4b29-8063-1c5f52a892d0
[deployment-guide-figure-azure-cli-version]:deployment-guide.md#0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda
[deployment-guide-install-vm-agent-windows]:deployment-guide.md#b2db5c9a-a076-42c6-9835-16945868e866
[deployment-guide-troubleshooting-chapter]:deployment-guide.md#564adb4f-5c95-4041-9616-6635e83a810b (Controles en probleem oplossing voor het instellen van end-to-end-bewaking)

[deploy-template-cli]:../../../resource-group-template-deploy-cli.md
[deploy-template-portal]:../../../resource-group-template-deploy-portal.md
[deploy-template-powershell]:../../../resource-group-template-deploy.md

[dr-guide-classic]:https://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:get-started.md
[getting-started-dbms]:get-started.md#1343ffe1-8021-4ce6-a08d-3a1553a4db82
[getting-started-deployment]:get-started.md#6aadadd2-76b5-46d8-8713-e8d63630e955
[getting-started-planning]:get-started.md#3da0389e-708b-4e82-b2a2-e92f132df89c

[getting-started-windows-classic]:../../virtual-machines-windows-classic-sap-get-started.md
[getting-started-windows-classic-dbms]:../../virtual-machines-windows-classic-sap-get-started.md#c5b77a14-f6b4-44e9-acab-4d28ff72a930
[getting-started-windows-classic-deployment]:../../virtual-machines-windows-classic-sap-get-started.md#f84ea6ce-bbb4-41f7-9965-34d31b0098ea
[getting-started-windows-classic-dr]:../../virtual-machines-windows-classic-sap-get-started.md#cff10b4a-01a5-4dc3-94b6-afb8e55757d3
[getting-started-windows-classic-ha-sios]:../../virtual-machines-windows-classic-sap-get-started.md#4bb7512c-0fa0-4227-9853-4004281b1037
[getting-started-windows-classic-planning]:../../virtual-machines-windows-classic-sap-get-started.md#f2a5e9d8-49e4-419e-9900-af783173481c

[ha-guide-classic]:https://go.microsoft.com/fwlink/?LinkId=613056

[install-extension-cli]:virtual-machines-linux-enable-aem.md

[Logo_Linux]:media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:media/virtual-machines-shared-sap-shared/Windows.png

[msdn-set-Azvmaemextension]:https://docs.microsoft.com/powershell/module/az.compute/set-azvmaemextension

[planning-guide]:planning-guide.md (Azure Virtual Machines planning en implementatie voor SAP)
[planning-guide-1.2]:planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff (Resources)
[planning-guide-11]:planning-guide.md#7cf991a1-badd-40a9-944e-7baae842a058 (Hoge Beschik baarheid en herstel na nood gevallen voor SAP NetWeaver die op Azure wordt uitgevoerd Virtual Machines)
[planning-guide-11.4.1]:planning-guide.md#5d9d36f9-9058-435d-8367-5ad05f00de77 (Hoge Beschik baarheid voor SAP-toepassings servers)
[planning-guide-11.5]:planning-guide.md#4e165b58-74ca-474f-a7f4-5e695a93204f (Automatisch starten voor SAP-instanties gebruiken)
[planning-guide-2.1]:planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803 (Cloud-only-implementaties van virtuele machines in azure zonder afhankelijkheden van het on-premises klanten netwerk)
[planning-guide-2.2]:planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10 (Cross-premises-implementatie van één of meerdere SAP-Vm's in azure volledig geïntegreerd met het on-premises netwerk)
[planning-guide-3.1]:planning-guide.md#be80d1b9-a463-4845-bd35-f4cebdb5424a (Azure-regio's)
[planning-guide-3.2.1]:planning-guide.md#df49dc09-141b-4f34-a4a2-990913b30358 (Fout domeinen)
[planning-guide-3.2.2]:planning-guide.md#fc1ac8b2-e54a-487c-8581-d3cc6625e560 (Upgrade domeinen)
[planning-guide-3.2.3]:planning-guide.md#18810088-f9be-4c97-958a-27996255c665 (Azure-beschikbaarheids sets)
[planning-guide-3.2]:planning-guide.md#8d8ad4b8-6093-4b91-ac36-ea56d80dbf77 (Concept van Microsoft Azure virtuele machines)
[planning-guide-3.3.2]:planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (Azure Premium Storage)
[planning-guide-5.1.1]:planning-guide.md#4d175f1b-7353-4137-9d2f-817683c26e53 (Een virtuele machine verplaatsen van on-premises naar Azure met een niet-gegeneraliseerde schijf)
[planning-guide-5.1.2]:planning-guide.md#e18f7839-c0e2-4385-b1e6-4538453a285c (Een VM implementeren met een klantspecifieke installatie kopie)
[planning-guide-5.2.1]:planning-guide.md#1b287330-944b-495d-9ea7-94b83aff73ef (Voor bereiding voor het verplaatsen van een virtuele machine van on-premises naar Azure met een niet-gegeneraliseerde schijf)
[planning-guide-5.2.2]:planning-guide.md#57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3 (Voor bereiding voor het implementeren van een VM met een klantspecifieke installatie kopie voor SAP)
[planning-guide-5.2]:planning-guide.md#6ffb9f41-a292-40bf-9e70-8204448559e7 (Vm's voorbereiden met SAP voor Azure)
[planning-guide-5.3.1]:planning-guide.md#6e835de8-40b1-4b71-9f18-d45b20959b79 (Verschil tussen een Azure-schijf en een Azure-installatie kopie)
[planning-guide-5.3.2]:planning-guide.md#a43e40e6-1acc-4633-9816-8f095d5a7b6a (Een VHD uploaden van on-premises naar Azure)
[planning-guide-5.4.2]:planning-guide.md#9789b076-2011-4afa-b2fe-b07a8aba58a1 (Schijven kopiëren tussen Azure Storage accounts)
[planning-guide-5.5.1]:planning-guide.md#4efec401-91e0-40c0-8e64-f2dceadff646 (VM-en VHD-structuur voor SAP-implementaties)
[planning-guide-5.5.3]:planning-guide.md#17e0d543-7e8c-4160-a7da-dd7117a1ad9d (Automatisch koppelen voor gekoppelde schijven instellen)
[planning-guide-7.1]:planning-guide.md#3e9c3690-da67-421a-bc3f-12c520d99a30 (Eén virtuele machine met SAP NetWeaver demo/training-scenario)
[planning-guide-7]:planning-guide.md#96a77628-a05e-475d-9df3-fb82217e8f14 (Concepten van de implementatie van SAP-instanties in de Cloud)
[planning-guide-9.1]:planning-guide.md#6f0a47f3-a289-4090-a053-2521618a28c3 (Azure-bewakings oplossing voor SAP)
[planning-guide-azure-premium-storage]:planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (Azure Premium Storage)
[planning-guide-managed-disks]:planning-guide.md#c55b2c6e-3ca1-4476-be16-16c81927550f (Managed Disks)
[planning-guide-figure-100]:media/virtual-machines-shared-sap-planning-guide/100-single-vm-in-azure.png
[planning-guide-figure-1300]:media/virtual-machines-shared-sap-planning-guide/1300-ref-config-iaas-for-sap.png
[planning-guide-figure-1400]:media/virtual-machines-shared-sap-planning-guide/1400-attach-detach-disks.png
[planning-guide-figure-1600]:media/virtual-machines-shared-sap-planning-guide/1600-firewall-port-rule.png
[planning-guide-figure-1700]:media/virtual-machines-shared-sap-planning-guide/1700-single-vm-demo.png
[planning-guide-figure-1900]:media/virtual-machines-shared-sap-planning-guide/1900-vm-set-vnet.png
[planning-guide-figure-200]:media/virtual-machines-shared-sap-planning-guide/200-multiple-vms-in-azure.png
[planning-guide-figure-2100]:media/virtual-machines-shared-sap-planning-guide/2100-s2s.png
[planning-guide-figure-2200]:media/virtual-machines-shared-sap-planning-guide/2200-network-printing.png
[planning-guide-figure-2300]:media/virtual-machines-shared-sap-planning-guide/2300-sapgui-stms.png
[planning-guide-figure-2400]:media/virtual-machines-shared-sap-planning-guide/2400-vm-extension-overview.png
[planning-guide-figure-2500]:media/virtual-machines-shared-sap-planning-guide/2500-vm-extension-details.png
[planning-guide-figure-2600]:media/virtual-machines-shared-sap-planning-guide/2600-sap-router-connection.png
[planning-guide-figure-2700]:media/virtual-machines-shared-sap-planning-guide/2700-exposed-sap-portal.png
[planning-guide-figure-2800]:media/virtual-machines-shared-sap-planning-guide/2800-endpoint-config.png
[planning-guide-figure-2900]:media/virtual-machines-shared-sap-planning-guide/2900-azure-ha-sap-ha.png
[planning-guide-figure-300]:media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png
[planning-guide-figure-3000]:media/virtual-machines-shared-sap-planning-guide/3000-sap-ha-on-azure.png
[planning-guide-figure-3200]:media/virtual-machines-shared-sap-planning-guide/3200-sap-ha-with-sql.png
[planning-guide-figure-400]:media/virtual-machines-shared-sap-planning-guide/400-vm-services.png
[planning-guide-figure-600]:media/virtual-machines-shared-sap-planning-guide/600-s2s-details.png
[planning-guide-figure-700]:media/virtual-machines-shared-sap-planning-guide/700-decision-tree-deploy-to-azure.png
[planning-guide-figure-800]:media/virtual-machines-shared-sap-planning-guide/800-portal-vm-overview.png
[planning-guide-microsoft-azure-networking]:planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd (Microsoft Azure netwerken)
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f (Opslagpad Microsoft Azure Storage en gegevens schijven)

[resource-group-authoring-templates]:../../../resource-group-authoring-templates.md
[resource-group-overview]:../../../azure-resource-manager/resource-group-overview.md
[resource-groups-networking]:../../../networking/network-overview.md
[sap-pam]: https://support.sap.com/pam (SAP-product beschikbaarheids matrix)
[sap-templates-2-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-2-tier-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-marketplace-image-md%2Fazuredeploy.json
[sap-templates-2-tier-os-disk]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-disk%2Fazuredeploy.json
[sap-templates-2-tier-os-disk-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-disk-md%2Fazuredeploy.json
[sap-templates-2-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-image%2Fazuredeploy.json
[sap-templates-2-tier-user-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-image-md%2Fazuredeploy.json
[sap-templates-3-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-3-tier-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-md%2Fazuredeploy.json
[sap-templates-3-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-user-image%2Fazuredeploy.json
[sap-templates-3-tier-user-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-user-image-md%2Fazuredeploy.json
[storage-azure-cli]:../../../storage/common/storage-azure-cli.md
[storage-azure-cli-copy-blobs]:../../../storage/common/storage-azure-cli.md#copy-blobs
[storage-introduction]:../../../storage/common/storage-introduction.md
[storage-powershell-guide-full-copy-vhd]:../../../storage/common/storage-powershell-guide-full.md#how-to-copy-blobs-from-one-storage-container-to-another
[storage-premium-storage-preview-portal]:../../windows/disks-types.md
[storage-redundancy]:../../../storage/common/storage-redundancy.md
[storage-scalability-targets]:../../../storage/common/storage-scalability-targets.md
[storage-use-azcopy]:../../../storage/common/storage-use-azcopy.md
[template-201-vm-from-specialized-vhd]:https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd
[templates-101-simple-windows-vm]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-simple-windows-vm
[templates-101-vm-from-user-image]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image
[virtual-machines-linux-attach-disk-portal]:../../linux/attach-disk-portal.md
[virtual-machines-azure-resource-manager-architecture]:../../../resource-manager-deployment-model.md
[virtual-machines-Az-versus-azuresm]:virtual-machines-linux-compare-deployment-models.md
[virtual-machines-windows-classic-configure-oracle-data-guard]:../../virtual-machines-windows-classic-configure-oracle-data-guard.md
[virtual-machines-linux-cli-deploy-templates]:../../linux/cli-deploy-templates.md (Virtuele machines implementeren en beheren met behulp van Azure Resource Manager sjablonen en de Azure CLI)
[virtual-machines-deploy-rmtemplates-powershell]:../../virtual-machines-windows-ps-manage.md (Virtuele machines beheren met behulp van Azure Resource Manager en Power shell)
[virtual-machines-windows-agent-user-guide]:../../extensions/agent-windows.md
[virtual-machines-linux-agent-user-guide]:../../extensions/agent-linux.md
[virtual-machines-linux-agent-user-guide-command-line-options]:../../extensions/agent-linux.md#command-line-options
[virtual-machines-linux-capture-image]:../../linux/capture-image.md
[virtual-machines-linux-capture-image-resource-manager]:../../linux/capture-image.md
[virtual-machines-linux-capture-image-resource-manager-capture]:../../linux/capture-image.md#step-2-capture-the-vm
[virtual-machines-linux-configure-raid]:../../linux/configure-raid.md
[virtual-machines-linux-configure-lvm]:../../linux/configure-lvm.md
[virtual-machines-linux-classic-create-upload-vhd-step-1]:../../virtual-machines-linux-classic-create-upload-vhd.md#step-1-prepare-the-image-to-be-uploaded
[virtual-machines-linux-create-upload-vhd-suse]:../../linux/suse-create-upload-vhd.md
[virtual-machines-linux-redhat-create-upload-vhd]:../../linux/redhat-create-upload-vhd.md
[virtual-machines-linux-how-to-attach-disk]:../../linux/add-disk.md
[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]:../../linux/add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk
[virtual-machines-linux-tutorial]:../../linux/quick-create-cli.md
[virtual-machines-linux-update-agent]:../../linux/update-agent.md
[virtual-machines-manage-availability]:../../linux/manage-availability.md
[virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]:../../virtual-machines-windows-ps-create.md
[virtual-machines-sizes]:../../linux/sizes.md
[virtual-machines-windows-classic-ps-sql-alwayson-availability-groups]:./../../windows/sqlclassic/virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md
[virtual-machines-windows-classic-ps-sql-int-listener]:./../../windows/sqlclassic/virtual-machines-windows-classic-ps-sql-int-listener.md
[virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions]:./../../windows/sql/virtual-machines-windows-sql-high-availability-dr.md
[virtual-machines-sql-server-infrastructure-services]:./../../windows/sql/virtual-machines-windows-sql-server-iaas-overview.md
[virtual-machines-sql-server-performance-best-practices]:./../../windows/sql/virtual-machines-windows-sql-performance.md
[virtual-machines-upload-image-windows-resource-manager]:../../virtual-machines-windows-upload-image.md
[virtual-machines-windows-tutorial]:../../virtual-machines-windows-hero-tutorial.md
[virtual-machines-workload-template-sql-alwayson]:https://azure.microsoft.com/documentation/templates/sql-server-2014-alwayson-dsc/
[virtual-network-deploy-multinic-arm-cli]:../linux/multiple-nics.md
[virtual-network-deploy-multinic-arm-ps]:../windows/multiple-nics.md
[virtual-network-deploy-multinic-arm-template]:../../../virtual-network/template-samples.md
[virtual-networks-configure-vnet-to-vnet-connection]:../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md
[virtual-networks-create-vnet-arm-pportal]:../../../virtual-network/manage-virtual-network.md#create-a-virtual-network
[virtual-networks-manage-dns-in-vnet]:../../../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md
[virtual-networks-multiple-nics]:../../../virtual-network/virtual-network-deploy-multinic-classic-ps.md
[virtual-networks-nsg]:../../../virtual-network/security-overview.md
[virtual-networks-reserved-private-ip]:../../../virtual-network/virtual-networks-static-private-ip-arm-ps.md
[virtual-networks-static-private-ip-arm-pportal]:../../../virtual-network/virtual-networks-static-private-ip-arm-pportal.md
[virtual-networks-udr-overview]:../../../virtual-network/virtual-networks-udr-overview.md
[vpn-gateway-about-vpn-devices]:../../../vpn-gateway/vpn-gateway-about-vpn-devices.md
[vpn-gateway-create-site-to-site-rm-powershell]:../../../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md
[vpn-gateway-cross-premises-options]:../../../vpn-gateway/vpn-gateway-plan-design.md
[vpn-gateway-site-to-site-create]:../../../vpn-gateway/vpn-gateway-site-to-site-create.md
[vpn-gateway-vpn-faq]:../../../vpn-gateway/vpn-gateway-vpn-faq.md
[xplat-cli]:../../../cli-install-nodejs.md
[xplat-cli-azure-resource-manager]:../../../xplat-cli-azure-resource-manager.md

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Azure Virtual Machines is de oplossing voor organisaties die behoefte hebben aan reken-en opslag resources, op minimale tijd en zonder langdurige aankoop cycli. U kunt Azure Virtual Machines gebruiken om klassieke toepassingen te implementeren, zoals toepassingen op basis van SAP netweave, in Azure. Breid de betrouw baarheid en beschik baarheid van een toepassing uit zonder extra on-premises resources. Azure Virtual Machines ondersteunt cross-premises-connectiviteit, zodat u Azure Virtual Machines kunt integreren in de on-premises domeinen, persoonlijke Clouds en het SAP-systeem van uw organisatie.

In dit artikel behandelen we de stappen voor het implementeren van SAP-toepassingen op virtuele machines (Vm's) in azure, inclusief alternatieve implementatie opties en probleem oplossing. Dit artikel is gebaseerd op de informatie in [Azure virtual machines planning en implementatie voor SAP][planning-guide]net-Weaver. Het is ook een aanvulling op de SAP-installatie documentatie en SAP-notities, die de primaire bronnen zijn voor het installeren en implementeren van SAP-software.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

Voor het instellen van een virtuele Azure-machine voor de SAP-software moet u meerdere stappen en resources uitvoeren. Voordat u begint, moet u ervoor zorgen dat u voldoet aan de vereisten voor het installeren van SAP-software op virtuele machines in Azure.

### <a name="local-computer"></a>Lokale computer

Als u virtuele Windows-of Linux-machines wilt beheren, kunt u een Power shell-script en de Azure Portal gebruiken. Voor beide hulpprogram ma's hebt u een lokale computer met Windows 7 of een nieuwere versie van Windows nodig. Als u alleen virtuele Linux-machines wilt beheren en u een Linux-computer voor deze taak wilt gebruiken, kunt u Azure CLI gebruiken.

### <a name="internet-connection"></a>Internet verbinding

Als u de hulpprogram ma's en scripts wilt downloaden en uitvoeren die vereist zijn voor de implementatie van SAP-software, moet u verbinding hebben met internet. De Azure-VM met de uitgebreide bewakings extensie van Azure heeft ook toegang nodig tot internet. Als de Azure-VM deel uitmaakt van een virtueel Azure-netwerk of een on-premises domein, moet u ervoor zorgen dat de relevante proxy-instellingen zijn ingesteld, zoals beschreven in [de proxy configureren][deployment-guide-configure-proxy].

### <a name="microsoft-azure-subscription"></a>Microsoft Azure-abonnement

U hebt een actief Azure-account nodig.

### <a name="topology-and-networking"></a>Topologie en netwerken

U moet de topologie en architectuur van de SAP-implementatie in azure definiëren:

* Te gebruiken Azure-opslag accounts
* Het virtuele netwerk waarop u het SAP-systeem wilt implementeren
* De resource groep waarvoor u het SAP-systeem wilt implementeren
* Azure-regio waar u het SAP-systeem wilt implementeren
* SAP-configuratie (twee lagen of drie lagen)
* VM-grootten en het aantal extra gegevens schijven dat moet worden gekoppeld aan de virtuele machines
* SAP-correctie en transport systeem configuratie (CTS)

Maak en configureer Azure Storage-accounts (indien nodig) of virtuele Azure-netwerken voordat u begint met het implementatie proces van SAP-software. Voor informatie over het maken en configureren van deze resources raadpleegt u [Azure virtual machines planning en implementatie voor SAP][planning-guide]NetWeaver.

### <a name="sap-sizing"></a>SAP-grootte

Ken de volgende informatie toe voor SAP-grootte:

* Geprojecteerde SAP-werk belasting, bijvoorbeeld door gebruik te maken van het hulp programma voor snelle grootte van SAP, en het SAP-nummer voor de Application Performance Standard (SAP'S)
* Vereist CPU-resource-en geheugen verbruik van het SAP-systeem
* Vereiste invoer/uitvoer-bewerkingen (I/O) per seconde
* Vereiste netwerk bandbreedte voor de uiteindelijke communicatie tussen Vm's in azure
* De vereiste netwerk bandbreedte tussen on-premises assets en het door Azure geïmplementeerde SAP-systeem

### <a name="resource-groups"></a>Resourcegroepen

In Azure Resource Manager kunt u resource groepen gebruiken om alle toepassings resources in uw Azure-abonnement te beheren. Zie voor meer informatie [Overzicht van Azure Resource Manager][resource-group-overview].

## <a name="resources"></a>Resources

### <a name="42ee2bdb-1efc-4ec7-ab31-fe4c22769b94"></a>SAP-bronnen

Wanneer u uw SAP-software-implementatie instelt, hebt u de volgende SAP-resources nodig:

* SAP-opmerking [1928533], die:
  * Lijst met Azure-VM-grootten die worden ondersteund voor de implementatie van SAP-software
  * Informatie over belang rijke capaciteit voor Azure VM-grootten
  * Ondersteunde SAP-software en besturings systemen (OS) en database combinaties
  * Vereiste versie van de SAP-kernel voor Windows en Linux op Microsoft Azure

* SAP-opmerking [2015553] bevat vereisten voor SAP-ondersteuning voor SAP-software-implementaties in Azure.
* SAP Note [2178632] bevat gedetailleerde informatie over alle bewakings gegevens die zijn gerapporteerd voor SAP in Azure.
* SAP Note [1409604] heeft de vereiste SAP host agent-versie voor Windows in Azure.
* SAP Note [2191498] heeft de vereiste SAP host agent-versie voor Linux in Azure.
* SAP Note [2243692] bevat informatie over SAP-licentie verlening op Linux in Azure.
* SAP Note [1984787] bevat algemene informatie over SuSE Linux Enterprise Server 12.
* SAP Note [2002167] bevat algemene informatie over Red Hat Enterprise Linux 7. x.
* SAP Note [2069760] bevat algemene informatie over Oracle Linux 7. x.
* SAP Note [1999351] bevat extra informatie over probleem oplossing voor de uitgebreide bewakings extensie van Azure voor SAP.
* SAP Note [1597355] bevat algemene informatie over wissel ruimte voor Linux.
* [SAP on Azure SCN-pagina](https://wiki.scn.sap.com/wiki/x/Pia7Gg) heeft nieuws en een verzameling nuttige resources.
* Op de [SAP Community wiki](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) zijn alle vereiste SAP-notities voor Linux geïnstalleerd.
* SAP-specifieke Power shell-cmdlets die deel uitmaken van [Azure PowerShell][azure-ps].
* SAP-specifieke Azure CLI-opdrachten die deel uitmaken van [Azure cli][azure-cli].

### <a name="42ee2bdb-1efc-4ec7-ab31-fe4c22769b94"></a>Windows-bronnen

Deze micro soft-artikelen behandelen SAP-implementaties in Azure:

* [Azure Virtual Machines planning en implementatie voor SAP net-Weaver][planning-guide]
* [Azure Virtual Machines-implementatie voor SAP NetWeaver (dit artikel)][deployment-guide]
* [Azure Virtual Machines DBMS-implementatie voor SAP net-Weaver][dbms-guide]

## <a name="b3253ee3-d63b-4d74-a49b-185e76c4088e"></a>Implementatie scenario's voor SAP-software op virtuele Azure-machines

U hebt meerdere opties voor het implementeren van Vm's en gekoppelde schijven in Azure. Het is belang rijk om de verschillen tussen implementatie opties te begrijpen, omdat u mogelijk verschillende stappen kunt ondernemen om uw virtuele machines voor te bereiden voor implementatie op basis van het implementatie type dat u kiest.

### <a name="db477013-9060-4602-9ad4-b0316f8bb281"></a>Scenario 1: Een virtuele machine implementeren vanuit Azure Marketplace voor SAP

U kunt een installatie kopie van micro soft of door derden op Azure Marketplace gebruiken om uw virtuele machine te implementeren. Marketplace biedt enkele standaard installatie kopieën van besturings systemen van Windows Server en verschillende Linux-distributies. U kunt ook een installatie kopie implementeren die Database Management System (DBMS) Sku's bevat, bijvoorbeeld Microsoft SQL Server. Voor meer informatie over het gebruik van installatie kopieën met DBMS-Sku's raadpleegt u [Azure virtual machines DBMS-implementatie voor SAP][dbms-guide]NetWeaver.

Het volgende stroom diagram toont de SAP-specifieke volg orde van de stappen voor het implementeren van een VM vanuit Azure Marketplace:

![Stroom diagram van VM-implementatie voor SAP-systemen met behulp van een VM-installatie kopie van Azure Marketplace][deployment-guide-figure-100]

#### <a name="create-a-virtual-machine-by-using-the-azure-portal"></a>Een virtuele machine maken met behulp van de Azure Portal

De eenvoudigste manier om een nieuwe virtuele machine te maken met een installatie kopie van Azure Marketplace is met behulp van de Azure Portal.

1.  Ga naar <https://portal.azure.com/#create/hub>.  U kunt ook **+ Nieuw**in het menu Azure Portal selecteren.
1.  Selecteer **Compute**en selecteer vervolgens het type besturings systeem dat u wilt implementeren. Bijvoorbeeld Windows Server 2012 R2, SUSE Linux Enterprise Server 12 (SLES 12), Red Hat Enterprise Linux 7,2 (RHEL 7,2) of Oracle Linux 7,2. In de standaard lijst weergave worden niet alle ondersteunde besturings systemen weer gegeven. Selecteer **alles weer geven** voor een volledige lijst. Voor meer informatie over ondersteunde besturings systemen voor de implementatie van SAP-software raadpleegt u SAP Note [1928533].
1.  Bekijk de voor waarden op de volgende pagina.
1.  Selecteer **Resource Manager**in het vak **een implementatie model selecteren** .
1.  Selecteer **Maken**.

De wizard begeleidt u bij het instellen van de vereiste para meters voor het maken van de virtuele machine, naast alle vereiste resources, zoals netwerk interfaces en opslag accounts. Enkele van deze para meters zijn:

1. **Grondbeginselen van**:
   * **Naam**: De naam van de resource (de naam van de virtuele machine).
   * **Type VM-schijf**: Selecteer het schijf type van de besturingssysteem schijf. Als u Premium Storage voor uw gegevens schijven wilt gebruiken, raden we u aan om ook Premium Storage te gebruiken voor de besturingssysteem schijf.
   * **Gebruikers naam en wacht woord** of **open bare SSH-sleutel**: Voer de gebruikers naam en het wacht woord in van de gebruiker die tijdens het inrichten is gemaakt. Voor een virtuele Linux-machine kunt u de open bare Secure shell-sleutel (SSH) invoeren die u gebruikt om u aan te melden bij de computer.
   * **Abonnement**: Selecteer het abonnement dat u wilt gebruiken om de nieuwe virtuele machine in te richten.
   * **Resourcegroep**: De naam van de resource groep voor de virtuele machine. U kunt de naam van een nieuwe resource groep of de naam van een resource groep opgeven die al bestaat.
   * **Locatie**: Waar de nieuwe virtuele machine moet worden geïmplementeerd. Als u de virtuele machine wilt verbinden met uw on-premises netwerk, moet u ervoor zorgen dat u de locatie van het virtuele netwerk selecteert waarmee Azure verbinding maakt met uw on-premises netwerk. Zie voor meer informatie [Microsoft Azure netwerken][planning-guide-microsoft-azure-networking] in [Azure virtual machines planning en implementatie van SAP][planning-guide]net-Weaver.
1. **Grootte**:

     Zie SAP Note [1928533]voor een lijst met ondersteunde VM-typen. Zorg ervoor dat u het juiste VM-type selecteert als u Azure Premium Storage wilt gebruiken. Niet alle VM-typen ondersteunen Premium Storage. Zie [opslag: voor meer informatie. Microsoft Azure Storage-en gegevens][planning-guide-storage-microsoft-azure-storage-and-data-disks] schijven en [Azure Premium Storage][planning-guide-azure-premium-storage] in [Azure virtual machines planning en implementatie van SAP][planning-guide]NetWeaver.

1. **Instellingen voor**:
   * **Storage**
     * **Schijf type**: Selecteer het schijf type van de besturingssysteem schijf. Als u Premium Storage voor uw gegevens schijven wilt gebruiken, raden we u aan om ook Premium Storage te gebruiken voor de besturingssysteem schijf.
     * **Beheerde schijven gebruiken**: Als u Managed Disks wilt gebruiken, selecteert u Ja. Zie hoofd stuk [Managed disks][planning-guide-managed-disks] in de plannings handleiding voor meer informatie over Managed disks.
     * **Opslagaccount**: Selecteer een bestaand opslag account of maak een nieuwe. Niet alle opslag typen werken voor het uitvoeren van SAP-toepassingen. Zie [opslag structuur van een virtuele machine voor RDBMS](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general#65fa79d6-a85f-47ee-890b-22e794f51a64)-implementaties voor meer informatie over opslag typen.
   * **Netwerk**
     * **Virtueel netwerk** en **subnet**: Als u de virtuele machine wilt integreren met uw intranet, selecteert u het virtuele netwerk dat is verbonden met uw on-premises netwerk.
     * **Openbaar IP-adres**: Selecteer het open bare IP-adres dat u wilt gebruiken of voer para meters in om een nieuw openbaar IP-adres te maken. U kunt een openbaar IP-adres gebruiken om toegang te krijgen tot uw virtuele machine via internet. Zorg ervoor dat u ook een netwerk beveiligings groep maakt om de toegang tot uw virtuele machine te beveiligen.
     * **Netwerk beveiligings groep**: Zie [netwerk verkeer beheren met netwerk beveiligings groepen][virtual-networks-nsg]voor meer informatie.
   * **Extensies**: U kunt virtuele-machine uitbreidingen installeren door ze toe te voegen aan de implementatie. U hoeft geen uitbrei dingen toe te voegen in deze stap. De uitbrei dingen die zijn vereist voor SAP-ondersteuning, worden later geïnstalleerd. Zie hoofd stuk [de uitgebreide bewakings uitbreiding van Azure configureren voor SAP][deployment-guide-4.5] in deze hand leiding.
   * **Hoge Beschik baarheid**: Selecteer een beschikbaarheidsset of voer de para meters in om een nieuwe beschikbaarheidsset te maken. Zie [Azure-beschikbaarheids sets][planning-guide-3.2.3]voor meer informatie.
   * **Controle**
     * **Diagnostische gegevens over opstarten**: U kunt **uitschakelen** selecteren voor diagnostische gegevens over opstarten.
     * **Diagnostische gegevens van het gast besturingssysteem**: U kunt **uitschakelen** selecteren voor bewakings diagnostiek.

1. **Samenvatting**:

   Controleer uw selecties en selecteer vervolgens **OK**.

Uw virtuele machine wordt geïmplementeerd in de resource groep die u hebt geselecteerd.

#### <a name="create-a-virtual-machine-by-using-a-template"></a>Een virtuele machine maken met behulp van een sjabloon

U kunt een virtuele machine maken met behulp van een van de SAP-sjablonen die zijn gepubliceerd in de [github-opslag plaats Azure-Quick Start-sjablonen][azure-quickstart-templates-github]. U kunt ook hand matig een virtuele machine maken met behulp van de [Azure Portal][virtual-machines-windows-tutorial], [Power shell][virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]of [Azure cli][virtual-machines-linux-tutorial].

* [**Configuratie met twee lagen (slechts één virtuele-machine)** (SAP-2-tier-Marketplace-afbeelding)][sap-templates-2-tier-marketplace-image]

  Gebruik deze sjabloon om een systeem met twee lagen te maken met slechts één virtuele machine.
* [**Configuratie met twee lagen (slechts één virtuele-machine) sjabloon-Managed disks** (SAP-2-tier-Marketplace-afbeelding-MD)][sap-templates-2-tier-marketplace-image-md]

  Gebruik deze sjabloon om een systeem met twee lagen te maken met slechts één virtuele machine en Managed Disks.
* [**Configuratie met drie lagen (meerdere virtuele machines)** (SAP-3-tier-Marketplace-afbeelding)][sap-templates-3-tier-marketplace-image]

  Gebruik deze sjabloon om een systeem met drie lagen te maken met behulp van meerdere virtuele machines.
* [**Configuratie met drie lagen (meerdere virtuele machines)-Managed disks** (SAP-3-tier-Marketplace-afbeelding-MD)][sap-templates-3-tier-marketplace-image-md]

  Gebruik deze sjabloon om een systeem met drie lagen te maken met meerdere virtuele machines en Managed Disks.

Voer in het Azure Portal de volgende para meters voor de sjabloon in:

1. **Grondbeginselen van**:
   * **Abonnement**: Het abonnement dat moet worden gebruikt voor het implementeren van de sjabloon.
   * **Resourcegroep**: De resource groep die moet worden gebruikt voor het implementeren van de sjabloon. U kunt een nieuwe resource groep maken, maar u kunt ook een bestaande resource groep in het abonnement selecteren.
   * **Locatie**: Waar de sjabloon moet worden geïmplementeerd. Als u een bestaande resource groep hebt geselecteerd, wordt de locatie van die resource groep gebruikt.

1. **Instellingen voor**:
   * **SAP-systeem-id**: De SAP-systeem-ID (SID).
   * **Type besturings systeem**: Het besturings systeem dat u wilt implementeren, bijvoorbeeld Windows Server 2012 R2, SUSE Linux Enterprise Server 12 (SLES 12), Red Hat Enterprise Linux 7,2 (RHEL 7,2) of Oracle Linux 7,2.

     In de lijst weergave worden niet alle ondersteunde besturings systemen weer gegeven. Voor meer informatie over ondersteunde besturings systemen voor de implementatie van SAP-software raadpleegt u SAP Note [1928533].
   * **SAP-systeem grootte**: De grootte van het SAP-systeem.

     Het aantal SAP'S dat het nieuwe systeem biedt. Als u niet zeker weet hoeveel SAP'S het systeem nodig heeft, vraagt u uw SAP-technologie partner of systeem integrator.
   * **Systeem beschikbaarheid** (alleen sjabloon met drie lagen): De beschik baarheid van het systeem.

     Selecteer **ha** voor een configuratie die geschikt is voor een installatie met een hoge Beschik baarheid. Er worden twee database servers en twee servers voor ABAP SAP Central Services (ASCS) gemaakt.
   * **Opslag type** (alleen sjabloon met twee lagen): Het type opslag dat moet worden gebruikt.

     Voor grotere systemen raden we u aan Azure Premium Storage te gebruiken. Zie de volgende bronnen voor meer informatie over opslag typen:
      * [Gebruik van Azure Premium-SSD Storage voor SAP DBMS-exemplaar][2367194]
      * [Opslag structuur van een virtuele machine voor RDBMS-implementaties](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general#65fa79d6-a85f-47ee-890b-22e794f51a64)
      * [Premium Storage: Opslag met hoge prestaties voor werk belastingen van virtuele Azure-machines][storage-premium-storage-preview-portal]
      * [Inleiding tot Microsoft Azure Storage][storage-introduction]
   * **Gebruikers naam** en **wacht woord**beheerder: Een gebruikers naam en wacht woord.
     Er wordt een nieuwe gebruiker gemaakt om u aan te melden bij de virtuele machine.
   * **Nieuw of bestaand subnet**: Hiermee wordt bepaald of een nieuw virtueel netwerk en subnet worden gemaakt of een bestaand subnet wordt gebruikt. Als u al een virtueel netwerk hebt dat is verbonden met uw on-premises netwerk, selecteert u **bestaande**.
   * **Subnet-id**: Als u de virtuele machine wilt implementeren in een bestaand VNet waarvoor u een subnet hebt gedefinieerd, moet de virtuele machine worden toegewezen aan, de ID van het specifieke subnet benoemen. De id ziet er ongeveer als volgt uit&lt;:/Subscriptions/-abonnements&lt;-id > naam van&lt;/resourceGroups/-resource groep >/providers/Microsoft.Network/virtualNetworks/virtuele netwerk naam >/subnets/&lt;de naam van het subnet >

1. Voor **waarden**:  
    Bekijk en accepteer de juridische voor waarden.

1. Selecteer **Aankoop**.

De Azure VM-agent wordt standaard geïmplementeerd wanneer u een installatie kopie gebruikt vanuit Azure Marketplace.

#### <a name="configure-proxy-settings"></a>Proxy-instellingen configureren

Afhankelijk van de configuratie van uw on-premises netwerk moet u mogelijk de proxy op uw virtuele machine instellen. Als uw virtuele machine is verbonden met uw on-premises netwerk via VPN of ExpressRoute, is de virtuele machine mogelijk niet in staat om toegang te krijgen tot internet en kunt u de vereiste extensies niet downloaden of bewakings gegevens verzamelen. Zie [Configure the proxy][deployment-guide-configure-proxy](Engelstalig) voor meer informatie.

#### <a name="join-a-domain-windows-only"></a>Lid worden van een domein (alleen Windows)

Als uw Azure-implementatie is verbonden met een on-premises Active Directory of een DNS-exemplaar via een Azure site-naar-site-VPN-verbinding of ExpressRoute (dit wordt *Cross-premises* genoemd in [Azure virtual machines planning en implementatie voor SAP ][planning-guide]NetWeaver), wordt verwacht dat de virtuele machine wordt toegevoegd aan een on-premises domein. Zie [een virtuele machine toevoegen aan een on-premises domein (alleen Windows)][deployment-guide-4.3]voor meer informatie over overwegingen voor deze taak.

#### <a name="ec323ac3-1de9-4c3a-b770-4ff701def65b"></a>Bewaking configureren

Als u wilt dat SAP uw omgeving ondersteunt, stelt u de Azure-bewakings extensie voor SAP in zoals beschreven in [de Azure Enhanced monitoring-extensie voor SAP configureren][deployment-guide-4.5]. Controleer de vereisten voor SAP-bewaking en de vereiste minimum versie van de SAP-kernel en SAP Hosta Gent, in de resources die worden vermeld in [SAP-resources][deployment-guide-2.2].

#### <a name="monitoring-check"></a>Controle controleren

Controleer of bewaking werkt, zoals wordt beschreven in [controles en probleem oplossing voor het instellen van end-to-end-bewaking][deployment-guide-troubleshooting-chapter].

#### <a name="post-deployment-steps"></a>Stappen na de implementatie

Nadat u de virtuele machine hebt gemaakt en de virtuele machine is geïmplementeerd, moet u de vereiste software onderdelen installeren in de virtuele machine. Vanwege de implementatie-en software-installatie volgorde in dit type VM-implementatie moet de software die moet worden geïnstalleerd, al beschikbaar zijn in azure, op een andere virtuele machine of als een schijf die kan worden bijgevoegd. U kunt ook een cross-premises scenario gebruiken, waarin de verbinding met de lokale assets (installatie shares) wordt gegeven.

Nadat u uw virtuele machine in azure hebt geïmplementeerd, volgt u dezelfde richt lijnen en hulpprogram ma's om de SAP-software op uw virtuele machine te installeren, net zoals u zou doen in een on-premises omgeving. Als u SAP-software wilt installeren op een virtuele Azure-machine, wordt u aangeraden SAP en micro soft te uploaden en op te slaan in azure Vhd's of Managed Disks, of door een Azure-VM te maken die werkt als een bestands server met alle vereiste SAP-installatie media.

### <a name="54a1fc6d-24fd-4feb-9c57-ac588a55dff2"></a>Scenario 2: Een virtuele machine implementeren met een aangepaste installatie kopie voor SAP

Omdat verschillende versies van een besturings systeem of DBMS andere patch vereisten hebben, zijn de installatie kopieën die u in de Azure Marketplace vindt mogelijk niet aan uw behoeften voldoet. U kunt in plaats daarvan een virtuele machine maken met behulp van uw eigen installatie kopie van het besturings systeem/DBMS-VM. deze moet u later opnieuw implementeren.
U kunt verschillende stappen gebruiken om een persoonlijke installatie kopie voor Linux te maken dan om er een te maken voor Windows.

---
> ![Windows][Logo_Windows] Windows
>
> Om een Windows-installatie kopie voor te bereiden die u kunt gebruiken om meerdere virtuele machines te implementeren, moeten de Windows-instellingen (zoals Windows SID en hostname) worden abstracted of gegeneraliseerd op de on-premises VM. U kunt [Sysprep](https://msdn.microsoft.com/library/hh825084.aspx) gebruiken om dit te doen.
>
> ![Linux][Logo_Linux] Linux
>
> Om een Linux-installatie kopie voor te bereiden die u kunt gebruiken om meerdere virtuele machines te implementeren, moeten bepaalde Linux-instellingen worden abstracted of gegeneraliseerd op de on-premises VM. U kunt dit `waagent -deprovision` gebruiken om dit te doen. Zie [een virtuele Linux-machine vastleggen die wordt uitgevoerd op Azure][virtual-machines-linux-capture-image] en de [Gebruikers handleiding voor de Azure Linux-agent][virtual-machines-linux-agent-user-guide-command-line-options]voor meer informatie.
>
>

---
U kunt een aangepaste installatie kopie voorbereiden en maken en deze vervolgens gebruiken om meerdere nieuwe Vm's te maken. Dit wordt beschreven in [Azure virtual machines planning en implementatie voor SAP][planning-guide]net-Weaver. Uw Data Base-inhoud instellen met behulp van SAP software Provisioning Manager voor het installeren van een nieuw SAP-systeem (herstelt een database back-up van een schijf die is gekoppeld aan de virtuele machine) of door een back-up van een Data Base rechtstreeks te herstellen vanuit Azure Storage, als uw DBMS ondersteunt. Zie voor meer informatie, [Azure virtuele Machines DBMS-implementatie voor SAP NetWeaver][dbms-guide]. Als u al een SAP-systeem op uw on-premises VM hebt geïnstalleerd (met name voor systemen met twee lagen), kunt u de SAP-systeem instellingen aanpassen na de implementatie van de Azure-VM met behulp van de procedure voor het wijzigen van het systeem dat wordt ondersteund door SAP software Provisioning Manager (SAP Opmerking [1619720]). Als dat niet het geval is, kunt u de SAP-software installeren nadat u de virtuele Azure-machine hebt geïmplementeerd.

Het volgende stroom diagram toont de SAP-specifieke volg orde van de stappen voor het implementeren van een virtuele machine vanuit een aangepaste installatie kopie:

![Stroom diagram van VM-implementatie voor SAP-systemen met behulp van een VM-installatie kopie in particuliere Marketplace][deployment-guide-figure-300]

#### <a name="create-a-virtual-machine-by-using-the-azure-portal"></a>Een virtuele machine maken met behulp van de Azure Portal

De eenvoudigste manier om een nieuwe virtuele machine te maken op basis van een installatie kopie van een beheerde schijf is met behulp van de Azure Portal. Lees [een beheerde installatie kopie van een gegeneraliseerde vm in azure vastleggen](https://docs.microsoft.com/azure/virtual-machines/windows/capture-image-resource) voor meer informatie over het maken van een installatie kopie van een schijf.

1.  Ga naar <https://ms.portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.Compute%2Fimages>. Of Selecteer in het menu Azure Portal de optie **installatie kopieën**.
1.  Selecteer de installatie kopie van de beheerde schijf die u wilt implementeren en klik op **VM maken**

De wizard begeleidt u bij het instellen van de vereiste para meters voor het maken van de virtuele machine, naast alle vereiste resources, zoals netwerk interfaces en opslag accounts. Enkele van deze para meters zijn:

1. **Grondbeginselen van**:
   * **Naam**: De naam van de resource (de naam van de virtuele machine).
   * **Type VM-schijf**: Selecteer het schijf type van de besturingssysteem schijf. Als u Premium Storage voor uw gegevens schijven wilt gebruiken, raden we u aan om ook Premium Storage te gebruiken voor de besturingssysteem schijf.
   * **Gebruikers naam en wacht woord** of **open bare SSH-sleutel**: Voer de gebruikers naam en het wacht woord in van de gebruiker die tijdens het inrichten is gemaakt. Voor een virtuele Linux-machine kunt u de open bare Secure shell-sleutel (SSH) invoeren die u gebruikt om u aan te melden bij de computer.
   * **Abonnement**: Selecteer het abonnement dat u wilt gebruiken om de nieuwe virtuele machine in te richten.
   * **Resourcegroep**: De naam van de resource groep voor de virtuele machine. U kunt de naam van een nieuwe resource groep of de naam van een resource groep opgeven die al bestaat.
   * **Locatie**: Waar de nieuwe virtuele machine moet worden geïmplementeerd. Als u de virtuele machine wilt verbinden met uw on-premises netwerk, moet u ervoor zorgen dat u de locatie van het virtuele netwerk selecteert waarmee Azure verbinding maakt met uw on-premises netwerk. Zie voor meer informatie [Microsoft Azure netwerken][planning-guide-microsoft-azure-networking] in [Azure virtual machines planning en implementatie van SAP][planning-guide]net-Weaver.
1. **Grootte**:

     Zie SAP Note [1928533]voor een lijst met ondersteunde VM-typen. Zorg ervoor dat u het juiste VM-type selecteert als u Azure Premium Storage wilt gebruiken. Niet alle VM-typen ondersteunen Premium Storage. Zie [opslag: voor meer informatie. Microsoft Azure Storage-en gegevens][planning-guide-storage-microsoft-azure-storage-and-data-disks] schijven en [Azure Premium Storage][planning-guide-azure-premium-storage] in [Azure virtual machines planning en implementatie van SAP][planning-guide]NetWeaver.

1. **Instellingen voor**:
   * **Storage**
     * **Schijf type**: Selecteer het schijf type van de besturingssysteem schijf. Als u Premium Storage voor uw gegevens schijven wilt gebruiken, raden we u aan om ook Premium Storage te gebruiken voor de besturingssysteem schijf.
     * **Beheerde schijven gebruiken**: Als u Managed Disks wilt gebruiken, selecteert u Ja. Zie hoofd stuk [Managed disks][planning-guide-managed-disks] in de plannings handleiding voor meer informatie over Managed disks.
   * **Netwerk**
     * **Virtueel netwerk** en **subnet**: Als u de virtuele machine wilt integreren met uw intranet, selecteert u het virtuele netwerk dat is verbonden met uw on-premises netwerk.
     * **Openbaar IP-adres**: Selecteer het open bare IP-adres dat u wilt gebruiken of voer para meters in om een nieuw openbaar IP-adres te maken. U kunt een openbaar IP-adres gebruiken om toegang te krijgen tot uw virtuele machine via internet. Zorg ervoor dat u ook een netwerk beveiligings groep maakt om de toegang tot uw virtuele machine te beveiligen.
     * **Netwerk beveiligings groep**: Zie [netwerk verkeer beheren met netwerk beveiligings groepen][virtual-networks-nsg]voor meer informatie.
   * **Extensies**: U kunt virtuele-machine uitbreidingen installeren door ze toe te voegen aan de implementatie. U hoeft in deze stap geen extensie toe te voegen. De uitbrei dingen die zijn vereist voor SAP-ondersteuning, worden later geïnstalleerd. Zie hoofd stuk [de uitgebreide bewakings uitbreiding van Azure configureren voor SAP][deployment-guide-4.5] in deze hand leiding.
   * **Hoge Beschik baarheid**: Selecteer een beschikbaarheidsset of voer de para meters in om een nieuwe beschikbaarheidsset te maken. Zie [Azure-beschikbaarheids sets][planning-guide-3.2.3]voor meer informatie.
   * **Controle**
     * **Diagnostische gegevens over opstarten**: U kunt **uitschakelen** selecteren voor diagnostische gegevens over opstarten.
     * **Diagnostische gegevens van het gast besturingssysteem**: U kunt **uitschakelen** selecteren voor bewakings diagnostiek.

1. **Samenvatting**:

   Controleer uw selecties en selecteer vervolgens **OK**.

Uw virtuele machine wordt geïmplementeerd in de resource groep die u hebt geselecteerd.

#### <a name="create-a-virtual-machine-by-using-a-template"></a>Een virtuele machine maken met behulp van een sjabloon

Als u een implementatie wilt maken met behulp van een installatie kopie van een persoonlijk besturings systeem uit de Azure Portal, gebruikt u een van de volgende SAP-sjablonen. Deze sjablonen worden gepubliceerd in de [github-opslag plaats Azure-Quick Start-sjablonen][azure-quickstart-templates-github]. U kunt ook hand matig een virtuele machine maken met behulp van [Power shell][virtual-machines-upload-image-windows-resource-manager].

* [**Configuratie met twee lagen (slechts één virtuele-machine)** (SAP-2-tier-User-image)][sap-templates-2-tier-user-image]

  Gebruik deze sjabloon om een systeem met twee lagen te maken met slechts één virtuele machine.
* [**Configuratie met twee lagen (slechts één virtuele-machine) sjabloon-beheerde schijf installatie kopie** (SAP-2-tier-User-image-MD)][sap-templates-2-tier-user-image-md]

  Gebruik deze sjabloon om een systeem met twee lagen te maken met slechts één virtuele machine en een beheerde schijf installatie kopie.
* [**Configuratie met drie lagen (meerdere virtuele machines)** (SAP-3-tier-gebruikers-Image)][sap-templates-3-tier-user-image]

  Gebruik deze sjabloon om een systeem met drie lagen te maken met meerdere virtuele machines of uw eigen installatie kopie van het besturings systeem.
* [**Configuratie met drie lagen (meerdere virtuele machines) sjabloon-beheerde schijf installatie kopie** (SAP-3-tier-User-image-MD)][sap-templates-3-tier-user-image-md]

  Gebruik deze sjabloon om een systeem met drie lagen te maken met meerdere virtuele machines of uw eigen installatie kopie van het besturings systeem en een installatie kopie van een beheerde schijf.

Voer in het Azure Portal de volgende para meters voor de sjabloon in:

1. **Grondbeginselen van**:
   * **Abonnement**: Het abonnement dat moet worden gebruikt voor het implementeren van de sjabloon.
   * **Resourcegroep**: De resource groep die moet worden gebruikt voor het implementeren van de sjabloon. U kunt een nieuwe resource groep maken of een bestaande resource groep selecteren in het abonnement.
   * **Locatie**: Waar de sjabloon moet worden geïmplementeerd. Als u een bestaande resource groep hebt geselecteerd, wordt de locatie van die resource groep gebruikt.
1. **Instellingen voor**:
   * **SAP-systeem-id**: De SAP-systeem-ID.
   * **Type besturings systeem**: Het type besturings systeem dat u wilt implementeren (Windows of Linux).
   * **SAP-systeem grootte**: De grootte van het SAP-systeem.

     Het aantal SAP'S dat het nieuwe systeem biedt. Als u niet zeker weet hoeveel SAP'S het systeem nodig heeft, vraagt u uw SAP-technologie partner of systeem integrator.
   * **Systeem beschikbaarheid** (alleen sjabloon met drie lagen): De beschik baarheid van het systeem.

     Selecteer **ha** voor een configuratie die geschikt is voor een installatie met een hoge Beschik baarheid. Er worden twee database servers en twee servers voor ASCS gemaakt.
   * **Opslag type** (alleen sjabloon met twee lagen): Het type opslag dat moet worden gebruikt.

     Voor grotere systemen raden we u aan Azure Premium Storage te gebruiken. Raadpleeg de volgende bronnen voor meer informatie over opslag typen:
      * [Gebruik van Azure Premium-SSD Storage voor SAP DBMS-exemplaar][2367194]
      * [Opslag structuur van een virtuele machine voor RDBMS-implementaties](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general#65fa79d6-a85f-47ee-890b-22e794f51a64)
      * [Premium Storage: Opslag met hoge prestaties voor werk belastingen van virtuele Azure-machines][storage-premium-storage-preview-portal]
      * [Inleiding tot Microsoft Azure Storage][storage-introduction]
   * **VHD-URI van gebruikers installatie kopie** (alleen sjabloon voor onbeheerde schijf installatie kopieën): De URI van de VHD van de persoonlijke installatie kopie, bijvoorbeeld https://&lt;AccountName >. blob. core. Windows. net/vhd's/image. VHD.
   * **Opslag account voor gebruikers installatie kopie** (alleen sjabloon voor onbeheerde schijf installatie kopieën): De naam van het opslag account waarin de persoonlijke installatie kopie van het besturings systeem wordt opgeslagen &lt;, bijvoorbeeld AccountName > in&lt;https://AccountName >. blob. core. Windows. net/vhd's/image. VHD.
   * **userImageId** (alleen sjabloon voor beheerde schijf installatie kopieën): Id van de beheerde schijf kopie die u wilt gebruiken
   * **Gebruikers naam** en **wacht woord**beheerder: De gebruikers naam en het wacht woord.

     Er wordt een nieuwe gebruiker gemaakt om u aan te melden bij de virtuele machine.
   * **Nieuw of bestaand subnet**: Hiermee wordt bepaald of een nieuw virtueel netwerk en subnet wordt gemaakt of een bestaand subnet wordt gebruikt. Als u al een virtueel netwerk hebt dat is verbonden met uw on-premises netwerk, selecteert u **bestaande**.
   * **Subnet-id**: Als u de virtuele machine wilt implementeren in een bestaand VNet waarvoor u een subnet hebt gedefinieerd, moet de virtuele machine worden toegewezen aan, de ID van het specifieke subnet benoemen. De id ziet er ongeveer als volgt uit&lt;:/Subscriptions/-abonnements&lt;-id > naam van&lt;/resourceGroups/-resource groep >/providers/Microsoft.Network/virtualNetworks/virtuele netwerk naam >/subnets/&lt;de naam van het subnet >

1. Voor **waarden**:  
    Bekijk en accepteer de juridische voor waarden.

1. Selecteer **Aankoop**.

#### <a name="install-the-vm-agent-linux-only"></a>De VM-agent installeren (alleen Linux)

Als u de sjablonen wilt gebruiken die in de voor gaande sectie worden beschreven, moet de Linux-agent al zijn geïnstalleerd in de installatie kopie van de gebruiker, anders mislukt de implementatie. Down load en installeer de VM-agent in de installatie kopie van de gebruiker, zoals beschreven in [de Azure VM-agent downloaden, installeren en inschakelen][deployment-guide-4.4]. Als u de sjablonen niet gebruikt, kunt u de VM-agent ook later installeren.

#### <a name="join-a-domain-windows-only"></a>Lid worden van een domein (alleen Windows)

Als uw Azure-implementatie is verbonden met een on-premises Active Directory of een DNS-exemplaar via een Azure site-naar-site-VPN-verbinding of Azure ExpressRoute (dit wordt *Cross-premises* genoemd in [Azure virtual machines planning en implementatie voor SAP ][planning-guide]NetWeaver), wordt verwacht dat de virtuele machine wordt toegevoegd aan een on-premises domein. Zie [een virtuele machine toevoegen aan een on-premises domein (alleen Windows)][deployment-guide-4.3]voor meer informatie over overwegingen voor deze stap.

#### <a name="configure-proxy-settings"></a>Proxy-instellingen configureren

Afhankelijk van de configuratie van uw on-premises netwerk moet u mogelijk de proxy op uw virtuele machine instellen. Als uw virtuele machine is verbonden met uw on-premises netwerk via VPN of ExpressRoute, is de virtuele machine mogelijk niet in staat om toegang te krijgen tot internet en kunt u de vereiste extensies niet downloaden of bewakings gegevens verzamelen. Zie [Configure the proxy][deployment-guide-configure-proxy](Engelstalig) voor meer informatie.

#### <a name="configure-monitoring"></a>Bewaking configureren

Als u wilt dat SAP uw omgeving ondersteunt, stelt u de Azure-bewakings extensie voor SAP in zoals beschreven in [de Azure Enhanced monitoring-extensie voor SAP configureren][deployment-guide-4.5]. Controleer de vereisten voor SAP-bewaking en de vereiste minimum versie van de SAP-kernel en SAP Hosta Gent, in de resources die worden vermeld in [SAP-resources][deployment-guide-2.2].

#### <a name="monitoring-check"></a>Controle controleren

Controleer of bewaking werkt, zoals wordt beschreven in [controles en probleem oplossing voor het instellen van end-to-end-bewaking][deployment-guide-troubleshooting-chapter].


### <a name="a9a60133-a763-4de8-8986-ac0fa33aa8c1"></a>Scenario 3: Een on-premises VM verplaatsen met behulp van een niet-gegeneraliseerde Azure VHD met SAP

In dit scenario bent u van plan een specifiek SAP-systeem te verplaatsen van een on-premises omgeving naar Azure. U kunt dit doen door de VHD met het besturings systeem, de SAP binaire bestanden en uiteindelijk de binaire bestanden van de DBMS te uploaden, plus de Vhd's met de gegevens-en logboek bestanden van het DBMS, naar Azure. In tegens telling tot het [scenario dat wordt beschreven in scenario 2: Een virtuele machine implementeren met een aangepaste installatie kopie][deployment-guide-3.3]voor SAP. in dit geval moet u de hostnaam, SAP sid en SAP-gebruikers accounts in de Azure VM blijven, omdat deze zijn geconfigureerd in de on-premises omgeving. U hoeft het besturings systeem niet te generaliseren. Dit scenario is het meest van toepassing op cross-premises scenario's waarbij een deel van het SAP-landschap on-premises en een deel ervan wordt uitgevoerd op Azure.

In dit scenario wordt de VM-agent **niet** automatisch geïnstalleerd tijdens de implementatie. Omdat de VM-agent en de Azure Enhanced monitoring-extensie voor SAP vereist zijn voor het uitvoeren van SAP NetWeaver op Azure, moet u beide onderdelen hand matig downloaden, installeren en inschakelen nadat u de virtuele machine hebt gemaakt.

Zie de volgende bronnen voor meer informatie over de Azure VM-agent.

---
> ![Windows][Logo_Windows] Windows
>
> [Overzicht van de agent voor virtuele Azure-machines][virtual-machines-windows-agent-user-guide]
>
> ![Linux][Logo_Linux] Linux
>
> [Gebruikers handleiding voor Azure Linux-agent][virtual-machines-linux-agent-user-guide]
>
>

---

Het volgende stroom diagram toont de volg orde van de stappen voor het verplaatsen van een on-premises VM met behulp van een niet-gegeneraliseerde Azure VHD:

![Stroom diagram van VM-implementatie voor SAP-systemen met behulp van een VM-schijf][deployment-guide-figure-400]

Als de schijf al is geüpload en gedefinieerd in azure (Zie [azure virtual machines planning en implementatie voor SAP][planning-guide]NetWeaver), voert u de taken uit die in de volgende secties worden beschreven.

#### <a name="create-a-virtual-machine"></a>Een virtuele machine maken

Als u een implementatie wilt maken met behulp van een persoonlijke besturingssysteem schijf via de Azure Portal, gebruikt u de SAP-sjabloon die is gepubliceerd in de [github-opslag plaats Azure-Quick Start-sjablonen][azure-quickstart-templates-github]. U kunt ook hand matig een virtuele machine maken met behulp van Power shell.

* [**Configuratie met twee lagen (slechts één virtuele-machine)** (SAP-2-tier-gebruikers-schijf)][sap-templates-2-tier-os-disk]

  Gebruik deze sjabloon om een systeem met twee lagen te maken met slechts één virtuele machine.
* [**Configuratie met twee lagen (slechts één virtuele-machine) sjabloon-beheerde schijf** (SAP-2-tier-User-Disk-MD)][sap-templates-2-tier-os-disk-md]

  Gebruik deze sjabloon om een systeem met twee lagen te maken met slechts één virtuele machine en een beheerde schijf.

Voer in het Azure Portal de volgende para meters voor de sjabloon in:

1. **Grondbeginselen van**:
   * **Abonnement**: Het abonnement dat moet worden gebruikt voor het implementeren van de sjabloon.
   * **Resourcegroep**: De resource groep die moet worden gebruikt voor het implementeren van de sjabloon. U kunt een nieuwe resource groep maken of een bestaande resource groep selecteren in het abonnement.
   * **Locatie**: Waar de sjabloon moet worden geïmplementeerd. Als u een bestaande resource groep hebt geselecteerd, wordt de locatie van die resource groep gebruikt.
1. **Instellingen voor**:
   * **SAP-systeem-id**: De SAP-systeem-ID.
   * **Type besturings systeem**: Het type besturings systeem dat u wilt implementeren (Windows of Linux).
   * **SAP-systeem grootte**: De grootte van het SAP-systeem.

     Het aantal SAP'S dat het nieuwe systeem biedt. Als u niet zeker weet hoeveel SAP'S het systeem nodig heeft, vraagt u uw SAP-technologie partner of systeem integrator.
   * **Opslag type** (alleen sjabloon met twee lagen): Het type opslag dat moet worden gebruikt.

     Voor grotere systemen raden we u aan Azure Premium Storage te gebruiken. Raadpleeg de volgende bronnen voor meer informatie over opslag typen:
      * [Gebruik van Azure Premium-SSD Storage voor SAP DBMS-exemplaar][2367194]
      * [Opslag structuur van een virtuele machine voor RDBMS-implementaties](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general#65fa79d6-a85f-47ee-890b-22e794f51a64)
      * [Premium Storage: Opslag met hoge prestaties voor werk belastingen van virtuele Azure-machines][storage-premium-storage-preview-portal]
      * [Inleiding tot Microsoft Azure Storage][storage-introduction]
   * **VHD-URI van besturingssysteem schijf** (alleen niet-beheerde schijf sjabloon): De URI van de persoonlijke besturingssysteem schijf, bijvoorbeeld https://&lt;AccountName >. blob. core. Windows. net/vhd's/osdisk. VHD.
   * **Schijf-id van de besturingssysteem schijf** die is beheerd (alleen beheerde schijf sjabloon): De id van de beheerde schijf voor het besturings systeem,/subscriptions/92d102f7-81a5-4df7-9877-54987ba97dd9/resourceGroups/group/providers/Microsoft.Compute/disks/WIN
   * **Nieuw of bestaand subnet**: Hiermee wordt bepaald of een nieuw virtueel netwerk en subnet worden gemaakt, of dat er een bestaand subnet wordt gebruikt. Als u al een virtueel netwerk hebt dat is verbonden met uw on-premises netwerk, selecteert u **bestaande**.
   * **Subnet-id**: Als u de virtuele machine wilt implementeren in een bestaand VNet waarvoor u een subnet hebt gedefinieerd, moet de virtuele machine worden toegewezen aan, de ID van het specifieke subnet benoemen. De id ziet er ongeveer als volgt uit&lt;:/Subscriptions/-abonnements&lt;-id > naam van&lt;/resourceGroups/-resource groep >/providers/Microsoft.Network/virtualNetworks/virtuele netwerk naam >/subnets/&lt;de naam van het subnet >

1. Voor **waarden**:  
    Bekijk en accepteer de juridische voor waarden.

1. Selecteer **Aankoop**.

#### <a name="install-the-vm-agent"></a>De VM-agent installeren

Als u de sjablonen wilt gebruiken die in de voor gaande sectie worden beschreven, moet de VM-agent op de besturingssysteem schijf zijn geïnstalleerd, anders mislukt de implementatie. Down load en installeer de VM-agent in de virtuele machine, zoals beschreven in [de Azure VM-agent downloaden, installeren en inschakelen][deployment-guide-4.4].

Als u geen gebruik maakt van de sjablonen die in de voor gaande sectie worden beschreven, kunt u de VM-agent ook later installeren.

#### <a name="join-a-domain-windows-only"></a>Lid worden van een domein (alleen Windows)

Als uw Azure-implementatie is verbonden met een on-premises Active Directory of een DNS-exemplaar via een Azure site-naar-site-VPN-verbinding of ExpressRoute (dit wordt *Cross-premises* genoemd in [Azure virtual machines planning en implementatie voor SAP ][planning-guide]NetWeaver), wordt verwacht dat de virtuele machine wordt toegevoegd aan een on-premises domein. Zie [een virtuele machine toevoegen aan een on-premises domein (alleen Windows)][deployment-guide-4.3]voor meer informatie over overwegingen voor deze taak.

#### <a name="configure-proxy-settings"></a>Proxy-instellingen configureren

Afhankelijk van de configuratie van uw on-premises netwerk moet u mogelijk de proxy op uw virtuele machine instellen. Als uw virtuele machine is verbonden met uw on-premises netwerk via VPN of ExpressRoute, is de virtuele machine mogelijk niet in staat om toegang te krijgen tot internet en kunt u de vereiste extensies niet downloaden of bewakings gegevens verzamelen. Zie [Configure the proxy][deployment-guide-configure-proxy](Engelstalig) voor meer informatie.

#### <a name="configure-monitoring"></a>Bewaking configureren

Als u wilt dat SAP uw omgeving ondersteunt, stelt u de Azure-bewakings extensie voor SAP in zoals beschreven in [de Azure Enhanced monitoring-extensie voor SAP configureren][deployment-guide-4.5]. Controleer de vereisten voor SAP-bewaking en de vereiste minimum versie van de SAP-kernel en SAP Hosta Gent, in de resources die worden vermeld in [SAP-resources][deployment-guide-2.2].

#### <a name="monitoring-check"></a>Controle controleren

Controleer of bewaking werkt, zoals wordt beschreven in [controles en probleem oplossing voor het instellen van end-to-end-bewaking][deployment-guide-troubleshooting-chapter].

## <a name="update-the-monitoring-configuration-for-sap"></a>De bewakings configuratie voor SAP bijwerken

Werk de SAP-bewakings configuratie bij in een van de volgende scenario's:
* Het Joint micro soft/SAP-team breidt de bewakings mogelijkheden uit en vraagt meer of minder tellers.
* Micro soft introduceert een nieuwe versie van de onderliggende Azure-infra structuur die de bewakings gegevens levert en de uitgebreide bewakings extensie van Azure voor SAP moet worden aangepast aan die wijzigingen.
* U kunt extra gegevens schijven koppelen aan uw Azure VM of een gegevens schijf verwijderen. In dit scenario werkt u het verzamelen van gegevens met betrekking tot opslag bij. Het wijzigen van uw configuratie door eind punten toe te voegen of te verwijderen of door IP-adressen toe te wijzen aan een virtuele machine heeft geen invloed op de bewakings configuratie.
* U kunt de grootte van uw virtuele Azure-machine wijzigen, bijvoorbeeld van grootte A5 naar een andere VM-grootte.
* U voegt nieuwe netwerk interfaces toe aan uw Azure-VM.

Als u bewakings instellingen wilt bijwerken, werkt u de bewakings infrastructuur bij door de stappen in [de Azure Enhanced monitoring-extensie configureren voor SAP][deployment-guide-4.5]te volgen.

## <a name="detailed-tasks-for-sap-software-deployment"></a>Gedetailleerde taken voor SAP-software-implementatie

Deze sectie bevat gedetailleerde stappen voor het uitvoeren van specifieke taken in het configuratie-en implementatie proces.

### <a name="604bcec2-8b6e-48d2-a944-61b0f5dee2f7"></a>Azure PowerShell-cmdlets implementeren

1. Ga naar [Microsoft Azure down loads](https://azure.microsoft.com/downloads/).
1. Onder **opdracht regel Programma's**, onder **Power shell**, selecteert u **Windows installeren**.
1. Selecteer in het dialoog venster micro soft Download beheer, voor het gedownloade bestand (bijvoorbeeld WindowsAzurePowershellGet. 3F. 3F. 3fnew. exe), de optie **uitvoeren**.
1. Selecteer **Ja**als u het installatie programma voor het micro soft-webplatform (micro soft Web PI) wilt uitvoeren.
1. Er wordt een pagina weer gegeven die er als volgt uitziet:

   ![Installatie pagina voor Azure PowerShell-cmdlets][deployment-guide-figure-500]<a name="figure-5"></a>

1. Selecteer **installeren**en accepteer de licentie voorwaarden voor micro soft-software.
1. Power shell is geïnstalleerd. Selecteer **volt ooien** om de installatie wizard te sluiten.

Controleer regel matig op updates voor de Power shell-cmdlets, die meestal maandelijks worden bijgewerkt. De eenvoudigste manier om te controleren of er updates zijn, is door de voor gaande installatie stappen uit te voeren, tot de installatie pagina die wordt weer gegeven in stap 5. De release datum en release nummer van de cmdlets zijn opgenomen op de pagina die wordt weer gegeven in stap 5. Tenzij anders vermeld in SAP Note [1928533] of sap opmerking [2015553], raden we u aan om te werken met de nieuwste versie van Azure PowerShell-cmdlets.

Als u de versie van de Azure PowerShell-cmdlets wilt controleren die op uw computer zijn geïnstalleerd, voert u de volgende Power shell-opdracht uit:
```powershell
(Get-Module Az.Compute).Version
```
Het resultaat ziet er als volgt uit:

![Resultaat van de Azure PowerShell-versie controle van de cmdlet][deployment-guide-figure-600]
<a name="figure-6"></a>

Als de versie van de Azure-cmdlet die op uw computer is geïnstalleerd, de huidige versie is, wordt deze door de eerste pagina van de installatie wizard aangegeven door **(geïnstalleerd)** toe te voegen aan de product titel (Zie de volgende scherm afbeelding). Uw Power shell Azure-cmdlets zijn bijgewerkt. Klik op **Afsluiten**om de installatie wizard te sluiten.

![Installatie pagina voor Azure PowerShell-cmdlets die aangeven dat de meest recente versie van Azure PowerShell-cmdlets is geïnstalleerd][deployment-guide-figure-700]
<a name="figure-7"></a>

### <a name="1ded9453-1330-442a-86ea-e0fd8ae8cab3"></a>Azure CLI implementeren

1. Ga naar [Microsoft Azure down loads](https://azure.microsoft.com/downloads/).
1. Onder **opdracht regel Programma's**, onder **Azure Command-Line Interface**, selecteert u de **installatie** koppeling voor uw besturings systeem.
1. Selecteer in het dialoog venster micro soft Download beheer, voor het gedownloade bestand (bijvoorbeeld WindowsAzureXPlatCLI. 3F. 3F. 3fnew. exe), de optie **uitvoeren**.
1. Selecteer **Ja**als u het installatie programma voor het micro soft-webplatform (micro soft Web PI) wilt uitvoeren.
1. Er wordt een pagina weer gegeven die er als volgt uitziet:

   ![Installatie pagina voor Azure PowerShell-cmdlets][deployment-guide-figure-500]<a name="figure-5"></a>

1. Selecteer **installeren**en accepteer de licentie voorwaarden voor micro soft-software.
1. Azure CLI is geïnstalleerd. Selecteer **volt ooien** om de installatie wizard te sluiten.

Controleer regel matig op updates voor Azure CLI, die meestal maandelijks worden bijgewerkt. De eenvoudigste manier om te controleren of er updates zijn, is door de voor gaande installatie stappen uit te voeren, tot de installatie pagina die wordt weer gegeven in stap 5.

Als u de versie van de Azure CLI wilt controleren die op uw computer is geïnstalleerd, voert u deze opdracht uit:
```
azure --version
```

Het resultaat ziet er als volgt uit:

![Resultaat van controle van Azure CLI-versie][deployment-guide-figure-760]
<a name="0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda"></a>

### <a name="31d9ecd6-b136-4c73-b61e-da4a29bbc9cc"></a>Een VM toevoegen aan een on-premises domein (alleen Windows)

Als u SAP-Vm's implementeert in een cross-premises scenario, waarbij on-premises Active Directory en DNS worden uitgebreid in azure, wordt ervan uitgegaan dat de virtuele machines worden toegevoegd aan een on-premises domein. De stappen die u moet nemen om een virtuele machine te koppelen aan een on-premises domein en de aanvullende software die moet worden toegevoegd aan een lid van een on-premises domein, varieert per klant. Als u een virtuele machine wilt toevoegen aan een on-premises domein, moet u meestal extra software, zoals antimalware-software, en back-up-of bewakings software installeren.

In dit scenario moet u er ook voor zorgen dat als de Internet proxy-instellingen worden afgedwongen wanneer een virtuele machine aan een domein in uw omgeving wordt toegevoegd, het lokale systeem account (S-1-5-18) in de gast-VM dezelfde proxy instellingen heeft. De eenvoudigste optie is om de proxy af te dwingen met behulp van een domein groepsbeleid, dat van toepassing is op systemen in het domein.

### <a name="c7cbb0dc-52a4-49db-8e03-83e7edc2927d"></a>De Azure VM-agent downloaden, installeren en inschakelen

Voor virtuele machines die worden geïmplementeerd vanuit een installatie kopie van een besturings systeem die niet gegeneraliseerd is (bijvoorbeeld een installatie kopie die niet afkomstig is van de Windows-systeem voorbereiding of Sysprep, hulp programma), moet u de Azure VM-agent hand matig downloaden, installeren en inschakelen.

Als u een virtuele machine implementeert vanuit Azure Marketplace, is deze stap niet vereist. Installatie kopieën van Azure Marketplace hebben al de Azure VM-agent.

#### <a name="b2db5c9a-a076-42c6-9835-16945868e866"></a>Windows

1. De Azure VM-agent downloaden:
   1.  Down load het [installatie pakket van de Azure VM-agent](https://go.microsoft.com/fwlink/?LinkId=394789).
   1.  Sla het MSI-pakket van de VM-agent lokaal op een personal computer of server op.
1. Installeer de Azure VM-agent:
   1.  Maak verbinding met de geïmplementeerde Azure-VM met behulp van Remote Desktop Protocol (RDP).
   1.  Open een Windows Verkenner-venster op de virtuele machine en selecteer de doelmap voor het MSI-bestand van de VM-agent.
   1.  Sleep het MSI-bestand van het installatie programma van de Azure VM-agent van uw lokale computer/server naar de doel directory van de VM-agent op de VM.
   1.  Dubbel klik op het MSI-bestand op de virtuele machine.
1. Voor virtuele machines die lid zijn van on-premises domeinen, moet u ervoor zorgen dat de uiteindelijke Internet proxy instellingen ook van toepassing zijn op het Windows lokale systeem account (S-1-5-18) in de VM, zoals beschreven in [de proxy configureren][deployment-guide-configure-proxy]. De VM-agent wordt uitgevoerd in deze context en moet verbinding kunnen maken met Azure.

Er is geen interactie van de gebruiker vereist om de Azure VM-agent bij te werken. De VM-agent wordt automatisch bijgewerkt en het opnieuw opstarten van de VM is niet vereist.

#### <a name="6889ff12-eaaf-4f3c-97e1-7c9edc7f7542"></a>Linux

Gebruik de volgende opdrachten om de VM-agent voor Linux te installeren:

* **SUSE Linux Enterprise Server (SLES)**

  ```
  sudo zypper install WALinuxAgent
  ```

* **Red Hat Enterprise Linux (RHEL) of Oracle Linux**

  ```
  sudo yum install WALinuxAgent
  ```

Als de agent al is geïnstalleerd, voert u de stappen uit die worden beschreven in [de Azure Linux-agent op een VM bijwerken naar de meest recente versie van github][virtual-machines-linux-update-agent]om de Azure Linux-agent bij te werken.

### <a name="baccae00-6f79-4307-ade4-40292ce4e02d"></a>De proxy configureren

De stappen die u moet nemen voor het configureren van de proxy in Windows verschillen van de manier waarop u de proxy in Linux configureert.

#### <a name="windows"></a>Windows

Proxy-instellingen moeten correct worden ingesteld voor het lokale systeem account om toegang te krijgen tot internet. Als uw proxy instellingen niet zijn ingesteld door groepsbeleid, kunt u de instellingen voor het lokale systeem account configureren.

1. Ga naar **Start**, Voer **gpedit. msc**in en selecteer vervolgens **Enter**.
1. Selecteer **computer configuratie** > **Beheersjablonen** > **Windows-onderdelen** > **Internet Explorer**. Zorg ervoor dat de instelling **proxy-instellingen per computer (in plaats van per gebruiker)** is uitgeschakeld of niet is geconfigureerd.
1. Ga in **het configuratie scherm**naar de**Internet opties**van >  **netwerk centrum en delen**.
1. Op het tabblad **verbindingen** selecteert u de knop **LAN-instellingen** .
1. Schakel het selectie vakje **instellingen automatisch detecteren** uit.
1. Schakel het selectie vakje **een proxy server gebruiken voor uw LAN** in en voer vervolgens het proxy adres en de poort in.
1. Selecteer de knop **Geavanceerd** .
1. Voer in het vak **uitzonde ringen** het IP-adres **168.63.129.16**in. Selecteer **OK**.

#### <a name="linux"></a>Linux

Configureer de juiste proxy in het configuratie bestand van de Microsoft Azure Guest-agent, die zich op \\etc\\waagent. conf bevindt.

Stel de volgende para meters in:

1. **Host voor http-proxy**. Stel deze bijvoorbeeld in op **proxy. Corp. local**.
   ```
   HttpProxy.Host=<proxy host>

   ```
1. **Http-proxy poort**. Stel deze bijvoorbeeld in op **80**.
   ```
   HttpProxy.Port=<port of the proxy host>

   ```
1. Start de agent opnieuw.

   ```
   sudo service waagent restart
   ```

De proxy-instellingen \\in\\etc waagent. conf zijn ook van toepassing op de vereiste VM-extensies. Als u de Azure-opslag plaatsen wilt gebruiken, moet u ervoor zorgen dat het verkeer naar deze opslag plaatsen niet via uw on-premises intranet gaat. Als u door de gebruiker gedefinieerde routes hebt gemaakt om geforceerde Tunneling in te scha kelen, moet u ervoor zorgen dat u een route toevoegt die verkeer naar de opslag plaatsen rechtstreeks naar het internet stuurt, en niet via uw site-naar-site-VPN-verbinding.

* **SLES**

  U moet ook routes toevoegen voor de IP-adressen die worden \\vermeld\\in etc regionserverclnt. cfg. In de volgende afbeelding ziet u een voor beeld:

  ![Geforceerde tunneling][deployment-guide-figure-50]


* **RHEL**

  U moet ook routes toevoegen voor de IP-adressen van de hosts die worden \\vermeld\\in etc yum. opslag plaatsen\\. d rhui-load balancers. Zie de voor gaande afbeelding voor een voor beeld.

* **Oracle Linux**

  Er zijn geen opslag plaatsen voor Oracle Linux op Azure. U moet uw eigen opslag plaatsen voor Oracle Linux configureren of gebruikmaken van de open bare opslag plaatsen.

Zie door de gebruiker [gedefinieerde routes en door sturen via IP][virtual-networks-udr-overview]voor meer informatie over door de gebruiker gedefinieerde routes.

### <a name="d98edcd3-f2a1-49f7-b26a-07448ceb60ca"></a>De uitgebreide bewakings extensie van Azure configureren voor SAP

Wanneer u de virtuele machine hebt voor bereid zoals beschreven in [implementatie scenario's van vm's voor SAP on Azure][deployment-guide-3], wordt de Azure VM-agent geïnstalleerd op de virtuele machine. De volgende stap is het implementeren van de uitgebreide bewakings extensie van Azure voor SAP, die beschikbaar is in de Azure extension-opslag plaats in de wereld wijde Azure-data centers. Zie [Azure virtual machines planning en implementatie voor SAP][planning-guide-9.1]net-Weaver voor meer informatie.

U kunt Power shell of Azure CLI gebruiken voor het installeren en configureren van de uitgebreide bewakings extensie van Azure voor SAP. Als u de uitbrei ding op een Windows-of Linux-VM wilt installeren met behulp van een Windows-computer, raadpleegt u [Azure PowerShell][deployment-guide-4.5.1]. Als u de uitbrei ding op een Linux-VM wilt installeren met behulp van een Linux-Desktop, raadpleegt u [Azure cli][deployment-guide-4.5.2].

#### <a name="987cf279-d713-4b4c-8143-6b11589bb9d4"></a>Azure PowerShell voor Linux-en Windows-Vm's

De Azure Enhanced monitoring-extensie voor SAP installeren met behulp van Power shell:

1. Zorg ervoor dat u de nieuwste versie van de cmdlet Azure PowerShell hebt geïnstalleerd. Zie [deploying Azure PowerShell cmdlets][deployment-guide-4.1](Engelstalig) voor meer informatie.  
1. Voer de volgende PowerShell-cmdlet uit.
    Voer uit `commandlet Get-AzEnvironment`voor een lijst met beschik bare omgevingen. Als u wereld wijd Azure wilt gebruiken, is uw omgeving **Cloud**. Voor Azure in China selecteert u **AzureChinaCloud**.

    ```powershell
    $env = Get-AzEnvironment -Name <name of the environment>
    Connect-AzAccount -Environment $env
    Set-AzContext -SubscriptionName <subscription name>

    Set-AzVMAEMExtension -ResourceGroupName <resource group name> -VMName <virtual machine name>
    ```

Nadat u uw account gegevens hebt ingevoerd en de virtuele Azure-machine hebt geïdentificeerd, implementeert het script de vereiste uitbrei dingen en schakelt de vereiste functies in. Dit kan enkele minuten duren.
Zie `Set-AzVMAEMExtension` [set-AzVMAEMExtension][msdn-set-Azvmaemextension]voor meer informatie over.

![De SAP-specifieke Azure-cmdlet Set-AzVMAEMExtension is uitgevoerd.][deployment-guide-figure-900]

De `Set-AzVMAEMExtension` configuratie bevat alle stappen voor het configureren van host-bewaking voor SAP.

De script uitvoer bevat de volgende informatie:

* Bevestiging dat de controle van de besturingssysteem schijf en alle extra gegevens schijven is geconfigureerd.
* De volgende twee berichten bevestigen de configuratie van metrische opslag gegevens voor een specifiek opslag account.
* Een regel van uitvoer geeft de status van de werkelijke update van de bewakings configuratie.
* Een andere regel van uitvoer bevestigt dat de configuratie is geïmplementeerd of bijgewerkt.
* De laatste regel van de uitvoer is informatief. Hier ziet u de opties voor het testen van de bewakings configuratie.
* Als u wilt controleren of alle stappen van de uitgebreide Azure-bewaking zijn uitgevoerd en dat de Azure-infra structuur de benodigde gegevens bevat, gaat u verder met de gereedheids controle voor de Azure Enhanced monitoring-extensie voor SAP, zoals beschreven in [ Gereedheids controle voor Azure Enhanced monitoring voor SAP][deployment-guide-5.1].
* Wacht 15-30 minuten tot Azure Diagnostics de relevante gegevens wilt verzamelen.

#### <a name="408f3779-f422-4413-82f8-c57a23b4fc2f"></a>Azure CLI voor Linux-Vm's

De Azure Enhanced monitoring-extensie voor SAP installeren met behulp van Azure CLI:

   1. Installeer de klassieke Azure-CLI, zoals beschreven in [Installeer de klassieke Azure-cli][azure-cli].
   1. Meld u aan met uw Azure-account:

      ```
      azure login
      ```

   1. Overschakelen naar Azure Resource Manager modus:

      ```
      azure config mode arm
      ```

   1. Uitgebreide Azure-bewaking inschakelen:

      ```
      azure vm enable-aem <resource-group-name> <vm-name>
      ```

1. Installeren met behulp van Azure CLI 2,0

   1. Installeer Azure CLI 2,0, zoals beschreven in [Azure cli 2,0 installeren][azure-cli-2].
   1. Meld u aan met uw Azure-account:

      ```
      az login
      ```

   1. Azure CLI AEM-extensie installeren
  
      ```
      az extension add --name aem
      ```
  
   1. Installeer de extensie met
  
      ```
      az vm aem set -g <resource-group-name> -n <vm name>
      ```

1. Controleer of de Azure Enhanced monitoring-extensie actief is op de virtuele Azure Linux-machine. Controleer of het bestand \\var\\lib\\AzureEnhancedMonitor\\PerfCounters bestaat. Als deze bestaat, voert u bij een opdracht prompt de volgende opdracht uit om de gegevens weer te geven die zijn verzameld door Azure Enhanced monitor:

   ```
   cat /var/lib/AzureEnhancedMonitor/PerfCounters
   ```

   De uitvoer ziet er als volgt uit:
   ```
   ...
   2;cpu;Current Hw Frequency;;0;2194.659;MHz;60;1444036656;saplnxmon;
   2;cpu;Max Hw Frequency;;0;2194.659;MHz;0;1444036656;saplnxmon;
   ...
   ```

## <a name="564adb4f-5c95-4041-9616-6635e83a810b"></a>Controles en probleem oplossing voor end-to-end-bewaking

Nadat u uw Azure-VM hebt geïmplementeerd en de relevante Azure-bewakings infrastructuur hebt ingesteld, controleert u of alle onderdelen van de Azure Enhanced monitoring-extensie werken zoals verwacht.

Voer de gereedheids controle uit voor de uitgebreide bewakings extensie van Azure voor SAP, zoals beschreven in [de gereedheids controle voor de uitgebreide bewakings extensie van Azure voor SAP][deployment-guide-5.1]. Als alle resultaten van de gereedheids controle positief zijn en alle relevante prestatie meter items worden weer gegeven, is Azure monitoring geslaagd. U kunt door gaan met de installatie van de SAP host-agent zoals beschreven in de SAP-notities in [SAP-bronnen][deployment-guide-2.2]. Als de gereedheids controle aangeeft dat er prestatie meter items ontbreken, voert u de status controle voor de infra structuur van Azure monitoring uit, zoals wordt beschreven in de [status controle voor de configuratie van de infra structuur van Azure][deployment-guide-5.2]. Zie [problemen met Azure-bewaking voor SAP oplossen][deployment-guide-5.3]voor meer opties voor probleem oplossing.

### <a name="bb61ce92-8c5c-461f-8c53-39f5e5ed91f2"></a>Gereedheids controle voor de uitgebreide bewakings extensie van Azure voor SAP

Met deze controle wordt ervoor gezorgd dat alle prestatie gegevens die in uw SAP-toepassing worden weer gegeven, worden verzorgd door de onderliggende infra structuur van Azure monitoring.

#### <a name="run-the-readiness-check-on-a-windows-vm"></a>De gereedheids controle uitvoeren op een Windows-VM

1. Meld u aan bij de virtuele machine van Azure (met een beheerders account is niet nodig).
1. Open een opdrachtpromptvenster.
1. Ga naar de opdracht prompt en wijzig de map in de installatiemap van de Azure Enhanced monitoring-extensie voor SAP: C:\\Packages\\Plugins\\Microsoft.AzureCAT.AzureEnhancedMonitoring.AzureCATExtensionHandler\\&lt;version>\\drop

   De *versie* in het pad naar de bewakings uitbreiding kan variëren. Als er mappen voor meerdere versies van de bewakings extensie in de installatiemap worden weer gegeven, controleert u de configuratie van de AzureEnhancedMonitoring Windows-service en schakelt u over naar de map die wordt aangegeven als *pad naar uitvoerbaar bestand*.

   ![Eigenschappen van de service die de uitgebreide bewakings extensie van Azure uitvoert voor SAP][deployment-guide-figure-1000]

1. Voer bij de opdracht prompt **azperflib. exe** zonder para meters uit.

   > [!NOTE]
   > Azperflib. exe wordt in een lus uitgevoerd en de verzamelde tellers worden elke 60 seconden bijgewerkt. Sluit het opdracht prompt venster om de lus te beëindigen.
   >
   >

Als de Azure Enhanced monitoring-extensie niet is geïnstalleerd of als de AzureEnhancedMonitoring-service niet wordt uitgevoerd, is de extensie niet op de juiste wijze geconfigureerd. Zie [problemen met de Azure-bewakings infrastructuur voor SAP oplossen][deployment-guide-5.3]voor meer informatie over het implementeren van de uitbrei ding.

> [!NOTE]
> Azperflib. exe is een onderdeel dat niet kan worden gebruikt voor eigen doel einden. Het is een onderdeel dat Azure-bewakings gegevens levert die betrekking hebben op de virtuele machine voor de SAP Hosta Gent.
> 

##### <a name="check-the-output-of-azperflibexe"></a>Controleer de uitvoer van azperflib. exe

Azperflib. exe-uitvoer toont alle gevulde Azure-prestatie meter items voor SAP. Onder aan de lijst met verzamelde items ziet u de status van Azure-bewaking in een samen vatting en status indicator.

![Uitvoer van de status controle door azperflib. exe uit te voeren, wat aangeeft dat er geen problemen zijn][deployment-guide-figure-1100]
<a name="figure-11"></a>

Controleer het resultaat dat wordt geretourneerd voor de **tellers totale** uitvoer. dit wordt gerapporteerd als leegen voor de status die in de voor gaande afbeelding wordt weer gegeven.

Interpreteer de resulterende waarden als volgt:

| Resultaat waarden van Azperflib. exe | Status van Azure-bewaking |
| --- | --- |
| **API-aanroepen-niet beschikbaar** | Tellers die niet beschikbaar zijn, zijn mogelijk niet van toepassing op de configuratie van de virtuele machine of fouten. Zie de **integriteits status**. |
| **Totaal aantal tellers-leeg** |De volgende twee Azure-opslag-items kunnen leeg zijn: <ul><li>Lezen van de opslag op latentie server msec</li><li>Opslag gelezen op latentie E2E msec</li></ul>Alle andere tellers moeten waarden bevatten. |
| **Integriteits status** |Alleen OK als de retour status **OK**is. |
| **Diagnostics** |Gedetailleerde informatie over de status. |

Als de **status** waarde niet **OK**is, volgt u de instructies in de [status controle voor de configuratie van de Azure monitoring-infra structuur][deployment-guide-5.2].

#### <a name="run-the-readiness-check-on-a-linux-vm"></a>De gereedheids controle uitvoeren op een virtuele Linux-machine

1. Maak verbinding met de virtuele Azure-machine met behulp van SSH.

1. Controleer de uitvoer van de uitgebreide bewakings extensie van Azure.

   a.  Voer `more /var/lib/AzureEnhancedMonitor/PerfCounters` uit.

   **Verwacht resultaat**: Hiermee wordt een lijst met prestatie meter items geretourneerd. Het bestand mag niet leeg zijn.

   b. Voer `cat /var/lib/AzureEnhancedMonitor/PerfCounters | grep Error` uit.

   **Verwacht resultaat**: Retourneert één regel waarbij de fout **geen**is, bijvoorbeeld **3; config; Fout;; 0; 0; geen; 0; 1456416792; TST-servercs;**

   c. Voer `more /var/lib/AzureEnhancedMonitor/LatestErrorRecord` uit.

   **Verwacht resultaat**: Retourneert als leeg of bestaat niet.

Als de voor gaande controle niet is geslaagd, voert u de volgende extra controles uit:

1. Zorg ervoor dat de waagent is geïnstalleerd en ingeschakeld.

   a.  Voer `sudo ls -al /var/lib/waagent/` uit.

     **Verwacht resultaat**: Hiermee wordt de inhoud van de waagent-map weer gegeven.

   b.  Voer `ps -ax | grep waagent` uit.

   **Verwacht resultaat**: Hiermee wordt één item weer gegeven dat vergelijkbaar is met:`python /usr/sbin/waagent -daemon`

1. Zorg ervoor dat de Azure Enhanced monitoring-extensie is geïnstalleerd en wordt uitgevoerd.

   a.  Voer `sudo sh -c 'ls -al /var/lib/waagent/Microsoft.OSTCExtensions.AzureEnhancedMonitorForLinux-*/'` uit.

   **Verwacht resultaat**: Geeft een lijst van de inhoud van de Azure Enhanced monitoring-extensie Directory.

   b. Voer `ps -ax | grep AzureEnhanced` uit.

   **Verwacht resultaat**: Hiermee wordt één item weer gegeven dat vergelijkbaar is met:`python /var/lib/waagent/Microsoft.OSTCExtensions.AzureEnhancedMonitorForLinux-2.0.0.2/handler.py daemon`

1. Installeer de SAP host agent zoals beschreven in SAP Note [1031096]en controleer de uitvoer van `saposcol`.

   a.  Voer `/usr/sap/hostctrl/exe/saposcol -d` uit.

   b.  Voer `dump ccm` uit.

   c.  Controleer of de metrische gegevens van de **Virtualization_Configuration\Enhanced-toegangs meting** **waar**zijn.

Als u al een SAP NetWeaver ABAP-toepassings server hebt geïnstalleerd, opent u Trans Action ST06 en controleert u of verbeterde controle is ingeschakeld.

Als een van deze controles mislukt en als u meer wilt weten over het opnieuw implementeren van de uitbrei ding, raadpleegt u [problemen met de Azure-bewakings infrastructuur voor SAP oplossen][deployment-guide-5.3].

### <a name="e2d592ff-b4ea-4a53-a91a-e5521edb6cd1"></a>Status controle voor de configuratie van de Azure monitoring-infra structuur

Als sommige bewakings gegevens niet correct worden afgeleverd, zoals wordt aangegeven door de test die wordt beschreven in de [gereedheids controle voor Azure Enhanced monitoring voor SAP][deployment-guide-5.1], voert u de `Test-AzVMAEMExtension` cmdlet uit om te controleren of de infra structuur van Azure monitoring en de bewaking de extensie voor SAP is correct geconfigureerd.

1. Zorg ervoor dat u de nieuwste versie van de cmdlet Azure PowerShell hebt geïnstalleerd, zoals beschreven in [Azure PowerShell-cmdlets implementeren][deployment-guide-4.1].
1. Voer de volgende PowerShell-cmdlet uit. Voer de cmdlet `Get-AzEnvironment`uit voor een lijst met beschik bare omgevingen. Als u wereld wijd Azure wilt gebruiken, selecteert u de **Cloud** -omgeving. Voor Azure in China selecteert u **AzureChinaCloud**.
   ```powershell
   $env = Get-AzEnvironment -Name <name of the environment>
   Connect-AzAccount -Environment $env
   Set-AzContext -SubscriptionName <subscription name>
   Test-AzVMAEMExtension -ResourceGroupName <resource group name> -VMName <virtual machine name>
   ```

1. Voer uw account gegevens in en Identificeer de virtuele Azure-machine.

   ![Invoer pagina van een SAP-specifieke Azure-cmdlet test-VMConfigForSAP_GUI][deployment-guide-figure-1200]

1. Het script test de configuratie van de virtuele machine die u selecteert.

   ![Uitvoer van een geslaagde test van de Azure monitoring-infra structuur voor SAP][deployment-guide-figure-1300]

Zorg ervoor dat elk resultaat van de status controle **OK**is. Als sommige controles niet **OK**worden weer gegeven, voert u de update-cmdlet uit zoals beschreven in [de Azure Enhanced monitoring-extensie voor SAP configureren][deployment-guide-4.5]. Wacht 15 minuten en herhaal de controles die zijn beschreven in de [gereedheids controle voor uitgebreide controle van Azure][deployment-guide-5.1] voor de SAP-en [status controle voor de configuratie van de infra structuur van Azure monitoring][deployment-guide-5.2]. Als de controles nog steeds een probleem met enkele of alle items aangeven, raadpleegt u [problemen met de Azure-bewakings infrastructuur voor SAP oplossen][deployment-guide-5.3].

> [!Note]
> U kunt waarschuwingen krijgen in gevallen waarin u beheerde standaard-Azure-schijven gebruikt. Er worden waarschuwingen weer gegeven in plaats van de tests die ' OK ' retour neren. Dit is normaal en bedoeld voor dat schijf type. Zie ook [problemen met de Azure monitoring-infra structuur voor SAP oplossen][deployment-guide-5.3]
> 

### <a name="fe25a7da-4e4e-4388-8907-8abc2d33cfd8"></a>Problemen met de Azure monitoring-infra structuur voor SAP oplossen

#### <a name="windowslogo_windows-azure-performance-counters-do-not-show-up-at-all"></a>![Windows][Logo_Windows] Prestatie meter items van Azure worden helemaal niet weer gegeven

De AzureEnhancedMonitoring Windows-service verzamelt prestatie gegevens in Azure. Als de service niet juist is geïnstalleerd of niet op uw virtuele machine wordt uitgevoerd, kunnen er geen metrische gegevens voor prestaties worden verzameld.

##### <a name="the-installation-directory-of-the-azure-enhanced-monitoring-extension-is-empty"></a>De installatiemap van de uitgebreide bewakings extensie van Azure is leeg

###### <a name="issue"></a>Probleem

De installatie directory C:\\pakketten\\invoeg\\toepassingen micro soft. AzureCAT. AzureEnhancedMonitoring\\. AzureCATExtensionHandler\\&lt;versie > Drop is leeg.

###### <a name="solution"></a>Oplossing

De extensie is niet geïnstalleerd. Bepaal of dit een proxy probleem is (zoals eerder beschreven). Mogelijk moet u de computer opnieuw opstarten of het `Set-AzVMAEMExtension` configuratie script opnieuw uitvoeren.

##### <a name="service-for-azure-enhanced-monitoring-does-not-exist"></a>De service voor uitgebreide Azure-bewaking bestaat niet

###### <a name="issue"></a>Probleem

De AzureEnhancedMonitoring Windows-service bestaat niet.

Azperflib. exe-uitvoer genereert een fout:

![Het uitvoeren van azperflib. exe geeft aan dat de service van de Azure Enhanced monitoring-extensie voor SAP niet wordt uitgevoerd][deployment-guide-figure-1400]
<a name="figure-14"></a>

###### <a name="solution"></a>Oplossing

Als de service niet bestaat, is de Azure Enhanced monitoring-extensie voor SAP niet op de juiste wijze geïnstalleerd. Implementeer de uitbrei ding opnieuw met behulp van de stappen die worden beschreven in het implementatie scenario in [implementatie scenario's van vm's voor SAP in azure][deployment-guide-3].

Nadat u de uitbrei ding hebt geïmplementeerd, controleert u na één uur of de prestatie meter items van Azure zijn opgenomen in de Azure VM.

##### <a name="service-for-azure-enhanced-monitoring-exists-but-fails-to-start"></a>De uitgebreide bewakings service van Azure bestaat, maar kan niet worden gestart

###### <a name="issue"></a>Probleem

De AzureEnhancedMonitoring Windows-service bestaat en is ingeschakeld, maar kan niet worden gestart. Raadpleeg het gebeurtenis logboek van de toepassing voor meer informatie.

###### <a name="solution"></a>Oplossing

De configuratie is onjuist. Start de bewakings extensie voor de virtuele machine opnieuw op, zoals beschreven in [de Azure Enhanced monitoring-extensie voor SAP configureren][deployment-guide-4.5].

#### <a name="windowslogo_windows-some-azure-performance-counters-are-missing"></a>![Windows][Logo_Windows] Sommige prestatie meter items van Azure ontbreken

De AzureEnhancedMonitoring Windows-service verzamelt prestatie gegevens in Azure. De service haalt gegevens op uit verschillende bronnen. Sommige configuratie gegevens worden lokaal verzameld en sommige prestatie gegevens worden gelezen uit Azure Diagnostics. Opslag tellers worden gebruikt vanuit uw logboek registratie op het niveau van het opslag abonnement.

Als u het probleem niet kunt oplossen met SAP Note [1999351] , voert u `Set-AzVMAEMExtension` het configuratie script opnieuw uit. Mogelijk moet u een uur wachten, omdat er mogelijk geen opslag analyse-of diagnostische tellers worden gemaakt direct nadat deze zijn ingeschakeld. Als het probleem zich blijft voordoen, opent u een SAP-klant bericht voor ondersteuning op de component BC-OP-NT-AZR voor Windows of BC-OP-LNX-AZR voor een virtuele Linux-machine.

#### <a name="linuxlogo_linux-azure-performance-counters-do-not-show-up-at-all"></a>![Linux][Logo_Linux] Prestatie meter items van Azure worden helemaal niet weer gegeven

Metrische gegevens over prestaties in Azure worden verzameld door een daemon. Als de daemon niet wordt uitgevoerd, kunnen er geen prestatie gegevens worden verzameld.

##### <a name="the-installation-directory-of-the-azure-enhanced-monitoring-extension-is-empty"></a>De installatiemap van de uitgebreide bewakings extensie van Azure is leeg

###### <a name="issue"></a>Probleem

De map \\var\\lib\\waagent\\ heeft geen submap voor de uitgebreide bewakings extensie van Azure.

###### <a name="solution"></a>Oplossing

De extensie is niet geïnstalleerd. Bepaal of dit een proxy probleem is (zoals eerder beschreven). Mogelijk moet u de computer opnieuw opstarten en/of het `Set-AzVMAEMExtension` configuratie script opnieuw uitvoeren.

##### <a name="the-execution-of-set-azvmaemextension-and-test-azvmaemextension-show-warning-messages-stating-that-standard-managed-disks-are-not-supported"></a>Bij de uitvoering van set-AzVMAEMExtension en test-AzVMAEMExtension worden waarschuwings berichten weer gegeven met de mede deling dat standaard Managed Disks niet worden ondersteund

###### <a name="issue"></a>Probleem

Bij het uitvoeren van set-AzVMAEMExtension of test-AzVMAEMExtension-berichten, zoals deze worden weer gegeven:

<pre><code>
WARNING: [WARN] Standard Managed Disks are not supported. Extension will be installed but no disk metrics will be available.
WARNING: [WARN] Standard Managed Disks are not supported. Extension will be installed but no disk metrics will be available.
WARNING: [WARN] Standard Managed Disks are not supported. Extension will be installed but no disk metrics will be available.
</code></pre>

Als u azperfli. exe uitvoert zoals eerder beschreven, kunt u een resultaat krijgen dat een niet-goede status aangeeft. 

###### <a name="solution"></a>Oplossing

De berichten worden veroorzaakt door het feit dat standaard Managed Disks geen Api's leveren die worden gebruikt door de bewakings extensie om te controleren op statistische gegevens van de standaard Azure Storage accounts. Dit is niet van belang. Reden voor het introduceren van de bewaking voor standaard Disk Storage accounts is het beperken van I/O's die regel matig plaatsvonden. Managed disks voor komt een dergelijke beperking door het aantal schijven in een opslag account te beperken. Daarom is het niet zo dat dat type bewakings gegevens niet kritiek zijn.


#### <a name="linuxlogo_linux-some-azure-performance-counters-are-missing"></a>![Linux][Logo_Linux] Sommige prestatie meter items van Azure ontbreken

De metrische gegevens over prestaties in Azure worden verzameld door een daemon. deze haalt informatie uit verschillende bronnen op. Sommige configuratie gegevens worden lokaal verzameld en sommige prestatie gegevens worden gelezen uit Azure Diagnostics. Opslag items zijn afkomstig uit de logboeken in uw opslag abonnement.

Voor een volledige en bijgewerkte lijst met bekende problemen raadpleegt u SAP Note [1999351], die aanvullende informatie over probleem oplossing biedt voor verbeterde Azure-bewaking voor SAP.

Als problemen oplossen met SAP Note [1999351] het probleem niet wordt opgelost, voert u `Set-AzVMAEMExtension` het configuratie script opnieuw uit zoals beschreven in [de Azure Enhanced monitoring-extensie voor SAP configureren][deployment-guide-4.5]. Mogelijk moet u een uur wachten, omdat er mogelijk geen opslag analyse-of diagnostische tellers worden gemaakt direct nadat deze zijn ingeschakeld. Als het probleem zich blijft voordoen, opent u een SAP-klant bericht voor ondersteuning op de component BC-OP-NT-AZR voor Windows of BC-OP-LNX-AZR voor een virtuele Linux-machine.
