---
title: 'Quick Start: verbinding maken met een app voor aangepaste opdrachten met Speech SDK-Speech Service'
titleSuffix: Azure Cognitive Services
description: In dit artikel maakt u een Speech SDK-client toepassing met aangepaste opdrachten.
services: cognitive-services
author: nitinme
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/09/2019
ms.author: nitinme
ms.openlocfilehash: f2e8292ccace38e1a167c3ecdfb924589e4ebf0e
ms.sourcegitcommit: 5504d5a88896c692303b9c676a7d2860f36394c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/08/2020
ms.locfileid: "84508927"
---
# <a name="quickstart-connect-to-a-custom-commands-application-with-the-speech-sdk-preview"></a>Quick Start: verbinding maken met een toepassing voor aangepaste opdrachten met de spraak-SDK (preview)

Nadat u een gehoste aangepaste opdrachten toepassing hebt gemaakt, kunt u aan de slag met het praten vanaf een client apparaat.

In dit artikel voert u de volgende handelingen uit:

- Een toepassing voor aangepaste opdrachten publiceren en een toepassings-id ophalen (App-ID)
- Een Universeel Windows-platform-client-app (UWP) maken met behulp van de Speech SDK zodat u kunt praten met uw toepassing voor aangepaste opdrachten

## <a name="prerequisites"></a>Vereisten

Er is een toepassing voor aangepaste opdrachten vereist om dit artikel te volt ooien. Als u nog geen toepassing voor aangepaste opdrachten hebt gemaakt, kunt u dit doen in deze vorige Snelstartgids:
> [!div class = "checklist"]
> * [Snelstartgids: een aangepaste opdracht maken (preview)](./quickstart-custom-speech-commands-create-new.md)
> * [Snelstartgids: een aangepaste opdracht maken met para meters (preview)](./quickstart-custom-speech-commands-create-parameters.md)

U hebt ook het volgende nodig:
> [!div class = "checklist"]
> * [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/)
> * Een Azure-abonnements sleutel voor spraak Services. [Ontvang een gratis versie](get-started.md) of maak deze op de [Azure Portal](https://portal.azure.com)
> * [Uw apparaat inschakelen voor ontwikkeling](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development)

## <a name="optional-get-started-fast"></a>Optioneel: snel aan de slag

In deze Snelstartgids wordt stapsgewijs beschreven hoe u een client toepassing kunt maken om verbinding te maken met uw aangepaste opdrachten-app. Als u voor het eerst wilt beginnen, is de volledige, kant-en-assembly-bron code die wordt gebruikt in deze Quick Start, beschikbaar in de voor [beelden van Speech SDK](https://aka.ms/csspeech/samples) onder de- `quickstart` map.

## <a name="step-1-publish-custom-commands-application"></a>Stap 1: de toepassing voor het publiceren van aangepaste opdrachten

1. Open de [eerder gemaakte toepassing voor aangepaste opdrachten (preview)](./quickstart-custom-speech-commands-create-new.md) en selecteer **publiceren**

   > [!div class="mx-imgBorder"]
   > ![Uw toepassing publiceren](media/custom-speech-commands/fulfill-sdk-publish-application.png)

1. De App-ID uit de publicatie melding kopiëren voor later gebruik
1. Kopieer de spraak bron sleutel voor later gebruik

## <a name="step-2-create-a-visual-studio-project"></a>Stap 2: een Visual Studio-project maken

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-uwp-create-proj.md)]

## <a name="step-3-add-sample-code"></a>Stap 3: voorbeeld code toevoegen

In deze stap voegen we de XAML-code toe die de gebruikers interface van de toepassing definieert en voegt u de C#-code-behind-implementatie toe.

### <a name="xaml-code"></a>XAML-code

Maak de gebruikers interface van de toepassing door de XAML-code toe te voegen.

1. Open in **Solution Explorer**`MainPage.xaml`

