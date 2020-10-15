---
title: Benutzerprofil und ID für die Nutzung von Azure Notebooks Preview
description: Es wird beschrieben, wie Sie Ihr Benutzerprofil und die Benutzer-ID mit Azure Notebooks erstellen und verwalten. Dies fließt in die URL von freigegebenen Notebooks ein.
ms.topic: conceptual
ms.date: 02/25/2019
ms.openlocfilehash: 9a1ff7f92faec21f537f068f0a33473700ddfed8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "85831351"
---
# <a name="your-profile-and-user-id-for-azure-notebooks-preview"></a>Ihr Profil und Ihre Benutzer-ID für Azure Notebooks Preview

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

Innerhalb des leistungsstarken, auf Zusammenarbeit ausgelegten Bereichs von Azure Notebooks stellt Ihr Benutzerprofil Ihr öffentliches Bild für andere dar:

[![Eine Azure Notebooks-Profilseite](media/accounts/profile-page.png)](media/accounts/profile-page.png#lightbox)

Ihre Benutzer-ID ist Teil der URLs, die Sie zum Teilen von Projekten und Notebooks verwenden. In der folgenden Liste sind die verschiedenen URL-Muster beschrieben:

- `https://notebooks.azure.com/<user_id>`: Ihre Profilseite.
- `https://notebooks.azure.com/<user_id>/projects`: Ihre Projekte. Sie sehen alle Projekte; andere Benutzer sehen nur Ihre öffentlichen Projekte.
- `https://notebooks.azure.com/<user_id>/projects/<project_id>`: Projektdateien.
- `https://notebooks.azure.com/<user_id>/projects/<project_id>/clones`: Klone eines bestimmten Projekts.
- `https://notebooks.azure.com/<user_id>/projects/<project_id>/html/<notebook>.ipynb`: Die HTML-Vorschau eines bestimmten Notebooks oder einer bestimmten Datei.

## <a name="your-user-id"></a>Ihre Benutzer-ID

Wenn Sie sich zum ersten Mal bei Azure Notebooks anmelden, wird Ihrem Konto automatisch eine temporäre Benutzer-ID zugeordnet, z. B. „anon-idr3ca“. Solange Sie über eine Benutzer-ID verfügen, die mit „anon-“ beginnt, fordert Azure Notebooks Sie bei jeder Anmeldung auf, sie zu ändern:

![Aufforderung zum Erstellen einer Benutzer-ID bei der Anmeldung bei Azure Notebooks](media/accounts/create-user-id.png)

Ferner wird neben dem temporären Benutzernamen ein Befehl **Configure User ID** (Benutzer-ID konfigurieren) angezeigt:

![Befehl „Configure User ID“ (Benutzer-ID konfigurieren), der beim Verwenden einer temporären ID angezeigt wird](media/accounts/configure-user-id-command.png)

Darüber hinaus können Sie Ihre Benutzer-ID jederzeit auf Ihrer Profilseite ändern.

Eine Benutzer-ID muss aus vier bis sechzehn Buchstaben, Ziffern und Bindestrichen bestehen. Es sind keine anderen Zeichen zulässig, und die Benutzer-ID darf nicht mit einem Bindestrich beginnen oder enden oder mehrere Bindestriche hintereinander enthalten. Da Benutzer-IDs für alle Azure Notebooks-Konten eindeutig sind, könnte die Meldung „Benutzer-ID wird bereits verwendet“ angezeigt werden. (Die Nachricht wird auch angezeigt, wenn Sie versuchen, eine Microsoft-Marke als Benutzer-ID zu verwenden.) Wählen Sie in diesen Fällen eine andere Benutzer-ID aus.

> [!Important]
> Eine Änderung Ihrer ID macht alle URLs, die Sie mit Ihrer vorherigen ID geteilt haben, ungültig. Um die Gültigkeit der Links wiederherzustellen, können Sie Ihre ID wieder auf Ihre vorherige ID umstellen. Allerdings kann in der Zwischenzeit ein anderer Benutzer Anspruch auf eine nicht verwendete ID erheben.

## <a name="your-profile"></a>Ihr Profil

Ihr Profil setzt sich aus öffentlich sichtbaren Informationen unter der URL `https://notebooks.azure.com/<user_id>` zusammen. Auf Ihrer Profilseite werden außerdem Ihre zuletzt verwendeten sowie alle mit Stern gekennzeichneten Projekte angezeigt.

Zum Bearbeiten Ihres Profils verwenden Sie den Befehl **Edit Profile Information** (Profilinformationen bearbeiten) auf Ihrer Profilseite. Dies sind die Abschnitte Ihres Profils:

| `Section` | Contents |
| --- | --- |
| Profilfoto | Ein Bild, das auf Ihrer Profilseite angezeigt wird. |
| Azure-Kontoinformationen | Ihr Anzeigename, Ihre Benutzer-ID und Ihr öffentliches E-Mail-Konto. Das E-Mail-Konto stellt ein Mittel für andere Benutzer dar, mit Ihnen in Kontakt zu treten, und kann sich von dem [Konto](azure-notebooks-user-account.md) unterscheiden, das Sie für die Anmeldung bei Azure Notebooks selbst verwenden. |
| Profilinformationen | Ihr Standort, Ihr Unternehmen, Ihre Position, Ihre Website und eine kurze Beschreibung Ihrer Person. |
| Soziale Profile | Ihre GItHub-, Twitter- und Facebook-IDs, wenn Sie diese teilen möchten. |
| Datenschutzeinstellungen | Bietet zwei Befehle:<ul><li>**Export My Profile** (Mein Profil exportieren): erstellt eine *ZIP*-Datei mit allen Informationen, die Azure Notebooks in Ihrem Profil speichert, einschließlich Ihres Fotos, Ihrer Profilinformationen und der Sicherheitsprotokolle, und lädt sie herunter.</li><li>**Delete My Account** (Mein Konto löschen): löscht endgültig alle Ihre in Azure Notebooks gespeicherten persönlichen Informationen.</li></ul> |
| Enable Site Features (Websitefunktionen aktivieren) | Ermöglicht es Ihnen, Verhaltensaspekte von Azure Notebooks zu steuern:<ul><li>**Unified Frontend for Notebooks** (Einheitliches Front-End für Notebooks): ermöglicht einen schnelleren Start von Notebooks und größere Beständigkeit.</li><li>**Run in JupyterLab by default** (Standardmäßig in JupyterLab ausführen): Azure Notebooks stellt als Standard eine einfache Benutzeroberfläche zur Verfügung, die für die meisten Benutzer geeignet ist. JupyterLab stellt erfahrenen Benutzern einer umfassendere, aber kompliziertere Oberfläche zur Verfügung.</li><li>**VNext Website** (VNext-Website): aktiviert das in dieser Dokumentation verwendete modernisierte Weblayout.</li></ul> |

## <a name="next-steps"></a>Nächste Schritte  

> [!div class="nextstepaction"]
> [Tutorial: Erstellen und Ausführen einer Jupyter Notebook-Datei mit Python](tutorial-create-run-jupyter-notebook.md)
