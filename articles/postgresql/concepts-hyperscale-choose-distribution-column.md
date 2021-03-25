---
title: Auswählen von Verteilungsspalten – Hyperscale (Citus) – Azure Database for PostgreSQL
description: Erfahren Sie, wie Verteilungsspalten in gängigen Szenarien in Azure Database for PostgreSQL – Hyperscale (Citus) – verwendet werden.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 05/06/2019
ms.openlocfilehash: 129eff8c954c0c5469d3607e6ae16ce3202630ed
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "91929335"
---
# <a name="choose-distribution-columns-in-azure-database-for-postgresql--hyperscale-citus"></a>Auswählen von Verteilungsspalten in Azure Database for PostgreSQL: Hyperscale (Citus)

Die Auswahl der Verteilungsspalte für die einzelnen Tabellen ist eine der wichtigsten Entscheidungen bei der Modellierung, die Sie treffen. Azure Database for PostgreSQL – Hyperscale (Citus) speichert Zeilen basierend auf dem Wert der Verteilungsspalte der Zeilen in Shards.

Durch die richtige Wahl werden verwandte Daten in denselben physischen Knoten gruppiert, was zu schnellen Abfragen führt und Unterstützung für alle SQL-Funktionen hinzufügt. Durch eine falsche Auswahl wird das System langsam ausgeführt und unterstützt nicht alle SQL-Funktionen auf allen Knoten.

In diesem Artikel erhalten Sie Tipps für die Verteilungsspalte für die beiden häufigsten Hyperscaleszenarien (Citus).

### <a name="multi-tenant-apps"></a>Mehrinstanzenfähige Apps

Die Architektur mit mehreren Mandanten verwendet eine Form der hierarchischen Datenbankmodellierung, um Abfragen auf Knoten in der Servergruppe zu verteilen. Die Spitze der Datenhierarchie ist die *Mandanten-ID*, die in jeder Tabelle in einer Spalte gespeichert werden muss.

Hyperscale (Citus) untersucht Abfragen, um die involvierte Mandanten-ID zu ermitteln, und sucht nach dem entsprechenden Tabellenshard. Die Abfrage wird an einen einzelnen Workerknoten weitergeleitet, der den Shard enthält. Die Ausführung einer Abfrage mit allen relevanten Daten auf demselben Knoten wird als Zusammenstellung bezeichnet.

Das folgende Diagramm veranschaulicht die Zusammenstellung im Datenmodell mit mehreren Mandanten. Es enthält zwei Tabellen, „Accounts“ und „Campaigns“, beide verteilt nach `account_id`. Die schattierten Felder stellen Shards dar. Grüne Shards werden zusammen auf einem Workerknoten gespeichert und blaue auf einem anderen Workerknoten. Beachten Sie, dass bei einer JOIN-Abfrage zwischen „Accounts“ und „Campaigns“ alle notwendigen Daten zusammen auf einem Knoten gespeichert werden, wenn beide Tabellen auf dieselbe account\_id eingeschränkt sind.

![Zusammenstellung mit mehreren Mandanten](media/concepts-hyperscale-choosing-distribution-column/multi-tenant-colocation.png)

Um diesen Entwurf in Ihrem eigenen Schema anzuwenden, stellen Sie fest, was in Ihrer Anwendung einen Mandanten ausmacht. Zu den gängigen Instanzen gehören Unternehmen, Konto, Organisation oder Kunde. Der Name der Spalte lautet beispielsweise `company_id` oder `customer_id`. Untersuchen Sie die einzelnen Abfragen, und stellen Sie sich folgende Frage: Würde die Abfrage funktionieren, wenn alle beteiligten Tabellen durch zusätzliche WHERE-Klauseln auf Zeilen mit derselben Mandanten-ID eingeschränkt würden?
Abfragen im mehrinstanzenfähigen Modell sind auf einen Mandanten beschränkt. Beispielsweise werden Abfragen für Verkäufe oder Inventar in einem bestimmten Speicher festgelegt.

#### <a name="best-practices"></a>Bewährte Methoden

-   **Partitionieren Sie verteilte Tabellen nach einer gemeinsamen Spalte „tenant\_id“.** In einer SaaS-Anwendung, in denen Mandanten Unternehmen darstellen, lautet die tenant\_id wahrscheinlich „company\_id“.
-   **Konvertieren Sie kleine mandantenübergreifende Tabellen in Verweistabellen.** Wenn mehrere Mandanten eine kleine Tabelle mit Informationen gemeinsam nutzen, verteilen Sie sie als Verweistabelle.
-   **Schränken Sie alle Anwendungsabfragen ein, indem Sie sie nach „tenant\_id“ filtern.** Jede Abfrage muss Informationen für jeweils einen Mandanten anfordern.

Im [Tutorial zur Verwendung mehrerer Mandanten](./tutorial-design-database-hyperscale-multi-tenant.md) finden Sie ein Beispiel für die Erstellung dieser Art von Anwendung.

### <a name="real-time-apps"></a>Echtzeit-Apps

