---
title: Verwenden privater Python-Pakete
titleSuffix: Azure Machine Learning
description: Greifen Sie in Azure Machine Learning-Umgebungen sicher auf private Python-Pakete zu.
services: machine-learning
author: rastala
ms.author: roastala
ms.reviewer: laobri
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 07/10/2020
ms.openlocfilehash: 58bb08cad111e0744f7831783169901cd76caef4
ms.sourcegitcommit: ef69245ca06aa16775d4232b790b142b53a0c248
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/06/2020
ms.locfileid: "91772633"
---
# <a name="use-private-python-packages-with-azure-machine-learning"></a>Verwenden privater Python-Pakete mit Azure Machine Learning


In diesem Artikel erfahren Sie, wie Sie private Python-Pakete sicher in Azure Machine Learning verwenden. Anwendungsfälle für private Python-Pakete umfassen Folgendes:

 * Sie haben ein privates Paket entwickelt, das Sie nicht öffentlich freigeben möchten.
 * Sie möchten ein zusammengestelltes Repository von Paketen verwenden, die in einer Unternehmensfirewall gespeichert sind.

Die empfohlene Vorgehensweise hängt davon ab, ob es nur um wenige Pakete für einen einzelnen Azure Machine Learning-Arbeitsbereich oder ein gesamtes Repository von Paketen für alle Arbeitsbereiche innerhalb einer Organisation geht.

Die privaten Pakete werden über die [Environment](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment)-Klasse verwendet. In einer Umgebung deklarieren Sie, welche Python-Pakete einschließlich privater verwendet werden sollen. Informationen zur Umgebung in Azure Machine Learning im Allgemeinen finden Sie unter [Wiederverwenden von Umgebungen für Training und Bereitstellung mithilfe von Azure Machine Learning](how-to-use-environments.md). 

## <a name="prerequisites"></a>Voraussetzungen

 * über das [Azure Machine Learning SDK für Python](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py&preserve-view=true)
 * Ein [Azure Machine Learning-Arbeitsbereich](how-to-manage-workspace.md).

## <a name="use-small-number-of-packages-for-development-and-testing"></a>Verwenden Sie eine kleine Anzahl von Paketen für Entwicklungs- und Testzwecke.

