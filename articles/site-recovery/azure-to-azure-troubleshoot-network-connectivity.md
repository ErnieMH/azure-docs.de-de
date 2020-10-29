---
title: Behandlung von Konnektivitätsproblemen bei der Azure-zu-Azure-Notfallwiederherstellung mit Azure Site Recovery
description: Behandeln von Konnektivitätsproblemen bei der Notfallwiederherstellung virtueller Azure-Computer
author: sideeksh
manager: rochakm
ms.topic: how-to
ms.date: 04/06/2020
ms.openlocfilehash: 674ce347f929dd70e32537e9bde3139c5fafc7ea
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2020
ms.locfileid: "92368008"
---
# <a name="troubleshoot-azure-to-azure-vm-network-connectivity-issues"></a>Problembehandlung für Azure-zu-Azure-VM-Netzwerkkonnektivitätsprobleme

In diesem Artikel werden allgemeine Probleme im Zusammenhang mit der Netzwerkkonnektivität beim Replizieren und Wiederherstellen virtueller Azure-Computer (VMs) aus einer Region in einer anderen Region beschrieben. Weitere Informationen zu den Netzwerkanforderungen finden Sie unter [Netzwerkkonzepte für die Replikation zwischen Azure-Standorten](azure-to-azure-about-networking.md).

Damit die Site Recovery-Replikation funktioniert, ist für die VM die ausgehende Konnektivität zu bestimmten URLs oder IP-Bereichen erforderlich. Wenn sich Ihr virtueller Computer hinter einer Firewall befindet oder Netzwerksicherheitsgruppen-Regeln (NSG-Regeln) zum Steuern der ausgehenden Konnektivität verwendet werden, wird ggf. eine dieser Fehlermeldungen angezeigt.

| **Name**                  | **Kommerziell**                               | **Behörden**                                 | **Beschreibung** |
| ------------------------- | -------------------------------------------- | ---------------------------------------------- | ----------- |
| Storage                   | `*.blob.core.windows.net`                  | `*.blob.core.usgovcloudapi.net`              | Erforderlich, damit Daten in das Cachespeicherkonto in der Quellregion über die VM geschrieben werden können. Wenn Sie alle Cachespeicherkonten für Ihre VMs kennen, können Sie eine Zulassungsliste für die spezifischen Speicherkonto-URLs verwenden. Beispielsweise `cache1.blob.core.windows.net` und `cache2.blob.core.windows.net` anstelle von `*.blob.core.windows.net`. |
| Azure Active Directory    | `login.microsoftonline.com`                | `login.microsoftonline.us`                   | Erforderlich für die Autorisierung und Authentifizierung bei den Site Recovery-Dienst-URLs. |
| Replikation               | `*.hypervrecoverymanager.windowsazure.com` | `*.hypervrecoverymanager.windowsazure.com`   | Erforderlich, um die Kommunikation mit dem Site Recovery-Dienst über die VM zu ermöglichen. Sie können die entsprechende _Site Recovery-IP_ verwenden, wenn Ihr Firewallproxy IPs unterstützt. |
| Service Bus               | `*.servicebus.windows.net`                 | `*.servicebus.usgovcloudapi.net`             | Erforderlich, damit die Site Recovery-Überwachungs- und -Diagnosedaten über die VM geschrieben werden können. Sie können die entsprechende _Site Recovery-Überwachungs-IP_ verwenden, wenn Ihr Firewallproxy IPs unterstützt. |

## <a name="outbound-connectivity-for-site-recovery-urls-or-ip-ranges-error-code-151037-or-151072"></a>Ausgehende Konnektivität für Site Recovery-URLs oder IP-Bereiche (Fehlercode 151037 oder 151072)

### <a name="issue-1-failed-to-register-azure-virtual-machine-with-site-recovery-151195"></a>Problem 1: Fehler beim Registrieren des virtuellen Azure-Computers bei Site Recovery (151195)

#### <a name="possible-cause"></a>Mögliche Ursache

Eine Verbindung mit den Site Recovery-Endpunkten kann aufgrund eines Fehlers bei der DNS-Auflösung (Domain Name System) nicht hergestellt werden. Dieses Problem tritt häufiger beim erneuten Schützen auf, wenn Sie für die VM ein Failover ausgeführt haben, aber der DNS-Server aus der Region für die Notfallwiederherstellung nicht erreichbar ist.

#### <a name="resolution"></a>Lösung

Stellen Sie bei Verwendung eines benutzerdefinierten DNS sicher, dass der DNS-Server in der Notfallwiederherstellungsregion erreichbar ist.

Überprüfen Sie wie folgt, ob für die VM eine Einstellung für das benutzerdefinierte DNS verwendet wird:

