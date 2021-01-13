---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: a5a286753e438b7d65f3d33a82669c4f7e79a282
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "86544331"
---
#### <a name="to-create-public-endpoints-on-the-cloud-appliance"></a>So erstellen Sie öffentliche Endpunkte auf dem Cloudgerät

1. Melden Sie sich beim Azure-Portal an.
2. Navigieren Sie zu **Virtuelle Computer**, und wählen Sie dann den virtuellen Computer aus, der als Cloudgerät verwendet werden soll, und klicken Sie darauf.
    
3. Sie müssen eine Netzwerksicherheitsgruppen-Regel (NSG-Regel) erstellen, um den ein- und ausgehenden Datenverkehr für Ihren virtuellen Computer zu steuern. Führen Sie die folgenden Schritte aus, um eine NSG-Regel zu erstellen:
    1. Wählen Sie die Option **Netzwerksicherheitsgruppe**.
        ![Screenshot der Seite für den virtuellen Computer. Im Abschnitt „Einstellungen“ ist die Netzwerksicherheitsgruppe hervorgehoben.](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt1.png)

    2. Klicken Sie auf die angezeigte Standard-Netzwerksicherheitsgruppe.
        ![Screenshot der Seite für die Netzwerksicherheitsgruppe. Die standardmäßige Netzwerksicherheitsgruppe ist hervorgehoben.](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt2.png)

    3. Wählen Sie die Option **Eingangssicherheitsregeln**.
        ![Screenshot einer Seite, auf der die Eigenschaften der standardmäßigen Netzwerksicherheitsgruppe angezeigt werden. Im Navigationsbereich sind die Eingangssicherheitsregeln hervorgehoben.](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt3.png)

    4. Klicken Sie auf **+ Hinzufügen**, um eine Sicherheitsregel für eingehenden Datenverkehr zu erstellen.
        ![Screenshot der Seite mit den Eingangssicherheitsregeln. Das Pluszeichen und das Wort „Hinzufügen“ befinden sich nebeneinander und sind hervorgehoben.](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt4.png)

        Gehen Sie auf dem Blatt „Eingangssicherheitsregel hinzufügen“ wie folgt vor:

        1. Geben Sie unter **Name** den folgenden Namen für den Endpunkt ein: WinRMHttps.
        
        2. Wählen Sie unter **Priorität** eine Zahl aus, die kleiner als 1.000 (Priorität für die Standardregel) ist. Je höher der Wert ist, desto geringer ist die Priorität.

        3. Legen Sie die Option **Quelle** auf **Alle** fest.

        4. Wählen Sie unter **Dienst** die Option **WinRM**. Das **Protokoll** wird automatisch auf **TCP** festgelegt, und der **Portbereich** lautet **5986**.

        5. Klicken Sie auf **OK** , um die Regel zu erstellen.

            ![Screenshot des Blatts „Eingangssicherheitsregel hinzufügen“. Wie Werte sind wie im Vorgang beschrieben ausgefüllt, und die Schaltfläche „OK“ ist hervorgehoben.](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt5.png)

4. Im letzten Schritt muss die Netzwerksicherheitsgruppe einem Subnetz oder einer bestimmten Netzwerkschnittstelle zugeordnet werden. Führen Sie die folgenden Schritte aus, um Ihre Netzwerksicherheitsgruppe einem Subnetz zuzuordnen.
    1. Navigieren Sie zu **Subnetze**.
    2. Klicken Sie auf **+ Zuordnen**.
        ![Screenshot der Seite „Subnetze“. Das Pluszeichen und das Wort „Zuordnen“ befinden sich nebeneinander und sind hervorgehoben.](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt7.png)

    3. Wählen Sie das virtuelle Netzwerk und dann das gewünschte Subnetz aus.
    4. Klicken Sie auf **OK** , um die Regel zu erstellen.

        ![Screenshot der Seite „Subnetz zuordnen“. Das virtuelle Netzwerk und die Schaltfläche „OK“ sind ausgewählt.](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt11.png)

Nach dem Erstellen der Regel können Sie die Details zum Ermitteln der öffentlichen virtuellen IP-Adresse (VIP) anzeigen. Notieren Sie sich diese Adresse.