1. Vervang in de XAML-weer gave van de ontwerp functie de volledige inhoud door het volgende code fragment:

   ```xml
   <Page
       x:Class="helloworld.MainPage"
       xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
       xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
       xmlns:local="using:helloworld"
       xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
       xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
       mc:Ignorable="d"
       Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

       <Grid>
           <StackPanel Orientation="Vertical" HorizontalAlignment="Center"
                       Margin="20,50,0,0" VerticalAlignment="Center" Width="800">
               <Button x:Name="EnableMicrophoneButton" Content="Enable Microphone"
                       Margin="0,0,10,0" Click="EnableMicrophone_ButtonClicked"
                       Height="35"/>
               <Button x:Name="ListenButton" Content="Talk"
                       Margin="0,10,10,0" Click="ListenButton_ButtonClicked"
                       Height="35"/>
               <StackPanel x:Name="StatusPanel" Orientation="Vertical"
                           RelativePanel.AlignBottomWithPanel="True"
                           RelativePanel.AlignRightWithPanel="True"
                           RelativePanel.AlignLeftWithPanel="True">
                   <TextBlock x:Name="StatusLabel" Margin="0,10,10,0"
                              TextWrapping="Wrap" Text="Status:" FontSize="20"/>
                   <Border x:Name="StatusBorder" Margin="0,0,0,0">
                       <ScrollViewer VerticalScrollMode="Auto"
                                     VerticalScrollBarVisibility="Auto" MaxHeight="200">
                           <!-- Use LiveSetting to enable screen readers to announce
                                the status update. -->
                           <TextBlock
                               x:Name="StatusBlock" FontWeight="Bold"
                               AutomationProperties.LiveSetting="Assertive"
                               MaxWidth="{Binding ElementName=Splitter, Path=ActualWidth}"
                               Margin="10,10,10,20" TextWrapping="Wrap"  />
                       </ScrollViewer>
                   </Border>
               </StackPanel>
           </StackPanel>
           <MediaElement x:Name="mediaElement"/>
       </Grid>
   </Page>
   ```

De Ontwerpweergave is bijgewerkt om de gebruikers interface van de toepassing weer te geven.

### <a name="c-code-behind-source"></a>C#-code-achter-bron

Voeg de code achter de bron toe zodat de toepassing werkt zoals verwacht. De bron code achter bevat:

- Vereiste `using` instructies voor de `Speech` en- `Speech.Dialog` naam ruimten
- Een eenvoudige implementatie om toegang tot de microfoon te garanderen, bekabeld tot een knop-handler
- Helpers van de Basic-gebruikers interface om berichten en fouten in de toepassing weer te geven
- Een overloop punt voor het pad van de initialisatie code dat later wordt ingevuld
- Een helper voor het afspelen van tekst naar spraak (zonder streaming-ondersteuning)
- Een lege knop-handler om te Luis teren die later wordt ingevuld

Voeg als volgt de code-behind bron toe:

1. Open in **Solution Explorer**het bron bestand van de code-behind `MainPage.xaml.cs` (gegroepeerd onder `MainPage.xaml` )

