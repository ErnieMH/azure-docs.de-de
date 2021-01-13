---
title: Erstellen eines Python-Runbooks in Azure Automation
description: Dieser Artikel vermittelt Ihnen, wie Sie ein einfaches Python-Runbook erstellen, testen und veröffentlichen.
services: automation
ms.subservice: process-automation
ms.date: 04/19/2020
ms.topic: tutorial
ms.custom: has-adal-ref, devx-track-python
ms.openlocfilehash: e12327651165606e6a9b571d410f547a09a8ec8e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87847923"
---
# <a name="tutorial-create-a-python-runbook"></a>Tutorial: Erstellen eines Python-Runbooks

Dieses Tutorial führt Sie durch die Erstellung eines [Python-Runbooks](../automation-runbook-types.md#python-runbooks) in Azure Automation. Python-Runbooks werden unter Python-2 kompiliert. Sie können den Code des Runbooks direkt mit dem Text-Editor im Azure-Portal bearbeiten.

> [!div class="checklist"]
> * Erstellen eines einfachen Python-Runbooks
> * Testen und Veröffentlichen des Runbooks
> * Ausführen des Runbookauftrags und Nachverfolgen seines Status
> * Aktualisieren des Runbooks zum Starten eines virtuellen Azure-Computers mit Runbookparametern

> [!NOTE]
> Die Verwendung eines Webhooks zum Starten eines Python-Runbooks wird nicht unterstützt.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

- Azure-Abonnement. Wenn Sie noch kein Abonnement haben, können Sie Ihre [MSDN-Abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder sich für ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) registrieren.
- [Automation-Konto](../index.yml) dient zur Aufbewahrung des Runbooks und zur Authentifizierung gegenüber Azure-Ressourcen. Dieses Konto muss über die Berechtigung zum Starten und Beenden des virtuellen Computers verfügen.
- Einen virtuellen Azure-Computer. Da dieser Computer gestartet und beendet wird, sollte es sich nicht um einen virtuellen Computer in der Produktionsumgebung handeln.

## <a name="create-a-new-runbook"></a>Erstellen eines neuen Runbooks

Erstellen Sie zunächst ein einfaches Runbook, das den Text *Hello World* ausgibt.

1. Öffnen Sie im Azure-Portal Ihr Automation-Konto.

    Die Seite des Automation-Kontos bietet einen kurzen Überblick über die Ressourcen des Kontos. Sie sollten bereits über einige Objekte verfügen. Bei den meisten Objekten handelt es sich um die Module, die automatisch in einem neuen Automation-Konto enthalten sind. Darunter müsste sich auch das in den [Voraussetzungen](#prerequisites)erwähnte Anmeldeinformationsobjekt befinden.

2. Wählen Sie unter **Prozessautomatisierung** die Option **Runbooks** aus, um die Liste der Runbooks zu öffnen.

3. Wählen Sie **Runbook hinzufügen** aus, um ein neues Runbook zu erstellen.

4. Nennen Sie das Runbook **MyFirstRunbook-Python**.

5. Wählen Sie als **Runbooktyp** die Option **Python 2** aus.

6. Klicken Sie auf **Erstellen** , um das Runbook zu erstellen und den Text-Editor zu öffnen.

## <a name="add-code-to-the-runbook"></a>Hinzufügen von Code zum Runbook

Fügen Sie nun einen einfachen Befehl hinzu, um den Text `Hello World` auszugeben.

```python
print("Hello World!")
```

Klicken Sie auf **Speichern**, um das Runbook zu speichern.

## <a name="test-the-runbook"></a>Testen des Runbooks

Bevor Sie das Runbook für die Verwendung in der Produktionsumgebung veröffentlichen, sollten Sie sich vergewissern, dass es ordnungsgemäß funktioniert. Beim Testen eines Runbooks führen Sie die Entwurfsversion des Runbooks aus und sehen sich interaktiv die Ausgabe an.

1. Klicken Sie auf **Testbereich** , um den Testbereich zu öffnen.

2. Klicken Sie auf **Starten** , um den Test zu starten. Andere Optionen dürften nicht zur Verfügung stehen.

3. Ein [Runbookauftrag](../automation-runbook-execution.md) wird erstellt, und der dazugehörige Status wird angezeigt.
   Der Auftrag besitzt zunächst den Status „In der Warteschlange“. Hiermit wird angegeben, dass der Auftrag darauf wartet, dass in der Cloud ein Runbook Worker verfügbar wird. Wird der Auftrag von einem Worker übernommen, wechselt der Status zu „Wird gestartet“ und anschließend zu „Wird ausgeführt“, wenn die Ausführung des Runbooks tatsächlich gestartet wurde.

4. Nach Abschluss des Runbookauftrags wird die Ausgabe angezeigt. In diesem Fall sollte `Hello World` angezeigt werden.

5. Schließen Sie den Testbereich, um zum Zeichenbereich zurückzukehren.

## <a name="publish-and-start-the-runbook"></a>Veröffentlichen und Starten des Runbooks

Das erstellte Runbook befindet sich immer noch im Entwurfsmodus. Sie müssen das Runbook veröffentlichen, um es in der Produktionsumgebung ausführen zu können. Beim Veröffentlichen eines Runbooks wird die vorhandene veröffentlichte Version durch die Entwurfsversion überschrieben. In diesem Fall ist noch keine veröffentlichte Version vorhanden, da Sie das Runbook gerade erst erstellt haben.

1. Klicken Sie auf **Veröffentlichen**, um das Runbook zu veröffentlichen, und bestätigen Sie den Vorgang mit **Ja**.

2. Wenn Sie nach links scrollen, um das Runbook auf der Seite „Runbooks“ anzuzeigen, wird für das Runbook **Veröffentlicht** als **Erstellungsstatus** angezeigt.

3. Scrollen Sie wieder nach rechts , um den Bereich für **MyFirstRunbook-Python** anzuzeigen.

   Mit den Optionen am oberen Rand können Sie das Runbook starten, das Runbook anzeigen oder seinen Start für einen späteren Zeitpunkt planen.

4. Klicken Sie auf **Start** und dann auf dem Blatt „Runbook starten“ auf **OK**.

5. Ein Auftragsbereich für den soeben erstellten Runbookauftrag wird angezeigt. Dieser Bereich kann zwar geschlossen werden, lassen Sie ihn jedoch geöffnet, um den Status des Auftrags verfolgen zu können.

6. Der Auftragsstatus wird unter **Auftragszusammenfassung** angezeigt und entspricht den Statusoptionen, die Sie bereits beim Testen des Runbooks gesehen haben.

7. Wenn der Runbookstatus „Abgeschlossen“ lautet, klicken Sie auf **Ausgabe**. Die Seite „Ausgabe“ wird geöffnet, auf der `Hello World` angezeigt wird.

8. Schließen Sie den Ausgabebereich.

9. Klicken Sie auf **Alle Protokolle**, um den Bereich „Datenströme“ für den Runbookauftrag zu öffnen. Im Ausgabestream sollte nur `Hello World` angezeigt werden. In diesem Bereich können aber auch andere Streams für einen Runbookauftrag angezeigt werden, etwa ausführliche Streams und Fehlerstreams, sofern das Runbook in diese schreibt.

10. Schließen Sie den Datenstrom- und den Auftragsbereich, um zum Bereich „MyFirstRunbook-Python“ zurückzukehren.

11. Klicken Sie auf **Aufträge**, um den Auftragsbereich für dieses Runbook zu öffnen. Diese Seite enthält alle von diesem Runbook erstellten Aufträge. Hier ist nur ein einzelner Auftrag aufgeführt, da Sie den Auftrag bislang erst einmal ausgeführt haben.

12. Wenn Sie auf diesen Auftrag klicken, wird wieder der Auftragsbereich geöffnet, den Sie sich beim Starten des Runbooks angesehen haben. In diesem Bereich können Sie bereits ausgeführte Aufträge öffnen und Details zu jedem Auftrag anzeigen, der für ein bestimmtes Runbook erstellt wurde.

## <a name="add-authentication-to-manage-azure-resources"></a>Hinzufügen von Authentifizierungsfunktionen für die Verwaltung von Azure-Ressourcen

Sie haben Ihr Runbook inzwischen zwar getestet und veröffentlicht, bislang ist es aber noch nicht sonderlich hilfreich. Sie möchten damit ja eigentlich Azure-Ressourcen verwalten.
Dazu muss das Skript bei der Authentifizierung die Anmeldeinformationen für Ihr Automation-Konto verwenden. Das [Azure Automation-Hilfsprogrammpaket](https://github.com/azureautomation/azure_automation_utility) vereinfacht die Authentifizierung und die Interaktion mit Azure-Ressourcen.

> [!NOTE]
> Das Automation-Konto muss mit dem Dienstprinzipalfeature erstellt worden sein, damit ein Zertifikat für das ausführende Konto vorhanden ist.
> Wenn Ihr Automation-Konto nicht mit dem Dienstprinzipal erstellt wurde, können Sie die Authentifizierung wie unter [Authentifizieren bei den Azure-Verwaltungsbibliotheken für Python](/azure/python/python-sdk-azure-authenticate) beschrieben durchführen.

1. Öffnen Sie den Text-Editor, indem Sie im Bereich „MyFirstRunbook-Python“ auf **Bearbeiten** klicken.

2. Fügen Sie für die Authentifizierung bei Azure den folgenden Code hinzu:

   ```python
   import os
   from azure.mgmt.compute import ComputeManagementClient
   import azure.mgmt.resource
   import automationassets

   def get_automation_runas_credential(runas_connection):
       from OpenSSL import crypto
       import binascii
       from msrestazure import azure_active_directory
       import adal

       # Get the Azure Automation RunAs service principal certificate
       cert = automationassets.get_automation_certificate("AzureRunAsCertificate")
       pks12_cert = crypto.load_pkcs12(cert)
       pem_pkey = crypto.dump_privatekey(crypto.FILETYPE_PEM,pks12_cert.get_privatekey())

       # Get run as connection information for the Azure Automation service principal
       application_id = runas_connection["ApplicationId"]
       thumbprint = runas_connection["CertificateThumbprint"]
       tenant_id = runas_connection["TenantId"]

       # Authenticate with service principal certificate
       resource ="https://management.core.windows.net/"
       authority_url = ("https://login.microsoftonline.com/"+tenant_id)
       context = adal.AuthenticationContext(authority_url)
       return azure_active_directory.AdalAuthentication(
       lambda: context.acquire_token_with_client_certificate(
               resource,
               application_id,
               pem_pkey,
               thumbprint)
       )

   # Authenticate to Azure using the Azure Automation RunAs service principal
   runas_connection = automationassets.get_automation_connection("AzureRunAsConnection")
   azure_credential = get_automation_runas_credential(runas_connection)
   ```

## <a name="add-code-to-create-python-compute-client-and-start-the-vm"></a>Hinzufügen von Code zum Erstellen eines Python-Computeclients und Starten des virtuellen Computers

Erstellen Sie für die Verwendung von virtuellen Azure-Computern eine Instanz des [Azure-Compute-Clients für Python](/python/api/azure-mgmt-compute/azure.mgmt.compute.computemanagementclient).

Verwenden Sie den Computeclient zum Starten des virtuellen Computers. Fügen Sie den folgenden Code zum Runbook hinzu:

```python
# Initialize the compute management client with the RunAs credential and specify the subscription to work against.
compute_client = ComputeManagementClient(
    azure_credential,
    str(runas_connection["SubscriptionId"])
)


print('\nStart VM')
async_vm_start = compute_client.virtual_machines.start(
    "MyResourceGroup", "TestVM")
async_vm_start.wait()
```

`MyResourceGroup` ist der Name der Ressourcengruppe, die den virtuellen Computer enthält, und `TestVM` ist der Name des virtuellen Computers, der gestartet werden soll.

Testen Sie das Runbook, und führen Sie es erneut aus, um zu ermitteln, ob der virtuelle Computer gestartet wird.

## <a name="use-input-parameters"></a>Verwenden von Eingabeparametern

Das Runbook verwendet derzeit hartcodierte Werte für die Namen der Ressourcengruppe und des virtuellen Computers. Wir fügen nun Code hinzu, der diese Werte aus Eingabeparametern abruft.

Sie verwenden die Variable `sys.argv`, um die Parameterwerte abzurufen. Fügen Sie im Runbook den folgenden Code unmittelbar nach den anderen `import`-Anweisungen hinzu:

```python
import sys

resource_group_name = str(sys.argv[1])
vm_name = str(sys.argv[2])
```

Dadurch wird das `sys`-Modul importiert, und es werden zwei Variablen für die Namen der Ressourcengruppe und des virtuellen Computers erstellt. Beachten Sie, dass das Element der Argumentliste (`sys.argv[0]`) der Name des Skripts ist und nicht vom Benutzer eingegeben wird.

Nun können Sie die letzten beiden Zeilen des Runbooks ändern, um anstelle hartcodierter Werte die Eingabeparameterwerte zu verwenden:

```python
async_vm_start = compute_client.virtual_machines.start(
    resource_group_name, vm_name)
async_vm_start.wait()
```

Beim Starten eines Python-Runbooks (entweder auf der Seite „Test“ oder als veröffentlichtes Runbook) können Sie die Werte für Parameter auf der Seite „Runbook starten“ unter **Parameter** eingeben.

Wenn Sie mit der Eingabe eines Werts im ersten Feld begonnen haben, wird jeweils ein weiteres Feld angezeigt, sodass Sie die erforderliche Anzahl von Parameterwerten eingeben können.

Die Werte stehen dem Skript wie im soeben hinzugefügten Code als `sys.argv`-Array zur Verfügung.

Geben Sie den Namen Ihrer Ressourcengruppe als Wert für den ersten Parameter und den Namen des zu startenden virtuellen Computers als Wert für den zweiten Parameter ein.

![Eingeben von Parameterwerten](../media/automation-tutorial-runbook-textual-python/runbook-python-params.png)

Klicken Sie auf **OK**, um das Runbook zu starten. Das Runbook wird ausgeführt und startet den angegebenen virtuellen Computer.

## <a name="error-handling-in-python"></a>Fehlerbehandlung in Python

Sie können auch die folgenden Konventionen verwenden, um verschiedene Datenströme aus Ihren Python-Runbooks abzurufen – einschließlich WARNUNG, FEHLER und DEBUGGEN.

```python
print("Hello World output")
print("ERROR: - Hello world error")
print("WARNING: - Hello world warning")
print("DEBUG: - Hello world debug")
print("VERBOSE: - Hello world verbose")
```

Das folgende Beispiel zeigt die Verwendung dieser Konvention in einem Block vom Typ `try...except`.

```python
try:
    raise Exception('one', 'two')
except Exception as detail:
    print 'ERROR: Handling run-time error:', detail
```

> [!NOTE]
> Azure Automation unterstützt `sys.stderr` nicht.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zu den ersten Schritten mit PowerShell-Runbooks finden Sie unter [Erstellen eines PowerShell-Runbooks](automation-tutorial-runbook-textual-powershell.md).
- Informationen zu den ersten Schritten mit grafischen Runbooks finden Sie unter [Erstellen eines grafischen Runbooks](automation-tutorial-runbook-graphical.md).
- Informationen zu den ersten Schritten mit PowerShell-Workflow-Runbooks finden Sie unter [Erstellen eines PowerShell-Workflow-Runbooks](automation-tutorial-runbook-textual.md).
- Weitere Informationen zu den verschiedenen Runbooktypen sowie zu ihren Vorteilen und Einschränkungen finden Sie unter [Azure Automation-Runbooktypen](../automation-runbook-types.md).
- Weitere Informationen zum Entwickeln für Azure mit Python finden Sie unter [Azure für Python-Entwickler](/azure/python/).
- Beispiel „Python 2 Runbooks“ finden Sie unter [Azure Automation GitHub](https://github.com/azureautomation/runbooks/tree/master/Utility/Python).
