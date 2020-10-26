---
title: Tarife – Azure Database for PostgreSQL (Einzelserver)
description: In diesem Artikel werden die Compute- und Speicheroptionen in Azure Database for PostgreSQL (Einzelserver) beschrieben.
author: lfittl-msft
ms.author: lufittl
ms.service: postgresql
ms.topic: conceptual
ms.date: 10/14/2020
ms.openlocfilehash: e0ac1ec93dba23eb6ab3380e342141618a1ab6e4
ms.sourcegitcommit: a92fbc09b859941ed64128db6ff72b7a7bcec6ab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2020
ms.locfileid: "92069655"
---
# <a name="pricing-tiers-in-azure-database-for-postgresql---single-server"></a>Tarife in Azure Database for PostgreSQL – Einzelserver

Sie können eine Azure Database for PostgreSQL-Serverinstanz basierend auf drei unterschiedlichen Tarifen erstellen: „Basic“, „Allgemein“ und „Arbeitsspeicheroptimiert“. Die Tarife unterscheiden sich anhand der bereitstellbaren Menge an Rechenleistung in V-Kernen, des Arbeitsspeichers pro V-Kern und der zum Speichern der Daten verwendeten Speichertechnologie. Alle Ressourcen werden auf der PostgreSQL-Serverebene bereitgestellt. Ein Server kann über eine oder mehrere Datenbanken verfügen.

| Ressource/Ebene | **Grundlegend** | **Allgemeiner Zweck** | **Arbeitsspeicheroptimiert** |
|:---|:----------|:--------------------|:---------------------|
| Computegeneration | Gen 4, Gen 5 | Gen 4, Gen 5 | Gen 5 |
| V-Kerne | 1, 2 | 2, 4, 8, 16, 32, 64 |2, 4, 8, 16, 32 |
| Arbeitsspeicher pro V-Kern | 2 GB | 5 GB | 10 GB |
| Speichergröße | 5 GB bis 1 TB | 5 GB bis 16 TB | 5 GB bis 16 TB |
| Aufbewahrungszeitraum von Datenbanksicherungen | 7 bis 35 Tage | 7 bis 35 Tage | 7 bis 35 Tage |

Um einen Tarif auszuwählen, verwenden Sie die folgende Tabelle als Ausgangspunkt.

| Tarif | Zielworkloads |
|:-------------|:-----------------|
| Basic | Workloads mit geringen Anforderungen an Rechen- und E/A-Leistung. Beispiele hierfür sind Server, die für die Entwicklung, für Tests oder für kleine, selten verwendete Anwendungen verwendet werden. |
| Universell | Geeignet für die meisten Unternehmensworkloads mit gängigen Compute- und Arbeitsspeicheranforderungen und skalierbarem E/A-Durchsatz. Hierzu zählen beispielsweise zum Hosten von Web- und mobilen Apps verwendete Server und andere Unternehmensanwendungen.|
| Arbeitsspeicheroptimiert | Geeignet für Hochleistungs-Datenbankworkloads, für die In-Memory-Leistung erforderlich ist, um eine schnellere Transaktionsverarbeitung und höhere Parallelität zu erzielen. Hierzu zählen beispielsweise Server für die Verarbeitung von Echtzeitdaten und leistungsstarke Transaktions- oder Analyse-Apps.|

