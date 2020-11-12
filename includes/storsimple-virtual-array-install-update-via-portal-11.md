---
title: include file
description: include file
services: storsimple
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 07/18/2018
ms.author: alkohli
ms.custom: include file
ms.openlocfilehash: 4ed290e861a0fdebeb76b9e08e62c03eda52be82
ms.sourcegitcommit: 6a902230296a78da21fbc68c365698709c579093
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2020
ms.locfileid: "93375964"
---
#### <a name="to-install-updates-via-the-azure-portal"></a>So installieren Sie Updates über das Azure-Portal

1. Wechseln Sie zu Ihrem StorSimple-Geräte-Manager, und wählen Sie **Devices** (Geräte) aus. Wählen Sie aus der Liste der mit dem Dienst verbundenen Geräte das Gerät aus, das Sie aktualisieren möchten, und klicken Sie darauf.

2. Klicken Sie unter **Einstellungen** auf **Geräteupdates**.  

3. Wenn Softwareupdates verfügbar sind, wird eine Meldung angezeigt. Sie können auch auf **Scan** (Überprüfen) klicken, um nach Updates zu suchen. Notieren Sie sich die Softwareversion, die Sie ausführen. 

    ![Gerät aktualisieren](../includes/media/storsimple-virtual-array-install-update-via-portal-11/azupdate3m1.png)

    Sie werden jeweils benachrichtigt, wenn die Überprüfung beginnt und erfolgreich abgeschlossen wurde.
 
4. Wenn die Updatesuche abgeschlossen wurde, klicken Sie auf **Download updates** (Updates herunterladen). Lesen Sie unter **Neue Updates** die Anmerkungen zu dieser Version. Beachten Sie, dass Sie, nachdem die Updates heruntergeladen wurden, die Installation bestätigen müssen. Klicken Sie auf **OK**.

    ![Aktualisieren des Geräts 2](../includes/media/storsimple-virtual-array-install-update-via-portal-11/azupdate6m.png)

    Sie werden jeweils benachrichtigt, wenn das Hochladen beginnt und erfolgreich abgeschlossen wurde.

5. Klicken Sie unter **Geräteupdates** auf **Installieren**.

     ![Aktualisieren des Geräts 3](../includes/media/storsimple-virtual-array-install-update-via-portal-11/azupdate11m1.png)

6. Unter **Neue Updates** werden Sie gewarnt, dass das Update zu einer Unterbrechung führt. Da es sich bei Virtual Array um ein Gerät mit einem Einzelknoten handelt, wird das Gerät nach dem Update neu gestartet. Dadurch werden alle laufenden E/A-Vorgänge unterbrochen. Klicken Sie auf **OK** , um die Updates zu installieren.

    ![Aktualisieren des Geräts 4](../includes/media/storsimple-virtual-array-install-update-via-portal-11/azupdate12m.png)

    Sie werden benachrichtigt, wenn der Installationsauftrag beginnt.

7.  Nachdem der Installationsauftrag erfolgreich abgeschlossen wurde, klicken Sie auf den Link **Auftrag anzeigen**. Durch diese Aktion gelangen Sie auf das Blatt **Install Updates** (Updates installieren). Hier finden Sie ausführliche Informationen zum Auftrag. 

    ![Aktualisieren des Geräts 5](../includes/media/storsimple-virtual-array-install-update-via-portal-11/azupdate16m1.png)

8. Wenn Sie mit einem virtuellen Array mit Softwareversion Update 1 (10.0.10296.0) begonnen haben, führen Sie jetzt Update 1.1 aus und sind fertig. Sie können die verbleibenden Schritte überspringen. 

    Wenn Sie mit einem virtuellen Array mit der Softwareversion Update 0.6 (10.0.10293.0) begonnen haben, haben Sie jetzt auf Update 1.0 aktualisiert. Daraufhin wird eine andere Meldung angezeigt, die angibt, dass Updates verfügbar sind. Wiederholen Sie die Schritte 4 bis 7, um Update 1.1 zu installieren.

    