1. Vervang de inhoud van het bestand door de volgende code: 

   ```csharp
   using Microsoft.CognitiveServices.Speech;
   using Microsoft.CognitiveServices.Speech.Audio;
   using Microsoft.CognitiveServices.Speech.Dialog;
   using System;
   using System.Diagnostics;
   using System.IO;
   using System.Text;
   using Windows.Foundation;
   using Windows.Storage.Streams;
   using Windows.UI.Xaml;
   using Windows.UI.Xaml.Controls;
   using Windows.UI.Xaml.Media;

   namespace helloworld
   {
       public sealed partial class MainPage : Page
       {
           private DialogServiceConnector connector;

           private enum NotifyType
           {
               StatusMessage,
               ErrorMessage
           };

           public MainPage()
           {
               this.InitializeComponent();
           }

           private async void EnableMicrophone_ButtonClicked(
               object sender, RoutedEventArgs e)
           {
               bool isMicAvailable = true;
               try
               {
                   var mediaCapture = new Windows.Media.Capture.MediaCapture();
                   var settings =
                       new Windows.Media.Capture.MediaCaptureInitializationSettings();
                   settings.StreamingCaptureMode =
                       Windows.Media.Capture.StreamingCaptureMode.Audio;
                   await mediaCapture.InitializeAsync(settings);
               }
               catch (Exception)
               {
                   isMicAvailable = false;
               }
               if (!isMicAvailable)
               {
                   await Windows.System.Launcher.LaunchUriAsync(
                       new Uri("ms-settings:privacy-microphone"));
               }
               else
               {
                   NotifyUser("Microphone was enabled", NotifyType.StatusMessage);
               }
           }

           private void NotifyUser(
               string strMessage, NotifyType type = NotifyType.StatusMessage)
           {
               // If called from the UI thread, then update immediately.
               // Otherwise, schedule a task on the UI thread to perform the update.
               if (Dispatcher.HasThreadAccess)
               {
                   UpdateStatus(strMessage, type);
               }
               else
               {
                   var task = Dispatcher.RunAsync(
                       Windows.UI.Core.CoreDispatcherPriority.Normal,
                       () => UpdateStatus(strMessage, type));
               }
           }

           private void UpdateStatus(string strMessage, NotifyType type)
           {
               switch (type)
               {
                   case NotifyType.StatusMessage:
                       StatusBorder.Background = new SolidColorBrush(
                           Windows.UI.Colors.Green);
                       break;
                   case NotifyType.ErrorMessage:
                       StatusBorder.Background = new SolidColorBrush(
                           Windows.UI.Colors.Red);
                       break;
               }
               StatusBlock.Text += string.IsNullOrEmpty(StatusBlock.Text)
                   ? strMessage : "\n" + strMessage;

               if (!string.IsNullOrEmpty(StatusBlock.Text))
               {
                   StatusBorder.Visibility = Visibility.Visible;
                   StatusPanel.Visibility = Visibility.Visible;
               }
               else
               {
                   StatusBorder.Visibility = Visibility.Collapsed;
                   StatusPanel.Visibility = Visibility.Collapsed;
               }
               // Raise an event if necessary to enable a screen reader
               // to announce the status update.
               var peer = Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer.FromElement(StatusBlock);
               if (peer != null)
               {
                   peer.RaiseAutomationEvent(
                       Windows.UI.Xaml.Automation.Peers.AutomationEvents.LiveRegionChanged);
               }
           }

           // Waits for and accumulates all audio associated with a given
           // PullAudioOutputStream and then plays it to the MediaElement. Long spoken
           // audio will create extra latency and a streaming playback solution
           // (that plays audio while it continues to be received) should be used --
           // see the samples for examples of this.
           private void SynchronouslyPlayActivityAudio(
               PullAudioOutputStream activityAudio)
           {
               var playbackStreamWithHeader = new MemoryStream();
               playbackStreamWithHeader.Write(Encoding.ASCII.GetBytes("RIFF"), 0, 4); // ChunkID
               playbackStreamWithHeader.Write(BitConverter.GetBytes(UInt32.MaxValue), 0, 4); // ChunkSize: max
               playbackStreamWithHeader.Write(Encoding.ASCII.GetBytes("WAVE"), 0, 4); // Format
               playbackStreamWithHeader.Write(Encoding.ASCII.GetBytes("fmt "), 0, 4); // Subchunk1ID
               playbackStreamWithHeader.Write(BitConverter.GetBytes(16), 0, 4); // Subchunk1Size: PCM
               playbackStreamWithHeader.Write(BitConverter.GetBytes(1), 0, 2); // AudioFormat: PCM
               playbackStreamWithHeader.Write(BitConverter.GetBytes(1), 0, 2); // NumChannels: mono
               playbackStreamWithHeader.Write(BitConverter.GetBytes(16000), 0, 4); // SampleRate: 16kHz
               playbackStreamWithHeader.Write(BitConverter.GetBytes(32000), 0, 4); // ByteRate
               playbackStreamWithHeader.Write(BitConverter.GetBytes(2), 0, 2); // BlockAlign
               playbackStreamWithHeader.Write(BitConverter.GetBytes(16), 0, 2); // BitsPerSample: 16-bit
               playbackStreamWithHeader.Write(Encoding.ASCII.GetBytes("data"), 0, 4); // Subchunk2ID
               playbackStreamWithHeader.Write(BitConverter.GetBytes(UInt32.MaxValue), 0, 4); // Subchunk2Size

               byte[] pullBuffer = new byte[2056];

               uint lastRead = 0;
               do
               {
                   lastRead = activityAudio.Read(pullBuffer);
                   playbackStreamWithHeader.Write(pullBuffer, 0, (int)lastRead);
               }
               while (lastRead == pullBuffer.Length);

               var task = Dispatcher.RunAsync(
                   Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
               {
                   mediaElement.SetSource(
                       playbackStreamWithHeader.AsRandomAccessStream(), "audio/wav");
                   mediaElement.Play();
               });
           }

           private void InitializeDialogServiceConnector()
           {
               // New code will go here
           }

           private async void ListenButton_ButtonClicked(
               object sender, RoutedEventArgs e)
           {
               // New code will go here
           }
       }
   }
   ```
    > [!NOTE]
    > Als u fout ziet: ' het type ' object ' is gedefinieerd in een assembly waarnaar niet wordt verwezen '
    > 1. Rechts-client uw oplossing.
    > 1. Kies **NuGet-pakketten beheren voor oplossing**, selecteer **updates** 
    > 1. Als u **micro soft. NetCore. UniversalWindowsPlatform** in de update lijst ziet, werkt u **micro soft. NetCore. UniversalWindowsPlatform** bij naar de nieuwste versie

