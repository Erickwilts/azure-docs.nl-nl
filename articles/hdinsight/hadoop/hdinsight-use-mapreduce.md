---
title: MapReduce met Apache Hadoop op HDInsight
description: Leer hoe u MapReduce-taken worden uitgevoerd op Apache Hadoop in HDInsight-clusters.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 03/20/2019
ms.openlocfilehash: 9da6b6ba3ab697887e55f9077b44cf6fa100a981
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64707967"
---
# <a name="use-mapreduce-in-apache-hadoop-on-hdinsight"></a>MapReduce gebruiken in een Apache Hadoop op HDInsight

Leer hoe u MapReduce-taken uitvoert op HDInsight-clusters. 

## <a id="whatis"></a>Wat is MapReduce

Apache Hadoop MapReduce is een softwareframework voor het schrijven van taken die grote hoeveelheden gegevens verwerken. Invoergegevens is opgedeeld in segmenten onafhankelijk. Elk segment worden parallel verwerkt op de knooppunten in uw cluster. Een MapReduce-taak bestaat uit twee functies:

* **Mapper**: Invoergegevens worden verbruikt, analyseert ze (meestal met filteren en sorteren operations) en verzendt tuples (sleutel / waarde-paren)

* **Reducer**: Tuples die zijn gegenereerd door het toewijzen van de verbruikt en wordt een samenvatting uitgevoerd die u een kleinere, gecombineerde resultaat van de gegevens toewijzen maakt

Een voorbeeld van een eenvoudige word aantal MapReduce-taak wordt weergegeven in het volgende diagram:

![HDI.WordCountDiagram][image-hdi-wordcountdiagram]

De uitvoer van deze taak is een telling van het aantal keren dat elk woord is opgetreden in de tekst.

* Het toewijzen van de wordt elke regel van de ingevoerde tekst als invoer en deze opgesplitst in woorden. Het verzendt een sleutel/waarde-paar telkens wanneer een woord vindt plaats van het woord wordt gevolgd door een 1. De uitvoer is gesorteerd voordat deze naar reducer verzonden.
* De reducer de som van deze afzonderlijke tellingen voor elk woord en verzendt een enkel sleutel/waarde-paar die het woord gevolgd door de som van de instanties bevat.

MapReduce kan worden geïmplementeerd in diverse talen. Java is de meest voorkomende implementatie, en wordt gebruikt voor demonstratiedoeleinden te gebruiken in dit document.

## <a name="development-languages"></a>Programmeertalen

Talen of frameworks die zijn gebaseerd op Java en de Java Virtual Machine kan rechtstreeks als een MapReduce-taak worden uitgevoerd. Het voorbeeld in dit document is een Java MapReduce-toepassing. Niet-Java-talen, zoals C#, Python of zelfstandige uitvoerbare bestanden, moeten gebruiken **Hadoop-streaming**.

Hadoop-streaming communiceert met de toewijzing en reducer via STDIN en STDOUT. De mapper reducer gegevens van een regel op een tijdstip van STDIN lezen en schrijven van de uitvoer naar de STDOUT. Elke regel lezen of verzonden door het toewijzen en reducer moet zich in de indeling van een sleutel/waarde-paar, gescheiden door een tabteken:

    [key]/t[value]

