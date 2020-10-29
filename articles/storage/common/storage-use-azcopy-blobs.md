---
title: Übertragen von Daten nach oder aus Azure Blob Storage mit AzCopy v10 | Microsoft-Dokumentation
description: Dieser Artikel enthält eine Sammlung von AzCopy-Beispielbefehlen, die Sie beim Erstellen von Containern, Kopieren von Dateien und Synchronisieren von Verzeichnissen zwischen lokalen Dateisystemen und Containern unterstützen.
author: normesta
ms.service: storage
ms.topic: how-to
ms.date: 07/27/2020
ms.author: normesta
ms.subservice: common
ms.reviewer: dineshm
ms.openlocfilehash: 294adce3dc312003d72336bd0752ba3aba5eaace
ms.sourcegitcommit: 400f473e8aa6301539179d4b320ffbe7dfae42fe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/28/2020
ms.locfileid: "92792853"
---
# <a name="transfer-data-with-azcopy-and-blob-storage"></a>Übertragen von Daten mit AzCopy und Blob Storage

AzCopy ist ein Befehlszeilenhilfsprogramm, das Sie verwenden können, um Daten in ein oder aus einem Speicherkonto zu kopieren. Dieser Artikel enthält Beispielbefehle, die mit Blob Storage funktionieren.

> [!TIP]
> In den Beispielen in diesem Artikel werden Pfadargumente in einfache Anführungszeichen ('') eingeschlossen. Verwenden Sie in allen Befehlsshells außer der Windows-Befehlszeile (cmd.exe) einfache Anführungszeichen. Wenn Sie eine Windows-Befehlszeile (cmd.exe) verwenden, müssen Sie Pfadargumente in doppelte Anführungszeichen ("") anstelle von einfachen Anführungszeichen ('') einschließen.

## <a name="get-started"></a>Erste Schritte

Lesen Sie den Artikel [Erste Schritte mit AzCopy](storage-use-azcopy-v10.md), um AzCopy herunterzuladen und zu erfahren, wie Sie dem Speicherdienst Autorisierungsanmeldeinformationen bereitstellen können.

> [!NOTE]
> Bei den Beispielen in diesem Artikel wird davon ausgegangen, dass Sie Ihre Identität mithilfe des `AzCopy login`-Befehls authentifiziert haben. AzCopy verwendet Ihr Azure AD-Konto, um den Zugriff auf Daten in Blob Storage zu autorisieren.
>
> Wenn Sie lieber ein SAS-Token für die Autorisierung des Zugriffs auf Blobdaten verwenden möchten, können Sie dieses Token in jedem AzCopy-Befehl an die Ressourcen-URL anfügen.
>
> Beispiel: `'https://<storage-account-name>.blob.core.windows.net/<container-name><SAS-token>'`.

## <a name="create-a-container"></a>Erstellen eines Containers

Sie können den Befehl [azcopy make](storage-ref-azcopy-make.md) verwenden, um einen Container zu erstellen. Die Beispiele in diesem Abschnitt erstellen einen Container namens `mycontainer`.

|    |     |
|--------|-----------|
| **Syntax** | `azcopy make 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>'` |
| **Beispiel** | `azcopy make 'https://mystorageaccount.blob.core.windows.net/mycontainer'` |
| **Beispiel** (hierarchischer Namespace) | `azcopy make 'https://mystorageaccount.dfs.core.windows.net/mycontainer'` |

Ausführliche Referenzdokumente finden Sie unter [azcopy make](storage-ref-azcopy-make.md).

## <a name="upload-files"></a>Hochladen von Dateien

Sie können den Befehl [azcopy copy](storage-ref-azcopy-copy.md) verwenden, um Dateien und Verzeichnisse von Ihrem lokalen Computer hochzuladen.

Dieser Abschnitt enthält folgende Beispiele:

> [!div class="checklist"]
> * Hochladen einer Datei
> * Hochladen eines Verzeichnisses
> * Hochladen der Inhalte eines Verzeichnisses 
> * Hochladen bestimmter Dateien

