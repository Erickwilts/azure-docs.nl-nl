---
title: 'Zelfstudie: Azure Firewall implementeren en configureren met de Azure-portal'
description: In deze zelfstudie leert u hoe u Azure Firewall kunt implementeren en configureren via de Azure-portal.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: tutorial
ms.date: 4/9/2019
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 9d7b9673101ed3b6ff85a9981ba061bc870762b1
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65405685"
---
# <a name="tutorial-deploy-and-configure-azure-firewall-using-the-azure-portal"></a>Zelfstudie: Azure Firewall implementeren en configureren met de Azure-portal

Het beheren van toegang tot uitgaande netwerken is een belangrijk onderdeel van een algemeen netwerkbeveiligingsabonnement. U wilt bijvoorbeeld de toegang tot websites beperken. Of u wilt om te beperken van de uitgaande IP-adressen en poorten die kunnen worden benaderd.

Een van de manieren waarop u de toegang tot uitgaande netwerken kunt beheren vanaf een Azure-subnet is met Azure Firewall. Met Azure Firewall kunt u het volgende configureren:

* Toepassingsregels die volledig gekwalificeerde domeinnamen (FQDN's) definiëren waartoe toegang kan worden verkregen via een subnet.
* Netwerkregels die een bronadres, protocol, doelpoort en doeladres definiëren.

Netwerkverkeer is onderhevig aan de geconfigureerde firewallregels wanneer u het routeert naar de firewall als standaardgateway van het subnet.

Voor deze zelfstudie maakt u één vereenvoudigd VNet met drie subnetten voor eenvoudige implementatie. Voor productie-implementaties, een [hub en spoke-model](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke) wordt aanbevolen, waarbij de firewall wordt in een eigen VNet. De workload-servers zijn in gekoppelde VNets in dezelfde regio met een of meer subnetten.

* **AzureFirewallSubnet** – De firewall bevindt zich in dit subnet.
* **Workload-SN** – De workloadserver bevindt zich in dit subnet. Het netwerkverkeer van dit subnet gaat via de firewall.
* **Jump-SN** – De "jumpserver" bevindt zich in dit subnet. De jumpserver heeft een openbaar IP-adres waarmee u verbinding kunt maken via een extern bureaublad. Van daaruit kunt u vervolgens (via een ander extern bureaublad) verbinding maken met de workloadserver.

![Netwerkinfrastructuur van zelfstudie](media/tutorial-firewall-rules-portal/Tutorial_network.png)

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een testnetwerkomgeving instellen
> * Een firewall implementeren
> * Een standaardroute maken
> * Een regel voor een toepassing voor toegang tot www.google.com configureren
> * Een netwerkregel configureren om toegang tot externe DNS-servers toe te staan
> * De firewall testen

U kunt deze zelfstudie desgewenst volgen met behulp van [Azure PowerShell](deploy-ps.md).

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="set-up-the-network"></a>Het netwerk instellen

Maak eerst een resourcegroep met de resources die nodig zijn om de firewall te implementeren. Maak vervolgens een VNet, subnetten en testservers.

### <a name="create-a-resource-group"></a>Een resourcegroep maken

De resourcegroep bevat alle resources voor de zelfstudie.

1. Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com).
2. Selecteer op de Azure portal startpagina **resourcegroepen** > **toevoegen**.
3. Bij **Resourcegroepnaam** typt u **Test-FW-RG**.
4. Bij **Abonnement** selecteert u uw abonnement.
5. Bij **Resourcegroeplocatie** selecteert u een locatie. Alle volgende resources die u maakt, moeten zich op dezelfde locatie bevinden.
6. Selecteer **Maken**.

### <a name="create-a-vnet"></a>Een VNet maken

Dit VNet bevat drie subnetten.

1. Selecteer in de Azure portal startpagina **een resource maken**.
2. Onder **netwerken**, selecteer **virtueel netwerk**.
4. Bij **Naam** typt u **Test-FW-VN**.
5. Bij **Adresruimte** typt u **10.0.0.0/16**.
6. Bij **Abonnement** selecteert u uw abonnement.
7. Voor **resourcegroep**, selecteer **Test-FW-RG**.
8. Bij **Locatie** selecteert u dezelfde locatie die u eerder hebt gebruikt.
9. Onder **Subnet** typt u bij **Naam** de naam **AzureFirewallSubnet**. De firewall zal zich in dit subnet bevinden, en de subnetnaam **moet** AzureFirewallSubnet zijn.
10. Bij **Adresbereik** typt u **10.0.1.0/24**.
11. Accepteer de standaardinstellingen, en selecteer vervolgens **maken**.

> [!NOTE]
> De minimale grootte van het subnet AzureFirewallSubnet is /26.

### <a name="create-additional-subnets"></a>Extra subnetten maken

Maak vervolgens subnetten voor de jumpserver en een subnet voor de workloadservers.

1. Selecteer op de Azure portal startpagina **resourcegroepen** > **Test-FW-RG**.
2. Selecteer de **Test-FW-VN** virtueel netwerk.
3. Selecteer **subnetten** > **+ Subnet**.
4. Bij **Naam** typt u **Workload-SN**.
5. Bij **Adresbereik** typt u **10.0.2.0/24**.
6. Selecteer **OK**.

