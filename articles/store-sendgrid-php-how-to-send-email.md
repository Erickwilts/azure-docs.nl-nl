---
title: Het gebruik van de e-mailservice van SendGrid (PHP) | Microsoft Docs
description: Informatie over hoe e-mailbericht met de e-mailservice van SendGrid verzenden op Azure. Voorbeelden van code geschreven in PHP.
documentationcenter: php
services: ''
manager: sendgrid
editor: mollybos
author: thinkingserious
ms.assetid: 51a9928a-4c9e-4b0a-aca8-9a408aeb6f47
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 10/30/2014
ms.author: elmer.thomas@sendgrid.com; erika.berkland@sendgrid.com; vibhork; matt.bernier@sendgrid.com
ms.openlocfilehash: db3333aa52782ceb949ef3f46a903b618f6e3f2f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60931225"
---
# <a name="how-to-use-the-sendgrid-email-service-from-php"></a>Het gebruik van de SendGrid-e-mailservice via PHP

Deze handleiding laat zien hoe u veelvoorkomende programming taken met de e-mailservice van SendGrid uitvoeren op Azure. De voorbeelden zijn geschreven in PHP.
De behandelde scenario's zijn **maken e**, **verzenden van e-mail**, en **bijlagen toevoegen**. Zie voor meer informatie over SendGrid en het verzenden van e-mail, het [Vervolgstappen](#next-steps) sectie.

## <a name="what-is-the-sendgrid-email-service"></a>Wat is het e-mailservice van SendGrid?
SendGrid is een [cloud-gebaseerde e-mailservice] die zorgt voor betrouwbare [transactionele e-mail delivery], schaalbaarheid en realtime analyses en flexibele API's waarmee u aangepaste integratie eenvoudig. Veelvoorkomende SendGrid gebruiksscenario's zijn onder andere:

* Automatisch verzenden van meldingen voor klanten
* Beheer van distributie bevat voor het verzenden van klanten maandelijks e-flyers en speciale aanbiedingen
* Realtime metrische gegevens voor zaken zoals geblokkeerde e-mail- en reactietijd van klanten verzamelen
* Genereren van rapporten voor het identificeren van trends
* Doorsturen van vragen van klanten
* E-mailmeldingen van uw toepassing

