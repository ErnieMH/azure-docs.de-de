---
title: Technische Konzepte für Azure-Containerangebote – kommerzieller Microsoft-Marketplace
description: Technische Ressourcen und Anleitungen, die Ihnen bei der Konfiguration eines Containerangebots im Azure Marketplace helfen.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
author: keferna
ms.author: keferna
ms.date: 04/09/2020
ms.openlocfilehash: 3599052e49b91747580bf506f06ebfb8d2e34423
ms.sourcegitcommit: 99955130348f9d2db7d4fb5032fad89dad3185e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2020
ms.locfileid: "93346807"
---
# <a name="create-an-azure-container-offer"></a>Erstellen eines Azure-Containerangebots

Dieser Artikel enthält technische Ressourcen und Empfehlungen, die Ihnen helfen, ein Containerangebot im Azure Marketplace zu erstellen.

## <a name="before-you-begin"></a>Voraussetzungen

Schnellstarts, Tutorials und Beispiele finden Sie in der [Dokumentation zu Azure Container Instances](../../container-instances/index.yml).

## <a name="fundamental-technical-knowledge"></a>Grundlegende technische Kenntnisse

Das Entwerfen, Erstellen und Testen dieser Ressourcen dauert lange, und es sind technische Kenntnisse in Bezug auf die Azure-Plattform und die Technologien erforderlich, die verwendet werden, um das Angebot zu erstellen.

Zusätzlich zur Vertrautheit mit Ihrer Lösungsdomäne sollte sich Ihr Engineeringteam mit den folgenden Microsoft-Technologien auskennen:

- Grundkenntnisse in Bezug auf [Azure-Dienste](https://azure.microsoft.com/services/)
- [Entwerfen und Erstellen der Architektur für Azure-Anwendungen](https://azure.microsoft.com/solutions/architecture/)
- Erweiterte Grundkenntnisse in Bezug auf [Azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines/), [Azure Storage](https://azure.microsoft.com/services/?filter=storage) und [Azure-Netzwerk](https://azure.microsoft.com/services/?filter=networking)
- Erweiterte Grundkenntnisse in Bezug auf [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/)
- Fundierte Kenntnisse zu [JSON](https://www.json.org/).

## <a name="suggested-tools"></a>Vorgeschlagene Tools

Wählen Sie eine oder beide der folgenden Skriptumgebungen als Unterstützung bei der Verwaltung Ihres Containerimages aus:

- [Azure PowerShell](/powershell/azure/)
- [Azure-Befehlszeilenschnittstelle](/cli/azure/).

Sie sollten Ihrer Entwicklungsumgebung diese Tools hinzufügen:

- [Azure Storage-Explorer](../../vs-azure-tools-storage-manage-with-storage-explorer.md?tabs=windows)
- [Visual Studio Code](https://code.visualstudio.com/)
  - Erweiterung: [Azure Resource Manager Tools](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)
  - Erweiterung: [Beautify](https://marketplace.visualstudio.com/items?itemName=HookyQR.beautify)
  - Erweiterung: [Prettify JSON](https://marketplace.visualstudio.com/items?itemName=mohsen1.prettify-json).

Machen Sie sich mit den verfügbaren Tools auf der Seite [Azure-Entwicklertools](https://azure.microsoft.com/) vertraut. Wenn Sie Visual Studio verwenden, machen Sie sich mit den im [Visual Studio Marketplace](https://marketplace.visualstudio.com/) verfügbaren Tools vertraut.

## <a name="create-the-container-image"></a>Erstellen des Containerimages

Weitere Informationen finden Sie in folgenden Tutorials:

- [Tutorial: Erstellen eines Containerimages für die Bereitstellung in Azure Container Instances](../../container-instances/container-instances-tutorial-prepare-app.md)
- [Tutorial: Erstellen und Bereitstellen von Containerimages in der Cloud mit Azure Container Registry Tasks](../../container-registry/container-registry-tutorial-quick-task.md).

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen Sie Ihr Containerangebot](create-azure-container-offer.md).