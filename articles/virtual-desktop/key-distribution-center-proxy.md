---
title: 'Einrichten von Windows Virtual Desktop zur Verwendung eines Kerberos-Schlüsselverteilungscenter-Proxys: Azure'
description: Es wird beschrieben, wie Sie einen Windows Virtual Desktop-Hostpool für die Verwendung eines Kerberos-Schlüsselverteilungscenter-Proxys einrichten.
author: Heidilohr
ms.topic: how-to
ms.date: 05/04/2021
ms.author: helohr
manager: femila
ms.openlocfilehash: 74fc1eeabe8af754d3ac5809818b6d648453ee45
ms.sourcegitcommit: 02d443532c4d2e9e449025908a05fb9c84eba039
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2021
ms.locfileid: "108732723"
---
# <a name="configure-a-kerberos-key-distribution-center-proxy"></a>Konfigurieren eines Kerberos-Schlüsselverteilungscenter-Proxys

Sicherheitsbewusste Kunden, z. B. Finanz- oder Regierungsorganisationen, verwenden häufig die Anmeldung mithilfe von Smartcards. Smartcards erhöhen die Sicherheit von Bereitstellungen, da sie eine mehrstufige Authentifizierung (Multi-Factor Authentication, MFA) verlangen. Für den RDP-Teil einer Windows Virtual Desktop-Sitzung ist bei Smartcards jedoch für die Kerberos-Authentifizierung eine direkte Verbindung („Line of Sight“) mit einem AD-Domänencontroller (Active Directory) erforderlich. Ohne diese direkte Verbindung können sich Benutzer nicht automatisch über Remoteverbindungen beim Netzwerk der Organisation anmelden. Benutzer in einer Windows Virtual Desktop-Bereitstellung können den KDC-Proxydienst verwenden, um einen Proxy für diesen Authentifizierungsdatenverkehr einzurichten und sich remote anzumelden. Der KDC-Proxy ermöglicht die Authentifizierung für das Remotedesktopprotokoll einer Windows Virtual Desktop-Sitzung, sodass der Benutzer sich sicher anmelden kann. Dies vereinfacht eine Arbeit von zu Hause, und ermöglicht eine reibungslosere Durchführung bestimmter Notfallwiederherstellungsszenarien.

Das Einrichten des KDC-Proxys umfasst in der Regel das Zuweisen der Windows Server-Gatewayrolle in Windows Server 2016 oder höher. Wie verwenden Sie eine Remotedesktopdienste-Rolle, um sich bei Windows Virtual Desktop anzumelden? Um dies zu beantworten, werfen wir einen kurzen Blick auf die Komponenten.

Es gibt zwei Komponenten für den virtuellen Windows Virtual Desktop-Dienst, die authentifiziert werden müssen:

- Der Feed im Windows Virtual Desktop-Client, mit dem Benutzer eine Liste der verfügbaren Desktops oder Anwendungen erhalten, auf die sie Zugriff haben. Dieser Authentifizierungsvorgang erfolgt in Azure Active Directory. Das bedeutet, dass diese Komponente nicht im Mittelpunkt dieses Artikels steht.
- Die RDP-Sitzung, die sich daraus ergibt, dass ein Benutzer eine dieser verfügbaren Ressourcen auswählt. Diese Komponente verwendet die Kerberos-Authentifizierung und erfordert einen KDC-Proxy für Remotebenutzer.

In diesem Artikel erfahren Sie, wie Sie den Feed im Windows Virtual Desktop-Client im Azure-Portal konfigurieren. Informationen zum Konfigurieren der RD-Gateway-Rolle finden Sie unter [Bereitstellen der RD-Gateway-Rolle](/windows-server/remote/remote-desktop-services/remote-desktop-gateway-role).

## <a name="requirements"></a>Anforderungen

Zum Konfigurieren eines Windows Virtual Desktop-Sitzungshosts mit einem KDC-Proxy benötigen Sie Folgendes:

- Zugriff auf das Azure-Portal und ein Azure-Administratorkonto.
- Auf den Remoteclientcomputern muss entweder Windows 10 oder Windows 7 ausgeführt werden, und der [Windows Desktop-Client](/windows-server/remote/remote-desktop-services/clients/windowsdesktop) muss installiert sein. Derzeit wird der Webclient nicht unterstützt.
- Auf dem Computer muss bereits ein KDC-Proxy installiert sein. Weitere Informationen hierzu finden Sie unter [Einrichten der RD-Gateway-Rolle für Windows Virtual Desktop](/windows-server/remote/remote-desktop-services/remote-desktop-gateway-role).
- Betriebssystem des Computers muss Windows Server 2016 oder höher sein.

Nachdem Sie sichergestellt haben, dass Sie diese Anforderungen erfüllen, kann es losgehen.

## <a name="how-to-configure-the-kdc-proxy"></a>Konfigurieren des KDC-Proxys

Konfigurieren Sie den KDC-Proxy wie folgt:

1. Melden Sie sich beim Azure-Portal als Administrator an.

2. Navigieren Sie zur Seite „Windows Virtual Desktop“.

3. Wählen Sie den Hostpool, für den Sie den KDC-Proxy aktivieren möchten, und dann die Option **RDP-Eigenschaften** aus.

    > [!div class="mx-imgBorder"]
    > ![Screenshot: Seite im Azure-Portal, auf der ein Benutzer die Option „Hostpools“, den Namen des Beispielhostpools und dann „RDP-Eigenschaften“ auswählt](media/rdp-properties.png)

4. Wählen Sie die Registerkarte **Erweitert** aus, und geben Sie dann einen Wert im folgenden Format ohne Leerzeichen ein:

    
    > kdcproxyname:s:\<fqdn\>
    

    > [!div class="mx-imgBorder"]
    > ![Screenshot: Ausgewählte Registerkarte „Erweitert“ mit eingegebenem Wert (Schritt 4)](media/advanced-tab-selected.png)

5. Klicken Sie auf **Speichern**.

6. Der ausgewählte Hostpool sollte nun damit beginnen, RDP-Verbindungsdateien mit dem von Ihnen in Schritt 4 eingegebenen Wert für „kdcproxyname“ auszugeben.

## <a name="next-steps"></a>Nächste Schritte

Informationen dazu, wie Sie den KDC-Proxy hinsichtlich Remotedesktopdienste verwalten und die RD-Gateway-Rolle zuweisen, finden Sie unter [Bereitstellen der RD-Gateway-Rolle](/windows-server/remote/remote-desktop-services/remote-desktop-gateway-role).

Wenn Sie Ihre KDC-Proxyserver skalieren möchten, finden Sie weitere Informationen zum Einrichten der Hochverfügbarkeit für den KDC-Proxy unter [Hinzufügen von Hochverfügbarkeit zu RD-Web und RD-Gateway (Webfront)](/windows-server/remote/remote-desktop-services/rds-rdweb-gateway-ha).