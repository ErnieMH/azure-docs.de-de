---
title: Azure Active Directory Graph-API
description: Eine Übersicht und eine Schnellstartanleitung für die Azure AD Graph-API, die den programmgesteuerten Zugriff auf Azure AD über REST-API-Endpunkte ermöglicht.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 11/26/2019
ms.author: ryanwi
ms.reviewer: dkershaw, sureshja
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: da99468b1582c4acab192ad3b96761172aa69580
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89068659"
---
# <a name="azure-active-directory-graph-api"></a>Azure Active Directory Graph-API

> [!IMPORTANT]
> Es wird dringend empfohlen, auf Azure Active Directory (Azure AD)-Ressourcen nicht mithilfe der Azure AD Graph-API, sondern mithilfe von [Microsoft Graph](https://developer.microsoft.com/graph) zuzugreifen. Unsere Entwicklungstätigkeiten konzentrieren sich nun auf Microsoft Graph, während für die Azure AD Graph-API keine weiteren Verbesserungen geplant sind. Es gibt nur eine sehr begrenzte Anzahl von Szenarien, in denen die Verwendung der Azure AD Graph-API möglicherweise weiterhin geeignet ist. Weitere Informationen finden Sie im Blogbeitrag [Microsoft Graph oder Azure AD Graph](https://developer.microsoft.com/office/blogs/microsoft-graph-or-azure-ad-graph/) und unter [Migrieren von Azure AD Graph-Apps zu Microsoft Graph](/graph/migrate-azure-ad-graph-planning-checklist).

Dieser Artikel gilt für die Azure AD Graph-API. Ähnliche Informationen bezogen auf die Microsoft Graph-API finden Sie unter [Verwenden der Microsoft Graph-API](/graph/use-the-api).

Die Azure Active Directory Graph-API ermöglicht programmgesteuerten Zugriff auf Azure AD über REST-API-Endpunkte. Anwendungen können die Azure AD Graph-API verwenden, um CRUD-Vorgänge (Erstellen, Lesen, Aktualisieren und Löschen) für Verzeichnisdaten und Objekte auszuführen. Die Azure AD Graph-API unterstützt beispielsweise die folgenden allgemeinen Vorgänge für ein Benutzerobjekt:

* Erstellen eines neuen Benutzers in einem Verzeichnis
* Abrufen der detaillierten Eigenschaften eines Benutzers, z. B. seiner Gruppen
* Aktualisieren der Eigenschaften eines Benutzers, z. B. seines Standorts und seiner Telefonnummer oder das Ändern seines Kennworts
* Überprüfen der Gruppenmitgliedschaft eines Benutzers für den rollenbasierten Zugriff
* Deaktivieren oder vollständiges Löschen des Kontos eines Benutzers

Darüber hinaus können Sie ähnliche Vorgänge auch für andere Objekte wie Gruppen oder Anwendungen ausführen. Um die Azure AD Graph-API in einem Verzeichnis aufzurufen, muss Ihre Anwendung in Azure AD registriert werden. Der Anwendung muss zudem Zugriff auf die Azure AD Graph-API gewährt werden. Dieser Zugriff wird in der Regel mit einem Zustimmungsvorgang durch den Benutzer oder Administrator erreicht.

Informationen zu den ersten Schritten mit der Azure Active Directory Graph-API finden Sie in der [Schnellstartanleitung für die Azure AD Graph-API](./microsoft-graph-intro.md) und in der [interaktiven Azure AD Graph-API-Referenz](/previous-versions/azure/ad/graph/api/api-catalog).

## <a name="features"></a>Features

Die Azure AD Graph-API bietet die folgenden Features:

* **REST-API-Endpunkte:** Die Azure AD Graph-API ist ein RESTful-Dienst, der aus Endpunkten besteht, auf die mithilfe von HTTP-Standardanforderungen zugegriffen wird. Die Azure AD Graph-API unterstützt die Inhaltstypen XML oder JavaScript Object Notation (JSON) für Anforderungen und Antworten. Weitere Informationen finden Sie unter [REST-API-Referenz zu Azure AD Graph](/previous-versions/azure/ad/graph/api/api-catalog).
* **Authentifizierung mit Azure AD:** Jede Anforderung an die Azure AD Graph-API muss durch Anfügen eines JSON-Webtokens (JWT) im Autorisierungsheader der Anforderung authentifiziert werden. Dieses Token wird durch eine Anforderung an den Tokenendpunkt von Azure AD und die Bereitstellung gültiger Anmeldeinformationen abgerufen. Sie können den OAuth 2.0-Datenfluss für Clientanmeldeinformationen oder den Datenfluss für die Autorisierungscodegewährung verwenden, um ein Token für den Graph-Aufruf abzurufen. Weitere Informationen finden Sie unter [OAuth 2.0 in Azure AD](/previous-versions/azure/dn645545(v=azure.100)).
* **Rollenbasierte Autorisierung (RBAC):** Um RBAC in der Azure AD Graph-API ausführen zu können, werden Sicherheitsgruppen verwendet. Wenn Sie beispielsweise ermitteln möchten, ob ein Benutzer über Zugriff auf eine bestimmte Ressource verfügt, kann die Anwendung den unter [Überprüfen der Gruppenmitgliedschaft (transitiv)](/previous-versions/azure/ad/graph/api/functions-and-actions#checkMemberGroups) beschriebenen Vorgang aufrufen, der TRUE oder FALSE zurückgibt.
* **Differenzielle Abfrage:** Mit einer differenziellen Abfrage können Sie ohne mehrfaches Abfragen der Azure AD Graph-API Änderungen an einem Verzeichnis zwischen zwei Zeiträumen nachverfolgen. Dieser Anforderungstyp gibt nur die Änderungen zurück, die zwischen der vorherigen differenziellen Abfrageanforderung und der aktuellen Anforderung erfolgt sind. Weitere Informationen finden Sie unter [Differenzielle Abfrage der Azure AD Graph-API](/previous-versions/azure/ad/graph/howto/azure-ad-graph-api-differential-query).
* **Verzeichniserweiterungen:** Sie können Verzeichnisobjekten benutzerdefinierte Eigenschaften ohne externen Datenspeicher hinzufügen. Wenn Ihre Anwendung beispielsweise eine Skype-ID-Eigenschaft für jeden Benutzer erfordert, können Sie die neue Eigenschaft im Verzeichnis registrieren. Sie steht dann für jedes Benutzerobjekt zur Verfügung. Weitere Informationen finden Sie unter [Verzeichnisschemaerweiterungen der Azure AD Graph-API](/previous-versions/azure/ad/graph/howto/azure-ad-graph-api-directory-schema-extensions).
* **Gesichert durch Berechtigungsbereiche:** Die Azure AD Graph-API macht Berechtigungsbereiche verfügbar, die einen sicheren Zugriff auf Azure AD-Daten mithilfe von OAuth 2.0 ermöglichen. Eine Vielzahl von Client-App-Typen wird unterstützt, einschließlich:
  
  * Benutzeroberflächen, die über die Autorisierung des angemeldeten Benutzers delegierten Zugriff auf Daten erhalten (delegiert)
  * Dienst-/Daemon-Anwendungen, die im Hintergrund ohne einen angemeldeten Benutzer ausgeführt werden und eine anwendungsspezifische rollenbasierte Zugriffssteuerung verwenden
    
    Sowohl delegierte als auch auf Anwendungsberechtigungen stellen eine durch die Azure AD Graph-API verfügbar gemachte Berechtigung dar und können von Clientanwendungen über Features für Anwendungsregistrierungsberechtigungen im [Azure-Portal](https://portal.azure.com) angefordert werden. [Azure AD Graph-API-Berechtigungsbereiche](/previous-versions/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes) enthält Informationen dazu, was zur Verwendung in Ihrer Clientanwendung verfügbar ist.

## <a name="scenarios"></a>Szenarien

Die Azure AD Graph-API ermöglicht eine Vielzahl von Anwendungsszenarios. Die gängigsten Szenarios sind die folgenden:

* **Branchenanwendung (Einzelinstanz):** In diesem Szenario arbeitet ein Unternehmensentwickler für eine Organisation, die über ein Office 365-Abonnement verfügt. Der Entwickler erstellt eine Webanwendung, die mit Azure AD interagiert, um Aufgaben wie das Zuweisen einer Lizenz zu einem Benutzer auszuführen. Für diese Aufgabe ist der Zugriff auf die Azure AD Graph-API erforderlich. Der Entwickler registriert daher die Einzelinstanzanwendung in Azure AD und konfiguriert Lese- und Schreibberechtigungen für die Azure AD Graph-API. Anschließend wird die Anwendung für die Verwendung eigener Anmeldeinformationen oder der Anmeldeinformationen des aktuell angemeldeten Benutzers konfiguriert, um ein Token zum Aufrufen der Azure AD Graph-API abzurufen.
* **Software-as-a-Service-Anwendung (mehrinstanzenfähig):** In diesem Szenario entwickelt ein unabhängiger Softwarehersteller (Independent Software Vendor, ISV) eine gehostete mehrinstanzenfähige Webanwendung, von der Benutzerverwaltungsfunktionen für andere Organisationen bereitgestellt werden, die Azure AD verwenden. Da für diese Features Zugriff auf Verzeichnisobjekte erforderlich ist, muss die Anwendung die Azure AD Graph-API aufrufen. Der Entwickler registriert die Anwendung in Azure AD und konfiguriert sie so, dass Lese- und Schreibberechtigungen für die Azure AD Graph-API erforderlich sind. Anschließend aktiviert er den externen Zugriff, damit andere Organisationen der Verwendung der Anwendung in ihrem Verzeichnis zustimmen können. Wenn sich ein Benutzer in einer anderen Organisation erstmals bei der Anwendung authentifiziert, wird ein Zustimmungsdialogfeld mit den Berechtigungen angezeigt, die die Anwendung anfordert. Durch die Zustimmung erhält die Anwendung dann die angeforderten Berechtigungen für die Azure AD Graph-API im Verzeichnis des Benutzers. Weitere Informationen zum Consent Framework finden Sie unter [Übersicht über das Consent Framework](consent-framework.md).

## <a name="next-steps"></a>Nächste Schritte

Um mit der Verwendung der Azure Active Directory Graph-API zu beginnen, lesen Sie die folgenden Themen:

* [Schnellstartanleitung für die Azure AD Graph-API](./microsoft-graph-intro.md)
* [Azure AD Graph-REST-Dokumentation](/previous-versions/azure/ad/graph/api/api-catalog)
