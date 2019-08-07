---
title: Azure AD Federation-meta gegevens | Microsoft Docs
description: In dit artikel wordt het document met federatieve meta gegevens beschreven dat Azure Active Directory publiceert voor services die Azure Active Directory-tokens accepteren.
services: active-directory
documentationcenter: .net
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: c2d5f80b-aa74-452c-955b-d8eb3ed62652
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/07/2017
ms.author: ryanwi
ms.reviewer: hirsin, dastrock
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: fefaf618ff29cc2186dc555eb6f452223f4cd097
ms.sourcegitcommit: bc3a153d79b7e398581d3bcfadbb7403551aa536
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68835128"
---
# <a name="federation-metadata"></a>Federatieve metagegevens
Azure Active Directory (Azure AD) publiceert een document met federatieve meta gegevens voor services die zijn geconfigureerd om de beveiligings tokens te accepteren die door Azure AD worden uitgegeven. De indeling van het federatieve meta gegevens document wordt beschreven in de [webservices-Federatie Language (WS-Federation) versie 1,2](https://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html), waarmee [de meta gegevens voor de Oasis Security Assertion Markup Language (SAML) v 2.0](https://docs.oasis-open.org/security/saml/v2.0/saml-metadata-2.0-os.pdf)worden uitgebreid.

## <a name="tenant-specific-and-tenant-independent-metadata-endpoints"></a>Tenant-en Tenant-onafhankelijke eind punten van meta gegevens
Azure AD publiceert Tenant-specifieke en Tenant-onafhankelijke eind punten.

Tenant-specifieke eind punten zijn ontworpen voor een bepaalde Tenant. De Tenant-specifieke federatieve meta gegevens bevatten informatie over de Tenant, met inbegrip van Tenant-specifieke uitgevers en eindpunt informatie. Toepassingen die de toegang tot één Tenant beperken, gebruiken Tenant-specifieke eind punten.

Tenant-onafhankelijke eind punten bieden informatie die gemeen schappelijk is voor alle Azure AD-tenants. Deze informatie is van toepassing op tenants die worden gehost op *login.microsoftonline.com* en worden gedeeld via tenants. Tenant-onafhankelijke eind punten worden aanbevolen voor toepassingen met meerdere tenants, omdat ze niet zijn gekoppeld aan een bepaalde Tenant.

## <a name="federation-metadata-endpoints"></a>Federatieve meta gegevens-eind punten
Azure AD publiceert federatieve meta gegevens `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`op.

Voor **Tenant-specifieke eind punten**kan dit `TenantDomainName` een van de volgende typen zijn:

* Een geregistreerde domein naam van een Azure AD-Tenant, zoals: `contoso.onmicrosoft.com`.
* De onveranderbare Tenant-ID van het domein, `72f988bf-86f1-41af-91ab-2d7cd011db45`zoals.

De`TenantDomainName` is`common`voor **Tenant-onafhankelijke eind punten**. In dit document worden alleen de federatieve meta gegevenselementen vermeld die gemeen schappelijk zijn voor alle Azure AD-tenants die worden gehost op login.microsoftonline.com.

Een Tenant-specifiek eind punt kan bijvoorbeeld zijn `https://login.microsoftonline.com/contoso.onmicrosoft.com/FederationMetadata/2007-06/FederationMetadata.xml`. Het Tenant-onafhankelijke eind punt [https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml](https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml)is. U kunt het document met federatieve meta gegevens weer geven door deze URL in een browser te typen.

## <a name="contents-of-federation-metadata"></a>Inhoud van federatieve meta gegevens
De volgende sectie bevat informatie die nodig is voor services die gebruikmaken van de tokens die zijn uitgegeven door Azure AD.

### <a name="entity-id"></a>Entiteits-id
Het `EntityDescriptor` element bevat een `EntityID` kenmerk. De waarde van het `EntityID` kenmerk vertegenwoordigt de uitgever, dat wil zeggen, de Security Token Service (STS) die het token heeft uitgegeven. Het is belang rijk dat u de uitgever valideert wanneer u een token ontvangt.

In de volgende meta gegevens ziet u een voor `EntityDescriptor` beeld van een `EntityID` Tenant-specifiek element met een-element.

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="_b827a749-cfcb-46b3-ab8b-9f6d14a1294b"
entityID="https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db45/">
```
U kunt de Tenant-id in het Tenant-onafhankelijke eind punt vervangen door de Tenant-id om een Tenant `EntityID` -specifieke waarde te maken. De resulterende waarde is hetzelfde als de uitgever van het token. Met de strategie kan een multi tenant-toepassing de verlener voor een bepaalde Tenant valideren.

In de volgende meta gegevens ziet u een voor `EntityID` beeld van een Tenant-onafhankelijk element. Houd er rekening mee dat `{tenant}` het een letterlijke waarde is, geen tijdelijke aanduiding.

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="="_0e5bd9d0-49ef-4258-bc15-21ce143b61bd"
entityID="https://sts.windows.net/{tenant}/">
```

### <a name="token-signing-certificates"></a>Token handtekening certificaten
Wanneer een service een token ontvangt dat is uitgegeven door een Azure AD-Tenant, moet de hand tekening van het token worden gevalideerd met een ondertekeningssleutel die is gepubliceerd in het document met federatieve meta gegevens. De federatieve meta gegevens bevatten het open bare deel van de certificaten die door de tenants worden gebruikt voor het ondertekenen van tokens. De onbewerkte certificaat bytes worden `KeyDescriptor` weer gegeven in het-element. Het token handtekening certificaat is alleen geldig voor ondertekening wanneer de waarde van het `use` kenmerk is `signing`.

Een document met federatieve meta gegevens dat door Azure AD wordt gepubliceerd, kan meerdere ondertekeningssleutel hebben, bijvoorbeeld wanneer Azure AD wordt voor bereid om het handtekening certificaat bij te werken. Wanneer een document met federatieve meta gegevens meer dan één certificaat bevat, moet een service die de tokens valideert alle certificaten in het document ondersteunen.

In de volgende meta gegevens ziet `KeyDescriptor` u een voorbeeld element met een handtekening sleutel.

```
<KeyDescriptor use="signing">
<KeyInfo xmlns="https://www.w3.org/2000/09/xmldsig#">
<X509Data>
<X509Certificate>
MIIDPjCCAiqgAwIBAgIQVWmXY/+9RqFA/OG9kFulHDAJBgUrDgMCHQUAMC0xKzApBgNVBAMTImFjY291bnRzLmFjY2Vzc2NvbnRyb2wud2luZG93cy5uZXQwHhcNMTIwNjA3MDcwMDAwWhcNMTQwNjA3MDcwMDAwWjAtMSswKQYDVQQDEyJhY2NvdW50cy5hY2Nlc3Njb250cm9sLndpbmRvd3MubmV0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEArCz8Sn3GGXmikH2MdTeGY1D711EORX/lVXpr+ecGgqfUWF8MPB07XkYuJ54DAuYT318+2XrzMjOtqkT94VkXmxv6dFGhG8YZ8vNMPd4tdj9c0lpvWQdqXtL1TlFRpD/P6UMEigfN0c9oWDg9U7Ilymgei0UXtf1gtcQbc5sSQU0S4vr9YJp2gLFIGK11Iqg4XSGdcI0QWLLkkC6cBukhVnd6BCYbLjTYy3fNs4DzNdemJlxGl8sLexFytBF6YApvSdus3nFXaMCtBGx16HzkK9ne3lobAwL2o79bP4imEGqg+ibvyNmbrwFGnQrBc1jTF9LyQX9q+louxVfHs6ZiVwIDAQABo2IwYDBeBgNVHQEEVzBVgBCxDDsLd8xkfOLKm4Q/SzjtoS8wLTErMCkGA1UEAxMiYWNjb3VudHMuYWNjZXNzY29udHJvbC53aW5kb3dzLm5ldIIQVWmXY/+9RqFA/OG9kFulHDAJBgUrDgMCHQUAA4IBAQAkJtxxm/ErgySlNk69+1odTMP8Oy6L0H17z7XGG3w4TqvTUSWaxD4hSFJ0e7mHLQLQD7oV/erACXwSZn2pMoZ89MBDjOMQA+e6QzGB7jmSzPTNmQgMLA8fWCfqPrz6zgH+1F1gNp8hJY57kfeVPBiyjuBmlTEBsBlzolY9dd/55qqfQk6cgSeCbHCy/RU/iep0+UsRMlSgPNNmqhj5gmN2AFVCN96zF694LwuPae5CeR2ZcVknexOWHYjFM0MgUSw0ubnGl0h9AJgGyhvNGcjQqu9vd1xkupFgaN+f7P3p3EVN5csBg5H94jEcQZT7EKeTiZ6bTrpDAnrr8tDCy8ng
</X509Certificate>
</X509Data>
</KeyInfo>
</KeyDescriptor>
  ```

Het `KeyDescriptor` element wordt weer gegeven op twee plaatsen in het document met federatieve meta gegevens, in de sectie WS-Federation-specific en de sectie SAML-specific. De certificaten die in beide secties worden gepubliceerd, zijn hetzelfde.

In de sectie WS-Federation-specific worden de certificaten van een `RoleDescriptor` -element met het `SecurityTokenServiceType` type door een WS-Federation-meta gegevens lezer gelezen.

In de volgende meta gegevens wordt `RoleDescriptor` een voorbeeld element weer gegeven.

```
<RoleDescriptor xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns:fed="https://docs.oasis-open.org/wsfed/federation/200706" xsi:type="fed:SecurityTokenServiceType"protocolSupportEnumeration="https://docs.oasis-open.org/wsfed/federation/200706">
```

In de sectie SAML-specifiek worden de certificaten van een `IDPSSODescriptor` -element gelezen met de WS-Federation-meta gegevens lezer.

In de volgende meta gegevens wordt `IDPSSODescriptor` een voorbeeld element weer gegeven.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
```
Er zijn geen verschillen in de indeling van Tenant-specifieke en Tenant-onafhankelijke certificaten.

### <a name="ws-federation-endpoint-url"></a>URL van WS-Federation-eind punt
De federatieve meta gegevens bevatten de URL die door Azure AD wordt gebruikt voor eenmalige aanmelding en eenmalige afmelding in het WS-Federation-Protocol. Dit eind punt wordt weer `PassiveRequestorEndpoint` gegeven in het-element.

In de volgende meta gegevens ziet `PassiveRequestorEndpoint` u een voorbeeld element voor een Tenant-specifiek eind punt.

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="https://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/72f988bf-86f1-41af-91ab-2d7cd011db45/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```
Voor het Tenant-onafhankelijke eind punt wordt de WS-Federation-URL weer gegeven in het WS-Federation-eind punt, zoals wordt weer gegeven in het volgende voor beeld.

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="https://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/common/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```

### <a name="saml-protocol-endpoint-url"></a>URL voor SAML-protocol eindpunt
De federatieve meta gegevens bevatten de URL die door Azure AD wordt gebruikt voor eenmalige aanmelding en eenmalige afmelding in het SAML 2,0-protocol. Deze eind punten worden weer gegeven `IDPSSODescriptor` in het-element.

De url's voor aanmelden en afmelden worden weer gegeven in de `SingleSignOnService` elementen `SingleLogoutService` en.

In de volgende meta gegevens ziet `PassiveResistorEndpoint` u een voor beeld van een Tenant-specifiek eind punt.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com /saml2" />
  </IDPSSODescriptor>
```

Net als de eind punten voor de gemeen schappelijke SAML 2,0-protocol eindpunten worden gepubliceerd in de Tenant-onafhankelijke federatieve meta gegevens, zoals wordt weer gegeven in het volgende voor beeld.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
  </IDPSSODescriptor>
```
