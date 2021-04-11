---
title: Überwachung der Dateiintegrität in Azure Security Center | Microsoft-Dokumentation
description: In dieser exemplarischen Vorgehensweise erfahren Sie, wie Sie die Überwachung der Dateiintegrität in Azure Security Center konfigurieren.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 411d7bae-c9d4-4e83-be63-9f2f2312b075
ms.service: security-center
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/22/2020
ms.author: memildin
ms.openlocfilehash: 834df2387d037c19672e3287970300f4e84ca34b
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105047982"
---
# <a name="file-integrity-monitoring-in-azure-security-center"></a>Überwachung der Dateiintegrität in Azure Security Center
In dieser exemplarischen Vorgehensweise erfahren Sie, wie Sie die Überwachung der Dateiintegrität in Azure Security Center konfigurieren.


## <a name="availability"></a>Verfügbarkeit

|Aspekt|Details|
|----|:----|
|Status des Release:|Allgemeine Verfügbarkeit (General Availability, GA)|
|Preise:|Erfordert [Azure Defender für Server](defender-for-servers-introduction.md).<br>Die Überwachung der Dateiintegrität lädt Daten in den Log Analytics-Arbeitsbereich hoch. Die Gebühren für diese Daten hängen von der hochgeladenen Datenmenge ab. Weitere Informationen erhalten Sie unter [Log Analytics – Preise](https://azure.microsoft.com/pricing/details/log-analytics/).|
|Erforderliche Rollen und Berechtigungen:|Der **Besitzer des Arbeitsbereichs** kann FIM aktivieren/deaktivieren (weitere Informationen finden Sie unter [Azure-Rolles für Log Analytics](/services-hub/health/azure-roles#azure-roles)).<br>**Leser** können Ergebnisse anzeigen.|
|Clouds:|![Ja](./media/icons/yes-icon.png) Kommerzielle Clouds<br>![Ja](./media/icons/yes-icon.png) National/Sovereign (US Gov, China Gov, andere Gov)<br>Wird nur in Regionen unterstützt, in denen die Änderungsnachverfolgungslösung von Azure Automation verfügbar ist.<br>![Ja](./media/icons/yes-icon.png) Geräte mit [Azure Arc](../azure-arc/servers/overview.md)-Unterstützung.<br>Siehe [Unterstützte Regionen für einen verknüpften Log Analytics-Arbeitsbereich](../automation/how-to/region-mappings.md).<br>Informieren Sie sich über die [Änderungsnachverfolgung](../automation/change-tracking/overview.md).|
|||

## <a name="what-is-fim-in-security-center"></a>Was ist die Überwachung der Dateiintegrität in Security Center?
Die Überwachung der Dateiintegrität, auch als Überwachung von Änderungen bezeichnet, untersucht unter anderem Betriebssystemdateien und Windows-Registrierungen, Anwendungssoftware und Linux-Systemdateien auf Änderungen, die auf einen Angriff hindeuten. 

Security Center empfiehlt Entitäten, die mit der Überwachung der Dateiintegrität überwacht werden sollen. Sie können auch eigene Richtlinien oder Entitäten für die Überwachung definieren. Die Überwachung der Dateiintegrität warnt Sie bei verdächtigen Aktivitäten wie den Folgenden:

- Erstellen und Löschen von Dateien und Registrierungsschlüsseln
- Änderungen an Dateien (an der Dateigröße, der Zugriffssteuerungsliste und dem Hash des Inhalts)
- Änderungen an Registrierungen (an der Größe, der Zugriffssteuerungsliste, am Typ und am Inhalt)

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Auswerten der Liste der Entitäten, für die eine Überwachung der Dateiintegrität empfohlen wird
> * Definieren Ihrer eigenen benutzerdefinierten Regeln für die Überwachung der Dateiintegrität
> * Überwachen von Änderungen an Ihren überwachten Entitäten
> * Verwenden von Platzhaltern, um die verzeichnisübergreifende Nachverfolgung zu vereinfachen


## <a name="how-does-fim-work"></a>Wie funktioniert die Überwachung der Dateiintegrität?

Indem der aktuelle Zustand dieser Elemente mit dem Zustand während des vorherigen Scans verglichen wird, werden Sie von der Überwachung der Dateiintegrität benachrichtigt, wenn verdächtige Änderungen vorgenommen wurden.

Die Überwachung der Dateiintegrität verwendet die Azure-Lösung „Änderungsnachverfolgung“, um Änderungen in Ihrer Umgebung nachzuverfolgen und zu identifizieren. Wenn die Überwachung der Dateiintegrität aktiviert ist, verfügen Sie über eine Ressource für die **Änderungsnachverfolgung** vom Typ **Lösung**. Ausführliche Informationen zur Häufigkeit der Datensammlung finden Sie unter [Details zur Datensammlung für die Änderungsnachverfolgung](../automation/change-tracking/overview.md#change-tracking-and-inventory-data-collection).

> [!NOTE]
> Wenn Sie die Ressource **Änderungsnachverfolgung** entfernen, deaktivieren Sie damit auch das Feature „Überwachung der Dateiintegrität“ in Security Center.

## <a name="which-files-should-i-monitor"></a>Welche Dateien sollten überwacht werden?
Beim Auswählen der zu überwachenden Dateien sollten Sie berücksichtigen, welche Dateien für Ihr System und Ihre Anwendungen unentbehrlich sind. Überwachen Sie Dateien, bei denen Sie keine ungeplanten Änderungen erwarten. Wenn Sie Dateien auswählen, die häufig von Anwendungen oder dem Betriebssystem geändert werden (z. B. Protokoll- und Textdateien), werden viele Meldungen generiert, die das Identifizieren eines Angriffs erschweren.

Security Center bietet die folgende Liste empfohlener Elemente, die auf der Grundlage bekannter Angriffsmuster überwacht werden sollen.

|Linux-Dateien|Windows-Dateien|Windows-Registrierungsschlüssel (HKLM = HKEY_LOCAL_MACHINE)|
|:----|:----|:----|
|/bin/login|C:\autoexec.bat|HKLM\SOFTWARE\Microsoft\Cryptography\OID\EncodingType 0\CryptSIPDllRemoveSignedDataMsg\{C689AAB8-8E78-11D0-8C47-00C04FC295EE}|
|/bin/passwd|C:\boot.ini|HKLM\SOFTWARE\Microsoft\Cryptography\OID\EncodingType 0\CryptSIPDllRemoveSignedDataMsg\{603BCC1F-4B59-4E08-B724-D2C6297EF351}|
|/etc/*.conf|C:\config.sys|HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\IniFileMapping\SYSTEM.ini\boot|
|/usr/bin|C:\Windows\system.ini|HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Windows|
|/usr/sbin|C:\Windows\win.ini|HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon|
|/bin|C:\Windows\regedit.exe|HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Shell Folders|
|/sbin|C:\Windows\System32\userinit.exe|HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders|
|/boot|C:\Windows\explorer.exe|HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run|
|/usr/local/bin|C:\Programme\Microsoft Security Client\msseces.exe|HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce|
|/usr/local/sbin||HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnceEx|
|/opt/bin||HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\RunServices|
|/opt/sbin||HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\RunServicesOnce|
|/etc/crontab||HKLM\SOFTWARE\WOW6432Node\Microsoft\Cryptography\OID\EncodingType 0\CryptSIPDllRemoveSignedDataMsg\{C689AAB8-8E78-11D0-8C47-00C04FC295EE}|
|/etc/init.d||HKLM\SOFTWARE\WOW6432Node\Microsoft\Cryptography\OID\EncodingType 0\CryptSIPDllRemoveSignedDataMsg\{603BCC1F-4B59-4E08-B724-D2C6297EF351}|
|/etc/cron.hourly||HKLM\SOFTWARE\WOW6432Node\Microsoft\Windows NT\CurrentVersion\IniFileMapping\system.ini\boot|
|/etc/cron.daily||HKLM\SOFTWARE\WOW6432Node\Microsoft\Windows NT\CurrentVersion\Windows|
|/etc/cron.weekly||HKLM\SOFTWARE\WOW6432Node\Microsoft\Windows NT\CurrentVersion\Winlogon|
|/etc/cron.monthly||HKLM\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\Explorer\Shell Folders|
|||HKLM\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders|
|||HKLM\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\Run|
|||HKLM\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\RunOnce|
|||HKLM\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\RunOnceEx|
|||HKLM\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\RunServices|
|||HKLM\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\RunServicesOnce|
|||HKLM\SYSTEM\CurrentControlSet\Control\hivelist|
|||HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\KnownDLLs|
|||HKLM\SYSTEM\CurrentControlSet\Services\SharedAccess\Parameters\FirewallPolicy\DomainProfile|
|||HKLM\SYSTEM\CurrentControlSet\Services\SharedAccess\Parameters\FirewallPolicy\PublicProfile|
|||HKLM\SYSTEM\CurrentControlSet\Services\SharedAccess\Parameters\FirewallPolicy\StandardProfile|


## <a name="enable-file-integrity-monitoring"></a>Aktivieren der Überwachung der Dateiintegrität 

Die Überwachung der Dateiintegrität ist nur auf den Security Center-Seiten im Azure-Portal verfügbar. Derzeit gibt es keine REST-API für die Arbeit mit der Überwachung der Dateiintegrität.

1. Wählen Sie im **Azure Defender**-Dashboard im Bereich **Erweiterter Schutz** die Option **Überwachung der Dateiintegrität** aus.

   :::image type="content" source="./media/security-center-file-integrity-monitoring/open-file-integrity-monitoring.png" alt-text="Starten von FIM" lightbox="./media/security-center-file-integrity-monitoring/open-file-integrity-monitoring.png":::

    Die Seite zur Konfiguration der **Überwachung der Dateiintegrität** wird geöffnet.

    Folgende Informationen werden für jeden Arbeitsbereich angegeben:

    - Die Gesamtanzahl der Änderungen, die in der letzten Woche aufgetreten sind (wenn die Überwachung der Dateiintegrität im Arbeitsbereich deaktiviert ist, wird „–“ angezeigt)
    - Die Gesamtanzahl der physischen und virtuellen Computer, die Berichte an den Arbeitsbereich senden
    - Der geografische Standort des Arbeitsbereichs
    - Das Azure-Abonnement, dem der Arbeitsbereich unterliegt

1. Auf dieser Seite können Sie folgende Aktionen durchführen:

    - Zugreifen auf den Status und die Einstellungen der einzelnen Arbeitsbereiche

    - ![Upgradeplansymbol][4] Aktualisieren Sie den Arbeitsbereich für die Verwendung von Azure Defender. Dieses Symbol zeigt an, dass der Arbeitsbereich oder das Abonnement nicht durch Azure Defender geschützt wird. Ihr Abonnement muss durch Azure Defender geschützt werden, damit Sie die Funktionen der Überwachung der Dateiintegrität verwenden können. Weitere Informationen finden Sie unter [Azure Security Center Free und Azure Defender-Aktivierung](security-center-pricing.md).

    - ![Symbol „Aktivieren“][3] Aktivieren Sie die Überwachung der Dateiintegrität auf allen Computern im Arbeitsbereich, und konfigurieren Sie die entsprechenden Optionen. Dieses Symbol zeigt an, dass die Überwachung der Dateiintegrität für den Arbeitsbereich nicht aktiviert ist.

        :::image type="content" source="./media/security-center-file-integrity-monitoring/workspace-list-fim.png" alt-text="Aktivieren der Überwachung der Dateiintegrität für einen bestimmten Arbeitsbereich":::


    > [!TIP]
    > Wenn keine Schaltfläche zum Aktivieren oder Aktualisieren vorhanden ist und der Bereich leer ist, bedeutet dies, dass die Überwachung der Dateiintegrität bereits für den Arbeitsbereich aktiviert ist.


1. Wählen Sie **AKTIVIEREN** aus. Die Details des Arbeitsbereichs einschließlich der Anzahl der Windows- und Linux-Computer im Arbeitsbereich werden angezeigt.

    :::image type="content" source="./media/security-center-file-integrity-monitoring/workspace-fim-status.png" alt-text="Seite mit den Arbeitsbereichsdetails für die Überwachung der Dateiintegrität ":::

   Die empfohlenen Einstellungen für Windows und Linux werden ebenfalls aufgeführt.  Erweitern Sie **Windows-Dateien**, **Registrierung** und **Linux-Dateien**, um die vollständige Liste der empfohlenen Elemente anzuzeigen.

1. Deaktivieren Sie die Kontrollkästchen für alle empfohlenen Entitäten, die nicht von der Überwachung der Dateiintegrität überwacht werden sollen.

1. Klicken Sie auf **Überwachung der Dateiintegrität anwenden**, um die Überwachung der Dateiintegrität zu aktivieren.

> [!NOTE]
> Sie können die Einstellungen jederzeit ändern. Weitere Informationen finden Sie im Folgenden unter [Edit monitored entities (Bearbeiten überwachter Entitäten)](#edit-monitored-entities).



## <a name="audit-monitored-workspaces"></a>Überprüfen überwachter Arbeitsbereiche 

Das Dashboard **Überwachung der Dateiintegrität** zeigt die Arbeitsbereiche an, für den die Überwachung der Dateiintegrität aktiviert ist. Das Dashboard „Überwachung der Dateiintegrität“ wird geöffnet, nachdem Sie die Überwachung der Dateiintegrität für einen Arbeitsbereich aktiviert haben oder wenn Sie einen Arbeitsbereich im Fenster **Überwachung der Dateiintegrität** auswählen, für den diese bereits aktiviert ist.

:::image type="content" source="./media/security-center-file-integrity-monitoring/fim-dashboard.png" alt-text="Das Dashboard der Überwachung der Dateiintegrität und die verschiedenen Informationsbereiche":::

Im Dashboard „Überwachung der Dateiintegrität“ für einen Arbeitsbereich werden folgende Details angezeigt:

- Die Gesamtanzahl der mit dem Arbeitsbereich verbundenen Computer
- Die Gesamtanzahl der Änderungen, die im ausgewählten Zeitraum aufgetreten sind
- Eine Aufschlüsselung des Änderungstyps (Dateien, Registrierungen)
- Eine Aufschlüsselung der Änderungskategorie (Geändert, Hinzugefügt, Entfernt)

Wählen Sie oben im Dashboard die Option **Filter** aus, um den Zeitraum zu ändern, für den Änderungen angezeigt werden.

:::image type="content" source="./media/security-center-file-integrity-monitoring/dashboard-filter.png" alt-text="Filter für den Zeitraum für das Dashboard der Überwachung der Dateiintegrität ":::

Auf der Registerkarte **Server** werden die Computer aufgelistet, die Meldungen an diesen Arbeitsbereich ausgeben. Für jeden Computer führt das Dashboard Folgendes auf:

- Die Gesamtanzahl der Änderungen, die im ausgewählten Zeitraum aufgetreten sind
- Eine Aufschlüsselung der Gesamtanzahl der Änderungen in Änderungen von Dateien oder Registrierungen

Die **Protokollsuche** wird geöffnet, wenn Sie den Namen eines Computers in das Suchfeld eingeben oder einen Computer auswählen, der auf der Registerkarte „Computer“ aufgeführt ist. Die Protokollsuche zeigt alle Änderungen an, die im ausgewählten Zeitraum am Computer durchgeführt wurden. Sie können eine Änderung erweitern, um weitere Informationen zu erhalten.

![Protokollsuche][8]

Die Registerkarte **Änderungen** (siehe unten) führt alle Änderungen auf, die im ausgewählten Zeitraum am Arbeitsbereich durchgeführt wurden. Für jede geänderte Entität führt das Dashboard Folgendes auf:

- Den Computer, auf dem die Änderung aufgetreten ist
- Den Änderungstyp (Registrierung oder Datei)
- Die Änderungskategorie (Geändert, Hinzugefügt, Entfernt)
- Das Datum und die Uhrzeit der Änderung

![Änderungen am Arbeitsbereich][9]

**Änderungsdetails** wird geöffnet, wenn Sie eine Änderung in das Suchfeld eingeben oder eine Entität auswählen, die in der Registerkarte **Änderungen** aufgeführt ist.

![Änderungsdetails][10]

## <a name="edit-monitored-entities"></a>Bearbeiten von überwachten Entitäten

1. Kehren Sie zum Dashboard **Überwachung der Dateiintegrität** zurück, und klicken Sie auf **Einstellungen**.

   ![Einstellungen][11]

   **Arbeitsbereichskonfiguration** wird geöffnet, und diese drei Registerkarten werden angezeigt: **Windows-Registrierung**, **Windows-Dateien** und **Linux-Dateien**. Jede Registerkarte führt die Entitäten auf, die Sie in dieser Kategorie bearbeiten können. Für jede aufgeführte Entität identifiziert Security Center, ob die Überwachung der Dateiintegrität aktiviert (true) (wahr) oder deaktiviert (false) (falsch) ist.  Wenn Sie eine Entität bearbeiten, können Sie die Überwachung der Dateiintegrität aktivieren oder deaktivieren.

   ![Arbeitsbereichskonfiguration][12]

2. Wählen Sie einen Identitätsschutz aus. In diesem Beispiel wurde ein Element unter „Windows-Registrierung“ ausgewählt. Das Fenster **Edit for Change Tracking** (Für die Änderungsnachverfolgung bearbeiten) wird geöffnet.

   ![Für die Änderungsnachverfolgung bearbeiten][13]

Unter **Edit for Change Tracking** (Für die Änderungsnachverfolgung bearbeiten) können Sie Folgendes durchführen:

- Aktivieren (True) (Wahr) oder deaktivieren (False) (Falsch) Sie die Überwachung der Dateiintegrität.
- Geben Sie den Namen der Entität an, oder ändern Sie diesen.
- Geben Sie den Wert oder Pfad an, oder ändern Sie diesen.
- Löschen Sie die Entität, verwerfen Sie die Änderung, oder speichern Sie die Änderung.

## <a name="add-a-new-entity-to-monitor"></a>Hinzufügen einer neuen zu überwachenden Entität
1. Kehren Sie zum Dashboard **Überwachung der Dateiintegrität** zurück, und wählen Sie im oberen Bereich **Einstellungen** aus. Die **Arbeitsbereichskonfiguration** wird geöffnet.
2. Klicken Sie unter **Arbeitsbereichskonfiguration** auf die Registerkarte für den Typ der Entität, die Sie hinzufügen möchten: Windows-Registrierung, Windows-Dateien oder Linux-Dateien. In diesem Beispiel wurde **Linux-Dateien** ausgewählt.

   ![Hinzufügen eines neuen zu überwachenden Elements][14]

3. Wählen Sie **Hinzufügen**. Das Fenster **Add for Change Tracking** (Für die Änderungsnachverfolgung hinzufügen) wird geöffnet.

   ![Eingabe der angeforderten Informationen][15]

4. Geben Sie auf der Seite **Hinzufügen** die angeforderten Informationen ein, und klicken Sie auf **Speichern**.

## <a name="disable-monitored-entities"></a>Deaktivieren von überwachten Entitäten
1. Kehren Sie zum Dashboard **Überwachung der Dateiintegrität** zurück.
2. Wählen Sie einen Arbeitsbereich aus, für den die Überwachung der Dateiintegrität derzeit aktiviert ist. Die Überwachung der Dateiintegrität ist für einen Arbeitsbereich aktiviert, wenn die Schaltflächen „Aktivieren“ und „Upgradeplan“ nicht vorhanden sind.

   ![Auswählen eines Arbeitsbereichs, bei dem die Überwachung der Dateiintegrität aktiviert ist][16]

3. Wählen Sie unter „Überwachung der Dateiintegrität“ **Einstellungen** aus.

   ![Klicken auf „Einstellungen“][17]

4. Klicken Sie unter **Arbeitsbereichskonfiguration** auf eine Gruppe, für die **Aktiviert** auf „true“ (wahr) festgelegt ist.

   ![Arbeitsbereichskonfiguration][18]

5. Legen Sie **Aktiviert** im Fenster **Edit for Change Tracking** (Für Änderungsnachverfolgung bearbeiten) auf „False“ (Falsch) fest.

   ![Festlegen von „Aktiviert“ auf „Falsch“][19]

6. Wählen Sie **Speichern** aus.

## <a name="folder-and-path-monitoring-using-wildcards"></a>Ordner- und Pfadüberwachung mithilfe von Platzhaltern

Verwenden Sie Platzhalter, um die verzeichnisübergreifende Nachverfolgung zu vereinfachen. Beim Konfigurieren der Ordnerüberwachung mithilfe von Platzhaltern gelten die folgenden Regeln:
-   Platzhalter werden zum Nachverfolgen mehrerer Dateien benötigt.
-   Platzhalter können nur im letzten Segment eines Pfads verwendet werden, z.B. „C:\Ordner\Datei“ oder „/etc/*.conf“.
-   Wenn eine Umgebungsvariable einen ungültigen Pfad enthält, ist die Überprüfung zwar erfolgreich, bei der Ausführung der Inventur tritt jedoch ein Fehler für den Pfad auf.
-   Vermeiden Sie beim Festlegen des Pfads die Verwendung allgemeiner Pfade wie „c:\*.*“, da in diesem Fall zu viele Ordner durchlaufen werden müssen.

## <a name="disable-fim"></a>Deaktivieren der Überwachung der Dateiintegrität
Sie können die Überwachung der Dateiintegrität deaktivieren. Die Überwachung der Dateiintegrität verwendet die Azure-Lösung „Änderungsnachverfolgung“, um Änderungen in Ihrer Umgebung nachzuverfolgen und zu identifizieren. Wenn Sie die Überwachung der Dateiintegrität deaktivieren, wird die Lösung für die Änderungsnachverfolgung aus dem ausgewählten Arbeitsbereich entfernt.

1. Kehren Sie zum Dashboard **Überwachung der Dateiintegrität** zurück, um die Überwachung der Dateiintegrität zu deaktivieren.
2. Wählen Sie einen Arbeitsbereich aus.
3. Klicken Sie unter **Überwachung der Dateiintegrität** auf **Deaktivieren**.

   ![Deaktivieren der Überwachung der Dateiintegrität][20]

4. Klicken Sie auf **Entfernen**, um sie zu deaktivieren.

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie mehr darüber erfahren, wie die Überwachung der Dateiintegrität in Security Center verwendet wird. Weitere Informationen zu Security Center finden Sie auf den folgenden Seiten:

* [Festlegen von Sicherheitsrichtlinien:](tutorial-security-policy.md) Erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
* [Verwalten von Sicherheitsempfehlungen:](security-center-recommendations.md) Erfahren Sie, wie Empfehlungen Ihnen beim Schutz Ihrer Azure-Ressourcen helfen.
* [Azure Security Blog](/archive/blogs/azuresecurity/)(Blog zur Azure-Sicherheit): Hier finden Sie Neuigkeiten und Informationen zur Azure-Sicherheit.

<!--Image references-->
[1]: ./media/security-center-file-integrity-monitoring/security-center-dashboard.png
[3]: ./media/security-center-file-integrity-monitoring/enable.png
[4]: ./media/security-center-file-integrity-monitoring/upgrade-plan.png
[5]: ./media/security-center-file-integrity-monitoring/enable-fim.png
[7]: ./media/security-center-file-integrity-monitoring/filter.png
[8]: ./media/security-center-file-integrity-monitoring/log-search.png
[9]: ./media/security-center-file-integrity-monitoring/changes-tab.png
[10]: ./media/security-center-file-integrity-monitoring/change-details.png
[11]: ./media/security-center-file-integrity-monitoring/fim-dashboard-settings.png
[12]: ./media/security-center-file-integrity-monitoring/workspace-config.png
[13]: ./media/security-center-file-integrity-monitoring/edit.png
[14]: ./media/security-center-file-integrity-monitoring/add.png
[15]: ./media/security-center-file-integrity-monitoring/add-item.png
[16]: ./media/security-center-file-integrity-monitoring/fim-dashboard-disable.png
[17]: ./media/security-center-file-integrity-monitoring/fim-dashboard-settings-disabled.png
[18]: ./media/security-center-file-integrity-monitoring/workspace-config-disable.png
[19]: ./media/security-center-file-integrity-monitoring/edit-disable.png
[20]: ./media/security-center-file-integrity-monitoring/disable-fim.png