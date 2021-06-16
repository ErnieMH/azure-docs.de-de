---
title: Konfigurieren von Azure-VM-Konnektivität
titleSuffix: Azure SQL Managed Instance
description: Herstellen einer Verbindung mit Azure SQL Managed Instance von einer Azure-VM aus über SQL Server Management Studio.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: connect
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: zoran-rilak-msft
ms.author: zoranrilak
ms.reviewer: mathoma, srbozovi, bonova
ms.date: 02/18/2019
ms.openlocfilehash: f8a708caf410ab53da6f9c2e77325307506ea9de
ms.sourcegitcommit: 3bb9f8cee51e3b9c711679b460ab7b7363a62e6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/14/2021
ms.locfileid: "112076022"
---
# <a name="quickstart-configure-an-azure-vm-to-connect-to-azure-sql-managed-instance"></a>Schnellstart: Konfigurieren einer Azure-VM für das Herstellen einer Verbindung mit Azure SQL Managed Instance
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

In dieser Schnellstartanleitung erfahren Sie, wie Sie einen virtuellen Azure-Computer konfigurieren, um über SQL Server Management Studio (SSMS) eine Verbindung mit Azure SQL Managed Instance herzustellen. 


Eine Schnellstartanleitung, die zeigt, wie Sie stattdessen eine Verbindung von einem lokalen Clientcomputer über eine Point-to-Site-Verbindung herstellen, finden Sie unter [Konfigurieren einer Point-to-Site-Verbindung](point-to-site-p2s-configure.md).

## <a name="prerequisites"></a>Voraussetzungen

Diese Schnellstartanleitung verwendet als Ausgangspunkt die Ressourcen, die in [Erstellen einer verwalteten Instanz](instance-create-quickstart.md) erstellt wurden.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

## <a name="create-a-new-subnet-vnet"></a>Erstellen eines neuen VNet-Subnetzes

In den folgenden Schritten wird im VNET der SQL Managed Instance ein neues Subnetz erstellt, sodass ein virtueller Azure-Computer eine Verbindung mit der verwalteten Instanz herstellen kann. Das Subnetz der SQL Managed Instance ist für verwaltete Instanzen reserviert. In diesem Subnetz können keine anderen Ressourcen (beispielsweise virtuelle Azure-Computer) erstellt werden.

1. Öffnen Sie die Ressourcengruppe für die verwaltete Instanz, die Sie in der Schnellstartanleitung [Erstellen einer verwalteten Instanz](instance-create-quickstart.md) erstellt haben. Wählen Sie das virtuelle Netzwerk für Ihre verwaltete Instanz aus.

   ![SQL Managed Instance-Ressourcen](./media/connect-vm-instance-configure/resources.png)

2. Wählen Sie **Subnetze** und anschließend **+ Subnetz** aus, um ein neues Subnetz zu erstellen.

   ![SQL Managed Instance-Subnetze](./media/connect-vm-instance-configure/subnets.png)

3. Füllen Sie das Formular mit den Angaben aus der folgenden Tabelle aus:

   | Einstellung| Vorgeschlagener Wert | BESCHREIBUNG |
   | ---------------- | ----------------- | ----------- |
   | **Name** | Ein gültiger Name|Gültige Namen finden Sie unter [Benennungskonventionen](/azure/architecture/best-practices/resource-naming).|
   | **Adressbereich (CIDR-Block)** | Ein gültiger Bereich | Der Standardwert reicht für diesen Schnellstart aus.|
   | **Netzwerksicherheitsgruppe** | Keine | Der Standardwert reicht für diesen Schnellstart aus.|
   | **Routingtabelle** | Keine | Der Standardwert reicht für diesen Schnellstart aus.|
   | **Dienstendpunkte** | 0 ausgewählt | Der Standardwert reicht für diesen Schnellstart aus.|
   | **Subnetzdelegierung** | Keine | Der Standardwert reicht für diesen Schnellstart aus.|

   ![Neues SQL Managed Instance-Subnetz für die Client-VM](./media/connect-vm-instance-configure/new-subnet.png)

4. Wählen Sie **OK** aus, um dieses zusätzliche Subnetz im VNET der SQL Managed Instance zu erstellen.

## <a name="create-a-vm-in-the-new-subnet"></a>Erstellen einer VM im neuen Subnetz 

In den folgenden Schritten wird veranschaulicht, wie Sie einen virtuellen Computer im neuen Subnetz erstellen, um eine Verbindung mit SQL Managed Instance herzustellen.

## <a name="prepare-the-azure-virtual-machine"></a>Vorbereiten des virtuellen Azure-Computers

