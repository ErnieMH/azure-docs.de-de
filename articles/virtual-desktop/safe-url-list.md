---
title: Liste der sicheren URLs für Windows Virtual Desktop – Azure
description: Eine Liste der URLs, die Sie freigeben sollten, um sicherzustellen, dass Ihre Windows Virtual Desktop-Bereitstellung wie vorgesehen funktioniert
author: Heidilohr
ms.topic: conceptual
ms.date: 08/12/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 90db861a4ef4fc951844d3ae82a51d20cf9dc8c5
ms.sourcegitcommit: fbb620e0c47f49a8cf0a568ba704edefd0e30f81
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91875103"
---
# <a name="safe-url-list"></a>Liste der sicheren URLs

Sie müssen bestimmte URLs freigeben,damit Ihre Windows Virtual Desktop-Bereitstellung ordnungsgemäß funktioniert. Diese URLs sind in diesem Artikel aufgeführt, damit Sie wissen, welche URLs sicher sind.

## <a name="virtual-machines"></a>Virtuelle Computer

Die virtuellen Azure-Computer, die Sie für Windows Virtual Desktop erstellen, müssen über Zugriff auf die folgenden URLs in der kommerziellen Azure-Cloud verfügen:

|Adresse|Ausgehender TCP-Port|Zweck|Diensttag|
|---|---|---|---|
|*.wvd.microsoft.com|443|Dienstdatenverkehr|WindowsVirtualDesktop|
|gcs.prod.monitoring.core.windows.net|443|Agent-Datenverkehr|AzureCloud|
|production.diagnostics.monitoring.core.windows.net|443|Agent-Datenverkehr|AzureCloud|
|*xt.blob.core.windows.net|443|Agent-Datenverkehr|AzureCloud|
|*eh.servicebus.windows.net|443|Agent-Datenverkehr|AzureCloud|
|*xt.table.core.windows.net|443|Agent-Datenverkehr|AzureCloud|
|catalogartifact.azureedge.net|443|Azure Marketplace|AzureCloud|
|kms.core.windows.net|1688|Aktivierung von Windows|Internet|
|mrsglobalsteus2prod.blob.core.windows.net|443|Agent- und SXS-Stapelupdates|AzureCloud|
|wvdportalstorageblob.blob.core.windows.net|443|Unterstützung des Azure-Portals|AzureCloud|
| 169.254.169.254 | 80 | [Azure-Instanzmetadatendienst-Endpunkt](../virtual-machines/windows/instance-metadata-service.md) | – |
| 168.63.129.16 | 80 | [Sitzungshost-Systemüberwachung](../virtual-network/security-overview.md#azure-platform-considerations) | – |

>[!IMPORTANT]
>Windows Virtual Desktop unterstützt jetzt das FQDN-Tag. Weitere Informationen finden Sie unter [Verwenden von Azure Firewall zum Schutz von Windows Virtual Desktop-Bereitstellungen](../firewall/protect-windows-virtual-desktop.md).
>
>Wir empfehlen Ihnen, FQDN- oder Diensttags anstelle von URLs zu verwenden, um Dienstprobleme zu verhindern. Die aufgeführten URLs und Tags beziehen sich nur auf Windows Virtual Desktop-Websites und -Ressourcen. Sie enthalten keine URLs für andere Dienste, z. B. Azure Active Directory.

Die virtuellen Azure-Computer, die Sie für Windows Virtual Desktop erstellen, müssen über Zugriff auf die folgenden URLs in der Azure Government-Cloud verfügen:

|Adresse|Ausgehender TCP-Port|Zweck|Diensttag|
|---|---|---|---|
|*.wvd.microsoft.us|443|Dienstdatenverkehr|WindowsVirtualDesktop|
|gcs.monitoring.core.usgovcloudapi.net|443|Agent-Datenverkehr|AzureCloud|
|monitoring.core.usgovcloudapi.net|443|Agent-Datenverkehr|AzureCloud|
|fairfax.warmpath.usgovcloudapi.net|443|Agent-Datenverkehr|AzureCloud|
|*xt.blob.core.usgovcloudapi.net|443|Agent-Datenverkehr|AzureCloud|
|*.servicebus.usgovcloudapi.net|443|Agent-Datenverkehr|AzureCloud|
|*xt.table.core.usgovcloudapi.net|443|Agent-Datenverkehr|AzureCloud|
|Kms.core.usgovcloudapi.net|1688|Aktivierung von Windows|Internet|
|mrsglobalstugviffx.core.usgovcloudapi.net|443|Agent- und SXS-Stapelupdates|AzureCloud|
|wvdportalstorageblob.blob.core.usgovcloudapi.net|443|Unterstützung des Azure-Portals|AzureCloud|
| 169.254.169.254 | 80 | [Azure-Instanzmetadatendienst-Endpunkt](../virtual-machines/windows/instance-metadata-service.md) | – |
| 168.63.129.16 | 80 | [Sitzungshost-Systemüberwachung](../virtual-network/security-overview.md#azure-platform-considerations) | – |

In der folgenden Tabelle sind optionale URLs aufgeführt, auf die Ihre virtuellen Azure-Computer Zugriff haben können:

|Adresse|Ausgehender TCP-Port|Zweck|Azure Gov|
|---|---|---|---|
|*.microsoftonline.com|443|Authentifizierung bei Microsoft Online Services|login.microsoftonline.us|
|*.events.data.microsoft.com|443|Telemetriedienst|Keine|
|www.msftconnecttest.com|443|Ermittelt, ob das Betriebssystem mit dem Internet verbunden ist.|Keine|
|*.prod.do.dsp.mp.microsoft.com|443|Windows-Update|Keine|
|login.windows.net|443|Anmelden bei Microsoft Online Services, Microsoft 365|login.microsoftonline.us|
|*.sfx.ms|443|Updates für die OneDrive-Clientsoftware|oneclient.sfx.ms|
|*.digicert.com|443|Überprüfung der Zertifikatsperre|Keine|

>[!NOTE]
>Windows Virtual Desktop verfügt derzeit über keine Liste mit IP-Adressbereichen, die Sie freigeben können, um Netzwerkdatenverkehr zuzulassen. Momentan wird nur das Freigeben von spezifischen URLs unterstützt.
>
>Eine Liste der sicheren URLs für Office, einschließlich der erforderlichen URLs für Azure Active Directory, finden Sie unter [Office 365-URLs und -IP-Adressbereiche](/office365/enterprise/urls-and-ip-address-ranges).
>
>Sie müssen das Platzhalterzeichen (*) für URLs für Dienstdatenverkehr verwenden. Wenn Sie kein Platzhalterzeichen (*) für Agent-Datenverkehr verwenden möchten, ermitteln Sie wie folgt die URLs ohne Platzhalter:
>
>1. Registrieren Sie Ihre virtuellen Computer für den Windows Virtual Desktop-Hostpool.
>2. Öffnen Sie die **Ereignisanzeige**, navigieren Sie dann zu **Windows-Protokolle** > **Anwendung** > **WVD-Agent**, und suchen Sie nach der Ereignis-ID 3701.
>3. Heben Sie die Blockierung von URLs auf, die Sie unter der Ereignis-ID 3701 finden. Die URLs unter der Ereignis-ID 3701 sind regionsspezifisch. Sie müssen den Freigabeprozess mit den relevanten URLs für jede Region wiederholen, in der Sie Ihre virtuellen Computer bereitstellen möchten.

## <a name="remote-desktop-clients"></a>Remotedesktopclients

Alle von Ihnen verwendeten Remotedesktopclients müssen auf die folgenden URLs zugreifen können:

|Adresse|Ausgehender TCP-Port|Zweck|Client(s)|Azure Gov|
|---|---|---|---|---|
|*.wvd.microsoft.com|443|Dienstdatenverkehr|All|*.wvd.microsoft.us|
|*.servicebus.windows.net|443|Problembehandlung für Daten|All|*.servicebus.usgovcloudapi.net|
|go.microsoft.com|443|Microsoft FWLinks|All|Keine|
|aka.ms|443|Microsoft-URL-Verkürzung|All|Keine|
|docs.microsoft.com|443|Dokumentation|All|Keine|
|privacy.microsoft.com|443|Datenschutzbestimmungen|All|Keine|
|query.prod.cms.rt.microsoft.com|443|Clientupdates|Windows Desktop|Keine|

>[!IMPORTANT]
>Das Öffnen dieser URLs ist für einen zuverlässigen Clientbetrieb von entscheidender Bedeutung. Der Zugriff auf diese URLs darf nicht blockiert werden; andernfalls wird die Dienstfunktionalität beeinträchtigt.
>
>Diese URLs gelten lediglich für die Clientstandorte und -ressourcen. Diese Liste enthält keine URLs für andere Dienste wie Azure Active Directory. Azure Active Directory-URLs finden Sie unter ID 56 in [URLs und IP-Adressbereiche von Office 365](/office365/enterprise/urls-and-ip-address-ranges#microsoft-365-common-and-office-online).