Die Architektur mit mehreren Mandanten führt eine hierarchische Struktur ein und verwendet die Zusammenstellung von Daten zum Weiterleiten von Abfragen pro Mandant. Im Gegensatz dazu sind Echtzeitarchitekturen von bestimmten Verteilungseigenschaften ihrer Daten abhängig, um eine hochgradige Parallelverarbeitung zu erreichen.

Wir verwenden „Entitäts-ID“ als Bezeichnung für Verteilungsspalten im Echtzeitmodell. Typische Entitäten sind Benutzer, Hosts oder Geräte.

Echtzeitabfragen fordern in der Regel numerische Aggregate an, die nach Datum oder nach Kategorie gruppiert sind. Hyperscale (Citus) sendet diese Abfragen an die einzelnen Shards, um Teilergebnisse zu erhalten, und fasst die endgültige Antwort auf dem Koordinatorknoten zusammen. Abfragen werden am schnellsten ausgeführt, wenn so viele Knoten wie möglich einen Beitrag leisten und wenn kein einzelner Knoten eine unverhältnismäßig große Menge an Arbeit bewältigen muss.

#### <a name="best-practices"></a>Bewährte Methoden

-   **Wählen Sie eine Spalte mit hoher Kardinalität als Verteilungsspalte aus.** Zum Vergleich: Ein Feld „Status“ mit den Werten "Neu", "Bezahlt" und "Geliefert" in einer Auftragstabelle ist als Verteilungsspalte eine ungünstige Wahl. Es kann nur diese wenigen Werte annehmen, wodurch die Anzahl von Shards zur Datenspeicherung und die Anzahl von Knoten zur Verarbeitung begrenzt werden. Außerdem empfiehlt es sich, unter den Spalten mit hoher Kardinalität diejenigen Spalten auszuwählen, die häufig in group-by-Klauseln oder als Joinschlüssel verwendet werden.
-   **Wählen Sie eine Spalte mit gleichmäßiger Verteilung.** Wenn Sie eine Tabelle anhand einer Spalte verteilen, die eine Neigung zu bestimmten häufig verwendeten Werten aufweist, sammeln sich die Daten tendenziell in bestimmten Shards an. Die Knoten mit den betreffenden Shards müssen in diesem Fall mehr leisten als die anderen Knoten.
-   **Verteilen Sie Fakten- und Dimensionstabellen anhand ihrer gemeinsamen Spalten.**
    Ihre Faktentabelle kann nur einen Verteilungsschlüssel aufweisen. Tabellen, die anhand eines anderen Schlüssels verknüpft werden, können nicht mit den Daten der Faktentabelle zusammengestellt werden. Wählen Sie eine Dimension basierend auf der Verknüpfungshäufigkeit und der Größe der verknüpften Zeilen aus.
-   **Ändern Sie einige Dimensionstabellen in Verweistabellen.** Wenn eine Dimensionstabelle nicht mit den Daten der Faktentabelle zusammengestellt werden kann, können Sie die Abfrageleistung verbessern, indem Sie Kopien der Dimensionstabelle in Form einer Verweistabelle an alle Knoten verteilen.

Im [Tutorial zum Echtzeitdashboard](./tutorial-design-database-hyperscale-realtime.md) finden Sie ein Beispiel für die Erstellung dieser Art von Anwendung.

### <a name="time-series-data"></a>Zeitreihendaten

In einer Zeitreihenworkload fragen Anwendungen neueste Informationen ab und archivieren alte Informationen.

Der häufigste Fehler beim Modellieren von Zeitreiheninformationen in Hyperscale (Citus) besteht darin, den Zeitstempel selbst als Verteilungsspalte zu verwenden. Eine Hashverteilung basierend auf der Zeit verteilt Zeiten scheinbar zufällig auf verschiedene Shards, statt Zeitbereiche in Shards zusammenzuhalten. Abfragen, die Zeitangaben beinhalten, verweisen im Allgemeinen auf Zeitbereiche, z.B. auf die neuesten Daten. Diese Art von Hashverteilung führt zu Netzwerkmehraufwand.

#### <a name="best-practices"></a>Bewährte Methoden

-   **Wählen Sie einen Zeitstempel nicht als Verteilungsspalte aus.** Wählen Sie eine andere Verteilungsspalte aus. Verwenden Sie in einer App mit mehreren Mandanten die Mandanten-ID, oder verwenden Sie in einer Echtzeit-App die Entitäts-ID.
-   **Verwenden Sie stattdessen die PostgreSQL-Tabellenpartitionierung für die Zeit.** Unterteilen Sie eine große Tabelle mit zeitlich strukturierten Daten mithilfe der Tabellenpartitionierung in mehrere geerbte Tabellen mit jeweils verschiedenen Zeiträumen. Durch die Verteilung einer mit Postgres partitionierten Tabelle in Hyperscale (Citus) werden Shards für die geerbten Tabellen erstellt.

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie, wie die [Zusammenstellung](concepts-hyperscale-colocation.md) zwischen verteilten Daten zu einer schnelleren Ausführung von Abfragen führt.
