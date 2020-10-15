---
title: Unterstützte FHIR-Features in Azure – Azure API for FHIR
description: In diesem Artikel wird erläutert, welche Features der FHIR-Spezifikation in Azure API for FHIR implementiert sind.
services: healthcare-apis
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 02/07/2019
ms.author: matjazl
ms.openlocfilehash: afb4026a7865f2cc8f831d8d1d7b1d332014d310
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90007569"
---
# <a name="features"></a>Features

Azure API for FHIR bietet eine vollständig verwaltete Bereitstellung des Microsoft FHIR-Servers für Azure. Der Server ist eine Implementierung des [FHIR](https://hl7.org/fhir)-Standards. In diesem Dokument sind die wichtigsten Features des FHIR-Servers aufgeführt.

## <a name="fhir-version"></a>FHIR-Version

Letzte unterstützte Version: `4.0.1`

Derzeit ebenfalls unterstützte vorherige Versionen: `3.0.2`

## <a name="rest-api"></a>REST-API

| API                            | Unterstützt: PaaS | Unterstützt: OSS (SQL) | Unterstützt: OSS (Cosmos DB) | Comment                                             |
|--------------------------------|-----------|-----------|-----------|-----------------------------------------------------|
| Lesen                           | Ja       | Ja       | Ja       |                                                     |
| vread                          | Ja       | Ja       | Ja       |                                                     |
| aktualisieren                         | Ja       | Ja       | Ja       |                                                     |
| Update mit optimistischer Sperre | Ja       | Ja       | Ja       |                                                     |
| update (bedingt)           | Ja       | Ja       | Ja       |                                                     |
| patch                          | Nein        | Nein        | Nein        |                                                     |
| delete                         | Ja       | Ja       | Ja       |                                                     |
| delete (bedingt)           | Nein        | Nein        | Nein        |                                                     |
| create                         | Ja       | Ja       | Ja       | Unterstützung für POST/PUT                               |
| create (bedingt)           | Ja       | Ja       | Ja       |                                                     |
| search                         | Teilweise   | Teilweise   | Teilweise   | Siehe unten                                           |
| Verkettete Suche                 | Nein        | Ja       | Nein        |                                           |
| Umgekehrte verkettete Suche         | Nein        | Nein        | Nein        |                                            |
| capabilities                   | Ja       | Ja       | Ja       |                                                     |
| Batch                          | Ja       | Ja       | Ja       |                                                     |
| transaction                    | Nein        | Ja       | Nein        |                                                     |
| history                        | Ja       | Ja       | Ja       |                                                     |
| paging                         | Teilweise   | Teilweise   | Teilweise   | `self` und `next` werden unterstützt                     |
| intermediaries                 | Nein        | Nein        | Nein        |                                                     |

## <a name="search"></a>Suchen,

Es werden alle Suchparametertypen unterstützt. 

| Suchparametertyp | Unterstützt: PaaS | Unterstützt: OSS (SQL) | Unterstützt: OSS (Cosmos DB) | Comment |
|-----------------------|-----------|-----------|-----------|---------|
| Number                | Ja       | Ja       | Ja       |         |
| Date/DateTime         | Ja       | Ja       | Ja       |         |
| String                | Ja       | Ja       | Ja       |         |
| Token                 | Ja       | Ja       | Ja       |         |
| Verweis             | Ja       | Ja       | Ja       |         |
| Composite             | Ja       | Ja       | Ja       |         |
| Menge              | Ja       | Ja       | Ja       |         |
| URI                   | Ja       | Ja       | Ja       |         |
| Sonderfunktionen               | Nein        | Nein        | Nein        |         |


| Modifizierer             | Unterstützt: PaaS | Unterstützt: OSS (SQL) | Unterstützt: OSS (Cosmos DB) | Comment |
|-----------------------|-----------|-----------|-----------|---------|
|`:missing`             | Ja       | Ja       | Ja       |         |
|`:exact`               | Ja       | Ja       | Ja       |         |
|`:contains`            | Ja       | Ja       | Ja       |         |
|`:text`                | Ja       | Ja       | Ja       |         |
|`:in` (Token)          | Nein        | Nein        | Nein        |         |
|`:below` (Token)       | Nein        | Nein        | Nein        |         |
|`:above` (Token)       | Nein        | Nein        | Nein        |         |
|`:not-in` (Token)      | Nein        | Nein        | Nein        |         |
|`:[type]` (Referenz)  | Nein        | Nein        | Nein        |         |
|`:below` (URI)         | Ja       | Ja       | Ja       |         |
|`:not`                 | Nein        | Nein        | Nein        |         |
|`:above` (URI)         | Nein        | Nein        | Nein        | Problem [#158](https://github.com/Microsoft/fhir-server/issues/158) |

| Allgemeiner Suchparameter | Unterstützt: PaaS | Unterstützt: OSS (SQL) | Unterstützt: OSS (Cosmos DB) | Comment |
|-------------------------| ----------| ----------| ----------|---------|
| `_id`                   | Ja       | Ja       | Ja       |         |
| `_lastUpdated`          | Ja       | Ja       | Ja       |         |
| `_tag`                  | Ja       | Ja       | Ja       |         |
| `_profile`              | Ja       | Ja       | Ja       |         |
| `_security`             | Ja       | Ja       | Ja       |         |
| `_text`                 | Nein        | Nein        | Nein        |         |
| `_content`              | Nein        | Nein        | Nein        |         |
| `_list`                 | Ja       | Ja       | Ja       |         |
| `_has`                  | Nein        | Nein        | Nein        |         |
| `_type`                 | Ja       | Ja       | Ja       |         |
| `_query`                | Nein        | Nein        | Nein        |         |

| Suchvorgänge       | Unterstützt: PaaS | Unterstützt: OSS (SQL) | Unterstützt: OSS (Cosmos DB) | Comment |
|-------------------------|-----------|-----------|-----------|---------|
| `_filter`               | Nein        | Nein        | Nein        |         |
| `_sort`                 | Partial        | Partial   | Teilweise        |   `_sort=_lastUpdated` wird unterstützt       |
| `_score`                | Nein        | Nein        | Nein        |         |
| `_count`                | Ja       | Ja       | Ja       |         |
| `_summary`              | Teilweise   | Teilweise   | Teilweise   | `_summary=count` wird unterstützt |
| `_include`              | Nein        | Ja       | Nein        |         |
| `_revinclude`           | Nein        | Ja       | Nein        | Enthaltene Elemente sind auf 100 beschränkt. |
| `_contained`            | Nein        | Nein        | Nein        |         |
| `_elements`             | Ja        | Ja        | Ja        |         |

## <a name="extended-operations"></a>Erweiterte Vorgänge

Alle unterstützten Vorgänge zur Erweiterung der RESTful-API.

| Suchparametertyp | Unterstützt: PaaS | Unterstützt: OSS (SQL) | Unterstützt: OSS (Cosmos DB) | Comment |
|-----------------------|-----------|-----------|-----------|---------|
| $export (gesamtes System)                | Ja       | Ja       | Ja       |         |
| Patient/$export         | Ja       | Ja       | Ja       |         |
| Gruppe/$export               | Ja       | Ja       | Ja       |         |

## <a name="persistence"></a>Persistenz

Der Microsoft FHIR-Server verfügt über ein austauschbares Persistenzmodul (siehe [`Microsoft.Health.Fhir.Core.Features.Persistence`](https://github.com/Microsoft/fhir-server/tree/master/src/Microsoft.Health.Fhir.Core/Features/Persistence)).

Derzeit umfasst der Open-Source-Code für FHIR-Server eine Implementierung für [Azure Cosmos DB](../cosmos-db/index-overview.md) und [SQL-Datenbank](https://azure.microsoft.com/services/sql-database/).

Cosmos DB ist eine global verteilte Datenbank, die mehrere Modelle (SQL-API, MongoDB-API, usw.) unterstützt. Es unterstützt verschiedene [Konsistenzebenen](../cosmos-db/consistency-levels.md). Die standardmäßige Bereitstellungsvorlage konfiguriert den FHIR-Server mit `Strong`-Konsistenz, aber die Konsistenzrichtlinie kann auf Anforderungsbasis unter Verwendung des Anforderungsheaders `x-ms-consistency-level` geändert (allgemein gelockert) werden.

## <a name="role-based-access-control"></a>Rollenbasierte Zugriffssteuerung

Der FHIR-Server verwendet [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) für die Zugriffssteuerung. Insbesondere wird die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) erzwungen, wenn der Konfigurationsparameter `FhirServer:Security:Enabled` auf `true` festgelegt ist, und bei allen Anforderungen (außer `/metadata`) an den FHIR-Server muss der Anforderungsheader `Authorization` auf `Bearer <TOKEN>` festgelegt sein. Das Token muss eine oder mehrere Rollen gemäß der Definition im `roles`-Anspruch enthalten. Eine Anforderung ist zulässig, wenn der Token eine Rolle enthält, die die angegebene Aktion für die angegebene Ressource gestattet.

Derzeit werden die zulässigen Aktionen für eine bestimmte Rolle *global* auf die API angewendet.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie Informationen zu den unterstützten FHIR-Features in Azure API for FHIR erhalten. Stellen Sie als Nächstes die Azure API for FHIR bereit.
 
>[!div class="nextstepaction"]
>[Bereitstellen von Azure API for FHIR](fhir-paas-portal-quickstart.md)
