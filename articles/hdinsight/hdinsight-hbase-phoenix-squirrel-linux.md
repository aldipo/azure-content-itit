<properties 
   pageTitle="Usare Apache Phoenix e SQuirrel in HDInsight | Microsoft Azure" 
   description="Informazioni su come usare Apache Phoenix in HDInsight e su come installare e configurare SQuirreL sulla workstation per la connessione a un cluster HBase in HDInsight." 
   services="hdinsight" 
   documentationCenter="" 
   authors="mumian" 
   manager="paulettm" 
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="05/27/2016"
   ms.author="jgao"/>

# Usare Apache Phoenix con cluster HBase basati su Linux in HDinsight  

Informazioni su come usare [Apache Phoenix](http://phoenix.apache.org/) in HDInsight e su come usare SQLLine. Per altre informazioni su Phoenix, vedere la [breve panoramica su Phoenix](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html). Per la grammatica Phoenix, vedere [Grammatica Phoenix](http://phoenix.apache.org/language/index.html).

>[AZURE.NOTE] Per informazioni sulla versione di Phoenix in HDInsight, vedere [Novità delle versioni cluster di Hadoop incluse in HDInsight][hdinsight-versions].

##Usare SQLLine
[SQLLine](http://sqlline.sourceforge.net/) è un'utilità della riga di comando per eseguire SQL.

###Prerequisiti
Per usare SQLLine sono necessari:

- **Un cluster HBase in HDInsight**. Per informazioni sul provisioning di un cluster HBase, vedere l'[introduzione all'uso di Apache HBase in HDInsight][hdinsight-hbase-get-started].
- **Connessione al cluster HBase tramite il file RDP (Remote Desktop Protocol)**. Per istruzioni, vedere [Gestire cluster Hadoop in HDInsight tramite il portale di Azure classico][hdinsight-manage-portal].


Quando ci si connette a un cluster HBase, sarà necessario connettersi a uno dei Zookeeper. Ogni cluster HDInsight ha tre Zookeeper:

**Per scoprire il nome host del Zookeeper**

1. Aprire Ambari passando ad **https://<ClusterName>.azurehdinsight.net**.
2. Immettere il nome utente e la password HTTP (cluster) per effettuare l’accesso.
3. Nel menu a sinistra fare clic su **ZooKeeper**. L’elenco mostrerà 3 voci **ZooKeeper Server**.
4. Fare clic su una delle voci **ZooKeeper Server** dell’elenco. Nel riquadro Riepilogo, trovare il **Nome host**. È simile a *zk1-jdolehb.3lnng4rcvp5uzokyktxs4a5dhd.bx.internal.cloudapp.net*.

**Per usare SQLLine**

1. Connettersi al cluster tramite SSH. Per le istruzioni, vedere [Usare SSH con Hadoop basato su Linux in HDInsight da Linux, Unix e OS X](hdinsight-hadoop-linux-use-ssh-unix.md) o [Usare SSH con Hadoop basato su Linux in HDInsight da Windows](hdinsight-hadoop-linux-use-ssh-windows.md), a seconda del sistema operativo del computer.

2. Da SSH eseguire i comandi seguenti per avviare SQLLine:

        cd /usr/hdp/2.2.9.1-7/phoenix/bin
        ./sqlline.py <ClusterName>:2181:/hbase-unsecure

2. Eseguire i comandi seguenti per creare una tabella HBase e inserire alcuni dati:

		CREATE TABLE Company (COMPANY_ID INTEGER PRIMARY KEY, NAME VARCHAR(225));
	
		!tables
		
		UPSERT INTO Company VALUES(1, 'Microsoft');
		
		SELECT * FROM Company;
        
        !quit

Per altre informazioni, vedere il [manuale di SQLLine](http://sqlline.sourceforge.net/#manual) e [Grammatica Phoenix](http://phoenix.apache.org/language/index.html).


 
##Passaggi successivi
In questo articolo si è appreso come usare Apache Phoenix in HDInsight. Per altre informazioni, vedere

- [Panoramica di HDInsight HBase][hdinsight-hbase-overview]\: HBase è un database NoSQL open source Apache basato su Hadoop che fornisce un accesso casuale e coerenza assoluta a grandi quantità di dati non strutturati e semistrutturati.
- [Provisioning di cluster HBase in Rete virtuale di Azure][hdinsight-hbase-provision-vnet]\: con l'integrazione con la rete virtuale, i cluster HBase possono essere distribuiti alle stessa rete virtuale delle applicazioni in modo che queste ultime possano comunicare direttamente con HBase.
- [Configurare la replica HBase in HDInsight](hdinsight-hbase-geo-replication.md): informazioni su come configurare la replica HBase in due data center di Azure. 
- [Analisi dei sentimenti Twitter con HBase in HDInsight][hbase-twitter-sentiment]\: informazioni su come eseguire [l'analisi dei sentimenti](http://en.wikipedia.org/wiki/Sentiment_analysis) dei Big Data in tempo reale usando HBase in un cluster Hadoop in HDInsight.

[azure-portal]: https://portal.azure.com
[vnet-point-to-site-connectivity]: https://msdn.microsoft.com/library/azure/09926218-92ab-4f43-aa99-83ab4d355555#BKMK_VNETPT

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md#connect-to-hdinsight-clusters-by-using-rdp
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md

[hdinsight-hbase-phoenix-sqlline]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-phoenix-sqlline.png
[img-certificate]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vpn-certificate.png
[img-vnet-diagram]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vnet-point-to-site.png
[img-squirrel-driver]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-driver.png
[img-squirrel-alias]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-alias.png
[img-squirrel]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel.png
[img-squirrel-sql]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-sql.png


 

<!---HONumber=AcomDC_0601_2016-->