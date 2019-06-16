---
title: OpenVPN Clients configureren voor Azure VPN-Gateway | Microsoft Docs
description: Stappen voor het OpenVPN Clients configureren voor Azure VPN-Gateway
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 5/21/2019
ms.author: cherylmc
ms.openlocfilehash: fdfabf328ddfa6b5e4b578be5a1b329cb3219a18
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65989083"
---
# <a name="configure-openvpn-clients-for-azure-vpn-gateway"></a>OpenVPN clients voor Azure VPN-Gateway configureren

Dit artikel helpt u bij het configureren **OpenVPN® Protocol** clients.

## <a name="before-you-begin"></a>Voordat u begint

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Controleer of dat u de stappen voor het configureren van OpenVPN voor uw VPN-gateway hebt voltooid. Zie voor meer informatie, [OpenVPN configureren voor Azure VPN-Gateway](vpn-gateway-howto-openvpn.md).

## <a name="windows"></a>Windows-clients

1. Download en installeer de client OpenVPN via de officiële [OpenVPN website](https://openvpn.net/index.php/open-source/downloads.html).
2. Download het VPN-profiel voor de gateway. Dit kan worden uitgevoerd vanaf het tabblad voor punt-naar-site-configuratie in Azure portal of 'New-AzVpnClientConfiguration' in PowerShell.
3. Pak het profiel uit. Open vervolgens de *vpnconfig.ovpn* configuratiebestand op van de OpenVPN-map met Kladblok.
4. [Exporteren](vpn-gateway-certificates-point-to-site.md#clientexport) de P2S-clients u hebt gemaakt en geüpload naar uw P2S-configuratie op de gateway van het certificaat.
5. De persoonlijke sleutel en de vingerafdruk van het base64 uit te halen en de *pfx*. Er zijn meerdere manieren om dit te doen. Met behulp van OpenSSL op uw computer is één manier. De *profileinfo.txt* -bestand bevat de persoonlijke sleutel en de vingerafdruk voor de CA en het clientcertificaat. Zorg ervoor dat de vingerafdruk van het clientcertificaat gebruiken.

   ```
   openssl pkcs12 -in "filename.pfx" -nodes -out "profileinfo.txt"
   ```
6. Open *profileinfo.txt* in Kladblok. Als u de vingerafdruk van het clientcertificaat (onderliggend), selecteert u de tekst (met inbegrip van en tussen) "---BEGIN CERTIFICATE---" en "---END CERTIFICATE---' voor de onderliggende van het certificaat en deze te kopiëren. U kunt het onderliggende certificaat identificeren door te kijken de = onderwerp / lijn.
7. Schakel over naar de *vpnconfig.ovpn* -bestand dat u in stap 3 in Kladblok hebt geopend. De onderstaande sectie zoeken en vervangen alles tussen 'cert' en ' / cert '.

   ```
   # P2S client certificate
   # please fill this field with a PEM formatted cert
   <cert>
   $CLIENTCERTIFICATE
   </cert>
   ```
8. Open de *profileinfo.txt* in Kladblok. Als u de persoonlijke sleutel, selecteert u de tekst (met inbegrip van en tussen) "---BEGIN PRIVATE KEY---" en '---END PRIVATE KEY---' en deze te kopiëren.
9. Ga terug naar het bestand vpnconfig.ovpn in Kladblok en zoeken in deze sectie. Plak de persoonlijke sleutel vervangen alles tussen en "sleutel" en '/ sleutel'.

   ```
   # P2S client root certificate private key
   # please fill this field with a PEM formatted key
   <key>
   $PRIVATEKEY
   </key>
   ```
10. Wijzig geen andere velden. Gebruik de ingevulde configuratie in de clientinvoer om verbinding te maken met de VPN.
11. Kopieer het bestand vpnconfig.ovpn naar C:\Program Files\OpenVPN\config folder.
12. Klik met de rechtermuisknop op het pictogram van OpenVPN in het systeemvak en klik op Verbinding maken.

## <a name="mac"></a>Mac-clients

1. Download en installeer een OpenVPN-client, zoals [TunnelBlik](https://tunnelblick.net/downloads.html). 
2. Download het VPN-profiel voor de gateway. Dit kan worden uitgevoerd vanaf het tabblad punt-naar-site-configuratie in Azure portal of met behulp van 'New-AzVpnClientConfiguration' in PowerShell.
3. Pak het profiel uit. Open het configuratiebestand vpnconfig.ovpn vanuit de map OpenVPN in Kladblok.
4. Vul het gedeelte P2S client certificate met de openbare P2S-clientcertificatcode in base64. In een certificaat met PEM-indeling kunt u gewoon het .cer-bestand openen en de base64-code tussen de headers van het certificaat kopiëren. Zie [Exporteer de openbare sleutel](vpn-gateway-certificates-point-to-site.md#cer) voor informatie over het exporteren van een certificaat om de gecodeerde openbare sleutel.
5. Vul in het gedeelte voor de persoonlijke sleutel de persoonlijke P2S-clientcertificaatsleutel in Base64 in. Zie [uw persoonlijke sleutel exporteren](https://openvpn.net/community-resources/how-to/#pki) voor informatie over het ophalen van een persoonlijke sleutel.
6. Wijzig geen andere velden. Gebruik de ingevulde configuratie in de clientinvoer om verbinding te maken met de VPN.
7. Dubbelklik op het bestand van het profiel voor het maken van het profiel in tunnelblik.
8. Start Tunnelblik uit de toepassingsmap.
9. Klik op het pictogram Tunnelblik in het systeemvak en kies verbinding maken.

> [!IMPORTANT]
>Alleen iOS 11.0 en hoger en MacOS 10.13 en hoger worden ondersteund met OpenVPN-protocol.
>

## <a name="linux"></a>Linux-clients

1. Open een nieuwe sessie. U kunt een nieuwe sessie openen door te drukken 'Ctrl + Alt + t' op hetzelfde moment.
2. Voer de volgende opdracht om vereiste onderdelen te installeren:

   ```
   sudo apt-get install openvpn
   sudo apt-get -y install network-manager-openvpn
   sudo service network-manager restart
   ```
3. Download het VPN-profiel voor de gateway. Dit kan worden uitgevoerd vanaf het tabblad voor punt-naar-site-configuratie in Azure portal.
4. [Exporteren](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-certificates-point-to-site#clientexport) de P2S-clients u hebt gemaakt en geüpload naar uw P2S-configuratie op de gateway van het certificaat. 
5. De persoonlijke sleutel en de vingerafdruk van het base64 extraheren uit het .pfx-bestand. Er zijn meerdere manieren om dit te doen. Met behulp van OpenSSL op uw computer is één manier.

    ```
    openssl.exe pkcs12 -in "filename.pfx" -nodes -out "profileinfo.txt"
    ```
   De *profileinfo.txt* bestand bevat de persoonlijke sleutel en de vingerafdruk voor de CA en het clientcertificaat. Zorg ervoor dat de vingerafdruk van het clientcertificaat gebruiken.

6. Open *profileinfo.txt* in een teksteditor. Als u de vingerafdruk van het clientcertificaat (onderliggend), selecteert u de tekst zoals en tussen "---BEGIN CERTIFICATE---" en "---END CERTIFICATE---' voor de onderliggende van het certificaat en deze te kopiëren. U kunt het onderliggende certificaat identificeren door te kijken de = onderwerp / lijn.

7. Open de *vpnconfig.ovpn* -bestand en zoek de sectie hieronder wordt weergegeven. Vervang alles tussen de en 'cert' en ' / cert '.

   ```
   # P2S client certificate
   # please fill this field with a PEM formatted cert
   <cert>
   $CLIENTCERTIFICATE
   </cert>
   ```
8. Open de profileinfo.txt in een teksteditor. Als u de persoonlijke sleutel, selecteert u de tekst zoals en tussen "---BEGIN PRIVÉSLEUTEL---" en '---END PRIVATE KEY---' en deze te kopiëren.

9. Open het bestand vpnconfig.ovpn in een teksteditor en zoek in deze sectie. Plak de persoonlijke sleutel vervangen alles tussen en "sleutel" en '/ sleutel'.

   ```
   # P2S client root certificate private key
   # please fill this field with a PEM formatted key
   <key>
   $PRIVATEKEY
   </key>
   ```

10. Wijzig geen andere velden. Gebruik de ingevulde configuratie in de clientinvoer om verbinding te maken met de VPN.
11. Als u wilt verbinding maken met behulp van de opdrachtregel, typt u de volgende opdracht uit:
  
    ```
    sudo openvpn –-config <name and path of your VPN profile file>
    ```
12. Als u wilt verbinding maken met de gebruikersinterface, gaat u naar de systeeminstellingen.
13. Klik op **+** om toe te voegen een nieuwe VPN-verbinding.
14. Onder **toevoegen VPN**, kies **importeren uit bestand...**
15. Blader naar de profielbestand en dubbelklik op of kies **Open**.
16. Klik op **toevoegen** op de **toevoegen VPN** venster.
  
    ![Importeren vanuit bestand](./media/vpn-gateway-howto-openvpn-clients/importfromfile.png)
17. U kunt verbinding maken door het inschakelen van de VPN-verbinding **ON** op de **netwerkinstellingen** pagina of onder het netwerkpictogram in het systeemvak.

## <a name="next-steps"></a>Volgende stappen

Als u de VPN-clients kunnen toegang krijgen tot bronnen in een ander VNet wilt, klikt u vervolgens de instructies op de [VNet-naar-VNet](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) artikel voor het instellen van een vnet-naar-vnet-verbinding. Zorg ervoor dat u BGP wilt inschakelen op de gateways en verbindingen, anders niet het verkeer.

**"OpenVPN" is een handelsmerk van OpenVPN Inc.**
