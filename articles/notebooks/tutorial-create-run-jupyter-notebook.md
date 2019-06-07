---
title: Zelfstudie - maken en uitvoeren van een Jupyter-notebook op Azure
description: Hoe u een run van een Jupyter-notebook in Azure-notitieblokken waartoe ziet u het proces van het lineaire regressie in de gegevenswetenschap maakt.
services: app-service
documentationcenter: ''
author: kraigb
manager: douge
ms.assetid: 65bbb5fe-9939-4e8e-8f5b-c197d4be142a
ms.service: azure-notebooks
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/11/2019
ms.author: kraigb
ms.openlocfilehash: 09d4038e705fb3bc4ff2c82daf5dc4c07f346f94
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2019
ms.locfileid: "66751753"
---
# <a name="tutorial-create-and-run-a-jupyter-notebook-with-python"></a>Zelfstudie: maken en uitvoeren van een Jupyter-notebook met Python

In deze zelfstudie begeleidt u bij het proces van het gebruik van Azure-notitieblokken maken van een volledige Jupyter-notebook die eenvoudige lineaire regressie laat zien. In deze zelfstudie maakt vertrouwd u raken met de Jupyter-notebook gebruikersinterface, waaronder het maken van andere cellen, het uitvoeren van de cellen en presenteert de notebook als een diavoorstelling.