1. Öffnen Sie **Virtuelle Computer** , und wählen Sie die VM aus.
1. Navigieren Sie für die VM zu **Einstellungen** , und wählen Sie **Netzwerk** aus.
1. Wählen Sie unter **Virtuelles Netzwerk/Subnetz** den Link zum Öffnen der Ressourcenseite des virtuellen Netzwerks aus.
1. Navigieren Sie zu **Einstellungen** , und wählen Sie **DNS-Server** aus.

Versuchen Sie, über den virtuellen Computer auf den DNS-Server zuzugreifen. Wenn der DNS-Server nicht erreichbar ist, können Sie ein Failover für den DNS-Server ausführen oder eine „Sichtverbindung“ zwischen dem Notfallwiederherstellungsnetzwerk und dem DNS herstellen.

  :::image type="content" source="./media/azure-to-azure-troubleshoot-errors/custom_dns.png" alt-text="com-error":::

### <a name="issue-2-site-recovery-configuration-failed-151196"></a>Problem 2: Fehler bei der Site Recovery-Konfiguration (151196)

> [!NOTE]
> Falls die VMs hinter einem internen Lastenausgleichsmodul vom Typ **Standard** angeordnet sind, besteht standardmäßig kein Zugriff auf die IP-Adressen von Microsoft 365 ( z. B. `login.microsoftonline.com`). Ändern Sie dies entweder in den Typ **Basic** für das interne Lastenausgleichsmodul, oder ermöglichen Sie den Zugriff in ausgehender Richtung, wie dies im Artikel [Konfigurieren von Lastenausgleichs- und Ausgangsregeln in Load Balancer Standard mithilfe der Azure CLI](../load-balancer/quickstart-load-balancer-standard-public-cli.md?tabs=option-1-create-load-balancer-standard#create-outbound-rule-configuration) beschrieben ist.

#### <a name="possible-cause"></a>Mögliche Ursache

Eine Verbindung mit IP4-Endpunkten für die Authentifizierung und Identität von Microsoft 365 kann nicht hergestellt werden.

#### <a name="resolution"></a>Lösung

- Azure Site Recovery benötigt für die Authentifizierung Zugriff auf die IP-Adressbereiche von Microsoft 365.
- Wenn Sie zum Steuern der ausgehenden Netzwerkkonnektivität auf dem virtuellen Computer Azure-Netzwerksicherheitsgruppenregeln (NSG-Regeln) oder einen Firewallproxy verwenden, müssen Sie die Kommunikation mit den IP-Adressbereichen von Microsoft 365 zulassen. Erstellen Sie basierend auf der NSG-Regel ein [Azure Active Directory-Diensttag (Azure AD)](../virtual-network/network-security-groups-overview.md#service-tags), das den Zugriff auf alle IP-Adressen für Azure AD zulässt.
- Wenn Azure AD später neue Adressen hinzugefügt werden, müssen Sie neue NSG-Regeln erstellen.

### <a name="example-nsg-configuration"></a>Beispielkonfiguration für eine Netzwerksicherheitsgruppe

Dieses Beispiel zeigt, wie NSG-Regeln für eine zu replizierende VM konfiguriert werden.

- Wenn Sie die ausgehende Konnektivität mit NSG-Regeln steuern, verwenden Sie für alle erforderlichen IP-Adressbereiche Regeln vom Typ **Ausgehende HTTPS-Verbindungen zulassen** für Port 443.
- Im Beispiel wird davon ausgegangen, dass der VM-Quellstandort **USA, Osten** und der Zielstandort **USA, Mitte** lautet.

#### <a name="nsg-rules---east-us"></a>NSG-Regeln – USA, Osten

1. Erstellen Sie eine HTTPS-Ausgangssicherheitsregel für die NSG. Dies ist im folgenden Screenshot dargestellt. In diesem Beispiel werden das **Zieldiensttag** _Storage.EastUS_ und der **Zielportbereich** _443_ verwendet.

     :::image type="content" source="./media/azure-to-azure-about-networking/storage-tag.png" alt-text="com-error":::

1. Erstellen Sie eine HTTPS-Ausgangssicherheitsregel für die NSG. Dies ist im folgenden Screenshot dargestellt. In diesem Beispiel werden das **Zieldiensttag** _AzureActiveDirectory_ und der **Zielportbereich** _443_ verwendet.

     :::image type="content" source="./media/azure-to-azure-about-networking/aad-tag.png" alt-text="com-error":::

1. Erstellen Sie (ähnlich zu den oben erstellten Sicherheitsregeln) eine Sicherheitsregel für ausgehenden HTTPS-Datenverkehr (443) für „EventHub.CentralUS“ für die NSG, die dem Zielstandort entspricht. Dies ermöglicht den Zugriff auf Site Recovery-Überwachung.
1. Erstellen Sie eine NSG-Sicherheitsregel für ausgehende HTTPS-Verbindungen (443) für „AzureSiteRecovery“. Dies ermöglicht den Zugriff auf den Site Recovery-Dienst in jeder beliebigen Region.

#### <a name="nsg-rules---central-us"></a>NSG-Regeln – USA, Mitte

In diesem Beispiel sind diese NSG-Regeln erforderlich, damit nach dem Failover die Replikation von der Zielregion zur Quellregion aktiviert werden kann:

1. Erstellen Sie eine HTTPS-Ausgangssicherheitsregel für _Storage.CentralUS_ :

   - **Zieldiensttag** : _Storage.CentralUS_
   - **Zielportbereiche** : _443_

1. Erstellen Sie eine HTTPS-Ausgangssicherheitsregel für _AzureActiveDirectory_ .

   - **Zieldiensttag** : _AzureActiveDirectory_
   - **Zielportbereiche** : _443_

1. Erstellen Sie (ähnlich zu den oben erstellten Sicherheitsregeln) eine Sicherheitsregel für ausgehenden HTTPS-Datenverkehr (443) für „EventHub.EastUS“ für die NSG, die dem Quellstandort entspricht. Dies ermöglicht den Zugriff auf Site Recovery-Überwachung.
1. Erstellen Sie eine NSG-Sicherheitsregel für ausgehende HTTPS-Verbindungen (443) für „AzureSiteRecovery“. Dies ermöglicht den Zugriff auf den Site Recovery-Dienst in jeder beliebigen Region.

### <a name="issue-3-site-recovery-configuration-failed-151197"></a>Problem 3: Fehler bei der Site Recovery-Konfiguration (151197)

#### <a name="possible-cause"></a>Mögliche Ursache

Eine Verbindung mit Azure Site Recovery-Dienstendpunkten kann nicht hergestellt werden.

#### <a name="resolution"></a>Lösung

Wenn Sie zum Steuern der ausgehenden Netzwerkkonnektivität auf dem Computer Azure-Netzwerksicherheitsgruppen-Regeln (NSG-Regeln) oder einen Firewallproxy verwenden, müssen verschiedene Diensttags zugelassen werden. [Weitere Informationen](azure-to-azure-about-networking.md#outbound-connectivity-using-service-tags)

### <a name="issue-4-azure-to-azure-replication-failed-when-the-network-traffic-goes-through-on-premises-proxy-server-151072"></a>Problem 4: Fehler bei der Azure-zu-Azure-Replikation, wenn der Netzwerkdatenverkehr lokale Proxyserver durchläuft (151072).

#### <a name="possible-cause"></a>Mögliche Ursache

Die benutzerdefinierten Proxyeinstellungen sind ungültig, und der Agent des Azure Site Recovery-Mobilitätsdiensts hat die Proxyeinstellungen von Internet Explorer (IE) nicht automatisch erkannt.

#### <a name="resolution"></a>Lösung

1. Der Mobilitätsdienst-Agent erkennt die Proxyeinstellungen von IE unter Windows und unter `/etc/environment` für Linux.
1. Wenn nur für den Azure Site Recovery-Mobilitätsdienst ein Proxy festgelegt werden soll, können Sie die Proxydetails in _ProxyInfo.conf_ angeben, die sich hier befinden:

   - **Linux** : `/usr/local/InMage/config/`
   - **Windows** : `C:\ProgramData\Microsoft Azure Site Recovery\Config`

1. _ProxyInfo.conf_ sollte die Proxyeinstellungen im folgenden _INI_ -Format enthalten:

   ```plaintext
   [proxy]
   Address=http://1.2.3.4
   Port=567
   ```

> [!NOTE]
> Der Agent des Azure Site Recovery-Mobilitätsdiensts unterstützt nur **nicht authentifizierte Proxys** .

### <a name="fix-the-problem"></a>Beheben des Problems

Führen Sie die Schritte im [Dokument mit der Netzwerkanleitung](./azure-to-azure-about-networking.md) aus, um die [erforderlichen URLs](azure-to-azure-about-networking.md#outbound-connectivity-for-urls) bzw. die [erforderlichen IP-Bereiche](azure-to-azure-about-networking.md#outbound-connectivity-using-service-tags) zuzulassen.

## <a name="next-steps"></a>Nächste Schritte

[Replizieren von Azure-VMs in eine andere Azure-Region](azure-to-azure-how-to-enable-replication.md)