Verwenden Sie für eine kleine Anzahl von privaten Paketen für einen einzelnen Arbeitsbereich die statische [`Environment.add_private_pip_wheel()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py&preserve-view=true#&preserve-view=trueadd-private-pip-wheel-workspace--file-path--exist-ok-false-)-Methode. Mit diesem Ansatz können Sie dem Arbeitsbereich schnell ein privates Paket hinzufügen und er eignet sich gut für Entwicklungs- und Testzwecke.

Verweisen Sie das Dateipfadargument auf eine lokale Wheeldatei, und führen Sie den Befehl ```add_private_pip_wheel``` aus. Der Befehl gibt eine URL zurück, die zum Nachverfolgen des Paketspeicherorts innerhalb Ihres Arbeitsbereichs dient. Erfassen Sie die Speicher-URL und übergeben Sie sie der `add_pip_package()`-Methode.

```python
whl_url = Environment.add_private_pip_wheel(workspace=ws,file_path = "my-custom.whl")
myenv = Environment(name="myenv")
conda_dep = CondaDependencies()
conda_dep.add_pip_package(whl_url)
myenv.python.conda_dependencies=conda_dep
```

Intern ersetzt der Azure Machine Learning-Dienst die URL durch eine sichere SAS-URL, sodass Ihre Wheeldatei privat und sicher bleibt.

## <a name="use-a-repository-of-packages-from-azure-devops-feed"></a>Verwenden eines Repositorys von Paketen aus einem Azure DevOps-Feed

Wenn Sie Python-Pakete für Ihre Machine Learning-Anwendung entwickeln, können Sie sie als Artefakte in einem Azure DevOps-Repository hosten und als Feed veröffentlichen. So können Sie den DevOps-Workflow zum Entwickeln von Paketen in Ihren Azure Machine Learning-Arbeitsbereich integrieren. Weitere Informationen zum Einrichten von Python-Feeds mithilfe von Azure DevOps finden Sie unter [Erste Schritte mit Python-Paketen in Azure- Artefakten](https://docs.microsoft.com/azure/devops/artifacts/quickstarts/python-packages?view=azure-devops&preserve-view=true).

Bei diesem Ansatz wird ein persönliches Zugriffstoken zum Authentifizieren für das Repository verwendet. Der gleiche Ansatz gilt für andere Repositorys mit tokenbasierter Authentifizierung, z. B. private GitHub-Repositorys. 

 1. [Erstellen Sie ein persönliches Zugriffstoken (Personal Access Token, PAT)](https://docs.microsoft.com/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate?view=azure-devops&preserve-view=true&tabs=preview-page#create-a-pat) für Ihre Azure DevOps-Instanz. Legen Sie den Gültigkeitsbereich des Tokens auf __Verpacken > Lesen__ fest. 

 2. Fügen Sie Azure DevOps-URL und PAT mithilfe der [Workspace.set_connection](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py&preserve-view=true#&preserve-view=trueset-connection-name--category--target--authtype--value-)-Methode als Arbeitsbereichseigenschaften hinzu.

     ```python
    from azureml.core import Workspace
    
    pat_token = input("Enter secret token")
    ws = Workspace.from_config()
    ws.set_connection(name="connection-1", 
        category = "PythonFeed",
        target = "https://<my-org>.pkgs.visualstudio.com", 
        authType = "PAT", 
        value = pat_token) 
     ```

 3. Erstellen Sie eine Azure Machine Learning-Umgebung, und fügen Sie Python-Pakete aus dem Feed hinzu.
    
    ```python
    from azureml.core import Environment
    from azureml.core.conda_dependencies import CondaDependencies
    
    env = Environment(name="my-env")
    cd = CondaDependencies()
    cd.add_pip_package("<my-package>")
    cd.set_pip_option("--extra-index-url https://<my-org>.pkgs.visualstudio.com/<my-project>/_packaging/<my-feed>/pypi/simple")
    env.python.conda_dependencies=cd
    ```

Die Umgebung ist jetzt für die Verwendung in Trainingsausführungen oder Webdienst-Endpunktbereitstellungen bereit. Beim Erstellen der Umgebung verwendet der Azure Machine Learning-Dienst das PAT zur Authentifizierung bei dem Feed mit der entsprechenden Basis-URL.

## <a name="use-a-repository-of-packages-from-private-storage"></a>Verwenden eines Repositorys von Paketen aus einem privaten Speicher

Sie können Pakete aus einem Azure-Speicherkonto innerhalb der Firewall Ihrer Organisation nutzen. Das Speicherkonto kann eine zusammengestellte Gruppe von Paketen oder eine interne Spiegelung öffentlich verfügbarer Pakete enthalten.

Informationen zum Einrichten eines solchen privaten Speichers finden Sie unter [Sichern eines Azure Machine Learning-Arbeitsbereichs und zugehöriger Ressourcen](how-to-secure-workspace-vnet.md#secure-azure-storage-accounts-with-service-endpoints). Sie müssen außerdem die [Azure Container Registry (ACR) hinter dem VNet platzieren](how-to-secure-workspace-vnet.md#enable-azure-container-registry-acr).

> [!IMPORTANT]
> Sie müssen diesen Schritt ausführen, um Modelle mithilfe des privaten Paketrepositorys trainieren oder bereitstellen zu können.

Nach Abschluss dieser Konfigurationen können Sie in der Azure Machine Learning-Umgebungsdefinition mit der vollständigen URL der Pakete in Azure Blob Storage auf sie verweisen.

## <a name="next-steps"></a>Nächste Schritte

 * Erfahren Sie mehr über [Unternehmenssicherheit in Azure Machine Learning](concept-enterprise-security.md).