Zie voor meer informatie, [Hadoop-Streaming](https://hadoop.apache.org/docs/r1.2.1/streaming.html).

Zie het volgende document voor meer voorbeelden van het gebruik van Hadoop-streaming met HDInsight:

* [C# MapReduce-taken ontwikkelen](apache-hadoop-dotnet-csharp-mapreduce-streaming.md)

## <a id="data"></a>Voorbeeldgegevens

HDInsight biedt verschillende voorbeeld gegevenssets die zijn opgeslagen in de `/example/data` en `/HdiSamples` directory. Deze mappen zijn in de standaardopslag voor uw cluster. In dit document, gebruiken we de `/example/data/gutenberg/davinci.txt` bestand. Dit bestand bevat de laptops van Leonardo da Vinci.

## <a id="job"></a>Voorbeeld van MapReduce

Een voorbeeld van de toepassing van MapReduce word-count is opgenomen in uw HDInsight-cluster. In dit voorbeeld bevindt zich in `/example/jars/hadoop-mapreduce-examples.jar` op de standaardopslag voor uw cluster.

De volgende Java-code is de bron van de MapReduce-toepassing die is opgenomen in de `hadoop-mapreduce-examples.jar` bestand:

```java
package org.apache.hadoop.examples;

import java.io.IOException;
import java.util.StringTokenizer;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.Mapper;
import org.apache.hadoop.mapreduce.Reducer;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
import org.apache.hadoop.util.GenericOptionsParser;

public class WordCount {

    public static class TokenizerMapper
        extends Mapper<Object, Text, Text, IntWritable>{

    private final static IntWritable one = new IntWritable(1);
    private Text word = new Text();

    public void map(Object key, Text value, Context context
                    ) throws IOException, InterruptedException {
        StringTokenizer itr = new StringTokenizer(value.toString());
        while (itr.hasMoreTokens()) {
        word.set(itr.nextToken());
        context.write(word, one);
        }
    }
    }

    public static class IntSumReducer
        extends Reducer<Text,IntWritable,Text,IntWritable> {
    private IntWritable result = new IntWritable();

    public void reduce(Text key, Iterable<IntWritable> values,
                        Context context
                        ) throws IOException, InterruptedException {
        int sum = 0;
        for (IntWritable val : values) {
        sum += val.get();
        }
        result.set(sum);
        context.write(key, result);
    }
    }

    public static void main(String[] args) throws Exception {
    Configuration conf = new Configuration();
    String[] otherArgs = new GenericOptionsParser(conf, args).getRemainingArgs();
    if (otherArgs.length != 2) {
        System.err.println("Usage: wordcount <in> <out>");
        System.exit(2);
    }
    Job job = new Job(conf, "word count");
    job.setJarByClass(WordCount.class);
    job.setMapperClass(TokenizerMapper.class);
    job.setCombinerClass(IntSumReducer.class);
    job.setReducerClass(IntSumReducer.class);
    job.setOutputKeyClass(Text.class);
    job.setOutputValueClass(IntWritable.class);
    FileInputFormat.addInputPath(job, new Path(otherArgs[0]));
    FileOutputFormat.setOutputPath(job, new Path(otherArgs[1]));
    System.exit(job.waitForCompletion(true) ? 0 : 1);
    }
}
```

Zie het volgende document voor instructies om uw eigen MapReduce-toepassingen te schrijven:

* [Ontwikkel Java MapReduce-toepassingen voor HDInsight](apache-hadoop-develop-deploy-java-mapreduce-linux.md)

## <a id="run"></a>De MapReduce uitvoeren

HDInsight uitvoeren HiveQL taken met behulp van verschillende methoden. Gebruik de volgende tabel om te bepalen welke methode is geschikt voor u, en vervolgens de koppeling voor een overzicht.

| **Gebruik deze**... | **...om hiervoor** | ...door dit **cluster-besturingssysteem** | ...from dit **clientbesturingssysteem** |
|:--- |:--- |:--- |:--- |
| [SSH](apache-hadoop-use-mapreduce-ssh.md) |Gebruik de opdracht Hadoop via **SSH** |Linux |Linux, Unix, Mac OS X of Windows |
| [Curl](apache-hadoop-use-mapreduce-curl.md) |De taak op afstand indienen met behulp van **REST** |Linux of Windows |Linux, Unix, Mac OS X of Windows |
| [Windows PowerShell](apache-hadoop-use-mapreduce-powershell.md) |De taak op afstand indienen met behulp van **Windows PowerShell** |Linux of Windows |Windows |

## <a id="nextsteps"></a>Volgende stappen

Zie de volgende documenten voor meer informatie over het werken met gegevens in HDInsight:

* [Java MapReduce-programma's ontwikkelen voor HDInsight](apache-hadoop-develop-deploy-java-mapreduce-linux.md)

* [Apache Hive gebruiken met HDInsight][hdinsight-use-hive]

* [Apache Pig gebruiken met HDInsight][hdinsight-use-pig]


[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]:apache-hadoop-linux-tutorial-get-started.md
[hdinsight-develop-mapreduce-jobs]: apache-hadoop-develop-deploy-java-mapreduce-linux.md
[hdinsight-use-hive]:../hdinsight-use-hive.md
[hdinsight-use-pig]:hdinsight-use-pig.md


[powershell-install-configure]: /powershell/azureps-cmdlets-docs

[image-hdi-wordcountdiagram]: ./media/hdinsight-use-mapreduce/HDI.WordCountDiagram.gif
