---
title: Installieren des Agents für die Azure AD Connect-Cloudbereitstellung
description: In diesem Artikel wird das Installieren des Agents für die Azure AD Connect-Cloudbereitstellung beschrieben.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 05/19/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: dcb322805ac3368dd6ed8e193875e083b27195e1
ms.sourcegitcommit: e2dc549424fb2c10fcbb92b499b960677d67a8dd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/17/2020
ms.locfileid: "94695281"
---
# <a name="install-the-azure-ad-connect-cloud-provisioning-agent"></a>Installieren des Agents für die Azure AD Connect-Cloudbereitstellung
In diesem Dokument erhalten Sie Informationen zum Installationsvorgang für den Azure AD Connect-Bereitstellungs-Agent (Azure Active Directory) und dessen Erstkonfiguration im Azure-Portal.

>[!IMPORTANT]
>In den folgenden Installationsanweisungen wird davon ausgegangen, dass alle [Voraussetzungen](how-to-prerequisites.md) erfüllt sind.

Unter den folgenden Links erhalten Sie Informationen zur Installation und Konfiguration des Azure AD Connect-Bereitstellungs-Agents:
    
- [Installieren des Agents](#install-the-agent)
- [Überprüfen der Agent-Installation](#verify-agent-installation)


## <a name="install-the-agent"></a>Installieren des Agents
Führen Sie die folgenden Schritte aus, um den Agent zu installieren.

1. Melden Sie sich mit Berechtigungen eines Unternehmensadministrators bei dem Server an, den Sie verwenden werden.
1. Melden Sie sich beim Azure-Portal an, und navigieren Sie zu **Azure Active Directory**.
1. Wählen Sie im linken Menü die Option **Azure AD Connect** aus.
1. Wählen Sie **Bereitstellung verwalten (Vorschau)**  > **Alle Agents überprüfen** aus.
1. Laden Sie den Azure AD Connect-Bereitstellungs-Agent über das Azure-Portal herunter.

   ![Herunterladen des lokalen Agents](media/how-to-install/install-9.png)</br>
1. Führen Sie das Installationsprogramm für den Azure AD Connect-Bereitstellungs-Agent aus („AADConnectProvisioningAgent.Installer“).
1. Akzeptieren Sie auf dem Bildschirm **Microsoft Azure AD Connect-Bereitstellungs-Agent-Paket** die Lizenzbedingungen, und wählen Sie **Installieren** aus.

   ![Bildschirm „Microsoft Azure AD Connect-Bereitstellungs-Agent-Paket“](media/how-to-install/install-1.png)</br>

1. Nach Abschluss dieses Vorgangs wird der Konfigurations-Assistent gestartet. Melden Sie sich mit dem Konto Ihres globalen Azure AD-Administrators an.
1. Wählen Sie auf dem Bildschirm **Active Directory verbinden** die Option **Verzeichnis hinzufügen** aus. Melden Sie sich dann mit Ihrem Active Directory-Administratorkonto an. Dadurch wird Ihr lokales Verzeichnis hinzugefügt. Wählen Sie **Weiter** aus.

   ![Bildschirm „Active Directory verbinden“](media/how-to-install/install-3.png)</br>

1. Wählen Sie auf dem Bildschirm **Konfiguration abgeschlossen** die Option **Bestätigen** aus. Damit wird der Agent registriert und neu gestartet.

   ![Bildschirm „Konfiguration abgeschlossen“](media/how-to-install/install-4a.png)</br>

1. Nach Abschluss dieses Vorgangs sollte der Hinweis **Ihre Agent-Konfiguration wurde erfolgreich überprüft** angezeigt werden. Wählen Sie **Beenden** aus.

   ![Schaltfläche „Beenden“](media/how-to-install/install-5.png)</br>
1. Wenn weiterhin der erste Bildschirm **Microsoft Azure AD Connect-Bereitstellungs-Agent-Paket** angezeigt wird, wählen Sie **Schließen** aus.

## <a name="verify-agent-installation"></a>Überprüfen der Agent-Installation
Die Agent-Überprüfung erfolgt im Azure-Portal und auf dem lokalen Server, auf dem der Agent ausgeführt wird.

### <a name="azure-portal-agent-verification"></a>Agent-Überprüfung im Azure-Portal
Führen Sie die folgenden Schritte aus, um zu überprüfen, ob der Agent von Azure erkannt wird.

1. Melden Sie sich beim Azure-Portal an.
1. Wählen Sie auf der linken Seite **Azure Active Directory** > **Azure AD Connect** aus. Wählen Sie in der Mitte **Bereitstellung verwalten (Vorschau)** aus.

   ![Azure-Portal](media/how-to-install/install-6.png)</br>

1.  Wählen Sie auf dem Bildschirm **Azure AD-Bereitstellung (Vorschau)** die Option **Alle Agents überprüfen** aus.

    ![Option „Alle Agents überprüfen“](media/how-to-install/install-7.png)</br>
 
1. Auf dem Bildschirm **Lokale Bereitstellungs-Agents** werden die von Ihnen installierten Agents angezeigt. Vergewissern Sie sich, dass der betreffende Agent aufgeführt wird und als *Aktiviert* markiert ist.

   ![Bildschirm „Lokale Bereitstellungs-Agents“](media/how-to-install/verify-1.png)</br>



### <a name="on-the-local-server"></a>Auf dem lokalen Server
Führen Sie die folgenden Schritte aus, um zu überprüfen, ob der Agent ausgeführt wird.

1.  Melden Sie sich auf dem Server mit einem Administratorkonto an.
1.  Öffnen Sie die Seite **Dienste**, indem Sie zu dieser Seite navigieren oder **Start** > **Run** > **Services.msc** ausführen.
1.  Vergewissern Sie sich, dass unter **Dienste** die Dienste **Microsoft Azure AD Connect Agent Updater** und **Microsoft Azure AD Connect Provisioning Agent** mit dem Status *Wird ausgeführt* angezeigt werden.

    ![Bildschirm „Dienste“](media/how-to-install/troubleshoot-1.png)

>[!IMPORTANT]
>Nachdem Sie den Agent installiert haben, muss er konfiguriert und aktiviert werden, bevor er mit der Synchronisierung von Benutzern beginnen kann. Informationen zum Konfigurieren eines neuen Agents finden Sie unter [Erstellen einer neuen Konfiguration für die cloudbasierte Azure AD Connect-Bereitstellung](how-to-configure.md).



## <a name="next-steps"></a>Nächste Schritte 

- [Was ist die Identitätsbereitstellung?](what-is-provisioning.md)
- [Was ist die Azure AD Connect-Cloudbereitstellung?](what-is-cloud-provisioning.md)
 
