---
title: Hinzufügen eines Geschäfts-, Schul- oder Unikontos zur Microsoft Authenticator-App – Azure AD
description: Fügen Sie der Microsoft Authenticator-App Ihr Geschäfts-, Schul- oder Unikonto hinzu, um während der zweistufigen Überprüfung Ihre Identität zu bestätigen.
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: end-user-help
ms.date: 01/24/2019
ms.author: curtand
ms.reviewer: olhaun
ms.openlocfilehash: e003c45aa1e7d75b709b7fbf99532fb1302fcbb9
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88797648"
---
# <a name="add-your-work-or-school-account-to-the-microsoft-authenticator-app"></a>Hinzufügen Ihres Geschäfts-, Schul- oder Unikontos in der Microsoft Authenticator-App

Wenn Ihre Organisation eine zweistufige Überprüfung verwendet, können Sie Ihr Geschäfts-, Schul- oder Unikonto für die Verwendung der Microsoft Authenticator-App als eine der Überprüfungsmethoden einrichten.

>[!Important]
>Bevor Sie Ihr Konto hinzufügen können, müssen Sie die Microsoft Authenticator-App herunterladen und installieren. Wenn Sie dies noch nicht getan haben, führen Sie die im Artikel [Herunterladen und Installieren der App](user-help-auth-app-download-install.md) aufgeführten Schritte aus.

## <a name="add-your-work-or-school-account"></a>Hinzufügen Ihres Geschäfts-, Schul- oder Unikontos

1. Navigieren Sie auf Ihrem Computer zur Seite [Zusätzliche Sicherheitsüberprüfung](https://account.activedirectory.windowsazure.com/proofup.aspx?proofup=1).

    >[!Note]
    >Wenn die Seite **Zusätzliche Sicherheitsüberprüfung** nicht angezeigt wird, hat Ihr Administrator möglicherweise die Sicherheitsinformation (Vorschau) aktiviert. Befolgen Sie in diesem Fall die Anweisungen im Abschnitt [Einrichten der Sicherheitsinformation zur Verwendung einer Authentifikator-App](security-info-setup-auth-app.md). Ist dies nicht der Fall, bitten Sie den Helpdesk Ihrer Organisation um Unterstützung. Weitere Informationen zu Sicherheitsinformationen finden Sie unter [Übersicht über die Sicherheitsinformationen (Vorschau)](./security-info-setup-signin.md).

2. Aktivieren Sie das Kontrollkästchen neben **Authenticator-App**, und wählen Sie dann **Konfigurieren**.

    Die Seite **Mobile App konfigurieren** wird angezeigt.

    ![Bildschirm, der den QR-Code enthält](./media/user-help-auth-app-download-install/auth-app-barcode.png)

3. Öffnen Sie die Microsoft Authenticator-App, wählen Sie oben rechts das Symbol zum **Anpassen und Steuern** aus, und wählen Sie dann nacheinander **Konto hinzufügen** und **Geschäfts-, Schul- oder Unikonto** aus.

    >[!Note]
    >Wenn Sie die Microsoft Authenticator-App zum ersten Mal einrichten, werden Sie möglicherweise in einer Meldung gefragt, ob Sie der App den Zugriff auf Ihre Kamera (iOS) oder die Aufnahme von Foto- und Videodateien (Android) erlauben möchten. Sie müssen **Zulassen** auswählen, damit die Authentifikator-App im nächsten Schritt auf Ihre Kamera zugreifen und den QR-Code aufnehmen kann. Wenn Sie den Zugriff auf die Kamera nicht zulassen, können Sie die Authentifikator-App auch einrichten, müssen die Codeinformationen aber manuell hinzufügen. Informationen zum manuellen Hinzufügen des Codes finden Sie unter [Manuelles Hinzufügen eines Kontos zur App](user-help-auth-app-add-account-manual.md).

4. Verwenden Sie zum Scannen des QR-Codes auf dem Bildschirm **Mobile App konfigurieren** auf Ihrem Computer die Kamera Ihres Geräts, und wählen Sie dann **Fertig** aus.

    >[!Note]
    >Wenn die Kamera den QR-Code nicht erfassen kann, können Sie Ihre Kontoinformationen manuell der Microsoft Authenticator-App für die zweistufige Überprüfung hinzufügen. Weitere Informationen und eine Beschreibung der Vorgehensweise finden Sie unter [Manuelles Hinzufügen Ihres Kontos](user-help-auth-app-add-account-manual.md).

5. Überprüfen Sie den Bildschirm **Konten** der App auf Ihrem Gerät, um sicherzustellen, dass Ihre Kontoinformationen korrekt sind und ein zugehöriger sechsstelliger Prüfcode vorhanden ist. Um die Sicherheit zu erhöhen, wird der Prüfcode alle 30 Sekunden geändert, um zu verhindern, dass jemand einen Code mehrmals verwendet.

    ![Bildschirm „Konten“](./media/user-help-auth-app-download-install/auth-app-accounts.png)

## <a name="next-steps"></a>Nächste Schritte

- Nachdem Ihre Konten der App hinzugefügt wurden, können Sie sich unter Verwendung der Authenticator-App auf Ihrem Gerät anmelden. Weitere Informationen finden Sie unter [Anmelden mithilfe der App](user-help-auth-app-sign-in.md).

- Für iOS-Geräte können Sie Ihre Kontoanmeldeinformationen und zugehörige App-Einstellungen, etwa die Reihenfolge Ihrer Konten, in der Cloud sichern. Weitere Informationen finden Sie unter [Sichern und Wiederherstellen von Kontoanmeldeinformationen mithilfe der Microsoft Authenticator-App](user-help-auth-app-backup-recovery.md).