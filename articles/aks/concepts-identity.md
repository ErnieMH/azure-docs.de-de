---
title: Konzepte – Zugriff und Identität in Azure Kubernetes Service (AKS)
description: Erfahren Sie mehr über Zugriff und Identität in Azure Kubernetes Service (AKS), einschließlich Integration von Azure Active Directory, rollenbasierte Kubernetes-Zugriffssteuerung (RBAC) sowie Rollen und Bindungen.
services: container-service
ms.topic: conceptual
ms.date: 07/07/2020
author: palma21
ms.author: jpalma
ms.openlocfilehash: edb6a8e04537a74b7ea7d4c9bd9bd27fdc39e402
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88007079"
---
# <a name="access-and-identity-options-for-azure-kubernetes-service-aks"></a>Zugriffs- und Identitätsoptionen für Azure Kubernetes Service (AKS)

Es gibt verschiedene Möglichkeiten, Kubernetes-Cluster zu authentifizieren, den Zugriff/die Autorisierung zu steuern und sie zu schützen. Mit der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) für Kubernetes können Sie den Zugriff von Benutzern, Gruppen und Dienstkonten auf die Ressourcen beschränken, die benötigt werden. Mit Azure Kubernetes Service (AKS) können Sie die Sicherheits- und Berechtigungsstruktur weiter verbessern, indem Sie Azure Active Directory und Azure RBAC verwenden. Mit diesen Ansätzen können Sie den Zugriff auf den Cluster schützen, und es werden nur die mindestens erforderlichen Berechtigungen für Entwickler und Operatoren bereitgestellt.

In diesem Artikel werden die wichtigsten Konzepte für die Authentifizierung und das Zuweisen von Berechtigungen in AKS vorgestellt:

