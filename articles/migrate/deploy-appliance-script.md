---
title: Einrichten einer Azure Migrate-Appliance mit einem Skript
description: Erfahren Sie, wie Sie eine Azure Migrate-Appliance mit einem Skript einrichten.
ms.topic: how-to
author: vineetvikram
ms.author: vivikram
ms.manager: abhemraj
ms.date: 03/18/2021
ms.openlocfilehash: dfa96948b7e582457a9f09eed89d5cbe3bbc762d
ms.sourcegitcommit: 1b19b8d303b3abe4d4d08bfde0fee441159771e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/11/2021
ms.locfileid: "109750391"
---
# <a name="set-up-an-appliance-with-a-script"></a>Einrichten einer Appliance mit einem Skript

In diesem Artikel erstellen Sie eine [Azure Migrate-Appliance](./migrate-appliance-architecture.md) für die Bewertung/Migration von Servern mit VMware und Hyper-V. Sie führen ein Skript aus, um die Appliance zu erstellen und zu überprüfen, ob sie eine Verbindung mit Azure herstellen kann. 

Sie können die Appliance für Server mit VMware und Hyper-V mithilfe eines Skripts bereitstellen oder eine Vorlage nutzen, die Sie im Azure-Portal herunterladen. Die Verwendung eines Skripts ist sinnvoll, wenn Sie mit der heruntergeladenen Vorlage keine Appliance erstellen können.

- Wenn Sie eine Vorlage verwenden möchten, folgen Sie den Tutorials für [VMware](./tutorial-discover-vmware.md) oder [Hyper-V](./tutorial-discover-hyper-v.md).
- Für die Einrichtung einer Appliance für physische Server kann ausschließlich ein Skript verwendet werden. Folgen Sie den Anweisungen in [diesem Artikel](how-to-set-up-appliance-physical.md).
- Wenn Sie eine Appliance in einer Azure Government-Cloud einrichten möchten, lesen Sie [diesen Artikel](deploy-appliance-script-government.md).

## <a name="prerequisites"></a>Voraussetzungen

Das Skript richtet die Azure Migrate-Appliance auf einem vorhandenen Server ein.

- Der Server, der als Appliance fungieren soll, muss die folgenden Hardware- und Betriebssystemanforderungen erfüllen:

Szenario | Requirements (Anforderungen)
--- | ---
VMware | Windows Server 2016 mit 32 GB Arbeitsspeicher, acht vCPUs, ungefähr 80 GB Speicherplatz auf dem Datenträger
Hyper-V | Windows Server 2016 mit 16 GB Arbeitsspeicher, acht vCPUs, ungefähr 80 GB Speicherplatzauf dem Datenträger