Nach der Servererstellung können Sie die Anzahl von virtuellen Kernen, die Hardwaregeneration und den Tarif (mit Ausnahme eines Wechsels zu oder von Basic) innerhalb weniger Sekunden ändern. Außerdem haben Sie die Möglichkeit, die Speichermenge einzeln zu erhöhen und den Aufbewahrungszeitraum für Sicherungen zu erhöhen oder zu verringern, ohne dass bei der Anwendung Ausfallzeiten auftreten. Der Sicherungsspeichertyp kann nach der Servererstellung nicht mehr geändert werden. Weitere Informationen finden Sie im Abschnitt [Skalieren von Ressourcen](#scale-resources).

## <a name="compute-generations-and-vcores"></a>Computegenerationen und V-Kerne

Computeressourcen werden in Form von virtuellen Kernen bereitgestellt und repräsentieren die logische CPU der zugrunde liegenden Hardware. China, Osten 1, China, Norden 1, US DoD, Mitte und US DoD, Osten nutzen logische CPUs der 4. Generation, die auf Intel-Prozessoren vom Typ E5-2673 v3 (Haswell) mit 2,4 GHz basieren. Alle anderen Regionen nutzen logische CPUs der 5. Generation, die auf Intel-Prozessoren vom Typ E5-2673 v4 (Broadwell) mit 2,3 GHz basieren.

## <a name="storage"></a>Storage

Der von Ihnen bereitgestellte Speicher definiert die Speicherkapazität, die für Ihren Azure Database for PostgreSQL-Server zur Verfügung steht. Der Speicher wird für die Datenbankdateien, temporären Dateien, Transaktionsprotokolle und PostgreSQL-Serverprotokolle verwendet. Außerdem wird durch die Gesamtmenge an bereitgestelltem Speicher die E/A-Kapazität Ihres Servers definiert.

| Speicherattribute | **Grundlegend** | **Allgemeiner Zweck** | **Arbeitsspeicheroptimiert** |
|:---|:----------|:--------------------|:---------------------|
| Speichertyp | Storage Basic | Allgemeiner Speicher | Allgemeiner Speicher |
| Speichergröße | 5 GB bis 1 TB | 5 GB bis 16 TB | 5 GB bis 16 TB |
| Speicherinkrementgröße | 1 GB | 1 GB | 1 GB |
| IOPS | Variable |3 IOPS/GB<br/>Min. 100 IOPS<br/>Max. 20.000 IOPS | 3 IOPS/GB<br/>Min. 100 IOPS<br/>Max. 20.000 IOPS |

> [!NOTE]
> In den folgenden Regionen wird die Speicherung von bis zu 16 TB und 20.000 IOPS unterstützt: „USA, Osten“, „USA, Osten 2“, „USA, Mitte“, „USA, Westen“, „USA, Norden-Mitte“, „USA, Süden-Mitte“, „Europa, Norden“, „Europa, Westen“, „Vereinigtes Königreich, Süden“, „Vereinigtes Königreich, Westen“, „Asien, Südosten“, „Asien, Osten“, „Japan, Osten“, „Japan, Westen“, „Südkorea, Mitte“, „Südkorea, Süden“, „Australien, Osten“, „Australien, Südosten“, „USA, Westen 2“, „USA, Westen-Mitte“, „Kanada, Osten“ und „Kanada, Mitte“.
>
> Alle anderen Regionen unterstützen bis zu 4 TB Speicherplatz und 6000 IOPS.
>

Während und nach der Erstellung des Servers können Sie zusätzliche Speicherkapazität hinzufügen und dem System erlauben, den Speicher auf der Grundlage des Speicherbedarfs Ihrer Workload automatisch zu vergrößern.

>[!NOTE]
> Der Speicher kann nur zentral hochskaliert und nicht herunterskaliert werden.

Der Tarif „Basic“ umfasst keine IOPS-Garantie. Für die Tarife „Allgemein“ und „Arbeitsspeicheroptimiert“ wird der IOPS-Wert gegenüber der bereitgestellten Speichergröße in einem Verhältnis von 3:1 skaliert.

Sie können Ihren E/A-Verbrauch im Azure-Portal oder mit Azure CLI-Befehlen überwachen. Die wichtigen zu überwachenden Metriken sind das [Speicherlimit, der Speicherprozentsatz, der genutzte Speicher und der E/A-Prozentsatz](concepts-monitoring.md).

### <a name="reaching-the-storage-limit"></a>Erreichen der Speicherbegrenzung

Server mit bis zu 100 GB bereitgestelltem Speicher werden als schreibgeschützt gekennzeichnet, wenn der freie Speicher weniger als 512 MB oder 5 % der bereitgestellten Speichergröße beträgt. Server mit mehr als 100 GB bereitgestelltem Speicher werden als schreibgeschützt gekennzeichnet, wenn der freie Speicher weniger als 5 GB beträgt.

Wenn Sie also beispielsweise 110 GB Speicher bereitgestellt haben und die tatsächliche Auslastung 105 GB überschreitet, wird der Server als schreibgeschützt gekennzeichnet. Wenn Sie andererseits 5 GB Speicher bereitgestellt haben, wird der Server als schreibgeschützt gekennzeichnet, wenn der freie Speicher unter 512 MB sinkt.

Wenn der Server als schreibgeschützt festgelegt ist, werden alle bestehenden Sitzungen getrennt, und für Transaktionen ohne Commit wird ein Rollback ausgeführt. Nachfolgende Schreibvorgänge und Transaktionscommits ergeben einen Fehler. Alle nachfolgenden Abfragen werden ununterbrochen fortgesetzt.  

Sie können die Menge des bereitgestellten Speichers für Ihren Server erhöhen oder eine neue Sitzung im Lese-/ Schreibmodus starten und Daten löschen, um Speicherplatz freizugeben. Durch Ausführen von `SET SESSION CHARACTERISTICS AS TRANSACTION READ WRITE;` wird die aktuelle Sitzung in den Lese-/Schreibmodus versetzt. Um eine Datenbeschädigung zu verhindern, führen Sie keine Schreibvorgänge durch, solange der Server noch den schreibgeschützten Status aufweist.

Sie sollten die automatische Speichervergrößerung aktivieren oder eine Benachrichtigung einrichten, damit Sie informiert werden, wenn sich der Serverspeicher dem Grenzwert nähert. So können Sie den schreibgeschützten Zustand vermeiden. Weitere Informationen finden Sie in der Dokumentation zum [Einrichten einer Benachrichtigung](howto-alert-on-metric.md).

### <a name="storage-auto-grow"></a>Automatische Speichervergrößerung

Die automatische Speichervergrößerung verhindert, dass der Server nicht mehr über genügend Speicherplatz verfügt und schreibgeschützt wird. Wenn die automatische Speichervergrößerung aktiviert ist, wird der Speicher automatisch ohne Beeinträchtigung der Workload vergrößert. Bei Servern mit bis zu 100 GB bereitgestelltem Speicher wird die bereitgestellte Speichergröße um 5 GB erhöht, sobald der freie Speicher unter 1 GB oder 10 % des bereitgestellten Speichers sinkt. Bei Servern mit mehr als 100 GB bereitgestelltem Speicher wird die bereitgestellte Speichergröße um 5 % erhöht, sobald der freie Speicherplatz unter 10 GB oder 5 % der bereitgestellten Speichergröße sinkt. Dabei gelten die maximalen, oben beschriebenen Speichergrenzwerte.

Wenn Sie also beispielsweise 1.000 GB Speicher bereitgestellt haben und die tatsächliche Auslastung 950 GB überschreitet, wird die Speichergröße des Servers auf 1.050 GB erhöht. Bei 10 GB bereitgestelltem Speicher wird die Speichergröße alternativ auf 15 GB erhöht, wenn weniger als 1 GB Speicher frei ist.

Beachten Sie, dass der Speicher nur zentral hochskaliert und nicht herunterskaliert werden kann.

## <a name="backup-storage"></a>Sicherungsspeicher

Bei Azure Database for PostgreSQL werden bis zu 100% Ihres bereitgestellten Serverspeichers ohne zusätzliche Kosten als Sicherungsspeicher hinzugefügt. Alle Sicherungsspeicher, die Sie über diesen Betrag hinaus verwenden, werden in GB pro Monat abgerechnet. Beispiel: Wenn Sie einen Server mit 250 GB bereitstellen, verfügen Sie über 250 GB an zusätzlichem Speicher, der kostenlos für Serversicherungen zur Verfügung steht. Speicher für Sicherungen, die die 250 GB überschreiten, wird gemäß dem [Preismodell](https://azure.microsoft.com/pricing/details/postgresql/) abgerechnet. Informationen zu Faktoren, die sich auf die Sicherungsspeichernutzung sowie die Überwachung und Steuerung der Sicherungsspeicherkosten beziehen, finden Sie in der [Dokumentation zur Sicherung](concepts-backup.md).

## <a name="scale-resources"></a>Skalieren von Ressourcen

Nachdem Sie Ihren Server erstellt haben, können Sie die virtuellen Kerne, die Hardwaregeneration, den Tarif (mit Ausnahme eines Wechsels zu oder von Basic), die Speichermenge und den Aufbewahrungszeitraum für Sicherungen einzeln ändern. Der Sicherungsspeichertyp kann nach der Servererstellung nicht mehr geändert werden. Die Anzahl virtueller Kerne kann zentral hoch- oder herunterskaliert werden. Die Aufbewahrungsdauer für Sicherungen kann von 7 bis zu 35 Tagen zentral hoch- oder herunterskaliert werden. Die Speichergröße kann nur erhöht werden. Die Skalierung der Ressourcen kann entweder über das Portal oder per Azure CLI durchgeführt werden. Ein Beispiel für die Skalierung über die Azure CLI finden Sie unter [Überwachen und Skalieren eines einzelnen PostgreSQL-Servers mit der Azure CLI](scripts/sample-scale-server-up-or-down.md).

> [!NOTE]
> Die Speichergröße kann nur erhöht werden. Nach der Erhöhung können Sie nicht mehr zu einer kleineren Speichergröße zurückkehren.

Beim Ändern der Anzahl von virtuellen Kernen, der Hardwaregeneration oder des Tarifs wird eine Kopie des ursprünglichen Servers mit der neuen Computezuteilung erstellt. Sobald der neue Server betriebsbereit ist und ausgeführt wird, werden die Verbindungen auf den neuen Server verschoben. Während des Moments, in dem das System den Wechsel zum neuen Server durchführt, können keine neuen Verbindungen hergestellt werden, und für alle Transaktionen ohne Commit erfolgt ein Rollback. Die Länge dieses Zeitfensters variiert, aber normalerweise dauert der Vorgang nicht länger als eine Minute.

Das Skalieren des Speichers und das Ändern der Aufbewahrungsdauer für Sicherungen sind reine Onlinevorgänge. Es gibt keine Ausfallzeit, und Ihre Anwendung wird nicht beeinträchtigt. Da der IOPS-Wert in Abhängigkeit der Größe des bereitgestellten Speichers skaliert wird, können Sie die IOPS-Menge, die für Ihren Server verfügbar ist, durch das zentrale Hochskalieren des Speichers erhöhen.

## <a name="pricing"></a>Preise

Aktuelle Preisinformationen finden Sie auf der Seite [Azure-Datenbank für MySQL – Preise](https://azure.microsoft.com/pricing/details/PostgreSQL/). Informationen zu den Kosten der gewünschten Konfiguration können Sie im [Azure-Portal](https://portal.azure.com/#create/Microsoft.PostgreSQLServer) anzeigen. Die monatlichen Kosten für die von Ihnen ausgewählten Optionen werden auf der Registerkarte **Tarif** angegeben. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie den Azure-Preisrechner verwenden, um einen geschätzten Preis zu erhalten. Wählen Sie auf der Website des [Azure-Preisrechners](https://azure.microsoft.com/pricing/calculator/) die Option **Elemente hinzufügen** aus, erweitern Sie die Kategorie **Datenbanken** , und wählen Sie **Azure Database for PostgreSQL** aus, um die Optionen anzupassen.

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich, wie Sie [im Portal eine PostgreSQL-Serverinstanz erstellen](tutorial-design-database-using-azure-portal.md).
- Weitere Informationen zu [Dienstgrenzwerten](concepts-limits.md).
- Weitere Informationen zum [Aufskalieren mit Lesereplikaten](howto-read-replicas-portal.md).