1. Voeg de volgende code toe aan de methode hoofdtekst van`InitializeDialogServiceConnector`

   ```csharp
   // This code creates the `DialogServiceConnector` with your subscription information.
   // create a DialogServiceConfig by providing a Custom Commands application id and Cognitive Services subscription key
   // the RecoLanguage property is optional (default en-US); note that only en-US is supported in Preview
   const string speechCommandsApplicationId = "YourApplicationId"; // Your application id
   const string speechSubscriptionKey = "YourSpeechSubscriptionKey"; // Your subscription key
   const string region = "YourServiceRegion"; // The subscription service region. Note: only 'westus2' is currently supported

   var speechCommandsConfig = CustomCommandsConfig.FromSubscription(speechCommandsApplicationId, speechSubscriptionKey, region);
   speechCommandsConfig.SetProperty(PropertyId.SpeechServiceConnection_RecoLanguage, "en-us");
   connector = new DialogServiceConnector(speechCommandsConfig);
   ```

1. Vervang de teken reeksen `YourApplicationId` , `YourSpeechSubscriptionKey` en `YourServiceRegion` met uw eigen waarden voor uw app, het spraak abonnement en de [regio](regions.md)

1. Voeg het volgende code fragment toe aan het einde van de methode hoofdtekst van`InitializeDialogServiceConnector`

   ```csharp
   //
   // This code sets up handlers for events relied on by `DialogServiceConnector` to communicate its activities,
   // speech recognition results, and other information.
   //
   // ActivityReceived is the main way your client will receive messages, audio, and events
   connector.ActivityReceived += async (sender, activityReceivedEventArgs) =>
   {
       NotifyUser(
           $"Activity received, hasAudio={activityReceivedEventArgs.HasAudio} activity={activityReceivedEventArgs.Activity}");

       if (activityReceivedEventArgs.HasAudio)
       {
           SynchronouslyPlayActivityAudio(activityReceivedEventArgs.Audio);
       }
   };

   // Canceled will be signaled when a turn is aborted or experiences an error condition
   connector.Canceled += (sender, canceledEventArgs) =>
   {
       NotifyUser($"Canceled, reason={canceledEventArgs.Reason}");
       if (canceledEventArgs.Reason == CancellationReason.Error)
       {
           NotifyUser(
               $"Error: code={canceledEventArgs.ErrorCode}, details={canceledEventArgs.ErrorDetails}");
       }
   };

   // Recognizing (not 'Recognized') will provide the intermediate recognized text
   // while an audio stream is being processed
   connector.Recognizing += (sender, recognitionEventArgs) =>
   {
       NotifyUser($"Recognizing! in-progress text={recognitionEventArgs.Result.Text}");
   };

   // Recognized (not 'Recognizing') will provide the final recognized text
   // once audio capture is completed
   connector.Recognized += (sender, recognitionEventArgs) =>
   {
       NotifyUser($"Final speech-to-text result: '{recognitionEventArgs.Result.Text}'");
   };

   // SessionStarted will notify when audio begins flowing to the service for a turn
   connector.SessionStarted += (sender, sessionEventArgs) =>
   {
       NotifyUser($"Now Listening! Session started, id={sessionEventArgs.SessionId}");
   };

   // SessionStopped will notify when a turn is complete and
   // it's safe to begin listening again
   connector.SessionStopped += (sender, sessionEventArgs) =>
   {
       NotifyUser($"Listening complete. Session ended, id={sessionEventArgs.SessionId}");
   };
   ```

