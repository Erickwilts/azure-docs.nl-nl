---
title: Wederzijdse TLS-verificatie configureren
description: Meer informatie over het verifiëren van client certificaten op TLS. Azure App Service kunt het client certificaat beschikbaar maken voor de app-code voor verificatie.
ms.assetid: cd1d15d3-2d9e-4502-9f11-a306dac4453a
ms.topic: article
ms.date: 10/01/2019
ms.custom: seodec18
ms.openlocfilehash: aa4d0b5089c07cbbeb3d40224518b9eb9a927585
ms.sourcegitcommit: 265f1d6f3f4703daa8d0fc8a85cbd8acf0a17d30
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/02/2019
ms.locfileid: "74672128"
---
# <a name="configure-tls-mutual-authentication-for-azure-app-service"></a>Wederzijdse TLS-verificatie voor Azure App Service configureren

U kunt de toegang tot uw Azure App Service-app beperken door verschillende verificatie typen in te scha kelen. Een manier om dit te doen is door een client certificaat aan te vragen wanneer de client aanvraag over TLS/SSL gaat en het certificaat te valideren. Dit mechanisme heet TLS wederzijdse verificatie of verificatie van client certificaten. In dit artikel wordt beschreven hoe u uw app instelt voor gebruik van verificatie van client certificaten.

> [!NOTE]
> Als u toegang tot uw site hebt via HTTP en niet HTTPS, ontvangt u geen client certificaat. Als voor uw toepassing client certificaten zijn vereist, moet u dus geen aanvragen voor uw toepassing via HTTP toestaan.
>

## <a name="enable-client-certificates"></a>Client certificaten inschakelen

