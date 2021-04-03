---
title: Verwenden von Azure AD Connect Health mit AD FS | Microsoft-Dokumentation
description: Dies ist die Azure AD Connect Health-Seite, auf der Sie erfahren, wie Sie Ihre lokale AD FS-Infrastruktur überwachen.
services: active-directory
documentationcenter: ''
ms.reviewer: zhiweiwangmsft
author: billmath
manager: daveba
editor: curtand
ms.assetid: dc0e53d8-403e-462a-9543-164eaa7dd8b3
ms.service: active-directory
ms.subservice: hybrid
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 02/26/2019
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.collection: M365-identity-device-management
ms.openlocfilehash: 26fdf202cb9bcacee94c83578432f7a399f90a0c
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "91306275"
---
# <a name="monitor-ad-fs-using-azure-ad-connect-health"></a>Überwachen von AD FS mithilfe von Azure AD Connect Health
Die folgende Dokumentation bezieht sich auf die Überwachung Ihrer AD FS-Infrastruktur mit Azure AD Connect Health. Informationen zum Überwachen von Azure AD Connect (Sync) mit Azure AD Connect Health finden Sie unter [Verwenden von Azure AD Connect Health für die Synchronisierung](how-to-connect-health-sync.md). Informationen zur Überwachung der Active Directory-Domänendienste mit Azure AD Connect Health finden Sie unter [Verwenden von Azure AD Connect Health mit AD DS](how-to-connect-health-adds.md).

## <a name="alerts-for-ad-fs"></a>Warnungen für AD FS
Im Abschnitt "Azure AD Connect Health-Warnungen" wird eine Liste der aktiven Warnungen angezeigt. Jede Warnung umfasst relevante Informationen, Lösungsschritte und Links zur verwandten Dokumentation.

Sie können auf eine aktive oder gelöste Warnung doppelklicken, um ein neues Blatt mit weiteren Informationen, Schritten zum Lösen der Warnung und Links zu relevanter Dokumentation zu öffnen. Sie können außerdem Verlaufsdaten zu bereits behobenen Warnungen anzeigen.

