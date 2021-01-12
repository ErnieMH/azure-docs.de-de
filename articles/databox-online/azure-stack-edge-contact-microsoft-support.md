---
title: Erstellen eines Supporttickets für Azure Stack Edge Pro und Azure Data Box Gateway | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie bei Problemen im Zusammenhang mit Ihren Azure Stack Edge Pro- oder Data Box Gateway-Aufträgen Supportanfragen erstellen.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 12/17/2020
ms.author: alkohli
ms.openlocfilehash: a457c2816d4075190a2174339f7e52a2ea41730a
ms.sourcegitcommit: b6267bc931ef1a4bd33d67ba76895e14b9d0c661
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/19/2020
ms.locfileid: "97696023"
---
# <a name="open-a-support-ticket-for-azure-stack-edge-pro-and-azure-data-box-gateway"></a>Öffnen eines Supporttickets für Azure Stack Edge Pro und Azure Data Box Gateway

Dieser Artikel bezieht sich auf Azure Stack Edge Pro und Azure Data Box Gateway, die beide vom Azure Stack Edge Pro-/Azure Data Box Gateway-Dienst verwaltet werden. Wenn Probleme mit Ihrem Dienst auftreten, können Sie eine Serviceanfrage für den technischen Support erstellen. In diesem Artikel wird Folgendes beschrieben:

* Erstellen einer Supportanfrage
* Verwalten des Lebenszyklus einer Supportanfrage innerhalb des Portals

## <a name="create-a-support-request"></a>Erstellen einer Supportanfrage

Führen Sie die folgenden Schritte aus, um eine Supportanfrage zu erstellen:

1. Wechseln Sie zu Ihrem Azure Stack Edge Pro- oder Data Box Gateway-Auftrag. Navigieren Sie zum Abschnitt **Support + Problembehandlung**, und wählen Sie anschließend die Option **Neue Supportanfrage**.

2. Führen Sie unter **Neue Supportanfrage** auf der Registerkarte **Grundlagen** die folgenden Schritte aus:

    1. Wählen Sie aus der Dropdownliste **Problemtyp** den Eintrag **Technisch** aus.
    2. Wählen Sie Ihr **Abonnement** aus.
    3. Überprüfen Sie unter **Dienst** die Option **Meine Dienste**. Wählen Sie in der Dropdownliste die Option **Azure Stack Edge Pro und Data Box Gateway** aus.
    4. Wählen Sie Ihre **Ressource** aus. Dies entspricht dem Namen Ihres Auftrags.
    5. Geben Sie eine kurze **Zusammenfassung** des aufgetretenen Problems an. 
    6. Wählen Sie Ihren **Problemtyp** aus.
    7. Wählen Sie basierend auf dem ausgewählten Problemtyp den zugehörigen **Problemuntertyp** aus.
    8. Klicken Sie auf **Weiter: Lösungen >>** .

        ![Grundlagen](./media/azure-stack-edge-contact-microsoft-support/data-box-edge-support-request-1.png)

3. Führen Sie auf der Registerkarte **Details** die folgenden Schritte aus:

    1. Geben Sie Startdatum und -uhrzeit des Problems an.
    2. Geben Sie eine **Beschreibung** des Problems an.
    3. Wählen Sie unter **Dateiupload** das Ordnersymbol aus, um zu weiteren Dateien zu navigieren, die hochgeladen werden sollen.
    4. Aktivieren Sie **Diagnoseinformationen freigeben**.
    5. Basierend auf Ihrem Abonnement wird automatisch ein **Supportplan** mit Werten gefüllt.
    6. Wählen Sie in der Dropdownliste die Option **Schweregrad**.
    7. Geben Sie die **Bevorzugte Kontaktmethode** an.
    8. Die **Reaktionszeit in Stunden** wird anhand Ihres Abonnementplans automatisch ausgewählt.
    9. Geben Sie Ihre bevorzugte Sprache für den Support an.
    10. Geben Sie unter **Kontaktinformationen** Name, E-Mail-Adresse, Telefonnummer, optional einen Kontakt und das Land bzw. die Region an. Der Microsoft-Support verwendet diese Informationen, um Ihnen weitere Informationen, Diagnosen und Problemlösungen zur Verfügung zu stellen. 
    11. Klicken Sie auf **Weiter: Bewerten + erstellen >>** .

        ![Problem](./media/azure-stack-edge-contact-microsoft-support/data-box-edge-support-request-2.png)