- [Rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) für Kubernetes](#kubernetes-role-based-access-control-rbac)
  - [Rollen und Clusterrollen](#roles-and-clusterroles)
  - [Rollenbindungen und Clusterrollenbindungen](#rolebindings-and-clusterrolebindings) 
  - [Kubernetes-Dienstkonten](#kubernetes-service-accounts)
- [Azure Active Directory-Integration](#azure-active-directory-integration)
- [Azure RBAC](#azure-role-based-access-control-azure-rbac)
  - [Azure RBAC zum Autorisieren des Zugriffs auf die AKS-Ressource](#azure-rbac-to-authorize-access-to-the-aks-resource)
  - [Azure RBAC für die Kubernetes-Autorisierung (Vorschau)](#azure-rbac-for-kubernetes-authorization-preview)


## <a name="kubernetes-role-based-access-control-rbac"></a>Rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) für Kubernetes

Für eine präzise Filterung der Aktionen, die Benutzer ausführen können, verwendet Kubernetes die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC). Mit diesem Steuerungsmechanismus können Sie Benutzern oder Benutzergruppen die Berechtigung für bestimmte Aktionen (z. B. Ressourcen erstellen bzw. ändern oder Protokolle zur Workload ausgeführter Anwendungen anzeigen) zuweisen. Diese Berechtigungen können auf einen einzelnen Namespace begrenzt oder für den gesamten AKS-Cluster erteilt werden. Mit der rollenbasierten Zugriffssteuerung (RBAC) von Kubernetes können Sie *Rollen* erstellen, um Berechtigungen zu definieren, und anschließend diese Rollen mit *Rollenbindungen* Benutzern zuweisen.

Weitere Informationen finden Sie unter [Verwenden von RBAC-Autorisierung][kubernetes-rbac].


### <a name="roles-and-clusterroles"></a>Rollen und Clusterrollen

Bevor Sie Benutzern mit der rollenbasierten Zugriffssteuerung von Kubernetes Berechtigungen zuweisen, müssen Sie diese Berechtigungen als *Rolle* definieren. Kubernetes-Rollen *gewähren* Berechtigungen. In diesem Konzept ist eine Berechtigung des Typs *Verweigern* nicht vorgesehen.

Rollen werden zum Gewähren von Berechtigungen innerhalb eines Namespace verwendet. Wenn Sie Berechtigungen für den gesamten Cluster oder für Clusterressourcen außerhalb eines bestimmten Namespace erteilen müssen, können Sie stattdessen *Clusterrollen* verwenden.

Auch mit einer Clusterrolle werden Berechtigungen für Ressourcen erteilt. Allerdings kann sie auf Ressourcen im gesamten Cluster angewendet werden und ist nicht auf einen bestimmten Namespace beschränkt.

### <a name="rolebindings-and-clusterrolebindings"></a>Rollenbindungen und Clusterrollenbindungen

Sobald Rollen definiert sind, um Berechtigungen für Ressourcen zu gewähren, weisen Sie diese Berechtigungen der rollenbasierten Zugriffssteuerung von Kubernetes mit einer *Rollenbindung* zu. Wenn Ihr AKS-Cluster [in Azure Active Directory integriert](#azure-active-directory-integration) ist, erhalten die Azure AD-Benutzer über Bindungen Berechtigungen zum Durchführen von Aktionen innerhalb des Clusters. Dies wird unter [Steuern des Zugriffs auf Clusterressourcen per rollenbasierter Zugriffssteuerung und mit Azure Active Directory-Identitäten in Azure Kubernetes Service](azure-ad-rbac.md) beschrieben.

Rollenbindungen werden zum Zuweisen von Rollen für einen bestimmten Namespace verwendet. Mit diesem Ansatz können Sie einen einzelnen AKS-Cluster logisch aufteilen und den Benutzern nur den Zugriff auf die Anwendungsressourcen im jeweils zugewiesenen Namespace erlauben. Wenn Sie Rollen über den gesamten Cluster oder an Clusterressourcen außerhalb eines bestimmten Namespace binden müssen, können Sie stattdessen *Clusterrollenbindungen* verwenden.

Eine Clusterrollenbindung funktioniert auf dieselbe Weise wie die Rollenbindung für Benutzer, allerdings kann sie auf Ressourcen im gesamten Cluster angewendet werden und ist nicht auf einen bestimmten Namespace beschränkt. Mit diesem Ansatz können Sie Ihren Administratoren oder Supporttechnikern Zugriff auf alle Ressourcen im AKS-Cluster gewähren.


> [!NOTE]
> Alle von Microsoft/AKS ausgeführten Clusteraktionen werden mit Benutzereinwilligung unter der integrierten Kubernetes-Rolle `aks-service` und der integrierten Rollenbindung `aks-service-rolebinding` durchgeführt. Diese Rolle ermöglicht es AKS, Clusterprobleme zu beheben und zu diagnostizieren, damit können jedoch keine Berechtigungen geändert oder Rollen bzw. Rollenbindungen erstellt oder andere Aktionen mit hohen Berechtigungen ausgeführt werden. Der Rollenzugriff ist nur unter aktiven Supporttickets mit Just-in-time-Zugriff (JIT) aktiviert. [Hier finden Sie weitere Informationen zu AKS-Supportrichtlinien](support-policies.md).


### <a name="kubernetes-service-accounts"></a>Kubernetes-Dienstkonten

Einer der Hauptbenutzertypen in Kubernetes ist ein *Dienstkonto*. Ein Dienstkonto ist in der Kubernetes-API vorhanden und wird von dieser verwaltet. Die Anmeldeinformationen für Dienstkonten werden als Kubernetes-Geheimnisse gespeichert. Dadurch können sie von autorisierten Pods für die Kommunikation mit dem API-Server verwendet werden. Die meisten API-Anforderungen stellen einen Authentifizierungstoken für ein Dienstkonto oder ein normales Benutzerkonto bereit.

Normale Benutzerkonten ermöglichen den eher herkömmlichen Zugriff für Personen wie Administratoren oder Entwickler und nicht nur für Dienste und Prozesse. Kubernetes selbst bietet keine Identitätsverwaltungslösung, in der reguläre Benutzerkonten und Kennwörter gespeichert werden. Stattdessen können externe Identitätsverwaltungslösungen in Kubernetes integriert werden. Bei AKS-Clustern fungiert Azure Active Directory als integrierte Identitätsverwaltungslösung.

Weitere Informationen zu den Identitätsoptionen in Kubernetes finden Sie unter [Authentifizierung in Kubernetes][kubernetes-authentication].

## <a name="azure-active-directory-integration"></a>Azure Active Directory-Integration

Die Sicherheit der AKS-Cluster kann durch die Integration von Azure Active Directory (AD) erhöht werden. Azure Active Directory (Azure AD) ist ein mehrinstanzenfähiger, cloudbasierter Verzeichnis- und Identitätsverwaltungsdienst, der Kernverzeichnisdienste, Anwendungszugriffsverwaltung und Identitätsschutz in sich vereint und auf jahrzehntelange Erfahrung mit der Identitätsverwaltung in Unternehmen aufbaut. Mit Azure AD können Sie lokale Identitäten in AKS-Cluster integrieren und so eine zentrale Quelle für die Kontoverwaltung und Sicherheit bereitstellen.

![Azure Active Directory-Integration mit AKS-Clustern](media/concepts-identity/aad-integration.png)

Mit in Azure AD integrierten AKS-Clustern können Sie Benutzern oder Gruppen Zugriff auf Kubernetes-Ressourcen in einem Namespace oder im ganzen Cluster gewähren. Um einen `kubectl`-Konfigurationskontext abzurufen, kann ein Benutzer den Befehl [az aks get-credentials][az-aks-get-credentials] ausführen. Wenn ein Benutzer dann mit dem AKS-Cluster mit `kubectl` interagiert, wird er aufgefordert, sich mit seinen Azure AD-Anmeldeinformationen anzumelden. Dieser Ansatz stellt eine zentrale Quelle für die Verwaltung von Benutzerkonten und Anmeldekennwörtern bereit. Der Benutzer kann nur auf die Ressourcen zugreifen, die der Clusteradministrator definiert hat.

Die Azure AD-Authentifizierung wird für AKS-Cluster mit OpenID Connect bereitgestellt. OpenID Connect ist eine Identitätsebene, die auf dem OAuth 2.0-Protokoll aufbaut. Weitere Informationen zu OpenID Connect finden Sie in der [OpenID Connect-Dokumentation][openid-connect]. Innerhalb des Kubernetes-Clusters werden Authentifizierungstoken mithilfe der [Webhooktokenauthentifizierung][webhook-token-docs] überprüft. Die Webhooktokenauthentifizierung wird als Teil des AKS-Clusters konfiguriert und verwaltet.

### <a name="webhook-and-api-server"></a>Webhook und API-Server

![Authentifizierungsablauf für Webhook und API-Server](media/concepts-identity/auth-flow.png)

Wie in der obigen Abbildung gezeigt, ruft der API-Server den AKS-Webhookserver auf und führt die folgenden Schritte aus:

1. Die Azure AD-Clientanwendung wird von kubectl zum Anmelden von Benutzern mit dem [OAuth 2.0-Flow für die Geräteautorisierungsgenehmigung](../active-directory/develop/v2-oauth2-device-code.md) verwendet.
2. Azure AD stellt dabei ein access_token, ein id_token und ein refresh_token bereit.
3. Der Benutzer sendet mit einem access_token aus kubeconfig eine Anforderung an kubectl.
4. Kubectl sendet das access_token an den API-Server.
5. Der API-Server wird mit dem Auth-WebHook-Server konfiguriert, um die Validierung auszuführen.
6. Der Webhookserver für die Authentifizierung bestätigt, dass die Signatur des JSON-Webtokens gültig ist, indem der öffentliche Azure AD-Signaturschlüssel überprüft wird.
7. Die Serveranwendung verwendet vom Benutzer bereitgestellte Anmeldeinformationen, um die Gruppenmitgliedschaften des angemeldeten Benutzers aus der MS Graph-API abzufragen.
8. An den API-Server wird eine Antwort mit Benutzerinformationen gesendet, u. a. mit dem UPN-Anspruch (User Principal Name) des Zugriffstokens und der Gruppenmitgliedschaft des Benutzers basierend auf der Objekt-ID.
9. Die API führt eine Autorisierungsentscheidung basierend auf der Kubernetes-Rolle bzw. -Rollenbindung aus.
10. Nach der Autorisierung gibt der API-Server eine Antwort an kubectl zurück.
11. Kubectl stellt Feedback für den Benutzer bereit.
 
**[Hier](managed-aad.md) finden Sie Informationen zur Integration von AKS und AAD.**

## <a name="azure-role-based-access-control-azure-rbac"></a>Rollenbasierte Zugriffssteuerung von Azure (Azure RBAC)

Azure RBAC ist ein Autorisierungssystem, das auf [Azure Resource Manager](../azure-resource-manager/management/overview.md) basiert und eine präzise Verwaltung des Zugriffs auf Azure-Ressourcen ermöglicht.

 Die rollenbasierte Zugriffssteuerung von Azure ist für die Ressourcen in Ihrem Azure-Abonnement, und die rollenbasierte Zugriffssteuerung von Kubernetes ist für Ressourcen innerhalb Ihres Azure-Clusters bestimmt. 

Mit der rollenbasierten Zugriffssteuerung von Azure erstellen Sie eine *Rollendefinition*, welche die anzuwendenden Berechtigungen erläutert. Dieser Rollendefinition wird dann über eine *Rollenzuweisung* ein Benutzer oder eine Gruppe für einen bestimmten *Bereich* zugewiesen. Dieser Bereich kann eine einzelne Ressource oder eine Ressourcengruppe sein oder das ganze Abonnement beinhalten.

Weitere Informationen finden Sie unter [Was ist die rollenbasierte Zugriffssteuerung von Azure (Azure Role-Based Access Control, Azure RBAC)?][azure-rbac]

Für den vollständigen Betrieb eines AKS-Clusters sind zwei Zugriffsebenen erforderlich: 
1. [Zugriff auf die AKS-Ressource in Ihrem Azure-Abonnement.](#azure-rbac-to-authorize-access-to-the-aks-resource) Hiermit können Sie z. B. die Skalierung oder das Upgrade Ihres Clusters mithilfe der AKS-APIs steuern und kubeconfig pullen.
2. Zugriff auf die Kubernetes-API. Dieser Zugriff wird entweder durch [Kubernetes RBAC](#kubernetes-role-based-access-control-rbac) (herkömmlich) oder durch die [Integration von Azure RBAC in AKS für die Kubernetes-Autorisierung](#azure-rbac-for-kubernetes-authorization-preview) gesteuert.

### <a name="azure-rbac-to-authorize-access-to-the-aks-resource"></a>Azure RBAC zum Autorisieren des Zugriffs auf die AKS-Ressource

Mit Azure RBAC können Sie Ihren Benutzern (oder Identitäten) einen präzisen Zugriff auf AKS-Ressourcen in einem Abonnement oder abonnementsübergreifend bereitstellen. Beispielsweise können Sie mit der [Rolle „Mitwirkender“ für den Azure Kubernetes Service](../role-based-access-control/built-in-roles.md#azure-kubernetes-service-contributor-role) Aktionen wie das Skalieren und Aktualisieren Ihres Clusters durchführen. Ein anderer Benutzer mit der [Cluster-Admin-Rolle für den Azure Kubernetes Service](../role-based-access-control/built-in-roles.md#azure-kubernetes-service-cluster-admin-role) verfügt nur über die Berechtigung zum Pullen der kubeconfig-Administratordatei.

Alternativ dazu können Sie dem Benutzer die allgemeine Rolle [Mitwirkender](../role-based-access-control/built-in-roles.md#contributor) zuweisen. Diese umfasst die oben genannten Berechtigungen sowie alle Aktionen, die für die AKS-Ressource möglich sind, mit Ausnahme der Verwaltung der Berechtigungen selbst.

Weitere Informationen zum Schützen des Zugriffs auf die kubeconfig-Datei, die den Zugriff auf die Kubernetes-API ermöglicht, mit Azure RBAC finden Sie [hier](control-kubeconfig-access.md).

### <a name="azure-rbac-for-kubernetes-authorization-preview"></a>Azure RBAC für die Kubernetes-Autorisierung (Vorschau)

Mit der Azure-RBAC-Integration verwendet AKS einen Kubernetes-Webhookserver für die Autorisierung, damit Sie Berechtigungen und Zuweisungen von in Azure AD integrierten K8s-Clusterressourcen mit Azure-Rollendefinitionen und Rollenzuweisungen verwalten können.

![Azure RBAC für die Kubernetes-Autorisierungsablauf](media/concepts-identity/azure-rbac-k8s-authz-flow.png)

Wie im Diagramm oben gezeigt, folgen bei der Verwendung der Azure-RBAC-Integration alle Anforderungen an die Kubernetes-API demselben Authentifizierungsablauf, der im [Abschnitt zur Azure Active Directory-Integration](#azure-active-directory-integration) erläutert wurde. 

Die Anforderung wird danach aber nicht nur durch Kubernetes RBAC autorisiert, sondern wird von Azure autorisiert, solange die Identität, die die Anforderung gesendet hat, in AAD vorhanden ist. Wenn die Identität in AAD nicht vorhanden ist (z. B. ein Kubernetes-Dienstkonto), wird Azure RBAC nicht gestartet, und es wird nur die normale Kubernetes RBAC verwendet.

In diesem Szenario können Sie den Benutzern wie bei Kubernetes-Rollen eine der vier integrierten Rollen zuweisen oder benutzerdefinierte Rollen erstellen, wobei jedoch die Azure RBAC-Mechanismen und -APIs verwendet werden. 

Mit dieser Funktion können Sie z. B. nicht nur Benutzern Berechtigungen für die AKS-Ressource über Abonnements hinweg erteilen, sondern auch die Rolle und Berechtigungen einrichten und zuweisen, die sie in den einzelnen Clustern haben, die den Zugriff auf die Kubernetes-API steuern. Beispielsweise können Sie die Rolle `Azure Kubernetes Service RBAC Viewer` auf Abonnementebene gewähren, und der Empfänger kann alle Kubernetes-Objekte aus allen Clustern auflisten und abrufen, diese jedoch nicht ändern.


#### <a name="built-in-roles"></a>Integrierte Rollen

AKS stellt die folgenden vier integrierten Rollen bereit. Sie sind mit den [integrierten Rollen in Kubernetes](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#user-facing-roles) vergleichbar, weisen aber einige Unterschiede wie der Unterstützung von CRDs auf. Die vollständige Liste der Aktionen, die für jede integrierte Rolle zulässig sind, finden Sie [hier](../role-based-access-control/built-in-roles.md).

| Role                                | BESCHREIBUNG  |
|-------------------------------------|--------------|
| RBAC-Viewer von Azure Kubernetes Service  | Ermöglicht schreibgeschützten Zugriff, um die meisten Objekte in einem Namespace anzuzeigen. Es ist nicht möglich, Rollen oder Rollenbindungen anzuzeigen. Diese Rolle lässt das Anzeigen von `Secrets` nicht zu, da das Lesen des Inhalts von Geheimnissen den Zugriff auf `ServiceAccount`-Anmeldeinformationen im Namespace ermöglicht, was den API-Zugriff als beliebiger `ServiceAccount` im Namespace ermöglichen würde (eine Form von Berechtigungsausweitung).  |
| RBAC-Writer von Azure Kubernetes Service | Ermöglicht Lese-/Schreibzugriff auf die meisten Objekte in einem Namespace. Diese Rolle lässt das Anzeigen oder Ändern von Rollen oder Rollenbindungen nicht zu. Diese Rolle ermöglicht jedoch den Zugriff auf `Secrets` und das Ausführen von Pods als beliebiges Dienstkonto im Namespace, sodass sie verwendet werden kann, um die API-Zugriffsebenen eines beliebigen ServiceAccount im Namespace zu erhalten. |
| RBAC-Administrator von Azure Kubernetes Service  | Ermöglicht Administratorzugriff, der in einem Namespace erteilt werden soll. Ermöglicht Lese-/Schreibzugriff auf die meisten Ressourcen in einem Namespace (oder Clusterbereich), einschließlich der Möglichkeit zum Erstellen von Rollen und Rollenbindungen innerhalb des Namespace. Diese Rolle lässt keinen Schreibzugriff auf das Ressourcenkontingent oder den Namespace selbst zu. |
| RBAC-Clusteradministrator von Azure Kubernetes Service  | Ermöglicht Superuserzugriff, um beliebige Aktionen für beliebige Ressourcen auszuführen. Diese Rolle ermöglicht die vollständige Kontrolle über alle Ressourcen im Cluster und in allen Namespaces. |

**[Hier](manage-azure-rbac.md) finden Sie weitere Informationen zum Verwenden von Azure RBAC für die Kubernetes-Autorisierung.**

## <a name="next-steps"></a>Nächste Schritte

- Die ersten Schritte mit Azure AD und der rollenbasierten Zugriffssteuerung von Kubernetes sind unter [Integrieren von Azure Active Directory in AKS][aks-aad] beschrieben.
- Zugehörige bewährte Methoden finden Sie unter [Best Practices für die Authentifizierung und Autorisierung in Azure Kubernetes Service (AKS)][operator-best-practices-identity].
- Informationen zu den ersten Schritten mit Azure RBAC für die Kubernetes-Autorisierung finden Sie im [Artikel zum Autorisieren des Zugriffs im AKS-Cluster (Azure Kubernetes Service) mit Azure RBAC](manage-azure-rbac.md).
- Informationen zum Schützen der kubeconfig-Datei finden Sie unter [Definieren des Zugriffs auf die Kubernetes-Konfigurationsdatei in Azure Kubernetes Service (AKS) mithilfe der rollenbasierten Zugriffssteuerung von Azure](control-kubeconfig-access.md).

Weitere Informationen zu den wesentlichen Konzepten von Kubernetes und AKS finden Sie in den folgenden Artikeln:

- [Kubernetes/AKS: Cluster und Workloads][aks-concepts-clusters-workloads]
- [Kubernetes/AKS: Sicherheit][aks-concepts-security]
- [Kubernetes/AKS: virtuelle Netzwerke][aks-concepts-network]
- [Kubernetes/AKS: Speicher][aks-concepts-storage]
- [Kubernetes/AKS: Skalierung][aks-concepts-scale]

<!-- LINKS - External -->
[kubernetes-authentication]: https://kubernetes.io/docs/reference/access-authn-authz/authentication
[webhook-token-docs]: https://kubernetes.io/docs/reference/access-authn-authz/authentication/#webhook-token-authentication
[kubernetes-rbac]: https://kubernetes.io/docs/reference/access-authn-authz/rbac/

<!-- LINKS - Internal -->
[openid-connect]: ../active-directory/develop/v2-protocols-oidc.md
[az-aks-get-credentials]: /cli/azure/aks#az-aks-get-credentials
[azure-rbac]: ../role-based-access-control/overview.md
[aks-aad]: managed-aad.md
[aks-concepts-clusters-workloads]: concepts-clusters-workloads.md
[aks-concepts-security]: concepts-security.md
[aks-concepts-scale]: concepts-scale.md
[aks-concepts-storage]: concepts-storage.md
[aks-concepts-network]: concepts-network.md
[operator-best-practices-identity]: operator-best-practices-identity.md