Maak nog een subnet met **Jump-SN** als naam en **10.0.3.0/24** als adresbereik.

### <a name="create-virtual-machines"></a>Virtuele machines maken

Maak nu de virtuele jump- en workloadmachines en plaats ze in de toepasselijke subnetten.

1. Selecteer **Een resource maken** in de Azure-portal.
2. Selecteer **Compute** en selecteer vervolgens **Windows Server 2016 Datacenter** in de lijst Aanbevolen.
3. Voer deze waarden in voor de virtuele machine:

   |Instelling  |Waarde  |
   |---------|---------|
   |Resourcegroep     |**Test-FW-RG**|
   |Naam van de virtuele machine     |**SRV-landingspagina**|
   |Regio     |Hetzelfde als de vorige|
   |Gebruikersnaam van beheerder     |**azureuser**|
   |Wachtwoord     |**Azure123456!**|

4. Onder **regels voor binnenkomende poort**, voor **openbare binnenkomende poorten**, selecteer **geselecteerde poorten toestaat**.
5. Selecteer **RDP (3389)** voor **Binnenkomende poorten selecteren**.

6. Accepteer de overige standaardwaarden en selecteer **volgende: Schijven**.
7. Accepteer de standaardwaarden van de schijf en selecteer **volgende: Netwerken**.
8. Zorg ervoor dat **Test-FW-VN** is geselecteerd voor het virtuele netwerk en dat het subnet **Jump-SN** is.
9. Voor **openbaar IP-adres**, accepteer de nieuwe openbare ip-adres standaardnaam (Srv-Jump-ip).
11. Accepteer de overige standaardwaarden en selecteer **volgende: Beheer**.
12. Selecteer **uit** diagnostische gegevens over opstarten uitschakelen. Accepteer de overige standaardwaarden en selecteer **revisie + maken**.
13. Controleer de instellingen op de pagina Samenvatting en selecteer vervolgens **maken**.

Gebruik de informatie in de volgende tabel om te configureren van een andere virtuele machine met de naam **Srv-werk**. De rest van de configuratie is hetzelfde als voor de virtuele machine Srv-Jump.

|Instelling  |Value  |
|---------|---------|
|Subnet|**Workload-SN**|
|Openbaar IP|**Geen**|
|Openbare poorten voor inkomend verkeer|**Geen**|

## <a name="deploy-the-firewall"></a>De firewall implementeren

Implementeer de firewall in het VNet.

1. Selecteer in de portal startpagina **een resource maken**.
2. Type **firewall** in het zoekvak en druk op **Enter**.
3. Selecteer **Firewall** en selecteer vervolgens **maken**.
4. Gebruik op de pagina **Firewall maken** de volgende tabel om de firewall te configureren:

   |Instelling  |Value  |
   |---------|---------|
   |Abonnement     |\<uw abonnement\>|
   |Resourcegroep     |**Test-FW-RG** |
   |Name     |**Test-FW01**|
   |Locatie     |Selecteer dezelfde locatie die u eerder hebt gebruikt|
   |Een virtueel netwerk kiezen     |**Gebruik bestaande**: **Test-FW-VN**|
   |Openbaar IP-adres     |**Nieuwe maken**. Het openbare IP-adres moet van het type Standaard-SKU zijn.|

5. Selecteer **Controleren + maken**.
6. Bekijk het overzicht en selecteer vervolgens **maken** te maken van de firewall.

   Het duurt enkele minuten voordat deze is geïmplementeerd.
7. Nadat de implementatie is voltooid, gaat u naar de **Test-FW-RG** resource-groep, en selecteer de **Test FW01** firewall.
8. Noteer het privé-IP-adres. U zult het later gebruiken wanneer u de standaardroute maakt.

## <a name="create-a-default-route"></a>Een standaardroute maken

Voor het subnet **Workload-SN** configureert u de standaardroute voor uitgaand verkeer om via de firewall te gaan.

1. Selecteer in de Azure portal startpagina **alle services**.
2. Onder **netwerken**, selecteer **routetabellen**.
3. Selecteer **Toevoegen**.
4. Bij **Naam** typt u **Firewall-route**.
5. Bij **Abonnement** selecteert u uw abonnement.
6. Voor **resourcegroep**, selecteer **Test-FW-RG**.
7. Bij **Locatie** selecteert u dezelfde locatie die u eerder hebt gebruikt.
8. Selecteer **Maken**.
9. Selecteer **vernieuwen**, en selecteer vervolgens de **Firewall-route** routetabel.
10. Selecteer **subnetten** en selecteer vervolgens **koppelen**.
11. Selecteer **virtueel netwerk** > **Test-FW-VN**.
12. Voor **Subnet**, selecteer **Workload-SN**. Zorg ervoor dat u alleen selecteert de **Workload-SN** subnet voor deze route, anders uw firewall werkt niet correct.

