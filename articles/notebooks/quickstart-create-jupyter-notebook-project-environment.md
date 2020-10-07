---
title: Erstellen eines Projekts mit benutzerdefinierter Umgebung in Azure Notebooks (Vorschauversion)
description: Erstellen Sie ein neues Projekt in Azure Notebooks (Vorschauversion), das mit einem bestimmten Satz installierter Pakete und Startskripts konfiguriert wird.
ms.topic: quickstart
ms.date: 12/04/2018
ms.custom: devx-track-python
ms.openlocfilehash: 655c016b55abdcf4b6f546a1fe16348ec4c83724
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "87853363"
---
# <a name="quickstart-create-a-project-with-a-custom-environment-in-azure-notebooks-preview"></a>Schnellstart: Erstellen eines Projekts mit benutzerdefinierter Umgebung in Azure Notebooks (Vorschauversion)

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

Ein Projekt in Azure Notebooks ist eine Sammlung von Dateien, wie etwa Notebooks, Datendateien, Dokumentation, Bildern usw. in Verbindung mit einer Umgebung, die mit bestimmten Setupbefehlen konfiguriert werden kann. Indem Sie die Umgebung zugleich mit dem Projekt definieren, verhelfen Sie jedem, der das Projekt in das eigene Azure Notebooks-Konto klont, zu allen Informationen, die zum Neuerstellen der erforderlichen Umgebung benötigt werden.

## <a name="create-a-project"></a>Erstellen eines Projekts

1. Navigieren Sie zu [Azure Notebooks](https://notebooks.azure.com), und melden Sie sich an (ausführliche Informationen hierzu finden Sie unter [Schnellstart: Anmelden bei Azure Notebooks](quickstart-sign-in-azure-notebooks.md)).

1. Wählen Sie oben auf Ihrer öffentlichen Profilseite **Meine Projekte** aus:

    ![Link „Meine Projekte“ am oberen Rand des Browserfensters](media/quickstarts/my-projects-link.png)

1. Klicken Sie auf der Seite **My Projects** (Meine Projekte) auf **+ New Project** (+ Neues Projekt, Tastenkombination: n). Wenn Ihr Browserfenster schmal ist, wird möglicherweise nur **+** für die Schaltfläche angezeigt:

    ![Befehl „New Project“ (Neues Projekt) auf der Seite „My Projects“ (Meine Projekte)](media/quickstarts/new-project-command.png)

1. Geben Sie folgende Informationen im angezeigten Popupfenster **Create New Project** (Neues Projekt erstellen) an, und klicken Sie anschließend auf **Create** (Erstellen):

    - **Projektname**: Projekt mit benutzerdefinierter Umgebung
    - **Projekt-ID**: projekt-benutzerdefinierte-umgebung
    - **Public project** (Öffentliches Projekt): (nicht aktiviert)
    - **Create a README.md** (README-Datei erstellen): (nicht aktiviert)

1. Nach einigen Augenblicken navigiert Azure Notebooks zum neuen Projekt. Fügen Sie dem Projekt ein Notebook hinzu, indem Sie das Dropdownelement **+ New** (+ Neu) (das möglicherweise nur als **+** angezeigt wird) und dann **Notebook** auswählen.

1. Geben Sie dem Notebook einen Namen wie *Custom environment.ipynb*, wählen Sie **Python 3.6** als Sprache und dann **Neu** aus.

## <a name="add-a-custom-setup-step"></a>Hinzufügen eines benutzerdefinierten Setupschritts

1. Wählen Sie auf der Projektseite **Projekteinstellungen** aus.

    ![Befehl „Projekteinstellungen“](media/quickstarts/project-settings-command.png)

1. Wählen Sie im Popupfenster **Projekteinstellungen** die Registerkarte **Umgebung** und dann unter **Environment Setup Steps** (Schritte zum Einrichten der Umgebung) **+ Hinzufügen** aus:

    ![Befehl zum Hinzufügen eines neuen Setupschritts für die Umgebung](media/quickstarts/environment-add-command.png)

1. Der Befehl **+ Hinzufügen** erstellt einen Schritt, der durch einen Vorgang und eine Zieldatei definiert ist, die Sie aus den Dateien in Ihrem Projekt auswählen. Die folgenden Operationen werden unterstützt:

   | Vorgang | BESCHREIBUNG |
   | --- | --- |
   | Requirements.txt | Die Abhängigkeiten von Python-Projekten sind in einer Datei „requirements.txt“ definiert. Wählen Sie mit dieser Option die entsprechende Datei aus der Dateiliste des Projekts aus, und wählen Sie in dem zusätzlichen Dropdownfeld, das angezeigt wird, auch die Python-Version aus. Wählen Sie ggf. **Abbrechen** aus, um zum Projekt zurückzukehren, laden Sie die Datei hoch oder erstellen Sie sie, und kehren Sie dann zu **Projekteinstellungen** > **Umgebung** zurück, und erstellen Sie den neuen Schritt. Wenn dieser Schritt implementiert ist, bewirkt das Ausführen eines Notebooks im Projekt die automatische Ausführung von `pip install -r <file>` |
   | Shellskript | Verwenden Sie diese Option, um ein Bash-Shellskript festzulegen (normalerweise eine Datei mit der Erweiterung *.sh*), die alle Befehle enthält, die Sie in der Umgebung initialisieren möchten. |
   | Environment.yml | Ein Python-Projekt, das Conda für die Verwaltung der Umgebung einsetzt, verwendet eine Datei *environments.yml* zum Beschreiben der Abhängigkeiten. Wählen Sie für diese Option die entsprechende Datei in der Dateiliste des Projekts aus. |

   > [!WARNING]
   > Da es sich hierbei um einen in der Entwicklung befindlichen Vorschaudienst handelt, gibt es derzeit ein bekanntes Problem, das dazu führt, dass die Einstellung `Environment.yml` wie erwartet auf Ihr Projekt angewendet wird. Die angegebene Umgebungsdatei wird vom Projekt und den darin enthaltenen Jupyter-Notebooks momentan nicht geladen.

1. Um einen Setupschritt zu entfernen, wählen Sie das **X** rechts neben dem Schritt aus.

1. Wenn alle Setupschritte implementiert wurde, wählen Sie **Speichern** aus. (Wählen Sie **Abbrechen** aus, um Änderungen zu verwerfen).

1. Um Ihre Umgebung zu testen, erstellen Sie ein neues Notebook und führen es aus, erstellen Sie dann eine Codezelle mit Anweisungen, die von einem Paket in der Umgebung abhängig sind, etwa dadurch, dass sie eine Python-Anweisung `import` enthalten. Wenn die Anweisung erfolgreich ausgeführt wird, wurde das erforderliche Paket erfolgreich in der Umgebung installiert.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Verwalten und Konfigurieren von Projekten in Azure Notebooks](configure-manage-azure-notebooks-projects.md)

> [!div class="nextstepaction"]
> [Tutorial: Erstellen und Ausführen einer Jupyter Notebook-Datei mit Python](tutorial-create-run-jupyter-notebook.md)
