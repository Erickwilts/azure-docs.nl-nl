---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 11/30/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 42e983ead6f7562c6a31cf9ef4ad2d97d0ff9707
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67673311"
---
1. In de [Azure-portal](https://portal.azure.com), selecteer het virtuele netwerk van Resource Manager waarvoor u wenst te maken van een virtuele netwerkgateway.

2. In de **instellingen** sectie van uw virtuele netwerk weergeeft, schakelt **subnetten** om uit te breiden de **subnetten** pagina.

3. Op de **subnetten** weergeeft, schakelt **gatewaysubnet** openen de **subnet toevoegen** pagina.

   ![Het gatewaysubnet toevoegen](./media/vpn-gateway-add-gwsubnet-rm-portal-include/addgwsub.png "Het gatewaysubnet toevoegen")

4. De **naam** voor uw subnet automatisch autofilled met de waarde is *GatewaySubnet*. Deze waarde is vereist voor Azure voor het herkennen van het subnet als het gatewaysubnet. Pas de autofilled **adresbereik** vervolgens selecteert u waarden die moeten worden overeenkomstig uw configuratievereisten **OK** om het subnet te maken.

   ![Het subnet toevoegen](./media/vpn-gateway-add-gwsubnet-rm-portal-include/addsubnetgw.png "Het subnet toevoegen")
