---
title: 'Tutorial: Kopieren von Daten in Azure Data Box-Blobspeicher über REST-APIs'
description: In diesem Tutorial erfahren Sie, wie Sie mithilfe von REST-APIs über HTTP oder HTTPS eine Verbindung mit Azure Data Box-Blobspeicher herstellen und anschließend Daten von Azure Data Box Heavy kopieren.
services: databox
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: tutorial
ms.date: 07/03/2019
ms.author: alkohli
ms.openlocfilehash: e2fc174147b99b7b952c6d10084cfc969dacf5a6
ms.sourcegitcommit: a2d8acc1b0bf4fba90bfed9241b299dc35753ee6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/12/2020
ms.locfileid: "91949138"
---
# <a name="tutorial-copy-data-to-azure-data-box-blob-storage-via-rest-apis"></a>Tutorial: Kopieren von Daten in Azure Data Box-Blobspeicher über REST-APIs  

In diesem Tutorial werden Verfahren zum Herstellen einer Verbindung mit Azure Data Box-Blobspeicher über REST-APIs und *HTTP* oder *HTTPS* beschrieben. Außerdem werden die Schritte beschrieben, die nach dem Herstellen der Verbindung ausgeführt werden müssen, um die Daten in Data Box-Blobspeicher zu kopieren.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Voraussetzungen
> * Herstellen einer Verbindung mit Data Box-Blobspeicher über *HTTP* oder *HTTPS*
> * Kopieren von Daten auf Data Box Heavy

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie Folgendes sicher, bevor Sie beginnen:

1. Sie haben die Schritte unter [Tutorial: Verkabeln und Herstellen einer Verbindung mit der Azure Data Box](data-box-heavy-deploy-set-up.md) ausgeführt.
2. Sie haben Ihr Data Box Heavy-Gerät erhalten, und der Auftrag wird im Portal mit dem Status **Geliefert** angezeigt.
3. Sie haben die [Systemanforderungen für Data Box-Blobspeicher](data-box-system-requirements-rest.md) gelesen und sich mit den unterstützten Versionen der APIs, SDKs und Tools vertraut gemacht.
4. Sie haben Zugriff auf einen Hostcomputer mit den Daten, die Sie auf Data Box Heavy kopieren möchten. Für Ihren Hostcomputer müssen die folgenden Bedingungen erfüllt sein:
    - Es muss ein [unterstütztes Betriebssystem](data-box-system-requirements.md) ausgeführt werden.
    - Er muss mit einem Hochgeschwindigkeitsnetzwerk verbunden sein. Zur Erzielung der höchstmöglichen Kopiergeschwindigkeit können zwei 40-GbE-Verbindungen (jeweils eine pro Knoten) parallel genutzt werden. Falls Sie über keine 40-GbE-Verbindung verfügen, sollten Sie zwei Verbindungen mit mindestens 10 GbE verwenden (jeweils eine pro Knoten). 
