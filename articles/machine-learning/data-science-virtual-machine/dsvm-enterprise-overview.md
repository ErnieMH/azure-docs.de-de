---
title: Teamanalyse und KI-Umgebung
titleSuffix: Azure Data Science Virtual Machine
description: Muster für die Bereitstellung von Data Science VM als Umgebung für Unternehmensteams.
keywords: Deep Learning, KI, Data Science-Tools, virtueller Data Science-Computer, räumliche Analysen, Data Science-Teamprozess
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: vijetajo
ms.author: vijetaj
ms.topic: overview
ms.date: 05/08/2018
ms.openlocfilehash: 538802cc2129a8e8f379c8b569bd1f8696097dab
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2020
ms.locfileid: "93314181"
---
# <a name="data-science-virtual-machine-based-team-analytics-and-ai-environment"></a>Auf Data Science Virtual Machine basierende Teamanalyse und KI-Umgebung 
[Data Science Virtual Machine](overview.md) (DSVM) bietet eine umfangreiche Umgebung auf der Azure-Plattform mit vorgefertigter Software für künstliche Intelligenz (KI) und Datenanalyse.

Bisher wurde DSVM als einzelne Desktop-Analyseumgebung verwendet. Einzelne Data Scientists steigern ihre Produktivität mit dieser gemeinsam genutzten, vorgefertigten Analyseumgebung. Wenn große Analyseteams Umgebungen für ihre Data Scientists und KI-Entwickler planen, ist eines der wiederkehrenden Themen eine gemeinsam genutzte Analyseinfrastruktur für Entwicklung und Experimente. Diese Infrastruktur wird in Übereinstimmung mit den IT-Richtlinien des Unternehmens verwaltet, die auch die Zusammenarbeit und Konsistenz zwischen den Data Science- und Analyseteams erleichtern.

Eine gemeinsam genutzte Infrastruktur ermöglicht eine bessere IT-Nutzung der Analyseumgebung. Einige Organisationen bezeichnen die teambasierte Data Science- bzw. Analyseinfrastruktur als *Analyse-Sandbox*. Diese versetzt Data Scientists in die Lage, auf verschiedene Datenressourcen zuzugreifen, um Daten schnell zu verstehen. Diese Sandbox-Umgebung unterstützt Data Scientists außerdem dabei, Experimente durchzuführen, Hypothesen zu überprüfen und Vorhersagemodelle zu erstellen, ohne die Produktionsumgebung zu beeinträchtigen.

Da DSVM auf der Ebene der Azure-Infrastruktur arbeitet, können IT-Administratoren die DSVM-Instanz so konfigurieren, dass sie die IT-Richtlinien des Unternehmens erfüllt. Bei der Implementierung verschiedener Architekturen für die gemeinsame Nutzung bietet DSVM umfassende Flexibilität sowie gleichzeitig kontrollierten Zugriff auf Unternehmensdaten.

In diesem Abschnitt werden einige Muster und Richtlinien behandelt, die Sie zum Bereitstellen der DSVM-Instanz als teambasierte Data Science-Infrastruktur verwenden können. Weil die Bausteine für diese Muster von Azure-Infrastructure-as-a-Service (IaaS) stammen, gelten sie für alle Azure-VMs. In dieser Artikelreihe liegt der Schwerpunkt auf dem Anwenden dieser standardmäßigen Azure-Infrastrukturfunktionen auf die DSVM.

Zu den Kernbausteinen einer Teamanalyseumgebung für Unternehmen gehören:

* [Ein automatisch skalierter Pool von DSVMs](dsvm-pools.md)
* [Gemeinsame Identität und Zugriff auf einen Arbeitsbereich über jede DSVM-Instanz im Pool](dsvm-common-identity.md)
* [Sicherer Zugriff auf Datenquellen](dsvm-secure-access-keys.md)


In dieser Artikelreihe werden Anleitungen und Hinweise zu jedem der vorherigen Themen bereitgestellt. Sie deckt nicht alle Überlegungen und Anforderungen für ein Bereitstellen von DSVMs in großen Unternehmenskonfigurationen ab. Es folgen einige weitere Azure-Ressourcen, die Sie bei der Implementierung von DSVM-Instanzen in Ihrem Unternehmen verwenden können:

* [Netzwerksicherheit](../../security/fundamentals/network-overview.md)
* [Überwachung](../../azure-monitor/insights/monitor-vm-azure.md) und [Verwaltung](../../virtual-machines/maintenance-and-updates.md?bc=%252fazure%252fvirtual-machines%252fwindows%252fbreadcrumb%252ftoc.json%252c%252fazure%252fvirtual-machines%252fwindows%252fbreadcrumb%252ftoc.json&toc=%252fazure%252fvirtual-machines%252fwindows%252ftoc.json%253ftoc%253d%252fazure%252fvirtual-machines%252fwindows%252ftoc.json)
* [Protokollierung und Überwachung](../../security/fundamentals/log-audit.md)
* [Rollenbasierte Zugriffssteuerung von Azure (Azure RBAC)](../../role-based-access-control/overview.md)
* [Richtlinieneinstellung und -erzwingung](../../governance/policy/overview.md)
* [Antischadsoftware](../../security/fundamentals/antimalware.md)
* [Verschlüsselung](../../virtual-machines/windows/disk-encryption-overview.md)
* [Datenermittlung und Governance](../../data-catalog/index.yml)

Das [Azure Architecture Center](/azure/architecture/) stellt schließlich eine detaillierte End-to-End-Architektur sowie Modelle für das Erstellen und Verwalten Ihrer cloudbasierten Analyseinfrastruktur bereit.