---
title: Konfigurieren benutzerdefinierter Einstellungen
description: Konfigurieren Sie Einstellungen, die für die gesamte Azure App Service-Umgebung gelten. Hier erfahren Sie, wie Sie dazu Azure Resource Manager-Vorlagen verwenden.
author: stefsch
ms.assetid: 1d1d85f3-6cc6-4d57-ae1a-5b37c642d812
ms.topic: tutorial
ms.date: 10/03/2020
ms.author: stefsch
ms.custom: mvc, seodec18
ms.openlocfilehash: 88163c07d570df5e0ff343776c17c463010ce368
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "91713292"
---
# <a name="custom-configuration-settings-for-app-service-environments"></a>Benutzerdefinierte Konfigurationseinstellungen für App Service-Umgebungen
## <a name="overview"></a>Übersicht
Da App Service-Umgebungen (App Service Environments, ASEs) isoliert für einen einzelnen Kunden eingerichtet werden, gibt es bestimmte Konfigurationseinstellungen, die exklusiv auf eine App Service-Umgebung angewendet werden können. In diesem Artikel werden die verschiedenen Anpassungen dokumentiert, die für App Service-Umgebungen verfügbar sind.

Informationen zum Erstellen einer App Service-Umgebung finden Sie [hier](app-service-web-how-to-create-an-app-service-environment.md).

Sie können Anpassungen der App Service-Umgebung mithilfe eines Arrays im neuen Attribut **clusterSettings** speichern. Dieses Attribut befindet sich im Wörterbuch „Eigenschaften“ der Azure Resource Manager-Entität *hostingEnvironments* .

Der folgende gekürzte Codeausschnitt aus einer Resource Manager-Vorlage zeigt das Attribut **clusterSettings** :

```json
"resources": [
{
    "apiVersion": "2015-08-01",
    "type": "Microsoft.Web/hostingEnvironments",
    "name": ...,
    "location": ...,
    "properties": {
        "clusterSettings": [
            {
                "name": "nameOfCustomSetting",
                "value": "valueOfCustomSetting"
            }
        ],
        "workerPools": [ ...],
        etc...
    }
}
```

Das Attribut **clusterSettings** kann in eine Resource Manager-Vorlage eingefügt werden, um die App Service-Umgebung zu aktualisieren.

