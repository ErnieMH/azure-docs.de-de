---
title: Autorisieren des Zugriffs auf Azure Event Hubs
description: Dieser Artikel bietet Informationen zu verschiedenen Optionen zum Autorisieren des Zugriffs auf Azure Event Hubs-Ressourcen.
ms.topic: conceptual
ms.date: 06/23/2020
ms.author: spelluru
ms.openlocfilehash: 18b8bd80eaec316fbaefadad0dd7a19418bfa838
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "85323174"
---
# <a name="authorize-access-to-azure-event-hubs"></a>Autorisieren des Zugriffs auf Azure Event Hubs
Jedes Mal, wenn Sie Ereignisse/Daten von einem Event Hub veröffentlichen oder nutzen, versucht Ihr Client, auf Event Hubs-Ressourcen zuzugreifen. Jede Anforderung an eine sichere Ressource muss autorisiert sein, damit der Dienst sicherstellen kann, dass der Client über die erforderlichen Berechtigungen zum Veröffentlichen der Daten bzw. für den Zugriff darauf verfügt. 

Azure Event Hubs bietet die folgenden Optionen für die Autorisierung des Zugriffs auf sichere Ressourcen:

- Azure Active Directory
- Shared Access Signature (SAS)

> [!NOTE]
> Dieser Artikel gilt sowohl für Event Hubs als auch für [Apache Kafka](event-hubs-for-kafka-ecosystem-overview.md)-Szenarien. 

## <a name="azure-active-directory"></a>Azure Active Directory
Die Azure Active Directory-Integration (Azure AD) für Event Hubs-Ressourcen bietet rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) für eine präzise Steuerung des Zugriffs eines Clients auf Ressourcen. Sie können die rollenbasierte Zugriffssteuerung (RBAC) zum Gewähren von Berechtigungen für einen Sicherheitsprinzipal verwenden, bei dem es sich um einen Benutzer, eine Gruppe oder einen Anwendungsdienstprinzipal handeln kann. Der Sicherheitsprinzipal wird von Azure AD authentifiziert, um ein OAuth 2.0-Token zurückzugeben. Das Token kann zum Autorisieren einer Anforderung für den Zugriff auf eine Event Hubs-Ressource verwendet werden.

Weitere Informationen zur Authentifizierung mit Azure AD finden Sie in den folgenden Artikeln:

- [Authentifizieren von Anforderungen an Azure Event Hubs mithilfe von Azure Active Directory](authenticate-application.md)
- [Autorisieren des Zugriffs auf Event Hubs-Ressourcen mit Azure Active Directory](authorize-access-azure-active-directory.md).

## <a name="shared-access-signatures"></a>Shared Access Signatures 
Shared Access Signatures (SAS) für Event Hubs bieten begrenzten delegierten Zugriff auf Event Hubs-Ressourcen. Einschränkungen des Zeitintervalls, für das die Signatur gültig ist, oder von Berechtigungen, die sie gewährt, bieten Flexibilität beim Verwalten von Ressourcen. Weitere Informationen finden Sie unter [Authentifizieren mit Shared Access Signatures (SAS)](authenticate-shared-access-signature.md). 

Das Autorisieren von Benutzern oder Anwendungen mithilfe eines von Azure AD zurückgegebenen OAuth 2.0-Tokens bietet mehr Sicherheit und Benutzerfreundlichkeit als die Autorisierung mit SAS (Shared Access Signature). Mit Azure AD müssen die Zugriffstoken nicht mehr mit Ihrem Code gespeichert werden und so potenzielle Sicherheitsrisiken eingegangen werden. Sie können zwar weiterhin Shared Access Signatures (SAS) für einen fein aufgelösten Zugriff auf Event Hubs-Ressourcen verwenden, doch Azure AD bietet ähnliche Funktionen ohne die Notwendigkeit, SAS-Token verwalten oder sich um das Widerrufen einer gefährdeten SAS kümmern zu müssen. 

Standardmäßig sind alle Event Hubs-Ressourcen gesichert und stehen nur dem Kontobesitzer zur Verfügung. Obwohl Sie eine der oben beschriebenen Autorisierungsstrategien verwenden können, um Clients den Zugriff auf Event Hub-Ressourcen zu gewähren. Microsoft empfiehlt, falls möglich, die Verwendung von Azure AD für maximale Sicherheit und Benutzerfreundlichkeit.

Weitere Informationen zur Autorisierung mit SAS finden Sie unter [Autorisieren des Zugriffs auf Event Hubs-Ressourcen mithilfe von Shared Access Signatures](authorize-access-shared-access-signature.md).

## <a name="next-steps"></a>Nächste Schritte
- Sehen Sie sich [RBAC-Beispiele](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/Rbac) an, veröffentlicht in unserem GitHub-Repository. 
- Weitere Informationen finden Sie in folgenden Artikeln:
    - [Authentifizieren von Anforderungen an Event Hubs über eine Anwendung mithilfe von Azure Active Directory](authenticate-application.md)
    - [Authentifizieren einer verwalteten Identität mit Azure Active Directory für den Zugriff auf Event Hubs-Ressourcen](authenticate-managed-identity.md)
    - [Authentifizieren von Anforderungen an Azure Event Hubs mithilfe von Shared Access Signature](authenticate-shared-access-signature.md)
    - [Autorisieren des Zugriffs auf Event Hubs-Ressourcen mit Azure Active Directory](authorize-access-azure-active-directory.md)
    - [Autorisieren des Zugriffs auf Event Hubs-Ressourcen mit Shared Access Signature](authorize-access-shared-access-signature.md)