Da sich SQL Managed Instance in Ihrem privaten virtuellen Netzwerk befindet, müssen Sie einen virtuellen Azure-Computer erstellen, auf dem ein SQL-Clienttool wie SQL Server Management Studio oder Azure Data Studio installiert ist. Mithilfe dieses Tools können Sie eine Verbindung mit SQL Managed Instance herstellen und Abfragen ausführen. In diesem Schnellstart wird SQL Server Management Studio verwendet.

Die einfachste Möglichkeit zum Erstellen eines virtuellen Clientcomputers mit allen erforderlichen Tools ist die Verwendung von Azure Resource Manager-Vorlagen.

1. Vergewissern Sie sich auf einer anderen Browserregisterkarte, dass Sie beim Azure-Portal angemeldet sind. Wählen Sie anschließend die folgende Schaltfläche aus, um einen virtuellen Clientcomputer zu erstellen und SQL Server Management Studio zu installieren:

   [![Das Bild zeigt eine Schaltfläche mit der Bezeichnung „Bereitstellung in Azure“.](https://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjovanpop-msft%2Fazure-quickstart-templates%2Fsql-win-vm-w-tools%2F201-vm-win-vnet-sql-tools%2Fazuredeploy.json)

2. Füllen Sie das Formular mit den Angaben aus der folgenden Tabelle aus:

   | Einstellung| Vorgeschlagener Wert | BESCHREIBUNG |
   | ---------------- | ----------------- | ----------- |
   | **Abonnement** | Ein gültiges Abonnement | Hierbei muss es sich um ein Abonnement mit der Berechtigung zum Erstellen neuer Ressourcen handeln. |
   | **Ressourcengruppe** |Die Ressourcengruppe, die Sie im Schnellstart [Erstellen einer verwalteten SQL-Instanz](instance-create-quickstart.md) angegeben haben.|In dieser Ressourcengruppe muss das VNET enthalten sein.|
   | **Location** | Der Standort für die Ressourcengruppe | Dieser Wert wird basierend auf der ausgewählten Ressourcengruppe ausgefüllt. |
   | **Name des virtuellen Computers**  | Ein gültiger Name | Gültige Namen finden Sie unter [Benennungskonventionen](/azure/architecture/best-practices/resource-naming).|
   |**Benutzername des Administrators**|Ein beliebiger gültiger Benutzername|Gültige Namen finden Sie unter [Benennungskonventionen](/azure/architecture/best-practices/resource-naming). Verwenden Sie nicht „serveradmin“. Hierbei handelt es sich um eine reservierte Rolle auf Serverebene.<br>Dieser Benutzername wird beim [Herstellen einer Verbindung mit dem virtuellen Computer](#connect-to-the-virtual-machine) verwendet.|
   |**Kennwort**|Ein gültiges Kennwort|Das Kennwort muss mindestens zwölf Zeichen lang sein und die [definierten Anforderungen an die Komplexität](../../virtual-machines/windows/faq.yml#what-are-the-password-requirements-when-creating-a-vm-) erfüllen.<br>Dieses Kennwort wird beim [Herstellen einer Verbindung mit dem virtuellen Computer](#connect-to-the-virtual-machine) verwendet.|
   | **Größe des virtuellen Computers** | Eine beliebige gültige Größe | Für diese Schnellstartanleitung ist der Standardwert der Vorlage (**Standard_B2s**) ausreichend. |
   | **Location**|[resourceGroup().location].| Ändern Sie diesen Wert nicht. |
   | **Name des virtuellen Netzwerks**|Das virtuelle Netzwerk, in dem Sie die verwaltete Instanz erstellt haben.|
   | **Subnetzname**|Der Name des Subnetzes, das Sie im vorherigen Verfahren erstellt haben| Wählen Sie nicht das Subnetz aus, in dem Sie die verwaltete Instanz erstellt haben.|
   | **Artefaktspeicherort** | [deployment().properties.templateLink.uri] | Ändern Sie diesen Wert nicht. |
   | **SAS-Token des Artefaktspeicherorts** | Nicht ausfüllen | Ändern Sie diesen Wert nicht. |

   ![Erstellen des virtuellen Clientcomputers](./media/connect-vm-instance-configure/create-client-sql-vm.png)

   Wenn Sie unter [Erstellen einer verwalteten SQL-Instanz](instance-create-quickstart.md) den vorgeschlagenen VNet-Namen und das Standardsubnetz verwendet haben, müssen Sie die beiden letzten Parameter nicht ändern. Andernfalls müssen Sie diese Werte in die Werte ändern, die Sie beim Einrichten der Netzwerkumgebung eingegeben haben.

3. Aktivieren Sie das Kontrollkästchen **Ich stimme den oben genannten Geschäftsbedingungen zu**.
4. Wählen Sie **Kaufen** aus, um den virtuellen Azure-Computer in Ihrem Netzwerk bereitzustellen.
5. Wählen Sie das Symbol **Benachrichtigungen**, um den Status der Bereitstellung anzuzeigen.

> [!IMPORTANT]
> Warten Sie nach dem Erstellen des virtuellen Computers etwa 15 Minuten, damit die Skripts nach der Erstellung Zeit für die Installation von SQL Server Management Studio haben.

## <a name="connect-to-the-virtual-machine"></a>Verbinden mit dem virtuellen Computer

In den folgenden Schritten wird veranschaulicht, wie Sie für Ihren neu erstellten virtuellen Computer eine Remotedesktopverbindung herstellen.

1. Navigieren Sie zur VM-Ressource, nachdem die Bereitstellung abgeschlossen wurde.

    ![Im Screenshot ist das Azure-Portal zu sehen, in dem die „Übersicht“-Seite für eine virtuelle Maschine angezeigt wird, und ist „Verbinden“ markiert.](./media/connect-vm-instance-configure/vm.png)  

2. Wählen Sie **Verbinden**.

   Eine Formular für eine RDP-Datei (Remotedesktopprotokoll) wird mit der öffentlichen IP-Adresse und der Portnummer für den virtuellen Computer angezeigt.

   ![RDP-Formular](./media/connect-vm-instance-configure/rdp.png)  

3. Wählen Sie **RDP-Datei herunterladen** aus.

   > [!NOTE]
   > Sie können für die Verbindung mit Ihrem virtuellen Computer auch SSH verwenden.

4. Schließen Sie das Formular **Mit virtuellem Computer verbinden**.
5. Öffnen Sie die heruntergeladene RDP-Datei, um eine Verbindung mit Ihrem virtuellen Computer herzustellen.
6. Wenn Sie dazu aufgefordert werden, wählen Sie **Connect** aus. Auf einem Macintosh benötigen Sie einen RDP-Client, z. B. diesen [Remotedesktopclient](https://apps.apple.com/app/microsoft-remote-desktop-10/id1295203466?mt=12) aus dem Mac App Store.

7. Geben Sie den Benutzernamen und das Kennwort ein, den bzw. das Sie beim Erstellen des virtuellen Computers festgelegt haben, und wählen Sie anschließend **OK** aus.

8. Während des Anmeldevorgangs wird unter Umständen eine Zertifikatwarnung angezeigt. Wählen Sie **Ja** bzw. **Weiter** aus, um mit dem Herstellen der Verbindung fortzufahren.

Auf dem Server-Manager-Dashboard wird eine Verbindung mit Ihrem virtuellen Computer hergestellt.

## <a name="connect-to-sql-managed-instance"></a>Herstellen einer Verbindung mit einer verwalteten SQL-Instanz 

1. Öffnen Sie SQL Server Management Studio auf dem virtuellen Computer.

   Das Öffnen dauert einen Moment, da die Konfiguration bei diesem ersten Start von SSMS abgeschlossen wird.
2. Geben Sie im Dialogfeld **Mit Server verbinden** den vollqualifizierten **Hostnamen** für Ihre verwaltete Instanz in das Feld **Servername** ein. Wählen Sie **SQL Server-Authentifizierung** aus, geben Sie Ihren Benutzernamen und Ihr Kennwort ein, und wählen Sie anschließend **Verbinden** aus.

    ![SSMS-Verbindung](./media/connect-vm-instance-configure/ssms-connect.png)  

Nach der Verbindungsherstellung können Sie Ihre System- und Benutzerdatenbanken auf dem Knoten „Datenbanken“ und verschiedene Objekte auf den Knoten „Sicherheit“, „Serverobjekte, „Replikation“, „Verwaltung“, „SQL Server-Agent“ und „XEvent Profiler“ anzeigen.

## <a name="next-steps"></a>Nächste Schritte

- Eine Schnellstartanleitung, die zeigt, wie Sie von einem lokalen Clientcomputer über eine Point-to-Site-Verbindung eine Verbindung herstellen, finden Sie unter [Konfigurieren einer Point-to-Site-Verbindung](point-to-site-p2s-configure.md).
- Eine Übersicht über die Verbindungsoptionen für Anwendungen finden Sie unter [Herstellen einer Verbindung zwischen einer Anwendung und SQL Managed Instance](connect-application-instance.md).
- Zum Wiederherstellen einer vorhandenen SQL Server-Datenbank von einer lokalen Instanz in eine verwaltete Instanz können Sie entweder [Azure Database Migration Service für die Migration](../../dms/tutorial-sql-server-to-managed-instance.md) oder den [T-SQL-Befehl „RESTORE“](restore-sample-database-quickstart.md) zum Wiederherstellen aus einer Datenbanksicherungsdatei verwenden.