13. Selecteer **OK**.
14. Selecteer **Routes** en selecteer vervolgens **toevoegen**.
15. Voor **routenaam**, type **fw-distributiegroep**.
16. Bij **Adresvoorvoegsel** typt u **0.0.0.0/0**.
17. Bij **Volgend hoptype** selecteert u **Virtueel apparaat**.

    Azure Firewall is eigenlijk een beheerde service, maar Virtueel apparaat werkt in deze situatie.
18. Bij **Adres van de volgende hop** typt u het privé-IP-adres voor de firewall dat u eerder hebt genoteerd.
19. Selecteer **OK**.

## <a name="configure-an-application-rule"></a>Een toepassingsregel configureren

Dit is de toepassing-regel waarmee uitgaande toegang tot www.google.com.

1. Open de **Test-FW-RG**, en selecteer de **Test FW01** firewall.
2. Op de **Test FW01** pagina onder **instellingen**, selecteer **regels**.
3. Selecteer de **toepassing regelverzameling** tabblad.
4. Selecteer **regelverzameling toepassing toevoegen**.
5. Bij **Naam** typt u **App-Coll01**.
6. Bij **Prioriteit** typt u **200**.
7. Bij **Actie** selecteert u **Toestaan**.
8. Onder **regels**, **doel FQDN's**, voor **naam**, type **toestaan door Google**.
9. Bij **Bronadressen** typt u **10.0.2.0/24**.
10. Bij **Protocol:poort** typt u **http, https**.
11. Voor **doel FQDN's**, type **www.google.com**
12. Selecteer **Toevoegen**.

Azure Firewall bevat een ingebouwde regelverzameling voor infrastructuur-FQDN’s die standaard zijn toegestaan. Deze FQDN’s zijn specifiek voor het platform en kunnen niet voor andere doeleinden worden gebruikt. Zie [FQDN's voor infrastructuur](infrastructure-fqdns.md) voor meer informatie.

## <a name="configure-a-network-rule"></a>Een netwerkregel configureren

Dit is de netwerkregel waarmee uitgaande toegang tot twee IP-adressen op poort 53 (DNS) wordt toegestaan.

1. Selecteer de **netwerk regelverzameling** tabblad.
2. Selecteer **regelverzameling netwerk toevoegen**.
3. Bij **Naam** typt u **Net-Coll01**.
4. Bij **Prioriteit** typt u **200**.
5. Bij **Actie** selecteert u **Toestaan**.

6. Onder **regels**, voor **naam**, type **toestaan-DNS-**.
7. Bij **Protocol** selecteert u **UDP**.
8. Bij **Bronadressen** typt u **10.0.2.0/24**.
9. Bij doeladres typt u **209.244.0.3,209.244.0.4**.
10. Bij **Doelpoorten** typt u **53**.
11. Selecteer **Toevoegen**.

### <a name="change-the-primary-and-secondary-dns-address-for-the-srv-work-network-interface"></a>Het primaire en secundaire DNS-adres voor de netwerkinterface **Srv-Work** wijzigen

Configureren van de server primaire en secundaire DNS-adressen voor testdoeleinden in deze zelfstudie. Dit is een algemene vereiste voor de Firewall van Azure niet.

1. Open de resourcegroep **Test-FW-RG** in de Azure-portal.
2. Selecteer de netwerkinterface voor de **Srv-werk** virtuele machine.
3. Onder **instellingen**, selecteer **DNS-servers**.
4. Onder **DNS-servers**, selecteer **aangepaste**.
5. Typ **209.244.0.3** in het tekstvak **DNS-server toevoegen** en **209.244.0.4** in het volgende tekstvak.
6. Selecteer **Opslaan**.
7. Start de virtuele machine **Srv-Work** opnieuw.

## <a name="test-the-firewall"></a>De firewall testen

Test nu de firewall om te controleren of deze werkt zoals verwacht.

1. Controleer in de Azure-portal de netwerkinstellingen voor de virtuele machine **Srv-Work** en noteer het privé-IP-adres.
2. Verbinding maken met een extern bureaublad **Srv-springen** virtuele machine en meld u aan. Open vanuit een extern bureaublad verbinding met de **Srv-werk** privé IP-adres.

3. Open Internet Explorer en blader naar https://www.google.com.
4. Selecteer **OK** > **sluiten** op de beveiligingswaarschuwingen van Internet Explorer.

   U ziet de startpagina van Google.

5. Blader naar https://www.microsoft.com.

   U zou nu door de firewall moeten worden geblokkeerd.

U hebt nu gecontroleerd of de firewall-regels zijn werken:

* Kunt u bladeren naar de enige toegestane FQDN, maar niet naar andere.
* Kunt u DNS-namen omzetten met behulp van de geconfigureerde externe DNS-server.

## <a name="clean-up-resources"></a>Resources opschonen

U kunt de firewall-resources voor de volgende zelfstudie bewaren. Als u ze niet meer nodig hebt, verwijdert u de resourcegroep **Test-FW-RG** om alle firewall-gerelateerde resources te verwijderen.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Zelfstudie: Azure Firewall-logboeken bewaken](./tutorial-diagnostics.md)
