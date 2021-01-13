---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/04/2019
ms.author: alkohli
ms.openlocfilehash: 3312d1ec7c2535e103cf8959599c0d4c3014f520
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "86218316"
---
1. [Herstellen einer Verbindung mit der PowerShell-Schnittstelle](#connect-to-the-powershell-interface).
2. Verwenden Sie `Get-HcsApplianceInfo` zum Abrufen der Informationen für Ihr Gerät.

    Nachfolgend sehen Sie ein Beispiel für die Verwendung dieses Cmdlets:

    ```
    [10.100.10.10]: PS>Get-HcsApplianceInfo
    
    Id                            : b2044bdb-56fd-4561-a90b-407b2a67bdfc
    FriendlyName                  : DBE-NBSVFQR94S6
    Name                          : DBE-NBSVFQR94S6
    SerialNumber                  : HCS-NBSVFQR94S6
    DeviceId                      : 40d7288d-cd28-481d-a1ea-87ba9e71ca6b
    Model                         : Virtual
    FriendlySoftwareVersion       : Data Box Gateway 1902
    HcsVersion                    : 1.4.771.324
    IsClustered                   : False
    IsVirtual                     : True
    LocalCapacityInMb             : 1964992
    SystemState                   : Initialized
    SystemStatus                  : Normal
    Type                          : DataBoxGateway
    CloudReadRateBytesPerSec      : 0
    CloudWriteRateBytesPerSec     : 0
    IsInitialPasswordSet          : True
    FriendlySoftwareVersionNumber : 1902
    UploadPolicy                  : All
    DataDiskResiliencySettingName : Simple
    ApplianceTypeFriendlyName     : Data Box Gateway
    IsRegistered                  : False
    ```

    Hier sehen Sie eine Tabelle mit einer Zusammenfassung einiger wichtiger Geräteinformationen:

    | Parameter | BESCHREIBUNG |
    |-----------|-------------|
    | FriendlyName                   | Dies ist der Anzeigename des Geräts gemäß Konfiguration über die lokale Web-UI während der Gerätebereitstellung. Der Standardanzeigename ist die Seriennummer des Geräts.  |
    | SerialNumber                   | Die Seriennummer des Geräts ist eine eindeutige Nummer, die im Werk zugewiesen wird.                                                                             |
    | Modell                          | Das Modell für Ihr Azure Stack Edge- oder Data Box Gateway-Gerät. Das Modell ist für Azure Stack Edge physisch und für Data Box Gateway virtuell.                   |
    | FriendlySoftwareVersion        | Die benutzerfreundliche Zeichenfolge, die der Version der Gerätesoftware entspricht. Für ein System, auf dem eine Vorschau ausgeführt wird, lautet der Anzeigename der Softwareversion „Data Box Edge 1902“. |
    | HcsVersion                     | Die Version der HCS-Software, die auf Ihrem Gerät ausgeführt wird. Beispielsweise lautet die HCS-Softwareversion, die Data Box Edge 1902 entspricht, „1.4.771.324“.            |
    | LocalCapacityInMb              | Die lokale Gesamtkapazität des Geräts in MB.                                                                                                        |
    | IsRegistered                   | Dieser Wert gibt an, ob Ihr Gerät mit dem Dienst aktiviert wurde.                                                                                         |