De voltooide notebook kunt u vinden op [GitHub - voorbeelden voor Azure-notitieblokken](https://github.com/Microsoft/AzureNotebooks/tree/master/Samples/Linear%20Regression%20-%20Cricket%20Chirps). In deze zelfstudie begint echter met een nieuw project en de laptop van een leeg, zodat u te maken krijgen kunt met stap te maken.

## <a name="create-the-project"></a>Het project maken

1. Ga naar [Azure notitieblokken](https://notebooks.azure.com) en meld u aan. (Zie voor meer informatie, [-Snelstart: aanmelden bij Azure-notitieblokken](quickstart-sign-in-azure-notebooks.md)).

1. Selecteer in de pagina van uw openbare profiel **Mijn projecten** aan de bovenkant van de pagina:

    ![Mijn projecten koppeling boven aan het browservenster](media/quickstarts/my-projects-link.png)

1. Op de **Mijn projecten** weergeeft, schakelt **+ nieuw Project** (sneltoets: n); de knop wordt mogelijk weergegeven alleen als **+** als het browservenster smal is:

    ![Nieuw Project-opdracht op de pagina Mijn projecten](media/quickstarts/new-project-command.png)

1. In de **nieuw Project maken** pop-upvenster dat wordt weergegeven, invoeren of stel de volgende details en selecteer vervolgens **maken**:

    - **Naam van het project**: Voorbeeld van de lineaire regressie - Cricket lijken
    - **Project-ID**: lineaire regressie-voorbeeld
    - **Openbare project**: (uitgeschakeld)
    - **Maken van een README.md**: (uitgeschakeld)

1. Na enkele ogenblikken navigeert Azure notitieblokken u naar het nieuwe project.

## <a name="create-the-data-file"></a>Het gegevensbestand maken

Het lineaire regressiemodel dat u in het notitieblok maakt tekent u gegevens uit een bestand in uw project met de naam *cricket_chirps.csv*. U kunt dit bestand maken door te kopiëren vanuit [GitHub - voorbeelden voor Azure-notitieblokken](https://github.com/Microsoft/AzureNotebooks/tree/master/Samples/Linear%20Regression%20-%20Cricket%20Chirps), of door de gegevens rechtstreeks uit te voeren. De volgende secties worden beide methoden.

### <a name="upload-the-data-file"></a>Het bestand uploaden

1. Selecteer op uw projectdashboard in Azure-notitieblokken **uploaden** > **van URL**
1. Voer in het pop-upvenster de volgende URL in **bestands-URL** en *cricket_chirps.csv* in **bestandsnaam**en selecteer vervolgens **gedaan**.

    ```url
    https://raw.githubusercontent.com/Microsoft/AzureNotebooks/master/Samples/Linear%20Regression%20-%20Cricket%20Chirps/cricket_chirps.csv
    ```

1. De *cricket_chirps.csv* bestand wordt nu weergegeven in de lijst met bestanden van uw project:

    ![Nieuw gemaakte CSV-bestand weergegeven in de lijst met project-bestanden](media/tutorial/csv-file-in-project.png)

### <a name="create-a-file-from-scratch"></a>Maak een bestand maken

1. Selecteer op uw projectdashboard in Azure-notitieblokken **+ nieuw** > **leeg bestand**
1. Een veld wordt weergegeven in de lijst met bestanden van het project. ENTER *cricket_chirps.csv* en druk op Enter.
1. Met de rechtermuisknop op *cricket_chirps.csv* en selecteer **bestand bewerken**.
1. Voer de volgende gegevens in de editor die wordt weergegeven:

    ```csv
    Chirps/Minute,Temperature
    20,88.6
    16,71.6
    19.8,93.3
    18.4,84.3
    17.1,80.6
    15.5,75.2
    14.7,69.7
    17.1,82
    15.4,69.4
    16.2,83.3
    15,79.6
    17.2,82.6
    16,80.6
    17,83.5
    14.4,76.3
    ```

1. Selecteer **bestand opslaan** Sla het bestand op en gaat u terug naar het projectdashboard.

## <a name="install-project-level-packages"></a>Project-niveau pakketten installeren

Binnen een laptop, kunt u altijd opdrachten zoals gebruiken `!pip install` in een codecel om vereiste pakketten te installeren. Deze opdrachten worden echter uitgevoerd telkens als u van het notitieblok code cellen uitvoeren, en kan lang duren. Daarom kunt u in plaats daarvan installeren pakketten op het project op met een `requirements.txt` bestand.

1. Gebruik de procedure beschreven in [maken van een bestand helemaal](#create-a-file-from-scratch) te maken van een bestand met de naam `requirements.txt` met de volgende inhoud:

    ```text
    matplotlib==3.0.0
    numpy==1.15.0
    pandas==0.23.4
    scikit-learn==0.20.0
    ```

    U kunt ook uploaden een `requirements.txt` bestand vanaf uw lokale computer als u liever, zoals wordt beschreven op [uploaden van het gegevensbestand](#upload-the-data-file).

1. Selecteer in het projectdashboard **projectinstellingen**.
1. Selecteer in het pop-upvenster dat wordt weergegeven, de **omgeving** tabblad, en selecteer vervolgens **+ toevoegen**.
1. In de eerste vervolgkeuzelijst besturingselement (bewerking) onder **omgeving installatiestappen uit**, kiest u **Requirements.txt**.
1. Kies in het tweede vervolgkeuzelijst besturingselement (bestandsnaam), *requirements.txt* (het bestand dat u hebt gemaakt).
1. Kies in het derde vervolgkeuzelijst besturingselement (de Python-versie), **Python versie 3.6**.
1. Selecteer **Opslaan**.

![Het tabblad van het Project instellingen omgeving op te geven van een bestand requirements.txt](media/tutorial/tutorial-requirements-txt.png)

Met deze stap setup in plaats wordt een laptop die u in het project uitvoert uitgevoerd in een omgeving waar deze pakketten zijn geïnstalleerd.

## <a name="create-and-run-a-notebook"></a>Een notebook maken en uitvoeren

Met het gereed gegevensbestand en de project-omgeving is ingesteld, kunt u nu maakt en open het notitieblok.

1. Selecteer in het projectdashboard **+ nieuw** > **Notebook**.
1. Voer in het pop-upvenster *lineaire regressie-voorbeeld - Cricket Chirps.ipynb* voor **itemnaam**, kiest u **Python 3.6** voor de taal, selecteert u vervolgens **nieuw**.
1. Nadat de nieuwe notebook op de lijst met bestanden wordt weergegeven, selecteert u dit om te beginnen de notebook. Een nieuw browsertabblad wordt automatisch geopend.
1. Omdat u hebt een *requirements.txt* bestand in de omgevingsinstellingen voor ziet u het bericht, 'wachten op de container om te voltooien wordt voorbereid." U kunt selecteren **OK** om te sluiten van het bericht en doorgaan met werken in het notitieblok; u cellen met een code echter niet uitvoeren totdat de omgeving volledig is ingesteld.
1. De notebook wordt geopend in de Jupyter-interface met een één lege codecel als standaardwaarde.

    [![Weergave van een nieuwe notebook in Azure-notitieblokken](media/tutorial/tutorial-new-notebook.png)](media/tutorial/tutorial-new-notebook.png#lightbox)

## <a name="tour-the-notebook-interface"></a>Maakt een rondleiding door de notebook-interface

Met de laptop die wordt uitgevoerd, kunt u code en Markdown cellen die cellen uitvoeren en beheren van de werking van de notebook toevoegen. Eerst, het is echter waard een paar minuten om vertrouwd te raken met de interface. Voor volledige documentatie, selecteert u de **Help** > **Notebook Help** -opdracht.

Aan de bovenkant van het venster ziet u de volgende items:

(A) de naam van uw laptop, die u door te klikken bewerken kunt.
(B) de knoppen om te navigeren naar het project met en het dashboard van uw projecten, waarmee nieuwe tabbladen in uw browser worden geopend.
(C) een menu met de opdrachten voor het werken met de notebook.
(D) een werkbalk met snelkoppelingen voor algemene bewerkingen.
(E) de bewerken canvas met cellen.
(F) geeft aan of de notebook vertrouwd wordt (de standaardwaarde is **niet-vertrouwd**).
(G) de kernel gebruikt voor het uitvoeren van het notitieblok, samen met een indicator voor netwerkactiviteit.

[![Primaire gebruikersinterface gebieden van de Jupyter-interface](media/tutorial/tutorial-notebook-ui.png)](media/tutorial/tutorial-notebook-ui.png#lightbox)

Jupyter biedt een ingebouwde rondleiding door de primaire UI-elementen. Selecteer eerst de rondleiding door de **Help** > **rondleiding door de Interface van de gebruiker** opdracht en te klikken in het pop-upvensters.

De groepen van opdrachten zijn als volgt:

| Menu | Description |
| --- | --- |
| File | Opdrachten voor het beheren van de notebook-bestand, met inbegrip van opdrachten voor het maken en kopiëren van notebooks, een afdrukvoorbeeld weergeven en downloaden van het notitieblok in verschillende indelingen. |
| Bewerken | Gangbare opdrachten Knippen, kopiëren en cellen plakken, zoeken en vervangen door waarden, cel bijlagen beheren en het invoegen van afbeeldingen.  |
| Weergave | Opdrachten voor het beheren van de zichtbaarheid van de verschillende onderdelen van de Jupyter-UI. |
| Invoegen | Opdrachten voor het invoegen van een nieuwe cel boven of onder de huidige cel. U gebruikt deze opdrachten regelmatig bij het maken van een laptop. |
| Cel | De verschillende **uitvoeren** opdrachten een of meer cellen in verschillende combinaties worden uitgevoerd. De **Type Legendacel** opdrachten wijzigt u het type van een cel tussen **Code**, **Markdown**, en **onbewerkte NBConvert** (tekst zonder opmaak). De **huidige uitvoer** en **alle uitvoer** opdrachten bepalen hoe de uitvoer van de code uitvoeren wordt weergegeven en een opdracht om te wissen van alle uitvoer op te nemen. |
| Kernel | Opdrachten voor het beheren hoe code wordt uitgevoerd in de kernel, samen met **wijziging kernel** om de taal of Python-versie die wordt gebruikt om uit te voeren van de notebook te wijzigen. |
| Gegevens | Opdrachten om te uploaden en downloaden van bestanden van het project of de sessie. Zie [werken met gegevens projectbestanden](work-with-project-data-files.md) |
| widgets | Opdrachten voor het beheren van [Jupyter Widgets](https://ipywidgets.readthedocs.io/en/stable/examples/Widget%20Basics.html), die voorzien in extra functies voor visualisatie, toewijzing en plotten.|
| Help | Opdrachten die hulp documentatie voor de Jupyter-interface en. |

De meeste van de opdrachten op de werkbalk hebben gelijkwaardige opdrachten. Een uitzondering is **Enter/bewerken STIJGEN diavoorstelling**, die wordt beschreven op [Share- en aanwezig notitieblokken](present-jupyter-notebooks-slideshow.md).

U een aantal van deze opdrachten gebruiken als u de waarden voor de notebook in de volgende secties.

## <a name="create-a-markdown-cell"></a>Een cel Markdown maken

1. Klik in de eerste lege cel wordt weergegeven op het canvas notebook. Een cel is standaard een **Code** type, wat betekent is ontworpen dat om uitvoerbaar code voor de geselecteerde kernel bevatten (Python, R, of F#). Het huidige type wordt weergegeven in het type vervolgkeuzelijst op de werkbalk:

    ![Cel type werkbalk vervolgkeuzelijst](media/tutorial/tutorial-cell-type-drop-down.png)

1. Wijzigen van het celtype **Markdown** met behulp van de werkbalk vervolgkeuzelijst; u kunt ook gebruik van de **cel** > **Type Legendacel**  >   **Markdown** -opdracht:

    ![Cel type opdracht](media/tutorial/tutorial-cell-type-menu.png)

1. Klik in de cel wilt bewerken en voer de volgende Markdown:

    ```markdown
    # Example Linear Regression

    This notebook contains a walkthrough of a simple linear regression. The data, obtained from [college.cengage.com](https://college.cengage.com/mathematics/brase/understandable_statistics/7e/students/datasets/slr/frames/frame.html), relates the rate of cricket chirps to temperature from *The Song of Insects*, by Dr. G. W. Pierce, Harvard College Press.

    In this example we're using the count of chirps per minute as the independent varible to then predict the dependent variable, temperature. In short, we're using a little data science to make ourselves a cricket thermometer. (You could also reverse the data and use temperature to predict the number of chirps, but it's more fun to use crickets as the thermometer itself!)

    The methods shown in this example follow what's presented in the Udemy course, [Machine Learning A to Z](https://www.udemy.com/machinelearning/learn/v4/).

    A lovely aspect of Notebooks is that you can use Markdown cells to explain what the code is doing rather than code comments. There are several benefits to doing so:

    - Markdown allows for richer text formatting, like *italics*, **bold**, `inline code`, hyperlinks, and headers.
    - Markdown cells automatically word wrap whereas code cells do not. Code comments typically use explicit line breaks for formatting, but that's not necessary in Markdown.
    - Using Markdown cells makes it easier to run the Notebook as a slide show.
    - Markdown cells help you remove lengthy comments from the code, making the code easier to scan.

    When you run a code cell, Jupyter executes the code; when you run a Markdown cell, Jupyter renders all the formatting into text that's suitable for presentation.
    ```

1. Voor het renderen van de Markdown in HTML-code voor de browser, selecteer de **uitvoeren** opdracht op de werkbalk of gebruik de **cel** > **uitvoeren cellen** opdracht. De Markdown-code voor het formatteren en koppelingen worden nu weergegeven als u verwacht in een browser.

1. Wanneer u de laatste cel in de notebook uitvoert, maakt Jupyter automatisch een nieuwe cel onder het account dat u hebt uitgevoerd. Meer Markdown in deze cel geplaatst door de stappen in deze sectie met de volgende Markdown te herhalen:

    ```markdown
    ## Install packages using pip or conda

    Because the code in your notebook likely uses some Python packages, you need to make sure the Notebook environment contains those packages. You can do this directly within the notebook in a code block that contains the appropriate pip or conda commands prefixed by `!`:

    \```
    !pip install <pkg name>

    !conda install <pkg name> -y
    \```
    ```

1. Als u wilt de Markdown opnieuw bewerken, dubbelklikt u op in de gerenderde cel. Voor het renderen van HTML opnieuw nadat u hebt wijzigingen aangebracht, voer de cel.

## <a name="create-a-code-cell-with-commands"></a>Maken van een codecel met opdrachten

Als de vorige Markdown cel is uitgelegd, kunt u opdrachten rechtstreeks in het notitieblok opnemen. Opdrachten kunt u pakketten installeren, voert u curl of wget ophalen van gegevens of iets anders. Jupyter-notebooks effectief uitgevoerd binnen een virtuele Linux-machine, zodat u beschikt over de volledige Linux-opdracht om te werken met instellen.

1. Voer de onderstaande opdrachten in de codecel die werden weergegeven nadat u hebt gebruikt **uitvoeren** op de vorige cel van Markdown. Als u een nieuwe cel niet ziet, maakt u een met **invoegen** > **cel onder** of gebruik de **+** op de werkbalk.

    ```bash
    !pip install numpy
    !pip install matplotlib
    !pip install pandas
    !pip install sklearn
    ```

1. Voordat u de cel uitvoert, maakt u een nieuwe cel met de **+** knop op de werkbalk en voer de volgende uitleg instellen op Markdown:

    ```markdown
    Note that when you run a code block that contains install commands, and also those with `import` statements, it make take the notebooks a little time to complete the task. To the left of the code block you see `In [*]` to indicate that execution is happening. The Notebook's kernel on the upper right also shows a filled-in circle to indicate "busy."
    ```

1. Selecteer de **cel** > **alles uitvoeren** opdracht voor het uitvoeren van alle cellen in de notebook. U ziet dat de cellen met Markdown weergegeven als HTML, en de opdracht uitvoeren in de kernel en bekijk de kernel-indicator zoals beschreven in de Markdown zelf:

    ![Bezet indicator voor de notebook-kernel](media/tutorial/tutorial-kernel-busy.png)

1. Ook duurt het even de tijd voor alle van de `pip install` opdrachten uit te voeren, en omdat u al deze pakketten in de project-omgeving is geïnstalleerd (en omdat ze ook in Azure-notitieblokken standaard opgenomen zijn), ziet u veel berichten die worden gelezen, "vereiste al voldaan." Alle die uitvoer mag visueel leidt, dus selecteer die verkopen (met één klik) en gebruik vervolgens de **cel** > **cel uitvoer** > **in-/ uitschakelen**verbergen van de uitvoer. U kunt ook de **wissen** opdracht op die dezelfde submenu volledig verwijderen van de uitvoer.

    De **in-/ uitschakelen** opdracht wordt ingeschakeld, wordt alleen de meest recente uitvoer van de cel; als u de cel opnieuw uitvoert, de uitvoer wordt opnieuw weergegeven.

1. Omdat de pakketten zijn geïnstalleerd in de project-omgeving, opmerkingen bij de `! pip install` opdrachten met behulp van `#`; deze manier ze kunnen blijven in het notitieblok als instructievideo materiaal, maar wordt niet elk gewenst moment om uit te voeren en wordt niet onnodig uitvoer produceren. In dit geval blijven de opmerkingen opdrachten in het notitieblok geeft ook aan van het notitieblok afhankelijkheden.

    ```bash
    # !pip install numpy
    # !pip install matplotlib
    # !pip install pandas
    # !pip install sklearn
    ```

## <a name="create-the-remaining-cells"></a>De resterende cellen maken

Om in te vullen van de rest van het notitieblok, maakt u vervolgens een reeks van Markdown en de code cellen. Voor elke cel in de hieronder vermelde eerst maken van de nieuwe cel, klikt u vervolgens stelt u het type en plak de inhoud.

U kunt wachten met het uitvoeren van het notitieblok nadat u elke cel hebt gemaakt, is maar het interessant om uit te voeren van elke cel als u dit hebt gemaakt. Niet alle cellen weergeven uitvoer. Als u eventuele fouten niet ziet, wordt ervan uitgegaan dat de cel is normaal uitgevoerd.

Elke codecel, is afhankelijk van de code die wordt uitgevoerd in de vorige cellen en als u niet een van de cellen uit te voeren, hoger cellen kunnen leiden tot fouten. Als u vindt dat u vergeten bent om uit te voeren van een cel, kunt u proberen de **cel** > **alle uitgevoerd boven** voordat u de huidige cel uitvoert.

Als er onverwachte resultaten (die u waarschijnlijk wordt!), moet u controleren dat elke cel is ingesteld op 'Code' of 'Markdown' indien nodig. Bijvoorbeeld, een 'Ongeldige syntaxis'-fout treedt meestal op wanneer u Markdown in de codecel hebt ingevoerd.

1. Cel markdown:

    ```markdown
    ## Import packages and prepare the dataset

    In this example we're using numpy, pandas, and matplotlib. Data is in the file cricket_chirps.csv. Because this file is in the same project as this present Notebook, we can just load it using a relative pathname.
    ```

1. Codecel; Wanneer uitvoert, ziet de inhoud van de tabel als uitvoer. U kunt de uitvoer onderdrukt door het commentaarteken de `print` instructie.

    ```python
    import numpy as np
    import pandas as pd

    dataset = pd.read_csv('cricket_chirps.csv')
    print(dataset)
    X = dataset.iloc[:, :-1].values  # Matrix of independent variables -- remove the last column in this data set
    y = dataset.iloc[:, 1].values    # Matrix of dependent variables -- just the last column (1 == 2nd column)
    ```

    > [!Note]
    > Ziet u mogelijk waarschuwingen van deze code over 'numpy.dtype grootte gewijzigd'; de waarschuwingen kunnen worden genegeerd.

1. Cel markdown:

    ```markdown
    Next, split the dataset into a Training set (2/3rds) and Test set (1/3rd). We don't need to do any feature scaling because there is only one column of independent variables, and packages typically do scaling for you.
    ```

1. Codecel; Wanneer uitvoert, heeft deze cel geen uitvoer.

    ```python
    from sklearn.model_selection import train_test_split

    X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 1/3, random_state = 0)
    ```

1. Cel markdown:

    ```markdown
    ## Fit the data to the training set

    "Fitting" the data to a training set means making the line that describes the relationship between the independent and the dependent variables. With a simple data set like we're using here, you can visualize the line on a simple x-y plot: the x-axis is the independent variable (chirp count in this example), and the y-axis is the independent variable (temperature). Fitting the data means plotting all the points in the training set, then drawing the best-fit line through that data.

    With two independent variables you can imagine a three-dimensional plot with a line fitted to the data. At three or more independent variables, however, it's no longer easy to visualize the fit, but you get the idea. In the end, it's all just mathematics, which a computer can handle easily without having to form a mental picture!

    The regressor's `fit` method here creates the line, which algebraically is of the form `y = x*b1 + b0`, where b1 is the coefficient or slope of the line (which you can get to through `regressor.coef_`), and b0 is the intercept of the line at x=0 (which you can get to through `regressor.intercept_`).
    ```

1. Codecel; Wanneer uitvoeren, deze cel toont de uitvoer van de `LinearRegression(copy_X=True, fit_intercept=True, n_jobs=None,normalize=False)`.

    ```python
    from sklearn.linear_model import LinearRegression

    regressor = LinearRegression()    # This object is the regressor, that does the regression
    regressor.fit(X_train, y_train)   # Provide training data so the machine can learn to predict using a learned model.
    ```

1. Cel markdown:

    ```markdown
    ## Predict the results

    With the regressor in hand, we can predict the test set results using its `predict` method. That method takes a vector of independent variables for which you want predictions.

    Because the regressor is fit to the data by virtue of `coef_` and `intercept_`, a prediction is the result of `coef_ * x + intercept_`. (Indeed, `predict(0)` returns `intercept_` and `predict(1)` returns `intercept_ + coef_`.)

    In the code, the `y_test` matrix (from when we split the set) contains the real observations. `y_pred` assigned here contains the predictions for the same `X_test` inputs. It's not expected that the test or training points exactly fit the regression; the regression is trying to find the model that we can use to make predictions with new observations of the independent variables.
    ```

1. Codecel; Wanneer uitvoeren, deze cel toont resultaten, zoals `[79.49588055 75.98873911 77.87719989 80.03544077 75.17939878]`.

    ```python
    y_pred = regressor.predict(X_test)
    print(y_pred)
    ```

1. Cel markdown:

    ```markdown
    It's interesting to think that all the "predictions" we use in daily life, like weather forecasts, are just regression models of some sort working with various data sets. Those models are much more complicated than what's shown here, but the idea is the same.

    Knowing how predictions work help us understand that the actual observations we would collect in the moment will always be somewhat off from the predictions: the predictions fit exactly to the model, whereas the observations typically won't.

    Of course, such systems feed new observations back into the dataset to continually improve the model, meaning that predictions should get more accurate over time.

    The challenge is determining what data to actually use. For example, with weather, how far back in time do you go? How have weather patterns been changing decade by decade? In any case, something like weather predictions will be doing things hour by hour, day by day, for things like temperature, precipitation, winds, cloud cover, etc. Radar and other observations are of course fed into the model and the predictions are reduced to mathematics.
    ```

1. Cel markdown:

    ```markdown
    ## Visualize the results

    The following code generates a plot: green dots are training data, red dots are test data, blue dots are predictions. Gray line is the regression itself. You see that all the blue dots are exactly on the line, as they should be, because the predictions exactly fit the model (the line).
    ```

1. Codecel; Wanneer uitvoert, produceert deze cel grafische getekend. Als u niet hoeveel tijd plot de eerste zien (en in plaats daarvan Zie 'Afbeelding grootte 640 x 480 met 1 assen'), voer de cel opnieuw uit.

    ```python
    import matplotlib.pyplot as plt

    plt.scatter(X_train, y_train, color = 'green')
    plt.scatter(X_test, y_test, color = 'red')   
    plt.scatter(X_test, y_pred, color = 'blue')  # The predicted temperatures of the same X_test input.
    plt.plot(X_train, regressor.predict(X_train), color = 'gray')
    plt.title('Temperature based on chirp count')
    plt.xlabel('Chirps/minute')
    plt.ylabel('Temperature')
    plt.show()
    ```

    ![Grafiek-uitvoer van de code matplotlib](media/tutorial/tutorial-plot-output.png)

1. Cel markdown:

    ```markdown
    ## Closing comments

    At the end of the day, when you create a model, you use training data. Then you start feeding test data (real observations) to see how well the model actually works. You may find that the model is a little inaccurate over time, in which case you retrain the model with some new data. Retraining the model means you're creating a new fit line that's used for predictions.

    This regression can be used to examine the variability of the relationship between temperature and chirp count. Of course, if the model proves too inaccurate (that is, the predictions aren't very good), then it suggests that we might need to introduce more independent variables like humidity, time of year, latitude, amount of moonlight, and so on. If you have such data, you can do separate lines regressions for each independent variable, and then do multiple regressions with combinations of independent variables. 

    Again, once you are working with more than one or two independent variables, it's much easier to use machine learning to crunch the numbers than to try to visualize it graphically.
    ```

## <a name="clear-outputs-and-rerun-all-cells"></a>Schakel uitvoer en alle cellen opnieuw uitvoeren

Nadat u de stappen in de vorige sectie voor het vullen van het hele notitieblok, kunt u zowel een stukje code uitvoeren in de context van een volledige zelfstudie over het lineaire regressie hebt gemaakt. Deze direct combinatie van code en de tekst is een van de grote voordelen van notitieblokken.

Probeer nu opnieuw uit te voeren de hele laptop:

1. Alle aan de kernel-sessiegegevens wissen en alle cellen uitvoer door te selecteren **Kernel** >  **& wissen uitvoer opnieuw**. Met deze opdracht is altijd goed om uit te voeren wanneer u klaar bent met een laptop, om dit te controleren dat u eventuele vreemd afhankelijkheden tussen de cellen van de code nog niet hebt gemaakt.

1. Voer nogmaals uit de notebook met **cel** > **alles uitvoeren**. U ziet dat de kernel-indicator wordt ingevuld terwijl de code wordt uitgevoerd.

    Hebt u geen code die wordt uitgevoerd te lang of anders vastloopt, kunt u de kernel stoppen met het **Kernel** > **Interrupt** opdracht.

1. Blader door de notebook Bekijk de resultaten. (Als de grafiek niet wordt opnieuw weergegeven, opnieuw uitvoeren die cel.)

## <a name="save-halt-and-close-the-notebook"></a>Opslaan en sluiten van de notebook stoppen

Tijdens de tijd die u een laptop bewerkt, bespaart u de huidige status met het **bestand** > **opslaan en controlepunt** opdracht of het opslaan op de werkbalk. Een "controlepunt", maakt u een momentopname die u kunt teruggaan naar op elk gewenst moment tijdens de sessie. Controlepunten zodat u kunt een reeks experimentele wijzigingen aanbrengen en als deze wijzigingen niet werkt, kunt u alleen terugkeren naar een controlepunt met de **bestand** > **herstellen naar controlepunt** opdracht. Een alternatieve methode is het maken van extra cellen en uitcommentarieer code die u niet uitvoeren wilt. in beide gevallen werkt.

U kunt ook de **bestand** > **maakt u een kopie** opdracht op elk gewenst moment een kopie van de huidige status van het notitieblok maken in een nieuw bestand in uw project. Deze kopie wordt automatisch geopend in een nieuw browsertabblad.

Wanneer u klaar bent met een laptop, gebruikt u de **bestand** > **sluiten en stoppen** opdracht, waarbij de notebook wordt gesloten en de kernel dat actief is geweest deze wordt afgesloten. Het browsertabblad wordt in Azure-notitieblokken automatisch gesloten.

## <a name="debug-notebooks-using-visual-studio-code"></a>Fouten opsporen in notitieblokken met behulp van Visual Studio Code

Als de cellen met code in uw laptop niet gedragen zich zoals verwacht, kunt u code bugs of andere defecten mogelijk. Echter, anders dan met behulp van `print` instructies toe aan de waarden van variabelen, weer een typische Jupyter-omgeving niet alle foutopsporing opslagruimten bieden.

Gelukkig kunt u downloaden van het notitieblok *.ipynb* en het bestand vervolgens opent u het in Visual Studio Code met de Python-extensie. De extensie een laptop wordt als één codebestand, behoud van uw Markdown-cellen in opmerkingen rechtstreeks worden geïmporteerd. Nadat u de notebook hebt geïmporteerd, kunt u het foutopsporingsprogramma van Visual Studio Code aan uw code kunt doorlopen, onderbrekingspunten instellen, Controleer de status, enzovoort. Na het aanbrengen van correcties in uw code, die u vervolgens exporteren de *.ipynb* vanuit Visual Studio Code-bestand en upload het opnieuw bij de Azure-notitieblokken.

Zie voor meer informatie, [fouten opsporen in een Jupyter-notebook](https://code.visualstudio.com/docs/python/jupyter-support#debug-a-jupyter-notebook) in de documentatie van Visual Studio Code.

Zie ook [Visual Studio Code - ondersteuning voor Jupyter](https://code.visualstudio.com/docs/python/jupyter-support) voor aanvullende functies van Visual Studio Code voor de Jupyter-notebooks.

## <a name="next-steps"></a>Volgende stappen

- [Voorbeeldnotitieblokken verkennen](azure-notebooks-samples.md)

Artikelen met procedures:

- [Maken en projecten klonen](create-clone-jupyter-notebooks.md)
- [Configureren en beheren van projecten](configure-manage-azure-notebooks-projects.md)
- [Installeren van pakketten van binnen een laptop](install-packages-jupyter-notebook.md)
- [Een diavoorstelling](present-jupyter-notebooks-slideshow.md)
- [Werken met bestanden](work-with-project-data-files.md)
- [Toegang tot gegevensresources](access-data-resources-jupyter-notebooks.md)
- [Azure Machine Learning-Services gebruiken](use-machine-learning-services-jupyter-notebooks.md)