1. Voeg het volgende code fragment toe aan de hoofd tekst van de `ListenButton_ButtonClicked` methode in de `MainPage` klasse

   ```csharp
   // This code sets up `DialogServiceConnector` to listen, since you already established the configuration and
   // registered the event handlers.
   if (connector == null)
   {
       InitializeDialogServiceConnector();
       // Optional step to speed up first interaction: if not called,
       // connection happens automatically on first use
       var connectTask = connector.ConnectAsync();
   }

   try
   {
       // Start sending audio
       await connector.ListenOnceAsync();
   }
   catch (Exception ex)
   {
       NotifyUser($"Exception: {ex.ToString()}", NotifyType.ErrorMessage);
   }
   ```

1. Kies in de menu balk de optie **bestand**  >  **Alles opslaan** om uw wijzigingen op te slaan.

## <a name="build-and-run-the-application"></a>De toepassing bouwen en uitvoeren.

1. **Kies Build**  >  **Build Solution** in de menu balk om de toepassing te bouwen. De code moet zonder fouten worden gecompileerd.

1. Kies **fout**  >  **opsporing starten** (of druk op **F5**) om de toepassing te starten. Het venster **HelloWorld** wordt weer gegeven.

   ![Voor beeld van een UWP-toepassing voor virtuele assistenten in C#-Quick Start](media/sdk/qs-voice-assistant-uwp-helloworld-window.png)

1. Selecteer **microfoon inschakelen**. Als de toegangs machtigings aanvraag wordt weer gegeven, selecteert u **Ja**.

   ![Verzoek om toegang tot de microfoon](media/sdk/qs-csharp-uwp-10-access-prompt.png)

1. Selecteer **Talk**en spreek een Engelse zin of zin in op de microfoon van uw apparaat. Uw spraak wordt verzonden naar het directe lijn spraak kanaal en naar tekst getranscribeerd, die wordt weer gegeven in het venster.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Procedure: opdrachten op de client uitvoeren met de Speech-SDK (preview)](./how-to-custom-speech-commands-fulfill-sdk.md) 
>  [Procedure: validaties toevoegen aan aangepaste opdracht parameters (preview-versie)](./how-to-custom-speech-commands-validations.md)

## <a name="sample-source-code"></a>Voorbeeld bron code
Bekijk onze client sample codes op [github-VoiceAssistant](https://github.com/Azure-Samples/Cognitive-Services-Voice-Assistant)