> [!TIP]
> Sie können den Uploadvorgang mit optionalen Flags optimieren. Hier sind einige Beispiele angegeben.
>
> |Szenario|Flag|
> |---|---|
> |Dateien sollen als Anfügeblobs oder Seitenblobs hochgeladen werden.|**--blob-type**=\[BlockBlob\|PageBlob\|AppendBlob\]|
> |Hochladen auf eine bestimmte Zugriffsebene (z. B. die Archivebene)|**--block-blob-tier**=\[None\|Hot\|Cool\|Archive\]|
> 
> Eine vollständige Liste finden Sie unter [Optionen](storage-ref-azcopy-copy.md#options).

### <a name="upload-a-file"></a>Hochladen einer Datei

|    |     |
|--------|-----------|
| **Syntax** | `azcopy copy '<local-file-path>' 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<blob-name>'` |
| **Beispiel** | `azcopy copy 'C:\myDirectory\myTextFile.txt' 'https://mystorageaccount.blob.core.windows.net/mycontainer/myTextFile.txt'` |
| **Beispiel** (hierarchischer Namespace) | `azcopy copy 'C:\myDirectory\myTextFile.txt' 'https://mystorageaccount.dfs.core.windows.net/mycontainer/myTextFile.txt'` |

Sie können eine Datei auch mit einem Platzhaltersymbol (*) an einer beliebigen Stelle im Dateipfad oder Dateinamen hochladen. Beispiel: `'C:\myDirectory\*.txt'` oder `C:\my*\*.txt`

### <a name="upload-a-directory"></a>Hochladen eines Verzeichnisses

Dieses Beispiel kopiert ein Verzeichnis (sowie alle in diesem Verzeichnis enthaltenen Dateien) in einen Blobcontainer. Das Resultat ist ein gleichnamiges Verzeichnis im Container.

|    |     |
|--------|-----------|
| **Syntax** | `azcopy copy '<local-directory-path>' 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>' --recursive` |
| **Beispiel** | `azcopy copy 'C:\myDirectory' 'https://mystorageaccount.blob.core.windows.net/mycontainer' --recursive` |
| **Beispiel** (hierarchischer Namespace) | `azcopy copy 'C:\myDirectory' 'https://mystorageaccount.dfs.core.windows.net/mycontainer' --recursive` |

Um ein Verzeichnis innerhalb des Containers zu kopieren, geben Sie einfach den Namen des Verzeichnisses in Ihrer Befehlszeichenfolge an.

|    |     |
|--------|-----------|
| **Beispiel** | `azcopy copy 'C:\myDirectory' 'https://mystorageaccount.blob.core.windows.net/mycontainer/myBlobDirectory' --recursive` |
| **Beispiel** (hierarchischer Namespace) | `azcopy copy 'C:\myDirectory' 'https://mystorageaccount.dfs.core.windows.net/mycontainer/myBlobDirectory' --recursive` |

Wenn Sie den Namen eines Verzeichnisses angeben, das im Container nicht vorhanden ist, erstellt AzCopy ein neues Verzeichnis dieses Namens.

### <a name="upload-the-contents-of-a-directory"></a>Hochladen der Inhalte eines Verzeichnisses

Mithilfe des Platzhaltersymbols (*) können Sie die Inhalte eines Verzeichnisses hochladen, ohne das Verzeichnis selbst zu kopieren.

|    |     |
|--------|-----------|
| **Syntax** | `azcopy copy '<local-directory-path>\*' 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<directory-path>'` |
| **Beispiel** | `azcopy copy 'C:\myDirectory\*' 'https://mystorageaccount.blob.core.windows.net/mycontainer/myBlobDirectory'` |
| **Beispiel** (hierarchischer Namespace) | `azcopy copy 'C:\myDirectory\*' 'https://mystorageaccount.dfs.core.windows.net/mycontainer/myBlobDirectory'` |

> [!NOTE]
> Fügen Sie das Flag `--recursive` an, um Dateien in allen Unterverzeichnissen hochzuladen.

### <a name="upload-specific-files"></a>Hochladen bestimmter Dateien

Sie können bestimmte Dateien mithilfe von vollständigen Dateinamen, partiellen Namen mit Platzhalterzeichen (*) oder mithilfe von Datums- und Uhrzeitwerten hochladen.

#### <a name="specify-multiple-complete-file-names"></a>Angeben mehrerer vollständiger Dateinamen

Verwenden Sie den Befehl [azcopy copy](storage-ref-azcopy-copy.md) mit der Option `--include-path`. Trennen Sie einzelne Dateinamen durch ein Semikolon (`;`).

|    |     |
|--------|-----------|
| **Syntax** | `azcopy copy '<local-directory-path>' 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>' --include-path <semicolon-separated-file-list>` |
| **Beispiel** | `azcopy copy 'C:\myDirectory' 'https://mystorageaccount.blob.core.windows.net/mycontainer' --include-path 'photos;documents\myFile.txt' --recursive` |
| **Beispiel** (hierarchischer Namespace) | `azcopy copy 'C:\myDirectory' 'https://mystorageaccount.dfs.core.windows.net/mycontainer' --include-path 'photos;documents\myFile.txt' --recursive` |

In diesem Beispiel überträgt AzCopy das Verzeichnis `C:\myDirectory\photos` und die Datei `C:\myDirectory\documents\myFile.txt`. Sie müssen die Option `--recursive` verwenden, um alle Dateien im Verzeichnis `C:\myDirectory\photos` zu übertragen.

Mit der Option `--exclude-path` können Sie auch Dateien ausschließen. Weitere Informationen finden Sie in den Referenzdokumenten zu [azcopy copy](storage-ref-azcopy-copy.md).

#### <a name="use-wildcard-characters"></a>Verwenden von Platzhalterzeichen

Verwenden Sie den Befehl [azcopy copy](storage-ref-azcopy-copy.md) mit der Option `--include-pattern`. Geben Sie Namen mithilfe von Platzhalterzeichen teilweise an. Trennen Sie die Namen durch Semikolons (`;`). 

|    |     |
|--------|-----------|
| **Syntax** | `azcopy copy '<local-directory-path>' 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>' --include-pattern <semicolon-separated-file-list-with-wildcard-characters>` |
| **Beispiel** | `azcopy copy 'C:\myDirectory' 'https://mystorageaccount.blob.core.windows.net/mycontainer' --include-pattern 'myFile*.txt;*.pdf*'` |
| **Beispiel** (hierarchischer Namespace) | `azcopy copy 'C:\myDirectory' 'https://mystorageaccount.dfs.core.windows.net/mycontainer' --include-pattern 'myFile*.txt;*.pdf*'` |

Mit der Option `--exclude-pattern` können Sie auch Dateien ausschließen. Weitere Informationen finden Sie in den Referenzdokumenten zu [azcopy copy](storage-ref-azcopy-copy.md).

Die Optionen `--include-pattern` und `--exclude-pattern` gelten nur für Dateinamen und nicht für den Pfad.  Wenn Sie alle Textdateien in einer Verzeichnisstruktur kopieren möchten, verwenden Sie die Option `–recursive`, um die gesamte Verzeichnisstruktur abzurufen. Verwenden Sie dann `–include-pattern`, und geben Sie `*.txt` an, um alle Textdateien abzurufen.

#### <a name="upload-files-that-were-modified-after-a-date-and-time"></a>Hochladen von Dateien, die nach einem bestimmten Datum und einer bestimmten Uhrzeit geändert wurden 

Verwenden Sie den Befehl [azcopy copy](storage-ref-azcopy-copy.md) mit der Option `--include-after`. Geben Sie ein Datum und eine Uhrzeit im ISO 8601-Format an (z. B. `2020-08-19T15:04:00Z`). 

|    |     |
|--------|-----------|
| **Syntax** | `azcopy copy '<local-directory-path>\*' 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-or-directory-name>'  --include-after <Date-Time-in-ISO-8601-format>` |
| **Beispiel** | `azcopy copy 'C:\myDirectory\*' 'https://mystorageaccount.blob.core.windows.net/mycontainer/FileDirectory'  --include-after '2020-08-19T15:04:00Z'` |
| **Beispiel** (hierarchischer Namespace) | `azcopy copy 'C:\myDirectory\*' 'https://mystorageaccount.dfs.core.windows.net/mycontainer/FileDirectory'   --include-after '2020-08-19T15:04:00Z'` |

Ausführliche Informationen finden Sie in den [azcopy copy](storage-ref-azcopy-copy.md)-Referenzdokumenten.

## <a name="download-files"></a>Herunterladen von Dateien

Sie können den Befehl [azcopy copy](storage-ref-azcopy-copy.md) verwenden, um Blobs, Verzeichnisse und Container auf Ihren lokalen Computer herunterzuladen.

Dieser Abschnitt enthält folgende Beispiele:

> [!div class="checklist"]
> * Herunterladen einer Datei
> * Herunterladen eines Verzeichnisses
> * Herunterladen der Inhalte eines Verzeichnisses
> * Herunterladen bestimmter Dateien

> [!TIP]
> Sie können den Downloadvorgang mit optionalen Flags optimieren. Hier sind einige Beispiele angegeben.
>
> |Szenario|Flag|
> |---|---|
> |Automatisches Dekomprimieren von Dateien|**DECOMPRESS**|
> |Geben Sie an, wie detailliert die kopierbezogenen Protokolleinträge sein sollen.|**--log-level**=\[WARNING\|ERROR\|INFO\|NONE\]|
> |Geben Sie an, ob und wie die in Konflikt stehenden Dateien und Blobs im Ziel überschrieben werden sollen.|**--overwrite**=\[true\|false\|ifSourceNewer\|prompt\]|
> 
> Eine vollständige Liste finden Sie unter [Optionen](storage-ref-azcopy-copy.md#options).

> [!NOTE]
> Wenn der `Content-md5`-Eigenschaftswert eines Blobs einen Hash enthält, berechnet AzCopy einen MD5-Hash für heruntergeladene Daten und überprüft, ob der in der `Content-md5`-Eigenschaft des Blobs gespeicherte MD5-Hash mit dem berechneten Hash übereinstimmt. Wenn diese Werte nicht übereinstimmen, wird der Download nicht durchgeführt, es sei denn, Sie überschreiben dieses Verhalten, indem Sie `--check-md5=NoCheck` oder `--check-md5=LogOnly` an den Kopierbefehl anfügen.

### <a name="download-a-file"></a>Herunterladen einer Datei

|    |     |
|--------|-----------|
| **Syntax** | `azcopy copy 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<blob-path>' '<local-file-path>'` |
| **Beispiel** | `azcopy copy 'https://mystorageaccount.blob.core.windows.net/mycontainer/myTextFile.txt' 'C:\myDirectory\myTextFile.txt'` |
| **Beispiel** (hierarchischer Namespace) | `azcopy copy 'https://mystorageaccount.dfs.core.windows.net/mycontainer/myTextFile.txt' 'C:\myDirectory\myTextFile.txt'` |

### <a name="download-a-directory"></a>Herunterladen eines Verzeichnisses

|    |     |
|--------|-----------|
| **Syntax** | `azcopy copy 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<directory-path>' '<local-directory-path>' --recursive` |
| **Beispiel** | `azcopy copy 'https://mystorageaccount.blob.core.windows.net/mycontainer/myBlobDirectory' 'C:\myDirectory'  --recursive` |
| **Beispiel** (hierarchischer Namespace) | `azcopy copy 'https://mystorageaccount.dfs.core.windows.net/mycontainer/myBlobDirectory' 'C:\myDirectory'  --recursive` |

Dieses Beispiel erstellt ein Verzeichnis namens `C:\myDirectory\myBlobDirectory`, das alle heruntergeladenen Dateien enthält.

### <a name="download-the-contents-of-a-directory"></a>Herunterladen der Inhalte eines Verzeichnisses

Mithilfe des Platzhaltersymbols (*) können Sie die Inhalte eines Verzeichnisses herunterladen, ohne das Verzeichnis selbst zu kopieren.

> [!NOTE]
> Dieses Szenario wird derzeit nur für Konten unterstützt, die keinen hierarchischen Namespace besitzen.

|    |     |
|--------|-----------|
| **Syntax** | `azcopy copy 'https://<storage-account-name>.blob.core.windows.net/<container-name>/*' '<local-directory-path>/'` |
| **Beispiel** | `azcopy copy 'https://mystorageaccount.blob.core.windows.net/mycontainer/myBlobDirectory/*' 'C:\myDirectory'` |

> [!NOTE]
> Fügen Sie das Flag `--recursive` an, um Dateien in allen Unterverzeichnissen herunterzuladen.

### <a name="download-specific-files"></a>Herunterladen bestimmter Dateien

Sie können bestimmte Dateien mithilfe von vollständigen Dateinamen, partiellen Namen mit Platzhalterzeichen (*) oder mithilfe von Datums- und Uhrzeitwerten herunterladen. 

#### <a name="specify-multiple-complete-file-names"></a>Angeben mehrerer vollständiger Dateinamen

Verwenden Sie den Befehl [azcopy copy](storage-ref-azcopy-copy.md) mit der Option `--include-path`. Trennen Sie einzelne Dateinamen durch ein Semikolon (`;`).

|    |     |
|--------|-----------|
| **Syntax** | `azcopy copy 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-or-directory-name>' '<local-directory-path>'  --include-path <semicolon-separated-file-list>` |
| **Beispiel** | `azcopy copy 'https://mystorageaccount.blob.core.windows.net/mycontainer/FileDirectory' 'C:\myDirectory'  --include-path 'photos;documents\myFile.txt' --recursive` |
| **Beispiel** (hierarchischer Namespace) | `azcopy copy 'https://mystorageaccount.dfs.core.windows.net/mycontainer/FileDirectory' 'C:\myDirectory'  --include-path 'photos;documents\myFile.txt'--recursive` |

In diesem Beispiel überträgt AzCopy das Verzeichnis `https://mystorageaccount.blob.core.windows.net/mycontainer/FileDirectory/photos` und die Datei `https://mystorageaccount.blob.core.windows.net/mycontainer/FileDirectory/documents/myFile.txt`. Sie müssen die Option `--recursive` verwenden, um alle Dateien im Verzeichnis `https://mystorageaccount.blob.core.windows.net/mycontainer/FileDirectory/photos` zu übertragen.

Mit der Option `--exclude-path` können Sie auch Dateien ausschließen. Weitere Informationen finden Sie in den Referenzdokumenten zu [azcopy copy](storage-ref-azcopy-copy.md).

#### <a name="use-wildcard-characters"></a>Verwenden von Platzhalterzeichen

Verwenden Sie den Befehl [azcopy copy](storage-ref-azcopy-copy.md) mit der Option `--include-pattern`. Geben Sie Namen mithilfe von Platzhalterzeichen teilweise an. Trennen Sie die Namen durch Semikolons (`;`).

|    |     |
|--------|-----------|
| **Syntax** | `azcopy copy 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-or-directory-name>' '<local-directory-path>' --include-pattern <semicolon-separated-file-list-with-wildcard-characters>` |
| **Beispiel** | `azcopy copy 'https://mystorageaccount.blob.core.windows.net/mycontainer/FileDirectory' 'C:\myDirectory'  --include-pattern 'myFile*.txt;*.pdf*'` |
| **Beispiel** (hierarchischer Namespace) | `azcopy copy 'https://mystorageaccount.dfs.core.windows.net/mycontainer/FileDirectory' 'C:\myDirectory'  --include-pattern 'myFile*.txt;*.pdf*'` |

Mit der Option `--exclude-pattern` können Sie auch Dateien ausschließen. Weitere Informationen finden Sie in den Referenzdokumenten zu [azcopy copy](storage-ref-azcopy-copy.md).

Die Optionen `--include-pattern` und `--exclude-pattern` gelten nur für Dateinamen und nicht für den Pfad.  Wenn Sie alle Textdateien in einer Verzeichnisstruktur kopieren möchten, verwenden Sie die Option `–recursive`, um die gesamte Verzeichnisstruktur abzurufen. Verwenden Sie dann `–include-pattern`, und geben Sie `*.txt` an, um alle Textdateien abzurufen.

#### <a name="download-files-that-were-modified-after-a-date-and-time"></a>Herunterladen von Dateien, die nach einem bestimmten Datum und einer bestimmten Uhrzeit geändert wurden 

Verwenden Sie den Befehl [azcopy copy](storage-ref-azcopy-copy.md) mit der Option `--include-after`. Geben Sie ein Datum und eine Uhrzeit im ISO 8601-Format an (z. B. `2020-08-19T15:04:00Z`). 

|    |     |
|--------|-----------|
| **Syntax** | `azcopy copy 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-or-directory-name>/*' '<local-directory-path>' --include-after <Date-Time-in-ISO-8601-format>` |
| **Beispiel** | `azcopy copy 'https://mystorageaccount.blob.core.windows.net/mycontainer/FileDirectory/*' 'C:\myDirectory'  --include-after '2020-08-19T15:04:00Z'` |
| **Beispiel** (hierarchischer Namespace) | `azcopy copy 'https://mystorageaccount.dfs.core.windows.net/mycontainer/FileDirectory/*' 'C:\myDirectory'  --include-after '2020-08-19T15:04:00Z'` |

Ausführliche Informationen finden Sie in den [azcopy copy](storage-ref-azcopy-copy.md)-Referenzdokumenten.

#### <a name="download-previous-versions-of-a-blob"></a>Herunterladen früherer Versionen eines Blobs

Wenn Sie die [Blobversionsverwaltung](../blobs/versioning-enable.md) aktiviert haben, können Sie frühere Versionen eines Blobs herunterladen. 

Erstellen Sie zunächst eine Textdatei, die eine Liste mit [Versions-IDs](../blobs/versioning-overview.md) enthält. Jede Versions-ID muss in einer eigenen Zeile stehen. Beispiel: 

```
2020-08-17T05:50:34.2199403Z
2020-08-17T05:50:34.5041365Z
2020-08-17T05:50:36.7607103Z
```

Verwenden Sie dann den Befehl [azcopy copy](storage-ref-azcopy-copy.md) mit der Option `--list-of-versions`. Geben Sie den Speicherort der Textdatei mit der Liste der Versionen an (z. B. `D:\\list-of-versions.txt`).  

|    |     |
|--------|-----------|
| **Syntax** | `azcopy copy 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<blob-path>' '<local-directory-path>' --list-of-versions '<list-of-versions-file>'`|
| **Beispiel** | `azcopy copy 'https://mystorageaccount.blob.core.windows.net/mycontainer/myTextFile.txt' 'C:\myDirectory\myTextFile.txt' --list-of-versions 'D:\\list-of-versions.txt'` |
| **Beispiel** (hierarchischer Namespace) | `azcopy copy 'https://mystorageaccount.dfs.core.windows.net/mycontainer/myTextFile.txt' 'C:\myDirectory\myTextFile.txt' --list-of-versions 'D:\\list-of-versions.txt'` |

Der Name jeder heruntergeladenen Datei beginnt mit der Versions-ID, gefolgt vom Namen des Blobs. 

## <a name="copy-blobs-between-storage-accounts"></a>Kopieren von Blobs zwischen Speicherkonten

Sie können AzCopy verwenden, um Blobs in andere Speicherkonten zu kopieren. Der Kopiervorgang verläuft synchron – wenn also der Befehl zurückgegeben wird, bedeutet dies, dass alle Dateien kopiert wurden. 

AzCopy verwendet die [Server-zu-Server](/rest/api/storageservices/put-block-from-url)-[APIs](/rest/api/storageservices/put-page-from-url), sodass Daten direkt zwischen Speicherservern kopiert werden. Für diese Kopiervorgänge wird nicht die Netzwerkbandbreite Ihres Computers genutzt. Sie können den Durchsatz dieser Vorgänge erhöhen, indem Sie den Wert der Umgebungsvariable `AZCOPY_CONCURRENCY_VALUE` festlegen. Weitere Informationen finden Sie unter [Optimieren des Durchsatzes](storage-use-azcopy-configure.md#optimize-throughput).

> [!NOTE]
> In diesem Szenario gelten für das aktuelle Release die folgenden Einschränkungen.
>
> - An jede Quell-URL muss ein SAS-Token angefügt werden. Wenn Sie Autorisierungsanmeldeinformationen unter Verwendung von Azure AD (Active Directory) angeben, können Sie das SAS-Token bei der Ziel-URL weglassen. Stellen Sie sicher, dass Sie die richtigen Rollen in Ihrem Zielkonto eingerichtet haben. Weitere Informationen finden Sie unter [Option 1: Verwenden von Azure Active Directory](storage-use-azcopy-v10.md?toc=/azure/storage/blobs/toc.json#option-1-use-azure-active-directory).
>-  Premium-Blockblob-Speicherkonten unterstützen keine Zugriffsebenen. Lassen Sie die Zugriffsebene eines Blobs im Kopiervorgang aus, indem Sie `s2s-preserve-access-tier` auf `false` festlegen (z. B. `--s2s-preserve-access-tier=false`).

Dieser Abschnitt enthält folgende Beispiele:

> [!div class="checklist"]
> * Kopieren eines Blobs in ein anderes Speicherkonto
> * Kopieren eines Verzeichnisses in ein anderes Speicherkonto
> * Kopieren eines Containers in ein anderes Speicherkonto
> * Kopieren aller Container, Verzeichnisse und Dateien in ein anderes Speicherkonto

Diese Beispiele können auch für Konten mit einem hierarchischen Namespace verwendet werden. [Multiprotokollzugriff für Data Lake Storage](../blobs/data-lake-storage-multi-protocol-access.md) ermöglicht es Ihnen, dieselbe URL-Syntax (`blob.core.windows.net`) für diese Konten zu verwenden.

> [!TIP]
> Sie können den Kopiervorgang mit optionalen Flags optimieren. Hier sind einige Beispiele angegeben.
>
> |Szenario|Flag|
> |---|---|
> |Kopieren Sie Blobs als Block-, Seiten- oder Anfügeblobs.|**--blob-type**=\[BlockBlob\|PageBlob\|AppendBlob\]|
> |Kopieren auf eine bestimmte Zugriffsebene (z. B. die Archivebene)|**--block-blob-tier**=\[None\|Hot\|Cool\|Archive\]|
> |Dateien sollen automatisch dekomprimiert werden.|**--decompress**=\[gzip\|deflate\]|
> 
> Eine vollständige Liste finden Sie unter [Optionen](storage-ref-azcopy-copy.md#options).

### <a name="copy-a-blob-to-another-storage-account"></a>Kopieren eines Blobs in ein anderes Speicherkonto

Verwenden Sie dieselbe URL-Syntax (`blob.core.windows.net`) für Konten mit einem hierarchischen Namespace.

|    |     |
|--------|-----------|
| **Syntax** | `azcopy copy 'https://<source-storage-account-name>.blob.core.windows.net/<container-name>/<blob-path><SAS-token>' 'https://<destination-storage-account-name>.blob.core.windows.net/<container-name>/<blob-path>'` |
| **Beispiel** | `azcopy copy 'https://mysourceaccount.blob.core.windows.net/mycontainer/myTextFile.txt?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'https://mydestinationaccount.blob.core.windows.net/mycontainer/myTextFile.txt'` |
| **Beispiel** (hierarchischer Namespace) | `azcopy copy 'https://mysourceaccount.blob.core.windows.net/mycontainer/myTextFile.txt?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'https://mydestinationaccount.blob.core.windows.net/mycontainer/myTextFile.txt'` |

### <a name="copy-a-directory-to-another-storage-account"></a>Kopieren eines Verzeichnisses in ein anderes Speicherkonto

Verwenden Sie dieselbe URL-Syntax (`blob.core.windows.net`) für Konten mit einem hierarchischen Namespace.

|    |     |
|--------|-----------|
| **Syntax** | `azcopy copy 'https://<source-storage-account-name>.blob.core.windows.net/<container-name>/<directory-path><SAS-token>' 'https://<destination-storage-account-name>.blob.core.windows.net/<container-name>' --recursive` |
| **Beispiel** | `azcopy copy 'https://mysourceaccount.blob.core.windows.net/mycontainer/myBlobDirectory?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'https://mydestinationaccount.blob.core.windows.net/mycontainer' --recursive` |
| **Beispiel** (hierarchischer Namespace)| `azcopy copy 'https://mysourceaccount.blob.core.windows.net/mycontainer/myBlobDirectory?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'https://mydestinationaccount.blob.core.windows.net/mycontainer' --recursive` |

### <a name="copy-a-container-to-another-storage-account"></a>Kopieren eines Containers in ein anderes Speicherkonto

Verwenden Sie dieselbe URL-Syntax (`blob.core.windows.net`) für Konten mit einem hierarchischen Namespace.

|    |     |
|--------|-----------|
| **Syntax** | `azcopy copy 'https://<source-storage-account-name>.blob.core.windows.net/<container-name><SAS-token>' 'https://<destination-storage-account-name>.blob.core.windows.net/<container-name>' --recursive` |
| **Beispiel** | `azcopy copy 'https://mysourceaccount.blob.core.windows.net/mycontainer?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'https://mydestinationaccount.blob.core.windows.net/mycontainer' --recursive` |
| **Beispiel** (hierarchischer Namespace)| `azcopy copy 'https://mysourceaccount.blob.core.windows.net/mycontainer?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'https://mydestinationaccount.blob.core.windows.net/mycontainer' --recursive` |

### <a name="copy-all-containers-directories-and-blobs-to-another-storage-account"></a>Kopieren aller Container, Verzeichnisse und Blobs in ein anderes Speicherkonto

Verwenden Sie dieselbe URL-Syntax (`blob.core.windows.net`) für Konten mit einem hierarchischen Namespace.

|    |     |
|--------|-----------|
| **Syntax** | `azcopy copy 'https://<source-storage-account-name>.blob.core.windows.net/<SAS-token>' 'https://<destination-storage-account-name>.blob.core.windows.net/' --recursive` |
| **Beispiel** | `azcopy copy 'https://mysourceaccount.blob.core.windows.net/?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'https://mydestinationaccount.blob.core.windows.net' --recursive` |
| **Beispiel** (hierarchischer Namespace)| `azcopy copy 'https://mysourceaccount.blob.core.windows.net/?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'https://mydestinationaccount.blob.core.windows.net' --recursive` |

## <a name="synchronize-files"></a>Synchronisieren von Dateien

Sie können die Inhalte eines lokalen Dateisystems mit einem Blobcontainer synchronisieren. Sie können auch Container und virtuelle Verzeichnisse miteinander synchronisieren. Die Synchronisierung erfolgt unidirektional. Anders gesagt: Sie wählen aus, welcher der beiden Endpunkte die Quelle und welcher das Ziel ist. Bei der Synchronisierung werden auch Server-zu-Server-APIs verwendet. Die Beispiele in diesem Abschnitt können auch für Konten verwendet werden, die über einen hierarchischen Namespace verfügen. 

> [!NOTE]
> Das aktuelle Release von AzCopy synchronisiert nicht zwischen anderen Quellen und Zielen (beispielsweise: Dateispeicher oder Amazon Web Services (AWS) S3-Buckets).

Mit dem Befehl [sync](storage-ref-azcopy-sync.md) werden Dateinamen und die Zeitstempel der letzten Änderung verglichen. Legen Sie das optionale Flag `--delete-destination` auf den Wert `true` oder `prompt` fest, um Dateien im Zielverzeichnis zu löschen, wenn diese im Quellverzeichnis nicht mehr vorhanden sind.

Wenn Sie das `--delete-destination`-Flag auf `true` festlegen, löscht AzCopy Dateien, ohne zur Bestätigung aufzufordern. Wenn eine Bestätigungsaufforderung angezeigt werden soll, bevor AzCopy eine Datei löscht, legen Sie das `--delete-destination`-Flag auf `prompt` fest.

> [!NOTE]
> Um ein versehentliches Löschen zu verhindern, aktivieren Sie das Feature [Vorläufiges Löschen](../blobs/soft-delete-blob-overview.md), bevor Sie das Flag `--delete-destination=prompt|true` verwenden.

> [!TIP]
> Sie können den Synchronisierungsvorgang mit optionalen Flags optimieren. Hier sind einige Beispiele angegeben.
>
> |Szenario|Flag|
> |---|---|
> |Angeben, wie streng MD5-Hashes beim Herunterladen überprüft werden sollen|**--check-md5**=\[NoCheck\|LogOnly\|FailIfDifferent\|FailIfDifferentOrMissing\]|
> |Ausschließen von Dateien basierend auf einem Muster|**--exclude-path**|
> |Angeben, wie detailliert die synchronisierungsbezogenen Protokolleinträge sein sollen|**--log-level**=\[WARNING\|ERROR\|INFO\|NONE\]|
> 
> Eine vollständige Liste finden Sie unter [Optionen](storage-ref-azcopy-sync.md#options).

### <a name="update-a-container-with-changes-to-a-local-file-system"></a>Aktualisieren eines Containers mit Änderungen an einem lokalen Dateisystem

In diesem Fall ist der Container das Ziel und das lokale Dateisystem die Quelle. 

|    |     |
|--------|-----------|
| **Syntax** | `azcopy sync '<local-directory-path>' 'https://<storage-account-name>.blob.core.windows.net/<container-name>' --recursive` |
| **Beispiel** | `azcopy sync 'C:\myDirectory' 'https://mystorageaccount.blob.core.windows.net/mycontainer' --recursive` |

### <a name="update-a-local-file-system-with-changes-to-a-container"></a>Aktualisieren eines lokalen Dateisystems mit Änderungen an einem Container

In diesem Fall ist das lokale Dateisystem das Ziel und der Container die Quelle.

|    |     |
|--------|-----------|
| **Syntax** | `azcopy sync 'https://<storage-account-name>.blob.core.windows.net/<container-name>' 'C:\myDirectory' --recursive` |
| **Beispiel** | `azcopy sync 'https://mystorageaccount.blob.core.windows.net/mycontainer' 'C:\myDirectory' --recursive` |

### <a name="update-a-container-with-changes-in-another-container"></a>Aktualisieren eines Containers mit Änderungen in einem anderen Container

Der erste Container in diesem Befehl ist die Quelle. Das zweite ist das Ziel.

|    |     |
|--------|-----------|
| **Syntax** | `azcopy sync 'https://<source-storage-account-name>.blob.core.windows.net/<container-name>' 'https://<destination-storage-account-name>.blob.core.windows.net/<container-name>' --recursive` |
| **Beispiel** | `azcopy sync 'https://mysourceaccount.blob.core.windows.net/mycontainer' 'https://mydestinationaccount.blob.core.windows.net/mycontainer' --recursive` |

### <a name="update-a-directory-with-changes-to-a-directory-in-another-file-share"></a>Aktualisieren eines Verzeichnisses mit Änderungen in einem Verzeichnis in einer anderen Dateifreigabe

Das erste Verzeichnis in diesem Befehl ist die Quelle. Das zweite ist das Ziel.

|    |     |
|--------|-----------|
| **Syntax** | `azcopy sync 'https://<source-storage-account-name>.blob.core.windows.net/<container-name>/<directory-name>' 'https://<destination-storage-account-name>.blob.core.windows.net/<container-name>/<directory-name>' --recursive` |
| **Beispiel** | `azcopy sync 'https://mysourceaccount.blob.core.windows.net/<container-name>/myDirectory' 'https://mydestinationaccount.blob.core.windows.net/mycontainer/myDirectory' --recursive` |

## <a name="next-steps"></a>Nächste Schritte

Weitere Beispiele finden Sie in den folgenden Artikeln:

- [Erste Schritte mit AzCopy](storage-use-azcopy-v10.md)

- [Tutorial: Migrieren von lokalen Daten zum Cloudspeicher mithilfe von AzCopy](storage-use-azcopy-migrate-on-premises-data.md)

- [Übertragen von Daten mit AzCopy und Dateispeicher](storage-use-azcopy-files.md)

- [Übertragen von Daten mit AzCopy und Amazon S3-Buckets](storage-use-azcopy-s3.md)

- [Konfigurieren, Optimieren und Problembehandlung in AzCopy](storage-use-azcopy-configure.md)