## <a name="use-azure-resource-explorer-to-update-an-app-service-environment"></a>Aktualisieren einer App Service-Umgebung mit dem Azure-Ressourcen-Explorer
Alternativ dazu können Sie die App Service-Umgebung mit dem [Azure-Ressourcen-Explorer](https://resources.azure.com)aktualisieren.  

1. Navigieren Sie im Ressourcen-Explorer zum Knoten für die App Service-Umgebung (**subscriptions** > **resourceGroups** > **providers** > **Microsoft.Web** > **hostingEnvironments**). Klicken Sie dann auf die App Service-Umgebung, die aktualisiert werden soll.
2. Klicken Sie im rechten Bereich in der oberen Symbolleiste auf **Lesen/Schreiben** , um eine interaktive Bearbeitung im Ressourcen-Explorer zuzulassen.  
3. Klicken Sie dann auf die blaue Schaltfläche **Bearbeiten** , um die Bearbeitung der Resource Manager-Vorlage zu ermöglichen.
4. Scrollen Sie zum Ende des rechten Bereichs. Das Attribut **clusterSettings** wird ganz unten angezeigt. Sie können entweder einen Attributwert eingeben oder den vorhandenen Wert aktualisieren.
5. Geben Sie das Array der Konfigurationswerte ein, die Sie für das Attribut **clusterSettings** verwenden möchten. Alternativ können Sie die Werte auch kopieren und einfügen.  
6. Klicken Sie dann oben im rechten Bereich auf die grüne Schaltfläche **PUT** , um einen Commit für die Änderungen an der App Service-Umgebung auszuführen.

Unabhängig davon, wie Sie die Änderung übermitteln, dauert es ungefähr 30 Minuten, multipliziert mit der Anzahl der Front-Ends in der App Service-Umgebung, bis die Änderung wirksam wird.
Wenn eine App Service-Umgebung beispielsweise über vier Front-Ends verfügt, dauert es etwa zwei Stunden, bis die Konfigurationsaktualisierung abgeschlossen ist. Während die Konfigurationsänderung angewendet wird, können keine weiteren Skalierungsvorgänge oder Konfigurationsänderungen in der App Service-Umgebung ausgeführt werden.

## <a name="enable-internal-encryption"></a>Aktivieren der internen Verschlüsselung

Die App Service-Umgebung wird als Blackbox-System betrieben, bei dem Sie die internen Komponenten oder die Kommunikation innerhalb des Systems nicht sehen können. Um einen höheren Durchsatz zu ermöglichen, ist die Verschlüsselung zwischen internen Komponente standardmäßig nicht aktiviert. Das System ist sicher, da der Datenverkehr vollständig vor Überwachung und Zugriff geschützt ist. Bei einer Kompatibilitätsanforderung, die eine End-to-End-Verschlüsselung des Datenpfads voraussetzt, kann diese mit einer Clustereinstellung (clusterSetting) aktiviert werden.  

```json
"clusterSettings": [
    {
        "name": "InternalEncryption",
        "value": "true"
    }
],
```
Dadurch werden der interne Netzwerkdatenverkehr in Ihrer App Service-Umgebung (ASE) zwischen den Front-Ends und Workern, die Auslagerungsdatei und die Workerdatenträger verschlüsselt. Nach der Aktivierung der Clustereinstellung (clusterSetting) „InternalEncryption“ kann es zu einer Beeinträchtigung der Systemleistung kommen. Wenn Sie „InternalEncryption“ aktivieren, befindet sich die ASE in einem instabilen Zustand, bis die Änderung vollständig weitergegeben wurde. Die vollständige Weitergabe der Änderung kann abhängig von der Anzahl von Instanzen in Ihrer ASE einige Stunden in Anspruch nehmen. Es wird dringend empfohlen, diese Option nicht für eine ASE zu aktivieren, während sie verwendet wird. Wenn Sie die Option für eine aktiv genutzte ASE aktivieren müssen, wird dringend empfohlen, den Datenverkehr an eine Sicherungsumgebung umzuleiten, bis der Vorgang abgeschlossen ist. 


## <a name="disable-tls-10-and-tls-11"></a>Deaktivieren von TLS 1.0 und TLS 1.1

Wenn Sie TLS-Einstellungen auf App-Basis verwalten möchten, können Sie die Anleitung aus der Dokumentation [Erzwingen von TLS-Versionen](../configure-ssl-bindings.md#enforce-tls-versions) verwenden. 

Wenn Sie sämtlichen eingehenden TLS 1.0- und TLS 1.1-Datenverkehr für alle Apps in einer ASE deaktivieren möchten, legen Sie den folgenden **clusterSettings**-Eintrag fest:

```json
"clusterSettings": [
    {
        "name": "DisableTls1.0",
        "value": "1"
    }
],
```

Im Namen der Einstellung ist zwar 1.0 angegeben, sie deaktiviert jedoch sowohl TLS 1.0 als auch TLS 1.1, nachdem sie konfiguriert wurde.

## <a name="change-tls-cipher-suite-order"></a>Ändern der Reihenfolge der TLS-Verschlüsselungssammlung
Eine weitere Frage von Kunden lautet, ob sie die Liste der Verschlüsselungsverfahren ändern können, die von ihrem Server ausgehandelt wird. Dies ist, wie im Folgenden gezeigt, durch Ändern der Einstellung **clusterSettings** möglich. Die Liste der verfügbaren Verschlüsselungssammlungen kann aus [diesem MSDN-Artikel](https://msdn.microsoft.com/library/windows/desktop/aa374757\(v=vs.85\).aspx) abgerufen werden.

```json
"clusterSettings": [
    {
        "name": "FrontEndSSLCipherSuiteOrder",
        "value": "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384_P256,TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA_P256"
    }
],
```

> [!WARNING]
> Wenn für die Verschlüsselungssammlung falsche Werte festgelegt werden, die SChannel nicht verstehen kann, funktioniert die TLS-Kommunikation mit dem Server ggf. nicht mehr. In diesem Fall müssen Sie den Eintrag *FrontEndSSLCipherSuiteOrder* aus **clusterSettings** entfernen und die aktualisierte Resource Manager-Vorlage übermitteln, um die Standardeinstellungen der Verschlüsselungssammlung wiederherzustellen.  Verwenden Sie diese Funktion umsichtig.

## <a name="get-started"></a>Erste Schritte
Die Azure-Website mit Resource Manager-Schnellstartvorlagen umfasst eine Vorlage mit der Basisdefinition zum [Erstellen einer App Service-Umgebung](https://azure.microsoft.com/documentation/templates/201-web-app-ase-create/).

<!-- LINKS -->

<!-- IMAGES -->
