---
title: Erstellen und Teilen einer Jupyter Notebook-Instanz in Azure Notebooks (Vorschauversion)
description: Erstellen Sie schnell eine Jupyter Notebook-Instanz in Azure Notebooks (Vorschauversion), führen Sie sie aus, und teilen Sie sie dann mit anderen.
ms.topic: quickstart
ms.date: 12/04/2018
ms.custom: devx-track-python
ms.openlocfilehash: 77575b5b9e006ac9881d03dc95509d55c53c26d7
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "88589332"
---
# <a name="quickstart-create-and-share-a-notebook-in-azure-notebooks-preview"></a>Schnellstart: Erstellen und Teilen eines Notebooks in Azure Notebooks (Vorschauversion)

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

In dieser Schnellstartanleitung erstellen Sie eine Jupyter Notebook-Instanz in Azure Notebooks, führen sie aus und geben sie dann für andere Benutzer frei. Jupyter ermöglicht es Ihnen, auf einfache Weise Markdowntext, ausführbaren Code, persistente Daten, Grafiken und Visualisierungen auf einer einzelnen Canvas zu kombinieren, die als „Notebook“ bezeichnet wird. Azure Notebooks ist ein kostenloser gehosteter Dienst zum Entwickeln und Ausführen von Jupyter Notebook-Instanzen in der Cloud ohne Installation.

## <a name="prerequisites"></a>Voraussetzungen
Keine.

## <a name="create-a-new-project-and-notebook"></a>Erstellen eines neuen Projekts und Notebooks

1. Wechseln Sie zur [Azure Notebooks-Website (https://notebooks.azure.com)](https://notebooks.azure.com), und melden Sie sich an. Ausführliche Informationen finden Sie unter [Schnellstart: Anmelden bei Azure Notebooks](quickstart-sign-in-azure-notebooks.md).

1. Wählen Sie oben auf Ihrer öffentlichen Profilseite **Meine Projekte** aus:

    ![Link „Meine Projekte“ am oberen Rand des Browserfensters](media/quickstarts/my-projects-link.png)

1. Wählen Sie auf der Seite **Meine Projekte** die Option **+ Neues Projekt** aus (Tastenkombination: n). Ist das Browserfenster schmal, wird die Schaltfläche unter Umständen nur als **+** angezeigt:

    ![Befehl „New Project“ (Neues Projekt) auf der Seite „My Projects“ (Meine Projekte)](media/quickstarts/new-project-command.png)

1. Geben Sie folgende Informationen im angezeigten Popupfenster **Create New Project** (Neues Projekt erstellen) an, und klicken Sie anschließend auf **Create** (Erstellen):

   - **Projektname**: Hallo-Welt in Python
   - **Projekt-ID**: hello-world-python
   - **Public project** (Öffentliches Projekt): (nicht aktiviert)
   - **Create a README.md** (README-Datei erstellen): (nicht aktiviert)

     ![Popupfenster „Neues Projekt“ mit Detailangaben](media/quickstarts/new-project-popup.png)

1. Nach einigen Augenblicken navigiert Azure Notebooks zum neuen Projekt. Fügen Sie dem Projekt ein Notebook hinzu, indem Sie das Dropdownelement **+ New** (+ Neu) (das möglicherweise nur als **+** angezeigt wird) und dann **Notebook** auswählen:

    :::image type="content" source="media/quickstarts/empty-project-new-notebook-button.png" alt-text="Neues leeres Projekt und Befehl zum Hinzufügen eines Notebooks" lightbox="media/quickstarts/empty-project-new-notebook-button.png":::

1. Geben Sie im Popup **Create New Notebook** (Neues Notebook erstellen), das angezeigt wird, einen Dateinamen für Ihr Notebook ein, z. B. *HelloWorldInPython.ipynb* (*.ipynb* bedeutet IronPython (Jupyter) Notebook), und wählen Sie **Python 3.6** als Sprache aus (auch als *Kernel* bezeichnet):

    ![Popup „Create New Notebook“ (Neues Notebook erstellen)](media/quickstarts/new-notebook-popup.png)

1. Wählen Sie **New** (Neu) aus, um die Erstellung des Notebooks abzuschließen, das dann in der Dateiliste Ihres Projekts angezeigt wird:

    ![Neues Notizbuch in der Dateiliste des Projekts](media/quickstarts/new-notebook-created.png)

## <a name="run-the-notebook"></a>Ausführen des Notebooks

1. Wählen Sie das neue Notebook aus, um es im Editor auszuführen; der von Ihnen gewählte Kernel (in diesem Beispiel Python 3.6) wird automatisch für das Notebook aktiviert:

    ![Ansicht eines neuen Notebooks in Azure Notebooks](media/quickstarts/create-notebook-first-open.png)

1. Standardmäßig weist das Notebook eine leere Codezelle auf. Um den Zelltyp in **Markdown** zu ändern, wählen Sie **Markdown** in der Zelltyp-Dropdownliste aus:

    ![Ändern des Zelltyps in einem neuen Notebook](media/quickstarts/create-notebook-cell-type.png)

1. Geben oder fügen Sie den folgenden Überschriftstext in die Zelle ein:

    ```markdown
    # Hello World in Python
    ```

1. Da Sie Markdown bearbeiten, wird der Text als Überschrift mit dem Zeichen "#" angezeigt. Um das Markdown in HTML wiederzugeben, wählen Sie die Schaltfläche **Run** (Ausführen) aus. Anschließend erstellt Azure Notebooks dahinter automatisch eine neue Codezelle:

    ![Die Schaltfläche „Run“ (Ausführen) für eine Zelle und wiedergegebenes Markdown](media/quickstarts/run-cell-markdown-render.png)

1. Geben Sie in der Codezelle den folgenden Python-Code ein:

    ```python
    from datetime import datetime

    now = datetime.now()
    msg = "Hello, Azure Notebooks! Today is %s"  % now.strftime("%A, %d %B, %Y")
    print(msg)
    ```

1. Wählen Sie **Run** (Ausführen, Tastenkombination: UMSCHALT+EINGABE) aus, um den Code auszuführen. Unterhalb der Zelle sollten Sie bei Erfolg eine Ausgabe ähnlich dem folgenden Text sehen:

    ```output
    Hello, Azure Notebooks! Today is Thursday, 15 November, 2018
    ```

1. Wählen Sie die Schaltfläche „Speichern“ aus, um Ihre Arbeit zu speichern:

    ![Symbol „Speichern“ auf der Jupyter Notebook-Symbolleiste](media/quickstarts/hello-results-save-icon.png)

1. Wählen Sie den Menübefehl **File** (Datei)  > **Close and Halt** (Beenden und schließen) aus, um den Server zu beenden und das Browserfenster zu schließen.

## <a name="share-the-notebook"></a>Teilen des Notebooks

Um Ihr Notebook zu teilen, wechseln Sie ggf. zur Projektseite zurück, klicken Sie mit der rechten Maustaste auf die Notebookdatei, wählen Sie **Copy Link** (Link kopieren, Tastenkombination: y) aus, und fügen Sie diesen Link in eine geeignete Nachricht ein (E-Mail, Chat usw.).

Auf der Projektseite können Sie außerdem das Menü **Freigabe** verwenden, um einen Link abzurufen, eine E-Mail-Nachricht mit dem Link zu erstellen oder einen HTML- und Markdown-Einbindungscode anzufordern:

![Projektbefehl „Freigabe“](media/quickstarts/share-project-command.png)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Tutorial: Erstellen und Ausführen einer Jupyter Notebook-Instanz für lineare Regression](tutorial-create-run-jupyter-notebook.md)
