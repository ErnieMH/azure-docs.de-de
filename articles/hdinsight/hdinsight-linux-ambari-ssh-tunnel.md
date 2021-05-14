---
title: Verwenden von SSH-Tunneln für den Zugriff auf Azure HDInsight
description: Erfahren Sie, wie Sie einen SSH-Tunnel verwenden, um auf Webressourcen auf Ihren Linux-basierten HDInsight-Knoten zuzugreifen.
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive
ms.date: 04/14/2020
ms.openlocfilehash: 60e1e06dfdc904683f2d65e984d3c752e1e6a055
ms.sourcegitcommit: 5f785599310d77a4edcf653d7d3d22466f7e05e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/27/2021
ms.locfileid: "108064681"
---
# <a name="use-ssh-tunneling-to-access-apache-ambari-web-ui-jobhistory-namenode-apache-oozie-and-other-uis"></a>Verwenden von SSH-Tunneling zum Zugriff auf die Apache Ambari-Webbenutzeroberfläche, JobHistory, NameNode, Apache Oozie und andere Benutzeroberflächen

HDInsight-Cluster bieten über das Internet Zugriff auf die Apache Ambari-Webbenutzeroberfläche. Für einige Features ist ein SSH-Tunnel erforderlich. Beispielsweise kann ohne SSH-Tunnel nicht über das Internet auf die Webbenutzeroberfläche für Apache Oozie zugegriffen werden.

## <a name="why-use-an-ssh-tunnel"></a>Weshalb ist die Verwendung eines SSH-Tunnels empfehlenswert?

Einige der Menüs in Ambari funktionieren nur über einen SSH-Tunnel. Diese Menüs basieren auf Websites und Diensten, die auf anderen Knotentypen ausgeführt werden, z.B. Workerknoten.

Die folgenden Webbenutzeroberflächen erfordern einen SSH-Tunnel:

* JobHistory
* NameNode
* Threadstapel
* Oozie-Webbenutzeroberfläche
* Benutzeroberfläche für HBase-Master und -Protokolle

Dienste, die mit Skriptaktionen installiert wurden und einen Webdienst verfügbar machen, benötigen einen SSH-Tunnel. Mit einer Skriptaktion installiertes Hue erfordert einen SSH-Tunnel für den Zugriff auf die Webbenutzeroberfläche.

> [!IMPORTANT]  
> Wenn Sie über ein virtuelles Netzwerk Direktzugriff auf HDInsight haben, müssen Sie keine SSH-Tunnel verwenden. Ein Beispiel für Direktzugriff auf HDInsight über ein virtuelles Netzwerk finden Sie im Dokument [Connect HDInsight to your on-premise network](connect-on-premises-network.md) (Verbinden von HDInsight mit Ihrem lokalen Netzwerk, in englischer Sprache).

## <a name="what-is-an-ssh-tunnel"></a>Was ist ein SSH-Tunnel?

[SSH-Tunneling (Secure Shell)](https://en.wikipedia.org/wiki/Tunneling_protocol#Secure_Shell_tunneling) stellt eine Verbindung zwischen einem Port auf dem lokalen Computer und einem Hauptknoten in HDInsight her. Der an den lokalen Port gesendete Datenverkehr wird über eine SSH-Verbindung an den Hauptknoten weitergeleitet. Die Anforderung wird so aufgelöst, als ob sie vom Hauptknoten stammt. Die Antwort wird dann wieder durch den Tunnel an Ihre Arbeitsstation weitergeleitet.

## <a name="prerequisites"></a>Voraussetzungen

* Einen SSH-Client. Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit HDInsight (Hadoop) per SSH](hdinsight-hadoop-linux-use-ssh-unix.md).

