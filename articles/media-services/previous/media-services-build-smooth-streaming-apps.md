---
title: Smooth Streaming-zelfstudie voor Windows Store-Apps | Microsoft Docs
description: Informatie over het gebruik van Azure Media Services maken een C# Windows Store-toepassing met een XML-MediaElement-besturingselement voor het afspelen van Smooth Stream inhoud.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: 0fa5d8c5-3d5f-4886-ae55-fb6de4f5256d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2019
ms.author: juliako
ms.openlocfilehash: b8c1513838fb848388946e18698a0410aa7a0332
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2019
ms.locfileid: "65949624"
---
# <a name="how-to-build-a-smooth-streaming-windows-store-application"></a>Over het bouwen van een toepassing met probleemloze Streaming Windows Store  

De Smooth Streaming Client SDK voor Windows 8 biedt ontwikkelaars de mogelijkheid om Windows Store-toepassingen die on-demand en live Smooth Streaming inhoud kunnen spelen te bouwen. Naast de eenvoudige afspelen van Smooth Streaming-inhoud, biedt de SDK ook uitgebreide functies, zoals Microsoft PlayReady-bescherming, kwaliteit niveau beperking, Live DVR, audiostream overschakelen, luistert naar statusupdates (zoals kwaliteit verandert) en gebeurtenissen op foutniveau, enzovoort. Zie voor meer informatie van de ondersteunde functies, de [opmerkingen bij de release](https://www.iis.net/learn/media/smooth-streaming/smooth-streaming-client-sdk-for-windows-8-release-notes). Zie voor meer informatie, [Player Framework voor Windows 8](https://playerframework.codeplex.com/). 

In deze zelfstudie bevat vier lessen:

1. Een eenvoudige Smooth Streaming Store-toepassing maken
2. Toevoegen van een schuifregelaar voor het beheren van de voortgang van de Media
3. Selecteer Streams Smooth Streaming
4. Selecteer nummers Smooth Streaming

## <a name="prerequisites"></a>Vereisten
> [!NOTE]
> Windows Store-projecten versie 8.1 en oudere versies worden niet ondersteund in Visual Studio 2017.  Zie [Geschikte platforms voor Visual Studio 2017 en compatibiliteit](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs) voor meer informatie.

* Windows 8, 32-bits of 64-bits.
* Versies voor Visual Studio 2012 tot 2015.
* [Microsoft Smooth Streaming Client SDK voor Windows 8](https://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Home https://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Home).

De voltooide oplossing voor elke les kan worden gedownload van MSDN Developer-codevoorbeelden (Code-galerie): 

* [Les 1](https://code.msdn.microsoft.com/Smooth-Streaming-Client-0bb1471f) : een eenvoudige Windows 8-Smooth Streaming van MediaPlayer 
* [Les 2](https://code.msdn.microsoft.com/A-simple-Windows-8-Smooth-ee98f63a) : een eenvoudige Windows 8-Smooth Streaming van MediaPlayer met een schuifregelaar besturingselement 
* [Les 3](https://code.msdn.microsoft.com/A-Windows-8-Smooth-883c3b44) - een Windows 8-Smooth Streaming van MediaPlayer bij selectie van Stream  
* [Les 4](https://code.msdn.microsoft.com/A-Windows-8-Smooth-aa9e4907) - een Windows 8-Smooth Streaming van MediaPlayer bij selectie bijhouden.

## <a name="lesson-1-create-a-basic-smooth-streaming-store-application"></a>Les 1: Een eenvoudige Smooth Streaming Store-toepassing maken

In deze les gaat uitvoeren maakt u een Windows Store-toepassing met een besturingselement MediaElement Smooth Stream afspelen inhoud.  De actieve App ziet eruit zoals:

![Voorbeeld van Smooth Streaming Windows Store-toepassing][PlayerApplication]

Zie voor meer informatie over het ontwikkelen van Windows Store-toepassing [ontwikkelen van geweldige Apps voor Windows 8](https://msdn.microsoft.com/windows/apps/br229512.aspx). In deze les bevat de volgende procedures:

1. Een Windows Store-project maken
2. Ontwerp van de gebruikersinterface (XAML)
3. Wijzigen van het codebestand
4. Samenstellen en testen van de toepassing

### <a name="to-create-a-windows-store-project"></a>Een Windows Store-project maken

1. Visual Studio; uitvoeren 2012 tot 2015-versies worden ondersteund.
1. Klik in het menu **FILE** op **Nieuw** en klik vervolgens op **Project**.
1. Typ of Selecteer de volgende waarden in het dialoogvenster Nieuw Project:

    | Name | Value |
    | --- | --- |
    | Sjabloongroep |Geïnstalleerd/sjablonen/Visual C#Windows Store |
    | Template |Lege App (XAML) |
    | Name |SSPlayer |
    | Locatie |C:\SSTutorials |
    | De naam van oplossing |SSPlayer |
    | Map maken voor oplossing |(geselecteerd) |

1. Klik op **OK**.

### <a name="to-add-a-reference-to-the-smooth-streaming-client-sdk"></a>Een verwijzing naar de Smooth Streaming Client SDK toevoegen

1. Klik in Solution Explorer met de rechtermuisknop op **SSPlayer**, en klik vervolgens op **verwijzing toevoegen**.
1. Typ of selecteer de volgende waarden:

    | Name | Value |
    | --- | --- |
    | Referentie-groep |Windows/Extensions |
    | Referentie |Selecteer Microsoft Smooth Streaming Client SDK voor Windows 8 en Microsoft Visual C++ Runtime-pakket |

1. Klik op **OK**. 

Na het toevoegen van de referenties, moet u de betreffende platform (x64 of x86), toe te voegen verwijzingen werkt niet voor de configuratie van een CPU-platform.  Klik in solution explorer ziet u gele waarschuwing is ingeschakeld voor deze referenties toegevoegd.

### <a name="to-design-the-player-user-interface"></a>De gebruikersinterface van de speler ontwerpen

1. Klik in Solution Explorer dubbelklikt u op **MainPage.xaml** om dit te openen in de ontwerpweergave.
2. Zoek de **&lt;raster&gt;** en **&lt;/Grid&gt;** tags van het XAML-bestand en plak de volgende code tussen de twee labels:

   ```xml
         <Grid.RowDefinitions>

            <RowDefinition Height="20"/>    <!-- spacer -->
            <RowDefinition Height="50"/>    <!-- media controls -->
            <RowDefinition Height="100*"/>  <!-- media element -->
            <RowDefinition Height="80*"/>   <!-- media stream and track selection -->
            <RowDefinition Height="50"/>    <!-- status bar -->
         </Grid.RowDefinitions>

         <StackPanel Name="spMediaControl" Grid.Row="1" Orientation="Horizontal">
            <TextBlock x:Name="tbSource" Text="Source :  " FontSize="16" FontWeight="Bold" VerticalAlignment="Center" />
            <TextBox x:Name="txtMediaSource" Text="https://ecn.channel9.msdn.com/o9/content/smf/smoothcontent/elephantsdream/Elephants_Dream_1024-h264-st-aac.ism/manifest" FontSize="10" Width="700" Margin="0,4,0,10" />
            <Button x:Name="btnSetSource" Content="Set Source" Width="111" Height="43" Click="btnSetSource_Click"/>
            <Button x:Name="btnPlay" Content="Play" Width="111" Height="43" Click="btnPlay_Click"/>
            <Button x:Name="btnPause" Content="Pause"  Width="111" Height="43" Click="btnPause_Click"/>
            <Button x:Name="btnStop" Content="Stop"  Width="111" Height="43" Click="btnStop_Click"/>
            <CheckBox x:Name="chkAutoPlay" Content="Auto Play" Height="55" Width="Auto" IsChecked="{Binding AutoPlay, ElementName=mediaElement, Mode=TwoWay}"/>
            <CheckBox x:Name="chkMute" Content="Mute" Height="55" Width="67" IsChecked="{Binding IsMuted, ElementName=mediaElement, Mode=TwoWay}"/>
         </StackPanel>

         <StackPanel Name="spMediaElement" Grid.Row="2" Height="435" Width="1072"
                    HorizontalAlignment="Center" VerticalAlignment="Center">
            <MediaElement x:Name="mediaElement" Height="356" Width="924" MinHeight="225"
                          HorizontalAlignment="Center" VerticalAlignment="Center" 
                          AudioCategory="BackgroundCapableMedia" />
            <StackPanel Orientation="Horizontal">
                <Slider x:Name="sliderProgress" Width="924" Height="44"
                        HorizontalAlignment="Center" VerticalAlignment="Center"
                        PointerPressed="sliderProgress_PointerPressed"/>
                <Slider x:Name="sliderVolume" 
                        HorizontalAlignment="Right" VerticalAlignment="Center" Orientation="Vertical" 
                        Height="79" Width="148" Minimum="0" Maximum="1" StepFrequency="0.1" 
                        Value="{Binding Volume, ElementName=mediaElement, Mode=TwoWay}" 
                        ToolTipService.ToolTip="{Binding Value, RelativeSource={RelativeSource Mode=Self}}"/>
            </StackPanel>
         </StackPanel>

         <StackPanel Name="spStatus" Grid.Row="4" Orientation="Horizontal">
            <TextBlock x:Name="tbStatus" Text="Status :  " 
               FontSize="16" FontWeight="Bold" VerticalAlignment="Center" HorizontalAlignment="Center" />
            <TextBox x:Name="txtStatus" FontSize="10" Width="700" VerticalAlignment="Center"/>
         </StackPanel>
   ```
   Het besturingselement MediaElement wordt gebruikt voor het afspelen van media. Het besturingselement voor schuifregelaar met de naam sliderProgress wordt in de volgende les worden gebruikt voor het beheren van de voortgang van de media.
3. Druk op **CTRL + S** op te slaan.

Het besturingselement MediaElement biedt geen ondersteuning voor Smooth Streaming inhoud out-of-box. Als u wilt de Smooth Streaming-ondersteuning inschakelen, moet u de Smooth Streaming-byte-stream handler door bestandsnaamextensie en MIME-type registreren.  Als u wilt registreren, moet u de methode MediaExtensionManager.RegisterByteStreamHandler van de naamruimte Windows.Media gebruiken.

In dit bestand XAML zijn sommige gebeurtenis-handlers gekoppeld aan de besturingselementen.  U moet deze gebeurtenis-handlers definiëren.

### <a name="to-modify-the-code-behind-file"></a>Wijzigen van het codebestand

1. Klik in Solution Explorer met de rechtermuisknop op **MainPage.xaml**, en klik vervolgens op **Code weergeven**.
2. Voeg het volgende toe aan de bovenkant van het bestand met de instructie:
   
        using Windows.Media;
3. Aan het begin van de **MainPage** klasse, de volgende gegevenslid toevoegen:
   
         private MediaExtensionManager extensions = new MediaExtensionManager();
4. Aan het einde van de **MainPage** constructor, voeg de volgende twee regels:
   
        extensions.RegisterByteStreamHandler("Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", ".ism", "text/xml");
        extensions.RegisterByteStreamHandler("Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", ".ism", "application/vnd.ms-sstr+xml");
5. Aan het einde van de **MainPage** klasse, plak de volgende code:
   ```csharp
         # region UI Button Click Events
         private void btnPlay_Click(object sender, RoutedEventArgs e)
         {

         mediaElement.Play();
         txtStatus.Text = "MediaElement is playing ...";
         }
         private void btnPause_Click(object sender, RoutedEventArgs e)
         {

         mediaElement.Pause();
         txtStatus.Text = "MediaElement is paused";
         }
         private void btnSetSource_Click(object sender, RoutedEventArgs e)
         {

         sliderProgress.Value = 0;
         mediaElement.Source = new Uri(txtMediaSource.Text);

         if (chkAutoPlay.IsChecked == true)
         {
             txtStatus.Text = "MediaElement is playing ...";
         }
         else
         {
             txtStatus.Text = "Click the Play button to play the media source.";
         }
         }
         private void btnStop_Click(object sender, RoutedEventArgs e)
         {

         mediaElement.Stop();
         txtStatus.Text = "MediaElement is stopped";
         }
         private void sliderProgress_PointerPressed(object sender, PointerRoutedEventArgs e)
         {

         txtStatus.Text = "Seek to position " + sliderProgress.Value;
         mediaElement.Position = new TimeSpan(0, 0, (int)(sliderProgress.Value));
         }
         # endregion
   ```
   Hier wordt de gebeurtenis-handler sliderProgress_PointerPressed gedefinieerd.  Er zijn meer works te doen om het functioneert, waarvoor u in de volgende les gaat uitvoeren van deze zelfstudie.
6. Druk op **CTRL + S** op te slaan.

Het voltooide die het codebestand dient er als volgt:

![CodeView in Visual Studio van Smooth Streaming Windows Store-toepassing][CodeViewPic]

### <a name="to-compile-and-test-the-application"></a>Compileren en de toepassing testen

1. Uit de **bouwen** menu, klikt u op **Configuration Manager**.
2. Wijziging **Active oplossingsplatform** zodat deze overeenkomen met uw ontwikkelplatform.
3. Druk op **F6** voor het compileren van het project. 
4. Druk op **F5** om de toepassing uit te voeren.
5. Aan de bovenkant van de toepassing, kunt u de standaard-URL voor Smooth Streaming of een andere invoeren. 
6. Klik op **bron instellen**. Omdat **automatisch afspelen** is ingeschakeld standaard de media wordt afgespeeld.  U kunt bepalen de media met de **afspelen**, **onderbreken** en **stoppen** knoppen.  U kunt het Mediavolume met behulp van de verticale schuifbalk beheren.  Maar de horizontale schuifbalk voor het beheren van de voortgang van de media is volledig nog niet geïmplementeerd. 

U kunt lesson1 hebt voltooid.  In deze les gaat uitvoeren gebruikt u een besturingselement MediaElement af te spelen Smooth Streaming-inhoud.  In de volgende les gaat uitvoeren voegt u een schuifregelaar voor het beheren van de voortgang van de Smooth Streaming-inhoud.

## <a name="lesson-2-add-a-slider-bar-to-control-the-media-progress"></a>Les 2: Toevoegen van een schuifregelaar voor het beheren van de voortgang van de Media

In les 1, kunt u een Windows Store-toepassing gemaakt met een besturingselement MediaElement XAML af te spelen Smooth Streaming media-inhoud.  Sommige functies van basic media, zoals starten, stoppen en onderbreken wordt geleverd.  In deze les gaat uitvoeren voegt u een besturingselement voor schuifregelaar balk aan de toepassing.

In deze zelfstudie gebruiken we een timer om bij te werken van de positie van de schuifregelaar op basis van de huidige positie van het besturingselement MediaElement.  De schuifregelaar begin- en tijd ook moeten worden bijgewerkt in het geval van live-inhoud.  Dit kan beter worden verwerkt in de gebeurtenis voor het bijwerken van adaptieve gegevensbron.

Media-bronnen zijn de objecten die mediagegevens genereren.  De bron-resolver gebruikt van een stream-URL of bytes en de juiste mediabron voor die inhoud maakt.  De bronoplossing is de standaardmethode voor de toepassingen te maken van media-bronnen. 

In deze les bevat de volgende procedures:

1. De Smooth Streaming-handler registreren 
2. De adaptieve bron manager op gebeurtenis-handlers toevoegen
3. De adaptieve bron op gebeurtenis-handlers toevoegen
4. Gebeurtenis-handlers MediaElement toevoegen
5. Schuifregelaar gerelateerde code toevoegen
6. Samenstellen en testen van de toepassing

### <a name="to-register-the-smooth-streaming-byte-stream-handler-and-pass-the-propertyset"></a>Voor het registreren van de handler byte-stream Smooth Streaming en de propertyset doorgeven

1. Vanuit Solution Explorer, klik met de rechtermuisknop **MainPage.xaml**, en klik vervolgens op **Code weergeven**.
2. Voeg het volgende toe aan het begin van het bestand met de instructie:

   ```csharp
        using Microsoft.Media.AdaptiveStreaming;
   ```
3. Toevoegen aan het begin van de klasse MainPage, de gegevensleden van de volgende:

   ```csharp
         private Windows.Foundation.Collections.PropertySet propertySet = new Windows.Foundation.Collections.PropertySet();             
         private IAdaptiveSourceManager adaptiveSourceManager;
   ```
4. In de **MainPage** constructor, voeg de volgende code na de **dit. Components(); initialiseren**  regel en de registratie van de regels die zijn geschreven in de vorige les code:

   ```csharp
        // Gets the default instance of AdaptiveSourceManager which manages Smooth 
        //Streaming media sources.
        adaptiveSourceManager = AdaptiveSourceManager.GetDefault();
        // Sets property key value to AdaptiveSourceManager default instance.
        // {A5CE1DE8-1D00-427B-ACEF-FB9A3C93DE2D}" must be hardcoded.
        propertySet["{A5CE1DE8-1D00-427B-ACEF-FB9A3C93DE2D}"] = adaptiveSourceManager;
   ```
5. In de **MainPage** constructor wijzigen van de twee methoden RegisterByteStreamHandler om toe te voegen de beschreven parameters:

   ```csharp
         // Registers Smooth Streaming byte-stream handler for ".ism" extension and, 
         // "text/xml" and "application/vnd.ms-ss" mime-types and pass the propertyset. 
         // http://*.ism/manifest URI resources will be resolved by Byte-stream handler.
         extensions.RegisterByteStreamHandler(

            "Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", 
            ".ism", 
            "text/xml", 
            propertySet );
         extensions.RegisterByteStreamHandler(

            "Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", 
            ".ism", 
            "application/vnd.ms-sstr+xml", 
         propertySet);
   ```
6. Druk op **CTRL + S** op te slaan.

### <a name="to-add-the-adaptive-source-manager-level-event-handler"></a>De adaptieve bron manager op gebeurtenis-handler toevoegen

1. Vanuit Solution Explorer, klik met de rechtermuisknop **MainPage.xaml**, en klik vervolgens op **Code weergeven**.
2. In de **MainPage** klasse, de volgende gegevenslid toevoegen:

   ```csharp
     private AdaptiveSource adaptiveSource = null;
   ```
3. Aan het einde van de **MainPage** klasse, de volgende gebeurtenis-handler toevoegen:

   ```csharp
         # region Adaptive Source Manager Level Events
         private void mediaElement_AdaptiveSourceOpened(AdaptiveSource sender, AdaptiveSourceOpenedEventArgs args)
         {

            adaptiveSource = args.AdaptiveSource;
         }

         # endregion Adaptive Source Manager Level Events
   ```
4. Aan het einde van de **MainPage** constructor, voeg de volgende regel om u te abonneren op de gebeurtenis voor het openen van adaptieve bron:

   ```csharp
         adaptiveSourceManager.AdaptiveSourceOpenedEvent += 
           new AdaptiveSourceOpenedEventHandler(mediaElement_AdaptiveSourceOpened);
   ```
5. Druk op **CTRL + S** op te slaan.

### <a name="to-add-adaptive-source-level-event-handlers"></a>Adaptieve bron op gebeurtenis-handlers toevoegen

1. Vanuit Solution Explorer, klik met de rechtermuisknop **MainPage.xaml**, en klik vervolgens op **Code weergeven**.
2. In de **MainPage** klasse, de volgende gegevenslid toevoegen:

   ```csharp
     private AdaptiveSourceStatusUpdatedEventArgs adaptiveSourceStatusUpdate; 
     private Manifest manifestObject;
   ```
3. Aan het einde van de **MainPage** klasse, de volgende gebeurtenis-handlers toevoegen:

   ```csharp
         # region Adaptive Source Level Events
         private void mediaElement_ManifestReady(AdaptiveSource sender, ManifestReadyEventArgs args)
         {

            adaptiveSource = args.AdaptiveSource;
            manifestObject = args.AdaptiveSource.Manifest;
         }

         private void mediaElement_AdaptiveSourceStatusUpdated(AdaptiveSource sender, AdaptiveSourceStatusUpdatedEventArgs args)
         {

            adaptiveSourceStatusUpdate = args;
         }

         private void mediaElement_AdaptiveSourceFailed(AdaptiveSource sender, AdaptiveSourceFailedEventArgs args)
         {

            txtStatus.Text = "Error: " + args.HttpResponse;
         }

         # endregion Adaptive Source Level Events
   ```
4. Aan het einde van de **mediaElement AdaptiveSourceOpened** methode, voeg de volgende code om u te abonneren op gebeurtenissen:

   ```csharp
         adaptiveSource.ManifestReadyEvent +=

                    mediaElement_ManifestReady;
         adaptiveSource.AdaptiveSourceStatusUpdatedEvent += 

            mediaElement_AdaptiveSourceStatusUpdated;
         adaptiveSource.AdaptiveSourceFailedEvent += 

            mediaElement_AdaptiveSourceFailed;
   ```
5. Druk op **CTRL + S** op te slaan.

Dezelfde gebeurtenissen zijn beschikbaar op adaptieve bron Manager niveau, die kan worden gebruikt voor het verwerken van de functionaliteit voor alle media-elementen in de app. Elke AdaptiveSource zijn eigen gebeurtenissen bevat en worden alle gebeurtenissen voor AdaptiveSource trapsgewijs onder AdaptiveSourceManager.

### <a name="to-add-media-element-event-handlers"></a>Gebeurtenis-handlers voor Media-Element toevoegen

1. Vanuit Solution Explorer, klik met de rechtermuisknop **MainPage.xaml**, en klik vervolgens op **Code weergeven**.
2. Aan het einde van de **MainPage** klasse, de volgende gebeurtenis-handlers toevoegen:

   ```csharp
         # region Media Element Event Handlers
         private void MediaOpened(object sender, RoutedEventArgs e)
         {

            txtStatus.Text = "MediaElement opened";
         }

         private void MediaFailed(object sender, ExceptionRoutedEventArgs e)
         {

            txtStatus.Text= "MediaElement failed: " + e.ErrorMessage;
         }

         private void MediaEnded(object sender, RoutedEventArgs e)
         {

            txtStatus.Text ="MediaElement ended.";
         }

         # endregion Media Element Event Handlers
   ```
3. Aan het einde van de **MainPage** constructor, voeg de volgende code toe subscript op de gebeurtenissen:

   ```csharp
         mediaElement.MediaOpened += MediaOpened;
         mediaElement.MediaEnded += MediaEnded;
         mediaElement.MediaFailed += MediaFailed;
   ```
4. Druk op **CTRL + S** op te slaan.

### <a name="to-add-slider-bar-related-code"></a>Om toe te voegen schuifregelaar gerelateerde code

1. Vanuit Solution Explorer, klik met de rechtermuisknop **MainPage.xaml**, en klik vervolgens op **Code weergeven**.
2. Voeg het volgende toe aan het begin van het bestand met de instructie:

   ```csharp
        using Windows.UI.Core;
   ```
3. In de **MainPage** klasse, voeg de volgende gegevensleden toe:

   ```csharp
         public static CoreDispatcher _dispatcher;
         private DispatcherTimer sliderPositionUpdateDispatcher;
   ```
4. Aan het einde van de **MainPage** constructor, voeg de volgende code toe:

   ```csharp
         _dispatcher = Window.Current.Dispatcher;
         PointerEventHandler pointerpressedhandler = new PointerEventHandler(sliderProgress_PointerPressed);
         sliderProgress.AddHandler(Control.PointerPressedEvent, pointerpressedhandler, true);    
   ```
5. Aan het einde van de **MainPage** klasse, voeg de volgende code toe:

   ```csharp
         # region sliderMediaPlayer
         private double SliderFrequency(TimeSpan timevalue)
         {

            long absvalue = 0;
            double stepfrequency = -1;

            if (manifestObject != null)
            {
                absvalue = manifestObject.Duration - (long)manifestObject.StartTime;
            }
            else
            {
                absvalue = mediaElement.NaturalDuration.TimeSpan.Ticks;
            }

            TimeSpan totalDVRDuration = new TimeSpan(absvalue);

            if (totalDVRDuration.TotalMinutes >= 10 && totalDVRDuration.TotalMinutes < 30)
            {
               stepfrequency = 10;
            }
            else if (totalDVRDuration.TotalMinutes >= 30 
                     && totalDVRDuration.TotalMinutes < 60)
            {
                stepfrequency = 30;
            }
            else if (totalDVRDuration.TotalHours >= 1)
            {
                stepfrequency = 60;
            }

            return stepfrequency;
         }

         void updateSliderPositionoNTicks(object sender, object e)
         {

            sliderProgress.Value = mediaElement.Position.TotalSeconds;
         }

         public void setupTimer()
         {

            sliderPositionUpdateDispatcher = new DispatcherTimer();
            sliderPositionUpdateDispatcher.Interval = new TimeSpan(0, 0, 0, 0, 300);
            startTimer();
         }

         public void startTimer()
         {

            sliderPositionUpdateDispatcher.Tick += updateSliderPositionoNTicks;
            sliderPositionUpdateDispatcher.Start();
         }

         // Slider start and end time must be updated in case of live content
         public async void setSliderStartTime(long startTime)
         {

            await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
                TimeSpan timespan = new TimeSpan(adaptiveSourceStatusUpdate.StartTime);
                double absvalue = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero);
                sliderProgress.Minimum = absvalue;
            });
         }

         // Slider start and end time must be updated in case of live content
         public async void setSliderEndTime(long startTime)
         {

            await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
                TimeSpan timespan = new TimeSpan(adaptiveSourceStatusUpdate.EndTime);
                double absvalue = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero);
                sliderProgress.Maximum = absvalue;
            });
         }

         # endregion sliderMediaPlayer
   ```

   > [!NOTE]
   > CoreDispatcher wordt gebruikt om wijzigingen aanbrengen in de UI-thread van niet-UI-Thread. In het geval van knelpunt op dispatcher-thread kunt ontwikkelaars gebruiken functie voor berichtverzending geleverd door de UI-element ze van plan bent om bij te werken.  Bijvoorbeeld:

   ```csharp
         await sliderProgress.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => { TimeSpan 

         timespan = new TimeSpan(adaptiveSourceStatusUpdate.EndTime); 
         double absvalue  = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero); 

         sliderProgress.Maximum = absvalue; }); 
   ```
6. Aan het einde van de **mediaElement_AdaptiveSourceStatusUpdated** methode, voeg de volgende code toe:

   ```csharp
         setSliderStartTime(args.StartTime);
         setSliderEndTime(args.EndTime);
   ```
7. Aan het einde van de **MediaOpened** methode, voeg de volgende code toe:

   ```csharp
         sliderProgress.StepFrequency = SliderFrequency(mediaElement.NaturalDuration.TimeSpan);
         sliderProgress.Width = mediaElement.Width;
         setupTimer();
   ```
8. Druk op **CTRL + S** op te slaan.

### <a name="to-compile-and-test-the-application"></a>Compileren en de toepassing testen

1. Druk op **F6** voor het compileren van het project. 
2. Druk op **F5** om de toepassing uit te voeren.
3. Aan de bovenkant van de toepassing, kunt u de standaard-URL voor Smooth Streaming of een andere invoeren. 
4. Klik op **bron instellen**. 
5. Test de schuifregelaar.

U kunt les 2 hebt voltooid.  In deze les kunt u een schuifregelaar toegevoegd aan de toepassing. 

## <a name="lesson-3-select-smooth-streaming-streams"></a>Les 3: Selecteer Streams Smooth Streaming
Smooth Streaming is in staat om inhoud te streamen met meerdere taal-audionummers die kunnen geselecteerd door de viewers worden.  In deze les gaat uitvoeren schakelt u viewers streams selecteren. In deze les bevat de volgende procedures:

1. De XAML-bestand wijzigen
2. Wijzigen van het codebestand
3. Samenstellen en testen van de toepassing

### <a name="to-modify-the-xaml-file"></a>Om de XAML-bestand te wijzigen

1. Klik in Solution Explorer met de rechtermuisknop op **MainPage.xaml**, en klik vervolgens op **Weergaveontwerper**.
2. Zoek &lt;Grid.RowDefinitions&gt;, en de RowDefinitions wijzigt, zodat ze ziet eruit als:

   ```xml
         <Grid.RowDefinitions>            
            <RowDefinition Height="20"/>
            <RowDefinition Height="50"/>
            <RowDefinition Height="100"/>
            <RowDefinition Height="80"/>
            <RowDefinition Height="50"/>
         </Grid.RowDefinitions>
   ```
3. In de &lt;raster&gt;&lt;/Grid&gt; tags, voeg de volgende code voor het definiëren van een besturingselement keuzelijst, zodat gebruikers kunnen zien van de lijst met beschikbare streams, en selecteert u stromen:

   ```xml
         <Grid Name="gridStreamAndBitrateSelection" Grid.Row="3">
            <Grid.RowDefinitions>
                <RowDefinition Height="300"/>
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="250*"/>
                <ColumnDefinition Width="250*"/>
            </Grid.ColumnDefinitions>
            <StackPanel Name="spStreamSelection" Grid.Row="1" Grid.Column="0">
                <StackPanel Orientation="Horizontal">
                    <TextBlock Name="tbAvailableStreams" Text="Available Streams:" FontSize="16" VerticalAlignment="Center"></TextBlock>
                    <Button Name="btnChangeStreams" Content="Submit" Click="btnChangeStream_Click"/>
                </StackPanel>
                <ListBox x:Name="lbAvailableStreams" Height="200" Width="200" Background="Transparent" HorizontalAlignment="Left" 
                    ScrollViewer.VerticalScrollMode="Enabled" ScrollViewer.VerticalScrollBarVisibility="Visible">
                    <ListBox.ItemTemplate>
                        <DataTemplate>
                            <CheckBox Content="{Binding Name}" IsChecked="{Binding isChecked, Mode=TwoWay}" />
                        </DataTemplate>
                    </ListBox.ItemTemplate>
                </ListBox>
            </StackPanel>
         </Grid>
   ```
4. Druk op **CTRL + S** de wijzigingen op te slaan.

### <a name="to-modify-the-code-behind-file"></a>Wijzigen van het codebestand

1. Klik in Solution Explorer met de rechtermuisknop op **MainPage.xaml**, en klik vervolgens op **Code weergeven**.
2. Voeg een nieuwe klasse toe binnen de naamruimte SSPlayer:

   ```csharp
        #region class Stream
   
        public class Stream
        {
            private IManifestStream stream;
            public bool isCheckedValue;
            public string name;
   
            public string Name
            {
                get { return name; }
                set { name = value; }
            }
   
            public IManifestStream ManifestStream
            {
                get { return stream; }
                set { stream = value; }
            }
   
            public bool isChecked
            {
                get { return isCheckedValue; }
                set
                {
                    // mMke the video stream always checked.
                    if (stream.Type == MediaStreamType.Video)
                    {
                        isCheckedValue = true;
                    }
                    else
                    {
                        isCheckedValue = value;
                    }
                }
            }
   
            public Stream(IManifestStream streamIn)
            {
                stream = streamIn;
                name = stream.Name;
            }
        }
        #endregion class Stream
   ```
3. Aan het begin van de klasse MainPage, de volgende variabele definities hebt toegevoegd:

   ```csharp
         private List<Stream> availableStreams;
         private List<Stream> availableAudioStreams;
         private List<Stream> availableTextStreams;
         private List<Stream> availableVideoStreams;
   ```
4. Voeg de volgende regio in de klasse MainPage:
   ```csharp
        #region stream selection
        ///<summary>
        ///Functionality to select streams from IManifestStream available streams
        /// </summary>
   
        // This function is called from the mediaElement_ManifestReady event handler 
        // to retrieve the streams and populate them to the local data members.
        public void getStreams(Manifest manifestObject)
        {
            availableStreams = new List<Stream>();
            availableVideoStreams = new List<Stream>();
            availableAudioStreams = new List<Stream>();
            availableTextStreams = new List<Stream>();
   
            try
            {
                for (int i = 0; i<manifestObject.AvailableStreams.Count; i++)
                {
                    Stream newStream = new Stream(manifestObject.AvailableStreams[i]);
                    newStream.isChecked = false;
   
                    //populate the stream lists based on the types
                    availableStreams.Add(newStream);
   
                    switch (newStream.ManifestStream.Type)
                    {
                        case MediaStreamType.Video:
                            availableVideoStreams.Add(newStream);
                            break;
                        case MediaStreamType.Audio:
                            availableAudioStreams.Add(newStream);
                            break;
                        case MediaStreamType.Text:
                            availableTextStreams.Add(newStream);
                            break;
                    }
   
                    // Select the default selected streams from the manifest.
                    for (int j = 0; j<manifestObject.SelectedStreams.Count; j++)
                    {
                        string selectedStreamName = manifestObject.SelectedStreams[j].Name;
                        if (selectedStreamName.Equals(newStream.Name))
                        {
                            newStream.isChecked = true;
                            break;
                        }
                    }
                }
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
   
        // This function set the list box ItemSource
        private async void refreshAvailableStreamsListBoxItemSource()
        {
            try
            {
                //update the stream check box list on the UI
                await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, ()
                    => { lbAvailableStreams.ItemsSource = availableStreams; });
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
   
        // This function creates a selected streams list
        private void createSelectedStreamsList(List<IManifestStream> selectedStreams)
        {
            bool isOneVideoSelected = false;
            bool isOneAudioSelected = false;
   
            // Only one video stream can be selected
            for (int j = 0; j<availableVideoStreams.Count; j++)
            {
                if (availableVideoStreams[j].isChecked && (!isOneVideoSelected))
                {
                    selectedStreams.Add(availableVideoStreams[j].ManifestStream);
                    isOneVideoSelected = true;
                }
            }
   
            // Select the first video stream from the list if no video stream is selected
            if (!isOneVideoSelected)
            {
                availableVideoStreams[0].isChecked = true;
                selectedStreams.Add(availableVideoStreams[0].ManifestStream);
            }
   
            // Only one audio stream can be selected
            for (int j = 0; j<availableAudioStreams.Count; j++)
            {
                if (availableAudioStreams[j].isChecked && (!isOneAudioSelected))
                {
                    selectedStreams.Add(availableAudioStreams[j].ManifestStream);
                    isOneAudioSelected = true;
                    txtStatus.Text = "The audio stream is changed to " + availableAudioStreams[j].ManifestStream.Name;
                }
            }
   
            // Select the first audio stream from the list if no audio steam is selected.
            if (!isOneAudioSelected)
            {
                availableAudioStreams[0].isChecked = true;
                selectedStreams.Add(availableAudioStreams[0].ManifestStream);
            }
   
            // Multiple text streams are supported.
            for (int j = 0; j < availableTextStreams.Count; j++)
            {
                if (availableTextStreams[j].isChecked)
                {
                    selectedStreams.Add(availableTextStreams[j].ManifestStream);
                }
            }
        }
   
        // Change streams on a smooth streaming presentation with multiple video streams.
        private async void changeStreams(List<IManifestStream> selectStreams)
        {
            try
            {
                IReadOnlyList<IStreamChangedResult> returnArgs =
                    await manifestObject.SelectStreamsAsync(selectStreams);
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
        #endregion stream selection
   ```
5. Zoek de methode mediaElement_ManifestReady, de volgende code aan het einde van de functie toevoegen:
   ```csharp
        getStreams(manifestObject);
        refreshAvailableStreamsListBoxItemSource();
   ```
    Dus als MediaElement manifest klaar is, de code wordt een lijst van de beschikbare stromen en vult de UI-keuzelijst met de lijst.
6. Ga naar de gebruikersinterface binnen de klasse MainPage knoppen klikt u op gebeurtenissen regio en voegt u de functiedefinitie van de volgende:
   ```csharp
        private void btnChangeStream_Click(object sender, RoutedEventArgs e)
        {
            List<IManifestStream> selectedStreams = new List<IManifestStream>();
   
            // Create a list of the selected streams
            createSelectedStreamsList(selectedStreams);
   
            // Change streams on the presentation
            changeStreams(selectedStreams);
        }
   ```

### <a name="to-compile-and-test-the-application"></a>Compileren en de toepassing testen

1. Druk op **F6** voor het compileren van het project. 
2. Druk op **F5** om de toepassing uit te voeren.
3. Aan de bovenkant van de toepassing, kunt u de standaard-URL voor Smooth Streaming of een andere invoeren. 
4. Klik op **bron instellen**. 
5. De standaardtaal is audio_eng. Probeer om over te schakelen tussen audio_eng en audio_es. Telkens wanneer u een nieuwe stroom selecteert, moet u klikken op de knop verzenden.

Les 3 is voltooid.  In deze les gaat uitvoeren, moet u de functionaliteit om te kiezen streams toevoegen.

## <a name="lesson-4-select-smooth-streaming-tracks"></a>Les 4: Selecteer Smooth Streaming-nummers

Een Smooth Streaming-presentatie kan bevatten meerdere videobestanden gecodeerd met verschillende kwaliteitsniveaus (bitsnelheden) en oplossingen. In deze les gaat uitvoeren schakelt u gebruikers de nummers selecteren. In deze les bevat de volgende procedures:

1. De XAML-bestand wijzigen
2. Wijzigen van het codebestand
3. Samenstellen en testen van de toepassing

### <a name="to-modify-the-xaml-file"></a>Om de XAML-bestand te wijzigen

1. Klik in Solution Explorer met de rechtermuisknop op **MainPage.xaml**, en klik vervolgens op **Weergaveontwerper**.
2. Zoek de &lt;raster&gt; tag met de naam van de **gridStreamAndBitrateSelection**, de volgende code aan het einde van de tag toevoegen:
   ```xml
         <StackPanel Name="spBitRateSelection" Grid.Row="1" Grid.Column="1">
         <StackPanel Orientation="Horizontal">
             <TextBlock Name="tbBitRate" Text="Available Bitrates:" FontSize="16" VerticalAlignment="Center"/>
             <Button Name="btnChangeTracks" Content="Submit" Click="btnChangeTrack_Click" />
         </StackPanel>
         <ListBox x:Name="lbAvailableVideoTracks" Height="200" Width="200" Background="Transparent" HorizontalAlignment="Left" 
                  ScrollViewer.VerticalScrollMode="Enabled" ScrollViewer.VerticalScrollBarVisibility="Visible">
             <ListBox.ItemTemplate>
                 <DataTemplate>
                     <CheckBox Content="{Binding Bitrate}" IsChecked="{Binding isChecked, Mode=TwoWay}"/>
                 </DataTemplate>
             </ListBox.ItemTemplate>
         </ListBox>
         </StackPanel>
   ```
3. Druk op **CTRL + S** de wijzigingen op te slaan

### <a name="to-modify-the-code-behind-file"></a>Wijzigen van het codebestand

1. Klik in Solution Explorer met de rechtermuisknop op **MainPage.xaml**, en klik vervolgens op **Code weergeven**.
2. Voeg een nieuwe klasse toe binnen de naamruimte SSPlayer:
   ```csharp
        #region class Track
        public class Track
        {
            private IManifestTrack trackInfo;
            public string _bitrate;
            public bool isCheckedValue;
   
            public IManifestTrack TrackInfo
            {
                get { return trackInfo; }
                set { trackInfo = value; }
            }
   
            public string Bitrate
            {
                get { return _bitrate; }
                set { _bitrate = value; }
            }
   
            public bool isChecked
            {
                get { return isCheckedValue; }
                set
                {
                    isCheckedValue = value;
                }
            }
   
            public Track(IManifestTrack trackInfoIn)
            {
                trackInfo = trackInfoIn;
                _bitrate = trackInfoIn.Bitrate.ToString();
            }
            //public Track() { }
        }
        #endregion class Track
   ```
3. Aan het begin van de klasse MainPage, de volgende variabele definities hebt toegevoegd:
   ```csharp
        private List<Track> availableTracks;
   ```
4. Voeg de volgende regio in de klasse MainPage:
   ```csharp
        #region track selection
        /// <summary>
        /// Functionality to select video streams
        /// </summary>
   
        /// This Function gets the tracks for the selected video stream
        public void getTracks(Manifest manifestObject)
        {
            availableTracks = new List<Track>();
   
            IManifestStream videoStream = getVideoStream();
            IReadOnlyList<IManifestTrack> availableTracksLocal = videoStream.AvailableTracks;
            IReadOnlyList<IManifestTrack> selectedTracksLocal = videoStream.SelectedTracks;
   
            try
            {
                for (int i = 0; i < availableTracksLocal.Count; i++)
                {
                    Track thisTrack = new Track(availableTracksLocal[i]);
                    thisTrack.isChecked = true;
   
                    for (int j = 0; j < selectedTracksLocal.Count; j++)
                    {
                        string selectedTrackName = selectedTracksLocal[j].Bitrate.ToString();
                        if (selectedTrackName.Equals(thisTrack.Bitrate))
                        {
                            thisTrack.isChecked = true;
                            break;
                        }
                    }
                    availableTracks.Add(thisTrack);
                }
            }
            catch (Exception e)
            {
                txtStatus.Text = e.Message;
            }
        }
   
        // This function gets the video stream that is playing
        private IManifestStream getVideoStream()
        {
            IManifestStream videoStream = null;
            for (int i = 0; i < manifestObject.SelectedStreams.Count; i++)
            {
                if (manifestObject.SelectedStreams[i].Type == MediaStreamType.Video)
                {
                    videoStream = manifestObject.SelectedStreams[i];
                    break;
                }
            }
            return videoStream;
        }
   
        // This function set the UI list box control ItemSource
        private async void refreshAvailableTracksListBoxItemSource()
        {
            try
            {
                // Update the track check box list on the UI 
                await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, ()
                    => { lbAvailableVideoTracks.ItemsSource = availableTracks; });
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }        
        }
   
        // This function creates a list of the selected tracks.
        private void createSelectedTracksList(List<IManifestTrack> selectedTracks)
        {
            // Create a list of selected tracks
            for (int j = 0; j < availableTracks.Count; j++)
            {
                if (availableTracks[j].isCheckedValue == true)
                {
                    selectedTracks.Add(availableTracks[j].TrackInfo);
                }
            }
        }
   
        // This function selects the tracks based on user selection 
        private void changeTracks(List<IManifestTrack> selectedTracks)
        {
            IManifestStream videoStream = getVideoStream();
            try
            {
                videoStream.SelectTracks(selectedTracks);
            }
            catch (Exception ex)
            {
                txtStatus.Text = ex.Message;
            }
        }
        #endregion track selection
   ```
5. Zoek de methode mediaElement_ManifestReady, de volgende code aan het einde van de functie toevoegen:
   ```csharp
         getTracks(manifestObject);
         refreshAvailableTracksListBoxItemSource();
   ```
6. Ga naar de gebruikersinterface binnen de klasse MainPage knoppen klikt u op gebeurtenissen regio en voegt u de functiedefinitie van de volgende:
   ```csharp
         private void btnChangeStream_Click(object sender, RoutedEventArgs e)
         {
            List<IManifestStream> selectedStreams = new List<IManifestStream>();

            // Create a list of the selected streams
            createSelectedStreamsList(selectedStreams);

            // Change streams on the presentation
            changeStreams(selectedStreams);
         }
   ```
   
### <a name="to-compile-and-test-the-application"></a>Compileren en de toepassing testen

1. Druk op **F6** voor het compileren van het project. 
2. Druk op **F5** om de toepassing uit te voeren.
3. Aan de bovenkant van de toepassing, kunt u de standaard-URL voor Smooth Streaming of een andere invoeren. 
4. Klik op **bron instellen**. 
5. Standaard worden alle van de nummers van de stroom van de video geselecteerd. Als u wilt experimenteren de bits-tarief wordt gewijzigd, kunt u de laagste bitsnelheid beschikbaar selecteren en selecteer vervolgens de hoogste bitsnelheid die beschikbaar is. Na elke wijziging moet u klikken op verzenden.  Hier ziet u de wijzigingen van de kwaliteit van video.

Les 4 is voltooid.  In deze les gaat uitvoeren voegt u de functionaliteit om te kiezen sporen te wissen.

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="other-resources"></a>Andere bronnen:
* [Over het bouwen van een toepassing met Smooth Streaming Windows 8 JavaScript met geavanceerde functies](https://blogs.iis.net/cenkd/archive/2012/08/10/how-to-build-a-smooth-streaming-windows-8-javascript-application-with-advanced-features.aspx)
* [Technisch overzicht van Smooth Streaming](https://www.iis.net/learn/media/on-demand-smooth-streaming/smooth-streaming-technical-overview)

[PlayerApplication]: ./media/media-services-build-smooth-streaming-apps/SSClientWin8-1.png
[CodeViewPic]: ./media/media-services-build-smooth-streaming-apps/SSClientWin8-2.png