![Screenshot der Seite „Warnungen“ von Azure AD Connect Health mit einer ausgewählten Warnung und dem geöffneten Fenster „Warnungsdetails"](./media/how-to-connect-health-adfs/alert2.png)

## <a name="usage-analytics-for-ad-fs"></a>Nutzungsanalyse für AD FS
In der Azure AD Connect Health-Nutzungsanalyse wird der Authentifizierungsdatenverkehr für Ihre Verbundserver analysiert. Sie können auf das Feld „Nutzungsanalyse“ doppelklicken, um das Blatt „Nutzungsanalyse“ zu öffnen, auf dem mehrere Metriken und Gruppierungen angezeigt werden.

> [!NOTE]
> Um die Nutzungsanalyse mit AD FS verwenden zu können, müssen Sie sicherstellen, dass die AD FS-Überwachung aktiviert ist. Weitere Informationen finden Sie unter [Aktivieren der Überwachung für AD FS](how-to-connect-health-agent-install.md#enable-auditing-for-ad-fs).
>
>

![Screenshot der Seite „Nutzungsanalyse“ von Azure AD Connect Health](./media/how-to-connect-health-adfs/report1.png)

Um zusätzliche Metriken auszuwählen, einen Zeitraum anzugeben oder die Gruppierung zu ändern, klicken Sie mit der rechten Maustaste auf das Diagramm zur Nutzungsanalyse und wählen „Diagramm bearbeiten“. Anschließend können Sie den Zeitraum angeben, eine andere Metrik auswählen und die Gruppierung ändern. Sie können die Verteilung des Authentifizierungsdatenverkehrs basierend auf verschiedenen Metriken anzeigen und jede Metrik über die relevanten Parameter gruppieren. Diese Parameter werden im folgenden Abschnitt beschrieben:

**Metrik: Anforderungen insgesamt**: Gibt die Gesamtzahl von Anforderungen an, die von AD FS-Servern verarbeitet wurden.

|Gruppieren nach | Bedeutung und Nutzen der Gruppierung |
| --- | --- |
| All | Zeigt die Gesamtzahl von Anforderungen an, die von allen AD FS-Servern verarbeitet wurden.|
| Application | Gruppiert die Gesamtzahl von Anforderungen basierend auf der Zielanwendung (vertrauende Seite). Diese Gruppierung ist nützlich, wenn Sie die prozentuale Verteilung des gesamten Datenverkehrs auf die verschiedenen Anwendungen anzeigen möchten. |
|  Server |Gruppiert die Gesamtzahl von Anforderungen basierend auf dem Server, der die Anforderung verarbeitet hat. Diese Gruppierung ist nützlich, um die Lastverteilung für den gesamten Datenverkehr anzuzeigen.
| In den Arbeitsplatz eingebunden |Gruppiert die Gesamtzahl von Anforderungen danach, ob sie von Geräten stammen, die mit einem Arbeitsbereich verknüpft (bekannt) sind. Diese Gruppierung ist nützlich, wenn auf Ihre Ressourcen über Geräte zugegriffen wird, die der Identitätsinfrastruktur unbekannt sind. |
|  Authentifizierungsmethode | Gruppiert die Gesamtzahl von Anforderungen basierend auf der Authentifizierungsmethode. Diese Gruppierung ist nützlich, wenn Sie wissen möchten, welche Authentifizierungsmethode in der Regel für die Authentifizierung verwendet wird. Folgende Authentifizierungsmethoden sind möglich: <ol> <li>Integrierte Windows-Authentifizierung (Windows)</li> <li>Formularbasierte Authentifizierung (Forms)</li> <li>Einmaliges Anmelden (SSO)</li> <li>X509-Zertifikatauthentifizierung (Zertifikat)</li> <br>Wenn die Verbundserver die Anforderung mit einem SSO-Cookie empfangen, wird diese Anforderung als SSO-Anforderung eingestuft. In diesen Fällen wird, sofern es sich um ein gültiges Cookie handelt, der Benutzer nicht zur Eingabe von Anmeldeinformationen aufgefordert, sondern erhält nahtlos Zugriff auf die Anwendung. Dieses Verhalten ist üblich, wenn mehrere vertrauende Seiten von den Verbundservern geschützt werden. |
| Netzwerkadresse | Gruppiert die Gesamtzahl von Anforderungen basierend auf der Netzwerkadresse des Benutzers. Dies kann entweder ein Intranet oder Extranet sein. Diese Gruppierung ist nützlich, wenn Sie die prozentuale Verteilung des eingehenden Datenverkehrs für Intranet und Extranet anzeigen möchten. |


**Metrik: Anforderungen mit Fehlern gesamt**: Die Gesamtzahl von Anforderungen mit Fehlern, die vom Verbunddienst verarbeitet wurden. (Diese Metrik steht nur in AD FS für Windows Server 2012 R2 zur Verfügung.)

|Gruppieren nach | Bedeutung und Nutzen der Gruppierung |
| --- | --- |
| Fehlertyp | Zeigt die Anzahl von Fehlern basierend auf den vordefinierten Fehlertypen an. Diese Gruppierung ist nützlich, um die allgemeinen Arten von Fehlern zu verstehen. <ul><li>Falscher Benutzername oder falsches Kennwort: Fehler, die durch einen falschen Benutzernamen oder ein falsches Kennwort verursacht wurden</li> <li>„Extranetsperrung“: Fehler durch Anforderungen von einem Benutzer, der für das Extranet gesperrt wurde </li><li> „Kennwort abgelaufen“: Fehler durch Benutzer, die sich mit einem abgelaufenen Kennwort anmelden</li><li>„Konto deaktiviert“: Fehler durch Benutzer, die sich mit einem deaktivierten Konto anmelden</li><li>„Authentifizierung mit Gerät“: Fehler durch Benutzer, bei denen Probleme bei der Geräteauthentifizierung auftreten</li><li>„Authentifizierung mit Benutzerzertifikat“: Fehler durch Benutzer, bei denen die Authentifizierung aufgrund eines ungültigen Zertifikats nicht möglich ist</li><li>„MFA“: Fehler durch Benutzer, die sich nicht per Multi-Factor Authentication authentifizieren können</li><li>„Andere Anmeldeinformationen“ und „Ausstellungsautorisierung“: Fehler aufgrund von Autorisierungsproblemen</li><li>„Ausstellungsdelegierung“: Fehler aufgrund von Problemen bei der Ausstellungsdelegierung</li><li>„Tokenannahme“: Fehler durch das Ablehnen des Tokens von externen Identitätsanbietern durch AD FS</li><li>„Protokoll“: Fehler aufgrund von Protokollproblemen</li><li>„Unbekannt“: Fängt alle weiteren Fehler ab. Alle anderen Fehler, die nicht in die definierten Kategorien fallen.</li> |
| Server | Gruppiert die Fehler basierend auf dem Server. Diese Gruppierung ist nützlich, um die Fehlerverteilung auf die Server anzuzeigen. Eine ungleichmäßige Verteilung könnte darauf hinweisen, dass sich ein Server in fehlerhaftem Zustand befindet. |
| Netzwerkadresse | Gruppiert die Fehler basierend auf der Netzwerkadresse der Anforderungen (Intranet oder Extranet). Diese Gruppierung ist nützlich, um die Art der Anforderungen herauszufinden, für die Fehler auftreten. |
|  Application | Gruppiert die Fehler basierend auf der Zielanwendung (vertrauende Seite). Mithilfe dieser Gruppierung können Sie anzeigen, für welche Zielanwendung die meisten Fehler auftreten. |

**Metrik: Benutzeranzahl**: Durchschnittliche Anzahl von eindeutigen Benutzern, die aktiv eine Authentifizierung mit AD FS durchführen

|Gruppieren nach | Bedeutung und Nutzen der Gruppierung |
| --- | --- |
|All |Mit dieser Metrik wird die durchschnittliche Anzahl von Benutzern bereitgestellt, die den Verbunddienst im ausgewählten Zeitraum verwendet haben. Die Benutzer werden nicht gruppiert. <br>Die durchschnittliche Anzahl richtet sich nach dem ausgewählten Zeitraum. |
| Application |Gruppiert die durchschnittliche Anzahl von Benutzern basierend auf der Zielanwendung (vertrauende Seite). Über diese Gruppierung können Sie anzeigen, wie viele Benutzer welche Anwendung nutzen. |

## <a name="performance-monitoring-for-ad-fs"></a>Leistungsüberwachung für AD FS
Die Azure AD Connect Health-Leistungsüberwachung liefert Überwachungsinformationen zu verschiedenen Metriken. Wenn Sie das Kontrollkästchen aktivieren, wird ein neues Blatt mit ausführlichen Informationen zu den Metriken geöffnet.

![Screenshot der Seite „Leistungsüberwachung“ von Azure AD Connect Health](./media/how-to-connect-health-adfs/perf1.png)

Wenn Sie die Filteroption oben im Blatt auswählen, können Sie eine Filterung nach Server vornehmen, um Metriken zu einzelnen Servern anzuzeigen. Um Metriken zu ändern, klicken Sie mit der rechten Maustaste auf das Überwachungsdiagramm unterhalb des Überwachungsblatts und klicken dann auf „Diagramm bearbeiten“ (oder wählen Sie die Schaltfläche „Diagramm bearbeiten“). Auf dem dann geöffneten neuen Blatt können Sie in der Dropdownliste zusätzliche Metriken auswählen und einen Zeitbereich für die Anzeige von Leistungsdaten angeben.

## <a name="top-50-users-with-failed-usernamepassword-logins"></a>50 Benutzer mit den meisten fehlgeschlagenen Anmeldungen aufgrund eines falschen Benutzernamens/Kennworts
Einer der häufigsten Gründe für das Fehlschlagen von Authentifizierungsanforderungen bei einem AD FS-Server ist die Verwendung ungültiger Anmeldeinformationen, d. h. eines falschen Benutzernamens oder Kennworts. Dies passiert Benutzern normalerweise aufgrund von komplexen Kennwörtern, vergessenen Kennwörtern oder Schreibfehlern.

Es gibt jedoch weitere Gründe, die zu einer unerwartet hohen Anzahl von Anforderungen bei Ihren AD FS-Servern führen können, etwa von einer Anwendung zwischengespeicherte abgelaufene Benutzeranmeldeinformationen oder ein böswilliger Benutzer, der versucht, sich mit einer Reihe von bekannten Kennwörtern bei einem Konto anzumelden. Diese beiden Beispiele sind plausible Gründe für einen Anstieg der Anforderungsanzahl.

Azure AD Connect Health für AD FS bietet einen Bericht über die 50 Benutzer, bei denen Anmeldeversuche am häufigsten aufgrund ungültiger Benutzernamen oder Kennwörter fehlschlagen. Zur Erstellung dieses Berichts werden die von allen AD FS-Servern in den Farmen generierten Überwachungsereignisse verarbeitet.

![Screenshot mit dem Abschnitt „Berichte“ und der Anzahl der Anmeldeversuche mit ungültigen Kennwörtern in den letzten 30 Tagen](./media/how-to-connect-health-adfs/report1a.png)

In diesem Bericht können Sie ganz einfach auf die folgenden Informationen zugreifen:

* Gesamtanzahl fehlgeschlagener Anforderungen mit falschem Benutzernamen/Kennwort in den letzten 30 Tagen
* Durchschnittliche Anzahl von Benutzern pro Tag, bei denen die Anmeldung wegen eines ungültigen Benutzernamens/Kennworts fehlschlägt.

Wenn Sie auf diesen Bereich klicken, gelangen Sie zum Hauptberichtblatt, das zusätzliche Details enthält. Dieses Blatt enthält einen Graphen mit Trendinformationen zum Ermitteln einer Baseline für Anforderungen mit falschem Benutzernamen oder Kennwort. Außerdem wird die Liste mit den 50 Benutzern mit den meisten Fehlversuchen während der letzten Woche angegeben. Beachten Sie, dass die 50 Benutzer der letzten Woche zur Identifizierung von Spitzenwerten bei den fehlerhaften Kennwörtern beitragen können.  

Der Graph zeigt die folgenden Informationen:

* Gesamtanzahl fehlgeschlagener Anmeldeversuche aufgrund eines ungültigen Benutzernamens/Kennworts pro Tag
* Gesamtanzahl eindeutiger Benutzer, bei denen die Anmeldung fehlgeschlagen ist, pro Tag.
* Client-IP-Adresse der letzten Anforderung

![Azure AD Connect Health-Portal](./media/how-to-connect-health-adfs/report3a.png)

Der Bericht enthält die folgenden Informationen:

| Berichtselement | BESCHREIBUNG |
| --- | --- |
| Benutzer-ID |Die verwendete Benutzer-ID. Dies ist der Wert mit der Eingabe des Benutzers, also in einigen Fällen die Verwendung der falschen Benutzer-ID. |
| Fehlgeschlagene Anmeldeversuche |Die Gesamtanzahl fehlgeschlagener Anmeldeversuche für die jeweilige Benutzer-ID. Die Tabelle ist nach der höchsten Anzahl fehlgeschlagener Versuche in absteigender Reihenfolge sortiert. |
| Letzter Fehler |Zeigt den Zeitstempel des letzten Fehlers an. |
| Letzte Fehler-IP-Adresse |Zeigt die Client-IP-Adresse der letzten fehlerhaften Anforderung an. Sollten für diesen Wert mehrere IP-Adressen angezeigt werden, enthält er möglicherweise die Forward-Client-IP-Adresse sowie die IP-Adresse des letzten Anforderungsversuchs des Benutzers.  |

> [!NOTE]
> Der Bericht wird automatisch alle zwölf Stunden mit den neuen gesammelten Informationen aktualisiert. Anmeldeversuche innerhalb der letzten 12 Stunden sind daher unter Umständen nicht im Bericht enthalten.

## <a name="related-links"></a>Verwandte Links
* [Azure AD Connect Health](./whatis-azure-ad-connect.md)
* [Installieren des Azure AD Connect Health-Agents](how-to-connect-health-agent-install.md)
* [Bericht über riskante IP-Adressen](how-to-connect-health-adfs-risky-ip.md)