* Einen Webbrowser, der für die Verwendung eines SOCKS5-Proxys konfiguriert werden kann.

    > [!WARNING]  
    > Die in die Windows-Interneteinstellungen integrierte SOCKS-Proxyunterstützung unterstützt SOCKS5 nicht. Sie funktioniert auch nicht mit den in diesem Dokument beschriebenen Schritten. Die folgenden Browser basieren auf Windows-Proxyeinstellungen und funktionieren derzeit nicht mit den Schritten in diesem Dokument:
    >
    > * Microsoft Edge
    > * Microsoft Internet Explorer
    >
    > Google Chrome basiert auch auf den Windows-Proxyeinstellungen. Allerdings können Sie Erweiterungen installieren, die SOCKS5 unterstützen. Wir empfehlen [FoxyProxy Standard](https://chrome.google.com/webstore/detail/foxyproxy-standard/gcknhkkoolaabfmlnjonogaaifnjlfnp).

## <a name="create-a-tunnel-using-the-ssh-command"></a><a name="usessh"></a>Erstellen von Tunneln mit dem Befehl "ssh"

Verwenden Sie den folgenden Befehl zum Erstellen eines SSH-Tunnels mithilfe des Befehls `ssh` . Ersetzen Sie `sshuser` durch einen SSH-Benutzer für Ihren HDInsight-Cluster und `CLUSTERNAME` durch den Namen des HDInsight-Clusters:

```cmd
ssh -C2qTnNf -D 9876 sshuser@CLUSTERNAME-ssh.azurehdinsight.net
```

Durch diesen Befehl wird eine Verbindung erstellt, über die der Datenverkehr über SSH an den lokalen Port 9876 des Clusters weitergeleitet wird. Die Optionen sind:

|Option |BESCHREIBUNG |
|---|---|
|D 9876|Der lokale Port, der den Datenverkehr durch den Tunnel weiterleitet.|
|C|Komprimiert alle Daten, da der Webdatenverkehr hauptsächlich aus Text besteht.|
|2|Erzwingt für SSH, dass nur Protokollversion 2 verwendet wird.|
|q|Stiller Modus.|
|T|Deaktiviert die Pseudo-TTY-Zuordnung, da lediglich ein Port weitergeleitet wird.|
|n|Verhindert das Lesen von STDIN, da lediglich ein Port weitergeleitet wird.|
|N|Verhindert das Ausführen von Remotebefehlen, da lediglich ein Port weitergeleitet wird.|
|f|Erzwingt die Ausführung im Hintergrund.|

Nach Abschluss des Befehls wird der an den Port 9876 des lokalen Computers gesendete Datenverkehr an den Hauptknoten des Clusters weitergeleitet.

## <a name="create-a-tunnel-using-putty"></a><a name="useputty"></a>Erstellen von Tunneln mit PuTTY

[PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty) ist ein SSH-Client für Windows mit grafischer Benutzeroberfläche. Wenn Sie mit PuTTY nicht vertraut sind, konsultieren Sie die [PuTTY-Dokumentation](https://www.chiark.greenend.org.uk/~sgtatham/putty/docs.html). Führen Sie die folgenden Schritte aus, um mithilfe von PuTTY einen SSH-Tunnel zu erstellen:

### <a name="create-or-load-a-session"></a>Erstellen oder Laden einer Sitzung

1. Öffnen Sie PuTTY, und stellen Sie sicher, dass **Sitzung** im linken Menü ausgewählt ist. Wenn Sie bereits eine Sitzung gespeichert haben, wählen Sie den Sitzungsnamen in der Liste **Saved Sessions** (Gespeicherte Sitzungen) und dann **Load** (Laden) aus.

1. Wenn Sie noch keine gespeicherte Sitzung haben, geben Sie Ihre Verbindungsdaten ein:

    |Eigenschaft |Wert |
    |---|---|
    |Hostname (oder IP-Adresse)|Die SSH-Adresse des HDInsight-Clusters. Beispiel: **mycluster-ssh.azurehdinsight.net**.|
    |Port|22|
    |Verbindungstyp.|SSH|

1. Wählen Sie **Speichern** aus.

    :::image type="content" source="./media/hdinsight-linux-ambari-ssh-tunnel/hdinsight-create-putty-session.png" alt-text="HDInsight – Erstellen einer PuTTY-Sitzung":::

1. Erweitern Sie auf der linken Seite des Dialogfelds im Abschnitt **Category** erst **Connection**, dann **SSH**, und wählen Sie anschließend **Tunnels** aus.

1. Geben Sie die folgenden Informationen in das Formular **Options controlling SSH port forwarding** ein:

    |Eigenschaft |Wert |
    |---|---|
    |Quellport|Der Port auf dem Client, den Sie weiterleiten möchten. Beispiel: **9876**.|
    |Destination|Die SSH-Adresse des HDInsight-Clusters. Beispiel: **mycluster-ssh.azurehdinsight.net**.|
    |Dynamisch|Ermöglicht das dynamische SOCKS-Proxyrouting.|

    :::image type="content" source="./media/hdinsight-linux-ambari-ssh-tunnel/hdinsight-putty-tunnel.png" alt-text="Tunneloptionen für die PuTTY-Konfiguration":::

1. Wählen Sie **Add** (Hinzufügen) aus, um die Einstellungen hinzuzufügen. Wählen Sie dann **Open** (Öffnen) aus, um eine SSH-Verbindung zu öffnen.

1. Melden Sie sich bei entsprechender Aufforderung beim Server an.

## <a name="use-the-tunnel-from-your-browser"></a>Verwenden des Tunnels im Browser

> [!IMPORTANT]  
> Die Schritte in diesem Abschnitt verwenden den Mozilla FireFox-Browser, da dieser auf allen Plattformen die gleichen Proxyeinstellungen bietet. Andere moderne Browser wie z.B. Google Chrome erfordern möglicherweise eine Erweiterung wie z.B. FoxyProxy, um mit dem Tunnel zu arbeiten.

1. Konfigurieren Sie den Browser so, dass er **localhost** sowie den Port verwendet, den Sie bei der Erstellung des Tunnels als **SOCKS v5**-Proxy verwendet haben. Die Firefox-Einstellungen sollten wie folgt aussehen. Wenn Sie einen anderen Port als 9876 verwenden, ändern Sie den Port entsprechend:

    :::image type="content" source="./media/hdinsight-linux-ambari-ssh-tunnel/firefox-proxy-settings.png" alt-text="Proxyeinstellungen für den Firefox-Browser":::

   > [!NOTE]  
   > Durch die Auswahl von **Remote-DNS** werden DNS-Anforderungen (Domain Name System) mithilfe des HDInsight-Clusters aufgelöst. Diese Einstellung löst DNS mit dem Hauptknoten des Clusters auf.

2. Überprüfen Sie, ob der Tunnel funktioniert, indem Sie eine Website wie [https://www.whatismyip.com/](https://www.whatismyip.com/) aufrufen. Die zurückgegebene IP-Adresse sollte eine vom Microsoft Azure-Datencenter verwendete Adresse sein.

## <a name="verify-with-ambari-web-ui"></a>Überprüfen der Ambari-Webbenutzeroberfläche

Nachdem der Cluster eingerichtet wurde, gehen Sie folgendermaßen vor, um zu überprüfen, ob Sie von der Ambari-Webbenutzeroberfläche auf Dienst-Webbenutzeroberflächen zugreifen können:

1. Navigieren Sie im Browser zu `http://headnodehost:8080`. Die `headnodehost`-Adresse wird durch den Tunnel an den Cluster gesendet und in den Hauptknoten aufgelöst, auf dem Ambari ausgeführt wird. Geben Sie bei der entsprechenden Aufforderung den Benutzernamen ("admin") und das Kennwort des Administrators für den Cluster ein. Sie werden eventuell von der Ambari-Webbenutzeroberfläche ein zweites Mal zur Eingabe aufgefordert. Geben Sie in diesem Fall die Informationen erneut ein.

   > [!NOTE]  
   > Wenn Sie die Adresse `http://headnodehost:8080` für die Verbindung mit dem Cluster verwenden, wird diese über den Tunnel hergestellt. Die Kommunikation wird durch den SSH-Tunnel und nicht per HTTPS geschützt. Verwenden Sie zum Herstellen einer Verbindung über das Internet per HTTPS die Adresse `https://clustername.azurehdinsight.net`, wobei `clustername` dem Namen des Clusters entspricht.

2. Wählen Sie auf der Ambari-Webbenutzeroberfläche aus der Liste links auf der Seite „HDFS“ aus.

    :::image type="content" source="./media/hdinsight-linux-ambari-ssh-tunnel/hdfs-service-selected.png" alt-text="Ausgewählter Apache Ambari HDFS-Dienst":::

3. Wenn die Informationen zum HDFS-Dienst angezeigt werden, wählen Sie **QuickLinks** aus. Eine Liste der Clusterhauptknoten wird angezeigt. Wählen Sie einen der Hauptknoten und dann **NameNode UI** aus.

    :::image type="content" source="./media/hdinsight-linux-ambari-ssh-tunnel/namenode-drop-down-menu.png" alt-text="Bild mit erweitertem Menü &quot;QuickLinks&quot;":::

    > [!NOTE]  
    > Bei Auswahl von __Quicklinks__ wird möglicherweise eine Benachrichtigung zum Warten angezeigt. Dies kann geschehen, wenn Sie über eine langsame Internetverbindung verfügen. Warten Sie eine oder zwei Minuten auf den Empfang der Daten vom Server, und wiederholen Sie dann das Auflisten.
    >
    > Einige Einträge im Menü **Quicklinks** können an der rechten Seite des Bildschirms abgeschnitten sein. Erweitern Sie in diesem Fall das Menü mit der Maus, und verwenden Sie die Nach-Rechts-Taste, um nach rechts zu scrollen und das restliche Menü anzuzeigen.

4. Eine Seite ähnlich der folgenden Abbildung wird angezeigt:

    :::image type="content" source="./media/hdinsight-linux-ambari-ssh-tunnel/hdinsight-namenode-ui.png" alt-text="Abbildung der Hadoop NameNode-Benutzeroberfläche":::

    > [!NOTE]  
    > Beachten Sie die URL für diese Seite, die etwa so aussehen sollte: `http://hn*.randomcharacters.cx.internal.cloudapp.net:8088/cluster`. Bei diesem URI wird der interne vollqualifizierte Domänenname (FQDN) des Knotens verwendet, auf den nur über einen SSH-Tunnel zugegriffen werden kann.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie erfahren haben, wie Sie einen SSH-Tunnel erstellen und verwenden, finden Sie im folgenden Dokument weitere Möglichkeiten für die Verwendung von Ambari:

* [Verwalten von HDInsight-Clustern mit Apache Ambari](hdinsight-hadoop-manage-ambari.md)