Als u uw app wilt instellen op client certificaten vereist, moet u de `clientCertEnabled` instelling voor uw app instellen op `true`. Als u de instelling wilt instellen, voert u de volgende opdracht uit in de [Cloud shell](https://shell.azure.com).

```azurecli-interactive
az webapp update --set clientCertEnabled=true --name <app_name> --resource-group <group_name>
```

## <a name="exclude-paths-from-requiring-authentication"></a>Paden uitsluiten voor het vereisen van verificatie

Wanneer u wederzijdse verificatie inschakelt voor uw toepassing, is voor alle paden in de hoofdmap van uw app een client certificaat vereist voor toegang. Als u wilt toestaan dat bepaalde paden open blijven voor anonieme toegang, kunt u uitsluitings paden definiëren als onderdeel van de configuratie van uw toepassing.

Uitsluitings paden kunnen worden geconfigureerd door **configuratie** > **algemene instellingen** te selecteren en een uitgesloten pad te definiëren. In dit voor beeld wordt aan alles onder `/public` pad voor uw toepassing geen client certificaat gevraagd.

![Paden voor certificaat uitsluiting][exclusion-paths]


## <a name="access-client-certificate"></a>Client certificaat openen

In App Service wordt SSL-beëindiging van de aanvraag uitgevoerd op het front-end-load balancer. Bij het door sturen van de aanvraag naar uw app-code met [client certificaten ingeschakeld](#enable-client-certificates), App Service een `X-ARR-ClientCert` aanvraag header met het client certificaat injecteren. App Service doet niets met dit client certificaat, behalve het door sturen naar uw app. Uw app-code is verantwoordelijk voor het valideren van het client certificaat.

Voor ASP.NET is het client certificaat beschikbaar via de eigenschap **HttpRequest. ClientCertificate** .

Voor andere toepassings Stacks (node. js, PHP, enzovoort) is het client certificaat beschikbaar in uw app via een base64-gecodeerde waarde in de `X-ARR-ClientCert` aanvraag header.

## <a name="aspnet-sample"></a>ASP.NET-voor beeld

```csharp
    using System;
    using System.Collections.Specialized;
    using System.Security.Cryptography.X509Certificates;
    using System.Web;

    namespace ClientCertificateUsageSample
    {
        public partial class Cert : System.Web.UI.Page
        {
            public string certHeader = "";
            public string errorString = "";
            private X509Certificate2 certificate = null;
            public string certThumbprint = "";
            public string certSubject = "";
            public string certIssuer = "";
            public string certSignatureAlg = "";
            public string certIssueDate = "";
            public string certExpiryDate = "";
            public bool isValidCert = false;

            //
            // Read the certificate from the header into an X509Certificate2 object
            // Display properties of the certificate on the page
            //
            protected void Page_Load(object sender, EventArgs e)
            {
                NameValueCollection headers = base.Request.Headers;
                certHeader = headers["X-ARR-ClientCert"];
                if (!String.IsNullOrEmpty(certHeader))
                {
                    try
                    {
                        byte[] clientCertBytes = Convert.FromBase64String(certHeader);
                        certificate = new X509Certificate2(clientCertBytes);
                        certSubject = certificate.Subject;
                        certIssuer = certificate.Issuer;
                        certThumbprint = certificate.Thumbprint;
                        certSignatureAlg = certificate.SignatureAlgorithm.FriendlyName;
                        certIssueDate = certificate.NotBefore.ToShortDateString() + " " + certificate.NotBefore.ToShortTimeString();
                        certExpiryDate = certificate.NotAfter.ToShortDateString() + " " + certificate.NotAfter.ToShortTimeString();
                    }
                    catch (Exception ex)
                    {
                        errorString = ex.ToString();
                    }
                    finally 
                    {
                        isValidCert = IsValidClientCertificate();
                        if (!isValidCert) Response.StatusCode = 403;
                        else Response.StatusCode = 200;
                    }
                }
                else
                {
                    certHeader = "";
                }
            }

            //
            // This is a SAMPLE verification routine. Depending on your application logic and security requirements, 
            // you should modify this method
            //
            private bool IsValidClientCertificate()
            {
                // In this example we will only accept the certificate as a valid certificate if all the conditions below are met:
                // 1. The certificate is not expired and is active for the current time on server.
                // 2. The subject name of the certificate has the common name nildevecc
                // 3. The issuer name of the certificate has the common name nildevecc and organization name Microsoft Corp
                // 4. The thumbprint of the certificate is 30757A2E831977D8BD9C8496E4C99AB26CB9622B
                //
                // This example does NOT test that this certificate is chained to a Trusted Root Authority (or revoked) on the server 
                // and it allows for self signed certificates
                //

                if (certificate == null || !String.IsNullOrEmpty(errorString)) return false;

                // 1. Check time validity of certificate
                if (DateTime.Compare(DateTime.Now, certificate.NotBefore) < 0 || DateTime.Compare(DateTime.Now, certificate.NotAfter) > 0) return false;

                // 2. Check subject name of certificate
                bool foundSubject = false;
                string[] certSubjectData = certificate.Subject.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
                foreach (string s in certSubjectData)
                {
                    if (String.Compare(s.Trim(), "CN=nildevecc") == 0)
                    {
                        foundSubject = true;
                        break;
                    }
                }
                if (!foundSubject) return false;

                // 3. Check issuer name of certificate
                bool foundIssuerCN = false, foundIssuerO = false;
                string[] certIssuerData = certificate.Issuer.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
                foreach (string s in certIssuerData)
                {
                    if (String.Compare(s.Trim(), "CN=nildevecc") == 0)
                    {
                        foundIssuerCN = true;
                        if (foundIssuerO) break;
                    }

                    if (String.Compare(s.Trim(), "O=Microsoft Corp") == 0)
                    {
                        foundIssuerO = true;
                        if (foundIssuerCN) break;
                    }
                }

                if (!foundIssuerCN || !foundIssuerO) return false;

                // 4. Check thumprint of certificate
                if (String.Compare(certificate.Thumbprint.Trim().ToUpper(), "30757A2E831977D8BD9C8496E4C99AB26CB9622B") != 0) return false;

                return true;
            }
        }
    }
```

## <a name="nodejs-sample"></a>Voor beeld van node. js

Met de volgende voorbeeld code van node. js wordt de `X-ARR-ClientCert`-header opgehaald en wordt [knooppunt vervalsing](https://github.com/digitalbazaar/forge) gebruikt om de met base64 gecodeerde PEM-teken reeks te converteren naar een certificaat object en dit te valideren:

```javascript
import { NextFunction, Request, Response } from 'express';
import { pki, md, asn1 } from 'node-forge';

export class AuthorizationHandler {
    public static authorizeClientCertificate(req: Request, res: Response, next: NextFunction): void {
        try {
            // Get header
            const header = req.get('X-ARR-ClientCert');
            if (!header) throw new Error('UNAUTHORIZED');

            // Convert from PEM to pki.CERT
            const pem = `-----BEGIN CERTIFICATE-----${header}-----END CERTIFICATE-----`;
            const incomingCert: pki.Certificate = pki.certificateFromPem(pem);

            // Validate certificate thumbprint
            const fingerPrint = md.sha1.create().update(asn1.toDer(pki.certificateToAsn1(incomingCert)).getBytes()).digest().toHex();
            if (fingerPrint.toLowerCase() !== 'abcdef1234567890abcdef1234567890abcdef12') throw new Error('UNAUTHORIZED');

            // Validate time validity
            const currentDate = new Date();
            if (currentDate < incomingCert.validity.notBefore || currentDate > incomingCert.validity.notAfter) throw new Error('UNAUTHORIZED');

            // Validate issuer
            if (incomingCert.issuer.hash.toLowerCase() !== 'abcdef1234567890abcdef1234567890abcdef12') throw new Error('UNAUTHORIZED');

            // Validate subject
            if (incomingCert.subject.hash.toLowerCase() !== 'abcdef1234567890abcdef1234567890abcdef12') throw new Error('UNAUTHORIZED');

            next();
        } catch (e) {
            if (e instanceof Error && e.message === 'UNAUTHORIZED') {
                res.status(401).send();
            } else {
                next(e);
            }
        }
    }
}
```

[exclusion-paths]: ./media/app-service-web-configure-tls-mutual-auth/exclusion-paths.png