Zie voor meer informatie, [ https://sendgrid.com ] [ https://sendgrid.com].

## <a name="create-a-sendgrid-account"></a>Een SendGrid-Account maken

[!INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="using-sendgrid-from-your-php-application"></a>Met behulp van SendGrid van uw PHP-toepassing

SendGrid gebruiken in een Azure PHP-toepassing vereist geen speciale configuratie of coderen. Omdat SendGrid een service is, kunnen deze worden geopend op exact dezelfde manier uit een cloudtoepassing zoals kan vanuit een on-premises toepassing.

## <a name="how-to-send-an-email"></a>Procedure: Een E-mail verzenden

U kunt e-mailadres van de SMTP- of de Web-API die is geleverd door SendGrid verzenden.

### <a name="smtp-api"></a>SMTP-API

Gebruiken voor het verzenden van e-mailadres van de SMTP-API van SendGrid, *Swift e-mailprogramma*, een bibliotheek op basis van onderdelen voor het verzenden van e-mailberichten van PHP-toepassingen. U kunt downloaden de [Swift e-mailprogramma bibliotheek](https://swiftmailer.symfony.com/) v5.3.0 (Gebruik [Composer] Swift e-mailprogramma installeren). Verzenden van e-mailbericht met de bibliotheek omvat het maken van instanties van de `Swift\_SmtpTransport`, `Swift\_Mailer`, en `Swift\_Message` klassen, instellen van de gewenste eigenschappen en het aanroepen van de `Swift\_Mailer::send` methode.

```php
<?php
 include_once "vendor/autoload.php";
 /*
  * Create the body of the message (a plain-text and an HTML version).
  * $text is your plain-text email
  * $html is your html version of the email
  * If the receiver is able to view html emails then only the html
  * email will be displayed
  */
 $text = "Hi!\nHow are you?\n";
 $html = "<html>
       <head></head>
       <body>
           <p>Hi!<br>
               How are you?<br>
           </p>
       </body>
       </html>";
 // This is your From email address
 $from = array('someone@example.com' => 'Name To Appear');
 // Email recipients
 $to = array(
       'john@contoso.com'=>'Destination 1 Name',
       'anna@contoso.com'=>'Destination 2 Name'
 );
 // Email subject
 $subject = 'Example PHP Email';

 // Login credentials
 $username = 'yoursendgridusername';
 $password = 'yourpassword';

 // Setup Swift mailer parameters
 $transport = Swift_SmtpTransport::newInstance('smtp.sendgrid.net', 587);
 $transport->setUsername($username);
 $transport->setPassword($password);
 $swift = Swift_Mailer::newInstance($transport);

 // Create a message (subject)
 $message = new Swift_Message($subject);

 // attach the body of the email
 $message->setFrom($from);
 $message->setBody($html, 'text/html');
 $message->setTo($to);
 $message->addPart($text, 'text/plain');

 // send message
 if ($recipients = $swift->send($message, $failures))
 {
     // This will let us know how many users received this message
     echo 'Message sent out to '.$recipients.' users';
 }
 // something went wrong =(
 else
 {
     echo "Something went wrong - ";
     print_r($failures);
 }
```

### <a name="web-api"></a>Web-API
Gebruik van PHP [curl functie] [ curl function] e-mail verzenden via de Web-API van SendGrid.

```php
<?php

 $url = 'https://api.sendgrid.com/';
 $user = 'USERNAME';
 $pass = 'PASSWORD';

 $params = array(
      'api_user' => $user,
      'api_key' => $pass,
      'to' => 'john@contoso.com',
      'subject' => 'testing from curl',
      'html' => 'testing body',
      'text' => 'testing body',
      'from' => 'anna@contoso.com',
   );

 $request = $url.'api/mail.send.json';

 // Generate curl request
 $session = curl_init($request);

 // Tell curl to use HTTP POST
 curl_setopt ($session, CURLOPT_POST, true);

 // Tell curl that this is the body of the POST
 curl_setopt ($session, CURLOPT_POSTFIELDS, $params);

 // Tell curl not to return headers, but do return the response
 curl_setopt($session, CURLOPT_HEADER, false);
 curl_setopt($session, CURLOPT_RETURNTRANSFER, true);

 // obtain response
 $response = curl_exec($session);
 curl_close($session);

 // print everything out
 print_r($response);
```

Web-API van SendGrid is vergelijkbaar met een REST-API, maar het is niet echt een RESTful-API omdat, in de meeste aanroepen beide ophalen en POST-bewerkingen door elkaar kunnen worden gebruikt.

## <a name="how-to-add-an-attachment"></a>Procedure: Voeg een bijlage toe

### <a name="smtp-api"></a>SMTP-API

Een extra regel code aan het voorbeeldscript voor het verzenden van een e-mailbericht met Swift e-mailprogramma omvat het verzenden van een bijlage met behulp van de SMTP-API.

```php
<?php
 include_once "vendor/autoload.php";
 /*
  * Create the body of the message (a plain-text and an HTML version).
  * $text is your plain-text email
  * $html is your html version of the email
  * If the receiver is able to view html emails then only the html
  * email will be displayed
  */
 $text = "Hi!\nHow are you?\n";
  $html = "<html>
      <head></head>
      <body>
         <p>Hi!<br>
            How are you?<br>
         </p>
      </body>
      </html>";

 // This is your From email address
 $from = array('someone@example.com' => 'Name To Appear');

 // Email recipients
 $to = array(
      'john@contoso.com'=>'Destination 1 Name',
      'anna@contoso.com'=>'Destination 2 Name'
 );
 // Email subject
 $subject = 'Example PHP Email';

 // Login credentials
 $username = 'yoursendgridusername';
 $password = 'yourpassword';

 // Setup Swift mailer parameters
 $transport = Swift_SmtpTransport::newInstance('smtp.sendgrid.net', 587);
 $transport->setUsername($username);
 $transport->setPassword($password);
 $swift = Swift_Mailer::newInstance($transport);

 // Create a message (subject)
 $message = new Swift_Message($subject);

 // attach the body of the email
 $message->setFrom($from);
 $message->setBody($html, 'text/html');
 $message->setTo($to);
 $message->addPart($text, 'text/plain');
 $message->attach(Swift_Attachment::fromPath("path\to\file")->setFileName("file_name"));

 // send message
 if ($recipients = $swift->send($message, $failures))
 {
      // This will let us know how many users received this message
      echo 'Message sent out to '.$recipients.' users';
 }
 // something went wrong =(
 else
 {
      echo "Something went wrong - ";
      print_r($failures);
 }
```

De aanvullende coderegel is als volgt:

```php
 $message->attach(Swift_Attachment::fromPath("path\to\file")->setFileName('file_name'));
```

Deze regel code roept de methode koppelen op de `Swift\_Message` object en maakt gebruik van statische methode `fromPath` op de `Swift\_Attachment` klasse voor het ophalen en een bestand bijvoegen aan een bericht.

### <a name="web-api"></a>Web-API

Verzenden van een bijlage met behulp van de Web-API is vergelijkbaar met het verzenden van een e-mail met behulp van de Web-API. Houd er echter rekening mee dat in het volgende voorbeeld, de parametermatrix mag dit element:

```php
    'files['.$fileName.']' => '@'.$filePath.'/'.$fileName
```

#### <a name="example"></a>Voorbeeld

```php
<?php

 $url = 'https://api.sendgrid.com/';
 $user = 'USERNAME';
 $pass = 'PASSWORD';

 $fileName = 'myfile';
 $filePath = dirname(__FILE__);

 $params = array(
     'api_user' => $user,
     'api_key' => $pass,
     'to' =>'john@contoso.com',
     'subject' => 'test of file sends',
     'html' => '<p> the HTML </p>',
     'text' => 'the plain text',
     'from' => 'anna@contoso.com',
     'files['.$fileName.']' => '@'.$filePath.'/'.$fileName
 );

 print_r($params);

 $request = $url.'api/mail.send.json';

 // Generate curl request
 $session = curl_init($request);

 // Tell curl to use HTTP POST
 curl_setopt ($session, CURLOPT_POST, true);

 // Tell curl that this is the body of the POST
 curl_setopt ($session, CURLOPT_POSTFIELDS, $params);

 // Tell curl not to return headers, but do return the response
 curl_setopt($session, CURLOPT_HEADER, false);
 curl_setopt($session, CURLOPT_RETURNTRANSFER, true);

 // obtain response
 $response = curl_exec($session);
 curl_close($session);

 // print everything out
 print_r($response);
```

## <a name="how-to-use-filters-to-enable-footers-tracking-and-analytics"></a>Procedure: Filters gebruiken om in te schakelen voetteksten, tracering en Analytics

SendGrid zorgt voor extra e-mailfunctionaliteit via het gebruik van *filters*. Dit zijn de instellingen die kunnen worden toegevoegd aan een e-mailbericht om in te schakelen specifieke functionaliteit, zoals het inschakelen van klikken traceren in, Google analytics en abonnement bijhouden.

Filters kunnen worden toegepast op een bericht met behulp van de eigenschap filters. Elk filter is opgegeven door een hash met filter-specifieke instellingen. Het volgende voorbeeld wordt het filter voettekst en bevat een SMS-bericht dat wordt toegevoegd aan de onderkant van het e-mailbericht. Voor dit voorbeeld gebruiken we [sendgrid-php-bibliotheek].

Gebruik [Composer] bibliotheek te installeren:

```bash
php composer.phar require sendgrid/sendgrid 2.1.1
```

### <a name="example"></a>Voorbeeld  

```php
<?php
 /*
  * This example is used for sendgrid-php V2.1.1 (https://github.com/sendgrid/sendgrid-php/tree/v2.1.1)
  */
 include "vendor/autoload.php";

 $email = new SendGrid\Email();
 // The list of addresses this message will be sent to
 // [This list is used for sending multiple emails using just ONE request to SendGrid]
 $toList = array('john@contoso.com', 'anna@contoso.com');

 // Specify the names of the recipients
 $nameList = array('Name 1', 'Name 2');

 // Used as an example of variable substitution
 $timeList = array('4 PM', '5 PM');

 // Set all of the above variables
 $email->setTos($toList);
 $email->addSubstitution('-name-', $nameList);
 $email->addSubstitution('-time-', $timeList);

 // Specify that this is an initial contact message
 $email->addCategory("initial");

 // You can optionally setup individual filters here, in this example, we have
 // enabled the footer filter
 $email->addFilter('footer', 'enable', 1);
 $email->addFilter('footer', "text/plain", "Thank you for your business");
 $email->addFilter('footer', "text/html", "Thank you for your business");

 // The subject of your email
 $subject = 'Example SendGrid Email';

 // Where is this message coming from. For example, this message can be from
 // support@yourcompany.com, info@yourcompany.com
 $from = 'someone@example.com';

 // If you do not specify a sender list above, you can specify the user here. If
 // a sender list IS specified above, this email address becomes irrelevant.
 $to = 'john@contoso.com';

 # Create the body of the message (a plain-text and an HTML version).
 # text is your plain-text email
 # html is your html version of the email
 # if the receiver is able to view html emails then only the html
 # email will be displayed

 /*
  * Note the variable substitution here =)
  */
 $text = "
 Hello -name-,
 Thank you for your interest in our products. We have set up an appointment to call you at -time- EST to discuss your needs in more detail.
 Regards,
 Fred";

 $html = "
 <html>
 <head></head>
 <body>
 <p>Hello -name-,<br>
 Thank you for your interest in our products. We have set up an appointment
 to call you at -time- EST to discuss your needs in more detail.

 Regards,

 Fred<br>
 </p>
 </body>
 </html>";

 // set subject
 $email->setSubject($subject);

 // attach the body of the email
 $email->setFrom($from);
 $email->setHtml($html);
 $email->addTo($to);
 $email->setText($text);

 // Your SendGrid account credentials
 $username = 'sendgridusername@yourdomain.com';
 $password = 'example';

 // Create SendGrid object
 $sendgrid = new SendGrid($username, $password);

 // send message
 $response = $sendgrid->send($email);

 print_r($response);
 ```

## <a name="next-steps"></a>Volgende stappen

Nu dat u de basisprincipes van de SendGrid-e-mailservice hebt geleerd, volgt u deze koppelingen voor meer informatie.

* SendGrid-documentatie: <https://sendgrid.com/docs>
* SendGrid PHP-bibliotheek: <https://github.com/sendgrid/sendgrid-php>
* SendGrid speciaal aanbod voor Azure-klanten: <https://sendgrid.com/windowsazure.html>

Zie voor meer informatie, ook de [PHP-ontwikkelaarscentrum](https://azure.microsoft.com/develop/php/).

[https://sendgrid.com]: https://sendgrid.com
[https://sendgrid.com/transactional-email/pricing]: https://sendgrid.com/transactional-email/pricing
[special offer]: https://www.sendgrid.com/windowsazure.html
[Packaging and Deploying PHP Applications for Azure]: https://msdn.microsoft.com/library/windowsazure/hh674499(v=VS.103).aspx
[http://swiftmailer.org/download]: http://swiftmailer.org/download
[curl function]: https://php.net/curl
[cloud-gebaseerde e-mailservice]: https://sendgrid.com/email-solutions
[transactionele e-mail delivery]: https://sendgrid.com/transactional-email
[sendgrid-php-bibliotheek]: https://github.com/sendgrid/sendgrid-php/tree/v2.1.1
[Composer]: https://getcomposer.org/download/
