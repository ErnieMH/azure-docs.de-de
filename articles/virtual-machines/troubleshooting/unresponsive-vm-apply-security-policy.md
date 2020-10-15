---
title: Azure-VM reagiert nicht, während die Sicherheitsrichtlinie auf das System angewendet wird
description: Dieser Artikel nennt Schritte zum Beheben von Problemen, bei denen der Ladebildschirm hängen bleibt, wenn die VM nicht reagiert, während eine Sicherheitsrichtlinie auf einer Azure-VM auf das System angewendet wird.
services: virtual-machines-windows
documentationcenter: ''
author: TobyTu
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: a97393c3-351d-4324-867d-9329e31b5629
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.date: 06/15/2020
ms.author: v-mibufo
ms.openlocfilehash: 6b50bffd1a44c0cf53f15650f5ff4d938f45df4d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "84907974"
---
# <a name="azure-vm-is-unresponsive-while-applying-security-policy-to-the-system"></a>Azure-VM reagiert nicht, während die Sicherheitsrichtlinie auf das System angewendet wird

Dieser Artikel nennt Schritte zum Beheben von Problemen, bei denen das Betriebssystem hängt und nicht mehr reagiert, während es eine Sicherheitsrichtlinie auf einer Azure-VM anwendet.

## <a name="symptoms"></a>Symptome

Wenn Sie die [Startdiagnose](boot-diagnostics.md) verwenden, um den Screenshot der VM anzuzeigen, zeigt der Screenshot während des Startens das hängende Betriebssystem mit der folgenden Meldung an:

> „Die Sicherheitsrichtlinie für das System wird konfiguriert.“

:::image type="content" source="media/unresponsive-vm-apply-security-policy/apply-policy.png" alt-text="Screenshot: Windows Server 2012 R2-Startbildschirm bleibt hängen.":::

:::image type="content" source="media/unresponsive-vm-apply-security-policy/apply-policy-2.png" alt-text="Screenshot: Windows Server 2012 R2-Startbildschirm bleibt hängen.":::

## <a name="cause"></a>Ursache

Es gibt zahlreiche mögliche Gründe für dieses Problem. Sie können die Ursache nur mithilfe einer Speicherabbildanalyse ermitteln.

## <a name="resolution"></a>Lösung

### <a name="process-overview"></a>Prozessübersicht

1. [Erstellen und Aufrufen einer Reparatur-VM](#create-and-access-a-repair-vm)
2. [Aktivieren der seriellen Konsole und der Speicherabbilderfassung](#enable-serial-console-and-memory-dump-collection)
3. [Neuerstellen der VM](#rebuild-the-vm)
4. [Erfassen der Speicherabbilddatei](#collect-the-memory-dump-file)

### <a name="create-and-access-a-repair-vm"></a>Erstellen einer Reparatur-VM und Zugreifen darauf

1. Führen Sie die [Schritte 1-3 der VM-Reparaturbefehle](repair-windows-vm-using-azure-virtual-machine-repair-commands.md#repair-process-example) aus, um eine Reparatur-VM vorzubereiten.
2. Stellen Sie über eine Remotedesktopverbindung eine Verbindung mit der Reparatur-VM her.

### <a name="enable-serial-console-and-memory-dump-collection"></a>Aktivieren der seriellen Konsole und der Speicherabbilderfassung

Führen Sie dieses Skript aus, um die Speicherabbilderfassung und die serielle Konsole zu aktivieren:

1. Öffnen Sie eine Eingabeaufforderungssitzung mit erhöhten Rechten („Als Administrator ausführen“).
2. Listen Sie die BCD-Speicherdaten auf, und bestimmen Sie den Bezeichner des Startladeprogramms, den Sie im nächsten Schritt verwenden werden.

     1. Geben Sie für eine VM der Generation 1 den folgenden Befehl ein, und notieren Sie sich den angezeigten Bezeichner:

        ```console
        bcdedit /store <BOOT PARTITON>:\boot\bcd /enum
        ```

        Ersetzen Sie im Befehl \<BOOT PARTITON> durch den Buchstaben der Partition auf dem angefügten Datenträger, der den Startordner enthält.

        :::image type="content" source="media/unresponsive-vm-apply-security-policy/store-data.png" alt-text="Screenshot: Windows Server 2012 R2-Startbildschirm bleibt hängen." /v NMICrashDump /t REG_DWORD /d 1 /f
        ```

        Entladen des beschädigten Betriebssystemdatenträgers:

        ```console
        REG UNLOAD HKLM\BROKENSYSTEM
        ```

### <a name="rebuild-the-vm"></a>Neuerstellen der VM

Führen Sie [Schritt 5 der VM-Reparaturbefehle](repair-windows-vm-using-azure-virtual-machine-repair-commands.md#repair-process-example) aus, um die VM zu reassemblieren.

### <a name="collect-the-memory-dump-file"></a>Erfassen der Speicherabbilddatei

Sie müssen die erste Speicherabbilddatei für den Absturz erfassen und sich mit dieser an den Support wenden, um dieses Problem zu lösen. Gehen Sie wie folgt vor, um die Speicherabbilddatei zu erfassen:

1. Anfügen des Betriebssystemdatenträgers an eine neue Reparatur-VM

    - Führen Sie die [Schritte 1-3 der VM-Reparaturbefehle](repair-windows-vm-using-azure-virtual-machine-repair-commands.md#repair-process-example) aus, um eine neue Reparatur-VM vorzubereiten.
    - Stellen Sie über eine Remotedesktopverbindung eine Verbindung mit der Reparatur-VM her.

2. Suchen nach der Speicherabbilddatei und Senden eines Supporttickets:

    - Navigieren Sie auf der Reparatur-VM zum Windows-Ordner auf dem angefügten Betriebssystemdatenträger. Wenn der dem angefügten Betriebssystemdatenträger zugewiesene Laufwerkbuchstabe beispielsweise `F` lautet, müssen Sie zu `F:\Windows` navigieren.
    - Suchen Sie nach der Datei „memory.dmp“, und [senden Sie dann ein Supportticket](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) mit der Speicherabbilddatei.
    - Wenn Sie die Datei „memory.dmp“ nicht finden, sollten Sie möglicherweise stattdessen [NMI-Aufrufe (Nicht maskierbarer Interrupt) in der seriellen Konsole](serial-console-windows.md#use-the-serial-console-for-nmi-calls) verwenden. Sie können den Leitfaden zum [Generieren einer Absturzabbilddatei mithilfe von NMI-Aufrufen](/windows/client-management/generate-kernel-or-complete-crash-dump) befolgen.

## <a name="next-steps"></a>Nächste Schritte

Wenn beim Anwenden von lokalen Benutzern und Gruppenrichtlinien Probleme auftreten, finden Sie weitere Informationen unter [VM reagiert nicht beim Anwenden der Richtlinie „Gruppenrichtlinie für lokale Benutzer und Gruppen“](unresponsive-vm-apply-group-policy.md).