- Der Server benötigt auch einen externen virtuellen Switch. Er benötigt eine statische oder dynamische IP-Adresse. 
- Überprüfen Sie vor der Bereitstellung der Appliance ihre detaillierten Anforderungen für [Server mit VMware](migrate-appliance.md#appliance---vmware) und [mit Hyper-V](migrate-appliance.md#appliance---hyper-v).
- Führen Sie das Skript nicht auf einer vorhandenen Azure Migrate-Appliance aus.

## <a name="set-up-the-appliance-for-vmware"></a>Einrichten der Appliance für VMware

Zum Einrichten der Appliance für VMware laden Sie die ZIP-Datei mit dem Namen „AzureMigrateInstaller.zip“ entweder aus dem Portal oder [hier](https://go.microsoft.com/fwlink/?linkid=2140334) herunter, und extrahieren Sie den Inhalt. Zum Starten der Appliance-Web-App führen Sie das PowerShell-Skript aus. Dann richten Sie die Appliance ein und führen die Erstkonfiguration aus. Anschließend registrieren Sie die Appliance beim Projekt.

### <a name="verify-file-security"></a>Überprüfen der Dateisicherheit

Vergewissern Sie sich vor der Bereitstellung, dass die gezippte Datei sicher ist.

1. Öffnen Sie auf dem Server, auf den Sie die Datei heruntergeladen haben, ein Administratorbefehlsfenster.
2. Führen Sie den folgenden Befehl aus, um den Hash für die gezippte Datei zu generieren:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Beispiel: ```C:\>CertUtil -HashFile C:\Users\administrator\Desktop\AzureMigrateInstaller-VMware-Public.zip SHA256```
3. Überprüfen Sie die neueste Geräteversion und das Skript für die öffentliche Azure-Cloud:

    **Algorithmus** | **Download** | **SHA256**
    --- | --- | ---
    VMware (85,8 MB) | [Aktuelle Version](https://go.microsoft.com/fwlink/?linkid=2116601) | 85b74d93dfcee43412386141808d82147916330e6669df94c7969fe1b3d0fe72

### <a name="run-the-script"></a>Führen Sie das Skript aus.

Funktion des Skripts:

- Installation von Agents und einer Webanwendung.
- Installation von Windows-Rollen, darunter beispielsweise Windows-Aktivierungsdienst, IIS und PowerShell ISE.
- Download und Installation eines wiederbeschreibbaren IIS-Moduls.
- Aktualisierung eines Registrierungsschlüssels (HKLM) mit dauerhaften Einstellungen für Azure Migrate.
- Erstellen von Protokoll- und Konfigurationsdateien wie folgt:
    - **Konfigurationsdateien**: %ProgramData%\Microsoft Azure\Config
    - **Protokolldateien**: %ProgramData%\Microsoft Azure\Logs

So führen Sie das Skript aus

1. Extrahieren Sie die gezippte Datei in einem Ordner auf dem Server, der die Appliance hostet. Führen Sie das Skript nicht auf einer vorhandenen Azure Migrate-Appliance aus.
2. Starten Sie PowerShell auf dem Server mit Administratorberechtigungen (erhöhten Rechten).
3. Ändern Sie das PowerShell-Verzeichnis in den Ordner mit den Inhalten, die aus der gezippten Datei extrahiert wurden, die Sie heruntergeladen haben.
4. Führen Sie das Skript **AzureMigrateInstaller.ps1** wie folgt aus:

    ``` PS C:\Users\administrator\Desktop\AzureMigrateInstaller-Server-Public> .\AzureMigrateInstaller.ps1 -scenario VMware ```
  
5. Nachdem das Skript erfolgreich ausgeführt wurde, wird die Appliancewebanwendung gestartet, damit Sie das Gerät einrichten können. Bei Problemen sehen Sie sich die Skriptprotokolle in diesem Pfad an: C:\ProgramData\Microsoft Azure\Logs\AzureMigrateScenarioInstaller_<em>Zeitstempel</em>.log.

### <a name="verify-access"></a>Überprüfen des Zugriffs

Stellen Sie sicher, dass die Appliance eine Verbindung mit Azure-URLs für die [öffentliche](migrate-appliance.md#public-cloud-urls) Cloud herstellen kann.

## <a name="set-up-the-appliance-for-hyper-v"></a>Einrichten der Appliance für Hyper-V

Zum Einrichten der Appliance für Hyper-V laden Sie die ZIP-Datei mit dem Namen „AzureMigrateInstaller.zip“ entweder aus dem Portal oder [hier](https://go.microsoft.com/fwlink/?linkid=2105112) herunter, und extrahieren Sie den Inhalt. Zum Starten der Appliance-Web-App führen Sie das PowerShell-Skript aus. Dann richten Sie die Appliance ein und führen die Erstkonfiguration aus. Anschließend registrieren Sie die Appliance beim Projekt.


### <a name="verify-file-security"></a>Überprüfen der Dateisicherheit

Vergewissern Sie sich vor der Bereitstellung, dass die gezippte Datei sicher ist.

1. Öffnen Sie auf dem Server, auf den Sie die Datei heruntergeladen haben, ein Administratorbefehlsfenster.
2. Führen Sie den folgenden Befehl aus, um den Hash für die gezippte Datei zu generieren:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Beispiel: ```C:\>CertUtil -HashFile C:\Users\administrator\Desktop\AzureMigrateInstaller-Server-HyperV.zip SHA256```

3. Überprüfen Sie die neueste Geräteversion und das Skript für die öffentliche Azure-Cloud:

    **Szenario** | **Download** | **SHA256**
    --- | --- | ---
    Hyper-V (85,8 MB) | [Aktuelle Version](https://go.microsoft.com/fwlink/?linkid=2116657) |  9bbef62e2e22481eda4b77c7fdf05db98c3767c20f0a873114fb0dcfa6ed682a

### <a name="run-the-script"></a>Führen Sie das Skript aus.

Funktion des Skripts:

- Installation von Agents und einer Webanwendung.
- Installation von Windows-Rollen, darunter beispielsweise Windows-Aktivierungsdienst, IIS und PowerShell ISE.
- Download und Installation eines wiederbeschreibbaren IIS-Moduls.
- Aktualisierung eines Registrierungsschlüssels (HKLM) mit dauerhaften Einstellungen für Azure Migrate.
- Erstellen von Protokoll- und Konfigurationsdateien wie folgt:
    - **Konfigurationsdateien**: %ProgramData%\Microsoft Azure\Config
    - **Protokolldateien**: %ProgramData%\Microsoft Azure\Logs

So führen Sie das Skript aus

1. Extrahieren Sie die gezippte Datei in einem Ordner auf dem Server, der die Appliance hostet. Führen Sie das Skript nicht auf einer vorhandenen Azure Migrate-Appliance aus.
2. Starten Sie PowerShell auf dem Server mit Administratorberechtigungen (erhöhten Rechten).
3. Ändern Sie das PowerShell-Verzeichnis in den Ordner mit den Inhalten, die aus der gezippten Datei extrahiert wurden, die Sie heruntergeladen haben.
4. Führen Sie das Skript **AzureMigrateInstaller.ps1** wie folgt aus:

    ``` PS C:\Users\administrator\Desktop\AzureMigrateInstaller-Server-Public> .\AzureMigrateInstaller.ps1 -scenario Hyperv ```
   
5. Nachdem das Skript erfolgreich ausgeführt wurde, wird die Appliancewebanwendung gestartet, damit Sie das Gerät einrichten können. Bei Problemen sehen Sie sich die Skriptprotokolle in diesem Pfad an: C:\ProgramData\Microsoft Azure\Logs\AzureMigrateScenarioInstaller_<em>Zeitstempel</em>.log.

### <a name="verify-access"></a>Überprüfen des Zugriffs

Stellen Sie sicher, dass die Appliance eine Verbindung mit Azure-URLs für die [öffentliche](migrate-appliance.md#public-cloud-urls) Cloud herstellen kann.

## <a name="next-steps"></a>Nächste Schritte

Nach dem Bereitstellen der Appliance müssen Sie die Erstkonfiguration ausführen und die Appliance beim Projekt registrieren.

- Richten Sie die Appliance für [VMware](how-to-set-up-appliance-vmware.md#4-configure-the-appliance) ein.
- Richten Sie die Appliance für [Hyper-V](how-to-set-up-appliance-hyper-v.md#configure-the-appliance) ein.