4. Überprüfen Sie auf der Registerkarte **Bewerten + erstellen** die Informationen zum Supportticket. Klicken Sie auf **Erstellen**. 

    ![Problem 2](./media/azure-stack-edge-contact-microsoft-support/data-box-edge-support-request-3.png)

    Nach dem Erstellen des Supporttickets setzt sich baldmöglichst ein Supporttechniker mit Ihnen in Verbindung, um Ihre Anfrage zu bearbeiten.

## <a name="get-hardware-support"></a>Hardwaresupport

Diese Informationen gelten nur für Azure Stack-Geräte. Prozess zum Melden von Hardwareproblemen:

1. Erstellen Sie im Azure-Portal ein Supportticket für ein Hardwareproblem. Wählen Sie unter **Problemtyp** die Option **Azure Stack-Hardware** aus. Wählen Sie als **Problemuntertyp** die Option **Hardwarefehler**.

    ![Hardwareproblem](./media/azure-stack-edge-contact-microsoft-support/data-box-edge-hardware-issue-1.png)

    Nach dem Erstellen des Supporttickets setzt sich baldmöglichst ein Supporttechniker mit Ihnen in Verbindung, um Ihre Anfrage zu bearbeiten.

2. Wenn der Microsoft-Support ermittelt, dass es sich um ein Hardwareproblem handelt, wird eine der folgenden Aktionen durchgeführt:

    * Ein Ersatzteil (Field Replacement Unit, FRU) für das fehlerhafte Hardwareteil wird geschickt. Zurzeit werden Netzteile und SSD-Datenträger als einzige Ersatzteile unterstützt.
    * Nur Ersatzteile werden innerhalb des nächsten Werktags ersetzt; alles andere erfordert einen vollständigen Austausch des Systems (Full System Replacement, FSR).

3. Wenn ein Supportticket bis 16:30 Uhr Ortszeit (Montag bis Freitag) erstellt wird, wird ein Vor-Ort-Techniker für den nächsten Werktag eingeteilt, um bei Ihnen einen vollständigen Austausch des Systems durchzuführen. Ein vollständiger Austausch des Systems dauert in der Regel sehr viel länger, da die Teile von unserer Fabrik ausgeliefert werden und Verzögerungen bei Transport und Zoll auftreten könnten.

## <a name="manage-a-support-request"></a>Erstellen einer Supportanfrage

Nach dem Erstellen einer Supportanfrage können Sie den Lebenszyklus des Tickets vom Portal aus verwalten.

### <a name="to-manage-your-support-requests"></a>So verwalten Sie Ihre Supportanfragen

1. Um zur Hilfe- und Supportseite zu gelangen, navigieren Sie zu **Durchsuchen > Hilfe und Support**.

    ![Verwalten von Supportanfragen](./media/azure-stack-edge-contact-microsoft-support/data-box-edge-manage-support-request-1.png)

2. Die Tabelle **Kürzlich gesendete Supportanfragen** wird unter **Hilfe + Support** angezeigt.

    <!--[Manage support requests](./media/azure-stack-edge-contact-microsoft-support/data-box-edge-support-request-1.png)--> 

3. Klicken Sie auf eine Supportanfrage. Sie können den Status und die Details dieser Anfrage anzeigen. Klicken Sie auf **+ Neue Nachricht**, wenn Sie diese Anforderung nachverfolgen möchten.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über das [Behandeln von Problemen mit Azure Stack Edge Pro](azure-stack-edge-troubleshoot.md).
Erfahren Sie mehr über das [Behandeln von Problemen mit Data Box Gateway](data-box-gateway-troubleshoot.md).