5. [Laden Sie AzCopy 7.1.0 auf Ihren Hostcomputer herunter.](https://aka.ms/azcopyforazurestack20170417) Mit AzCopy kopieren Sie Daten von Ihrem Hostcomputer in Azure Data Box-Blobspeicher.


## <a name="connect-via-http-or-https"></a>Herstellen einer Verbindung über HTTP oder HTTPS

Sie können über *HTTP* oder *HTTPS* eine Verbindung mit Data Box-Blobspeicher herstellen.

- *HTTPS* ist die sichere und empfohlene Methode zum Herstellen einer Verbindung mit Data Box-Blobspeicher.
- *HTTP* wird zum Herstellen von Verbindungen über vertrauenswürdige Netzwerke verwendet.

Die Schritte zum Herstellen der Verbindung mit Data Box-Blobspeicher über *HTTP* und *HTTPS* sind unterschiedlich.

## <a name="connect-via-http"></a>Herstellen einer Verbindung über HTTP

Zum Herstellen einer Verbindung mit Data Box-Blobspeicher-REST-APIs über *HTTP* müssen die folgenden Schritte ausgeführt werden:

- Hinzufügen der Geräte-IP und des Blob-Dienstendpunkts zum Remotehost
- Konfigurieren von Drittanbietersoftware und Überprüfen der Verbindung

Die einzelnen Schritte werden in den folgenden Abschnitten beschrieben.

> [!IMPORTANT]
> Bei Data Box Heavy müssen die Schritte für die Verbindungsherstellung noch einmal ausgeführt werden, um eine Verbindung mit dem zweiten Knoten herzustellen.

### <a name="add-device-ip-address-and-blob-service-endpoint"></a>Hinzufügen der Geräte-IP-Adresse und des Blob-Dienstendpunkts

[!INCLUDE [data-box-add-device-ip](../../includes/data-box-add-device-ip.md)]



### <a name="configure-partner-software-and-verify-connection"></a>Konfigurieren von Partnersoftware und Überprüfen der Verbindung

[!INCLUDE [data-box-configure-partner-software](../../includes/data-box-configure-partner-software.md)]

[!INCLUDE [data-box-verify-connection](../../includes/data-box-verify-connection.md)]

## <a name="connect-via-https"></a>Herstellen einer Verbindung über HTTPS

Zum Herstellen einer Verbindung mit Azure-Blobspeicher-REST-APIs über HTTPS müssen die folgenden Schritte ausgeführt werden:

- Herunterladen des Zertifikats aus dem Azure-Portal
- Importieren des Zertifikats auf dem Client- oder Remotehost
- Hinzufügen der Geräte-IP und des Blob-Dienstendpunkts zum Client- oder Remotehost
- Konfigurieren von Drittanbietersoftware und Überprüfen der Verbindung

Die einzelnen Schritte werden in den folgenden Abschnitten beschrieben.

> [!IMPORTANT]
> Bei Data Box Heavy müssen die Schritte für die Verbindungsherstellung noch einmal ausgeführt werden, um eine Verbindung mit dem zweiten Knoten herzustellen.

### <a name="download-certificate"></a>Zertifikat herunterladen

Verwenden Sie zum Herunterladen des Zertifikats das Azure-Portal.

1. Melden Sie sich beim Azure-Portal an.
2. Navigieren Sie zu Ihrem Data Box-Auftrag und dann zu **Allgemein > Gerätedetails**.
3. Wechseln Sie unter **Geräteanmeldeinformationen** zu **API-Zugriff auf Gerät**. Klicken Sie auf **Download**. Mit dieser Aktion wird eine Zertifikatdatei ( **\<your order name>.cer**) heruntergeladen. **Speichern** Sie diese Datei. Sie installieren dieses Zertifikat auf dem Client- oder Hostcomputer, den Sie für die Verbindung mit dem Gerät verwenden werden.

    ![Herunterladen des Zertifikats im Azure-Portal](media/data-box-deploy-copy-data-via-rest/download-cert-1.png)
 
### <a name="import-certificate"></a>Importieren des Zertifikats 

Der Zugriff auf Data Box-Blobspeicher über HTTPS erfordert ein TLS-/SSL-Zertifikat für das Gerät. Die Art und Weise, wie dieses Zertifikat der Clientanwendung zur Verfügung gestellt wird, variiert von Anwendung zu Anwendung sowie zwischen Betriebssystemen und Distributionen. Einige Anwendungen können auf das Zertifikat zugreifen, nachdem es in den Zertifikatsspeicher des Systems importiert wurde, während andere Anwendungen diesen Mechanismus nicht nutzen.

Dieser Abschnitt enthält spezifische Informationen für einige Anwendungen. Weitere Informationen zu anderen Anwendungen finden Sie in der Dokumentation für die jeweilige Anwendung und für das verwendete Betriebssystem.

Führen Sie diese Schritte aus, um die `.cer`-Datei in den Stammspeicher eines Windows- oder Linux-Clients zu importieren. Auf einem Windows-System können Sie Windows PowerShell oder die Windows Server-Benutzeroberfläche nutzen, um das Zertifikat zu importieren und auf Ihrem System zu installieren.

#### <a name="use-windows-powershell"></a>Verwenden von Windows PowerShell

1. Starten Sie eine Windows PowerShell-Sitzung als Administrator.
2. Geben Sie an der Eingabeaufforderung Folgendes ein:

    ```
    Import-Certificate -FilePath C:\temp\localuihttps.cer -CertStoreLocation Cert:\LocalMachine\Root
    ```

#### <a name="use-windows-server-ui"></a>Verwenden der Windows Server-Benutzeroberfläche

1.  Klicken Sie mit der rechten Maustaste auf die `.cer`-Datei, und wählen Sie **Zertifikat installieren** aus. Mit dieser Aktion wird der Zertifikatimport-Assistent gestartet.
2.  Wählen Sie für **Speicherort** die Option **Lokaler Computer** aus, und klicken Sie dann auf **Weiter**.

    ![Importieren des Zertifikats mithilfe von PowerShell](media/data-box-deploy-copy-data-via-rest/import-cert-ws-1.png)

3.  Wählen Sie **Alle Zertifikate in folgendem Speicher speichern** aus, und klicken Sie dann auf **Durchsuchen**. Navigieren Sie zum Stammspeicher des Remotehosts, und klicken Sie dann auf **Weiter**.

    ![Importieren des Zertifikats mithilfe von PowerShell 2](media/data-box-deploy-copy-data-via-rest/import-cert-ws-2.png)

4.  Klicken Sie auf **Fertig stellen**. Eine Meldung wird angezeigt, die besagt, dass der Import erfolgreich war.

    ![Importieren des Zertifikats mithilfe von PowerShell 3](media/data-box-deploy-copy-data-via-rest/import-cert-ws-3.png)

#### <a name="use-a-linux-system"></a>Verwenden eines Linux-Systems

Die Methode zum Importieren eines Zertifikats hängt von der Distribution ab.

> [!IMPORTANT]
> Bei Data Box Heavy müssen die Schritte für die Verbindungsherstellung noch einmal ausgeführt werden, um eine Verbindung mit dem zweiten Knoten herzustellen.

Mehrere Distribution, wie etwa Ubuntu und Debian, verwenden den Befehl `update-ca-certificates`.  

- Benennen Sie die Base64-codierte Zertifikatdatei so um, dass sie die Erweiterung `.crt` aufweist, und kopieren Sie sie in das Verzeichnis `/usr/local/share/ca-certificates directory`.
- Führen Sie den Befehl `update-ca-certificates` aus.

Neuere Versionen von RHEL, Fedora und CentOS verwenden den Befehl `update-ca-trust`.

- Kopieren Sie die Zertifikatdatei in das Verzeichnis `/etc/pki/ca-trust/source/anchors`.
- Führen Sie `update-ca-trust` aus.

Spezifische Einzelheiten für Ihre Distribution finden Sie in der Dokumentation.

### <a name="add-device-ip-address-and-blob-service-endpoint"></a>Hinzufügen der Geräte-IP-Adresse und des Blob-Dienstendpunkts 

Führen Sie dieselben Schritte aus, um beim [Herstellen der Verbindung über *HTTP* die IP-Adresse des Geräts und den Endpunkt des Blobdiensts hinzuzufügen](#add-device-ip-address-and-blob-service-endpoint).

### <a name="configure-partner-software-and-verify-connection"></a>Konfigurieren von Partnersoftware und Überprüfen der Verbindung

Führen Sie die Schritte zum [Konfigurieren von Partnersoftware aus, die Sie beim Herstellen der Verbindung über *HTTP* verwendet haben](#configure-partner-software-and-verify-connection). Der einzige Unterschied besteht darin, dass die Option *HTTP verwenden* deaktiviert bleibt.

## <a name="copy-data-to-data-box-heavy"></a>Kopieren von Daten auf Data Box Heavy

Nachdem Sie eine Verbindung mit dem Data Box-Blobspeicher hergestellt haben, kopieren Sie im nächsten Schritt Ihre Daten. Beachten Sie vor dem Kopieren von Daten Folgendes:

-  Achten Sie beim Kopieren der Daten auf die Einhaltung der Größenbeschränkungen, die im Artikel zu den [Grenzwerten für Azure Storage und Data Box Heavy](data-box-limits.md) beschrieben sind.
- Falls von Data Box Heavy hochgeladene Daten gleichzeitig von anderen Anwendungen außerhalb von Data Box Heavy hochgeladen werden, kann dies zu Fehlern bei Uploadaufträgen und zu Datenbeschädigungen führen.

In diesem Tutorial wird AzCopy zum Kopieren von Daten in den Data Box-Blospeicher verwendet. Sie können zum Kopieren der Daten auch den Azure Storage-Explorer (falls Sie ein GUI-basiertes Tool bevorzugen) oder eine Partnersoftware nutzen.

Der Kopiervorgang umfasst die folgenden Schritte:

- Erstellen eines Containers
- Hochladen des Inhalts eines Ordners in Data Box-Blobspeicher
- Hochladen geänderter Dateien in Data Box-Blobspeicher


Jeder dieser Schritte wird in den folgenden Abschnitten ausführlich erläutert.

> [!IMPORTANT]
> Bei Data Box Heavy müssen die Kopierschritte noch einmal ausgeführt werden, um Daten auf den zweiten Knoten zu kopieren.

### <a name="create-a-container"></a>Erstellen eines Containers

Im ersten Schritt wird ein Container erstellt, da Blobs immer in einen Container hochgeladen werden. Mit Containern können Sie Gruppen von Blobs so wie Dateien in Ordnern auf Ihrem Computer organisieren. Führen Sie folgende Schritte aus, um einen Blobcontainer zu erstellen:

1. Öffnen Sie den Storage-Explorer.
2. Erweitern Sie im linken Bereich das Speicherkonto, in dem Sie den Blobcontainer erstellen möchten.
3. Klicken Sie mit der rechten Maustaste auf **Blobcontainer**, und wählen Sie im Kontextmenü die Option **Blobcontainer erstellen** aus.

   ![Kontextmenü „Blobcontainer erstellen“](media/data-box-deploy-copy-data-via-rest/create-blob-container-1.png)

4. Unter dem Ordner **Blobcontainer** wird ein Textfeld angezeigt. Geben Sie den Namen für den Blobcontainer ein. Informationen zu Regeln und Einschränkungen für die Benennung von Blobcontainern finden Sie unter [Erstellen des Containers und Festlegen von Berechtigungen](../storage/blobs/storage-quickstart-blobs-dotnet.md).
5. Drücken Sie die **EINGABETASTE**, wenn Sie mit dem Erstellen des Blobcontainers fertig sind, oder drücken Sie **ESC**, um den Vorgang abzubrechen. Nach der erfolgreichen Erstellung des Blobcontainers wird er im Ordner **Blobcontainer** für das ausgewählte Speicherkonto angezeigt.

   ![Blobcontainer erstellt](media/data-box-deploy-copy-data-via-rest/create-blob-container-2.png)

### <a name="upload-contents-of-a-folder-to-data-box-blob-storage"></a>Hochladen des Inhalts eines Ordners in Data Box-Blobspeicher

Mit AzCopy können Sie unter Windows oder Linux alle Dateien in einem Ordner in Blobspeicher hochladen. Um alle Blobs in einem Ordner hochzuladen, geben Sie den folgenden AzCopy-Befehl aus:

#### <a name="linux"></a>Linux

```azcopy
azcopy \
    --source /mnt/myfolder \
    --destination https://data-box-storage-account-name.blob.device-serial-no.microsoftdatabox.com/container-name/files/ \
    --dest-key <key> \
    --recursive
```

#### <a name="windows"></a>Windows

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://data-box-storage-account-name.blob.device-serial-no.microsoftdatabox.com/container-name/files/ /DestKey:<key> /S
```

Ersetzen Sie `<key>` durch Ihren Kontoschlüssel. Wechseln Sie im Azure-Portal zu Ihrem Speicherkonto, um Ihren Kontoschlüssel abzurufen. Navigieren Sie zu **Einstellungen > Zugriffsschlüssel**, wählen Sie einen Schlüssel aus, und fügen Sie ihn in den AzCopy-Befehl ein.

Wenn der angegebene Zielcontainer nicht vorhanden ist, wird er von AzCopy erstellt und die Datei anschließend in den Container hochgeladen. Aktualisieren Sie den Quellpfad zu Ihrem Datenverzeichnis, und ersetzen Sie `data-box-storage-account-name` in der Ziel-URL durch den Namen des Speicherkontos, das Ihrer Data Box zugeordnet ist.

Um die Inhalte des angegebenen Verzeichnisses rekursiv in Blob Storage hochzuladen, legen Sie die Option `--recursive` (Linux) oder `/S` (Windows) fest. Beim Ausführen von AzCopy mit einer dieser Optionen werden auch alle Unterordner und die darin enthaltenen Dateien hochgeladen.

### <a name="upload-modified-files-to-data-box-blob-storage"></a>Hochladen geänderter Dateien in Data Box-Blobspeicher

Verwenden Sie AzCopy, um Dateien basierend auf dem Zeitpunkt der letzten Änderung hochzuladen. Um dies zu testen, ändern Sie Dateien im Quellverzeichnis oder erstellen neue Dateien für Testzwecke. Um ausschließlich aktualisierte oder neue Dateien hochzuladen, fügen Sie den Parameter `--exclude-older` (Linux) oder `/XO` (Windows) zum AzCopy-Befehl hinzu.

Falls Sie nur Quellressourcen kopieren möchten, die im Ziel nicht vorhanden sind, können Sie beide Parameter – `--exclude-older` und `--exclude-newer` (Linux) oder `/XO` und `/XN` (Windows) – im AzCopy-Befehl angeben. AzCopy lädt nur die aktualisierten Daten basierend auf dem Zeitstempel hoch.

#### <a name="linux"></a>Linux

```azcopy
azcopy \
--source /mnt/myfolder \
--destination https://data-box-heavy-storage-account-name.blob.device-serial-no.microsoftdatabox.com/container-name/files/ \
--dest-key <key> \
--recursive \
--exclude-older
```

#### <a name="windows"></a>Windows

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://data-box-heavy-storage-account-name.blob.device-serial-no.microsoftdatabox.com/container-name/files/ /DestKey:<key> /S /XO
```

Wenn während des Vorgangs zum Herstellen einer Verbindung oder des Kopiervorgangs Fehler auftreten, lesen Sie [Behandeln von Problemen bei Data Box-Blobspeicher](data-box-troubleshoot-rest.md).

Im nächsten Schritt bereiten Sie das Gerät für den Versand vor.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Informationen zu Azure Data Box-Themen erhalten, darunter die folgenden:

> [!div class="checklist"]
> * Voraussetzungen
> * Herstellen einer Verbindung mit Data Box-Blobspeicher über *HTTP* oder *HTTPS*
> * Kopieren von Daten auf Data Box Heavy


Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie Ihre Data Box zurück an Microsoft senden.

> [!div class="nextstepaction"]
> [Zurücksenden von Azure Data Box Heavy und Überprüfen des Datenuploads in Azure (Vorschauversion)](./data-box-heavy-deploy-picked-up.md)
