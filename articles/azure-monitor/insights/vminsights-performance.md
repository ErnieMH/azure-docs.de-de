---
title: Darstellen der Leistung in Diagrammen mit Azure Monitor for VMs
description: Leistung ist ein Feature von Azure Monitor for VMs, das Anwendungskomponenten auf Windows- und Linux-Systemen automatisch ermittelt und die Kommunikation zwischen Diensten abbildet. Dieser Artikel enthält Details zu seiner Verwendung in einer Reihe von Szenarien.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 05/31/2020
ms.openlocfilehash: f9578fadfbe057b723af63e338bf8bda63cf6f21
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91330909"
---
# <a name="how-to-chart-performance-with-azure-monitor-for-vms"></a>Darstellen der Leistung in Diagrammen mit Azure Monitor for VMs

Azure Monitor for VMs beinhaltet einen Satz Leistungsdiagramme, die auf verschiedene Key Performance Indicators (KPIs) abzielen, um Sie beim Bestimmen der Leistung eines virtuellen Computers zu unterstützen. Die Diagramme zeigen die Ressourcennutzung über einen Zeitraum an, damit Sie Engpässe und Anomalien erkennen oder zu einer Perspektive wechseln können, in der jede VM aufgelistet ist, um die Ressourcennutzung nach der ausgewählten Metrik anzuzeigen. Beim Thema Leistung sind zwar eine Vielzahl von Elementen zu berücksichtigen, Azure Monitor für VMs überwacht jedoch Betriebssystem-Key Performance Indicators im Zusammenhang mit Prozessor, Arbeitsspeicher, Netzwerkadapter und Datenträgerverwendung. Leistung ergänzt das Feature zur Integritätsüberwachung und hilft bei der Offenlegung von Problemen, die auf einen möglichen Ausfall von Systemkomponenten hinweisen, Feinabstimmung und Optimierung unterstützen, um Effizienz zu erreichen, oder bei der Kapazitätsplanung helfen.  

## <a name="limitations"></a>Einschränkungen
Nachfolgend sind Einschränkungen bei der Leistungserfassung mit Azure Monitor für VMs aufgeführt.

- **Verfügbarer Arbeitsspeicher** ist für virtuelle Computer mit Red Hat Linux (RHEL) 6 nicht verfügbar. Diese Metrik wird aus **MemAvailable** berechnet, das in [Kernelversion 3.14](http://www.man7.org/linux/man-pages/man1/free.1.html) eingeführt wurde.
- Metriken sind nur für Datenträger auf virtuellen Linux-Computern mit dem XFS-Dateisystem oder der EXT-Dateisystemfamilie (EXT2, EXT3, EXT4) verfügbar.

## <a name="multi-vm-perspective-from-azure-monitor"></a>Multi-VM-Perspektive in Azure Monitor

In Azure Monitor stellt das Leistungsfeature eine Ansicht aller überwachten VMs zur Verfügung, die über alle Arbeitsgruppen in Ihren Abonnements oder in Ihrer Umgebung bereitgestellt sind. Führen Sie für den Zugriff aus Azure Monitor die folgenden Schritte aus. 

1. Wählen Sie im Azure-Portal die Option **Überwachen** aus. 
2. Wählen Sie im Abschnitt **Lösungen** die Option **Virtual Machines** aus.
3. Wählen Sie die Registerkarte **Leistung** aus.

![Top-N-Listenansicht Leistung in VM Insights](media/vminsights-performance/vminsights-performance-aggview-01.png)

Gehen Sie wie folgt vor, falls Sie über mehrere Log Analytics-Arbeitsbereiche verfügen: Wählen Sie auf der Registerkarte **Top-N-Diagramme** über den Selektor **Arbeitsbereich** oben auf der Seite den Arbeitsbereich aus, der für die Lösung aktiviert ist. Mit dem Selektor **Gruppe** werden Abonnements, Ressourcengruppen, [Computergruppen](../platform/computer-groups.md) und VM-Skalierungsgruppen für Computer des ausgewählten Arbeitsbereichs zurückgegeben. Sie können diese Daten nutzen, um die Ergebnisse, die in den Diagrammen auf dieser Seite und auf anderen Seiten angezeigt werden, noch weiter zu filtern. Ihre Auswahl gilt nur für das Leistungsfeature und nicht für die Integrität oder die Karte.  

Standardmäßig zeigen die Diagramme die letzten 24 Stunden an. Mithilfe des **TimeRange**-Selektors können Sie nach historischen Zeiträumen von bis zu 30 Tagen abfragen, um darzustellen, wie die Leistung in der Vergangenheit war.

Diese fünf Diagramme zur Kapazitätsauslastung werden auf der Seite angezeigt:

* CPU-Auslastung %: Zeigt die fünf Computer mit der höchsten durchschnittlichen Prozessorauslastung an. 
* Verfügbarer Arbeitsspeicher: Zeigt die fünf Computer mit der kleinsten Menge an verfügbarem Arbeitsspeicher an. 
* Genutzter Speicherplatz auf logischen Datenträgern %: Zeigt die fünf Computer mit der höchsten durchschnittlichen Speicherplatznutzung über alle Datenträgervolumes in Prozent an. 
* Rate der gesendeten Bytes: Zeigt die fünf Computer mit der durchschnittlich größten Anzahl von gesendeten Bytes an. 
* Bytes Receive Rate (Rate der empfangenen Byte): Zeigt die fünf Computer mit der durchschnittlich größten Anzahl von empfangen Byte an. 

Wenn Sie auf das Stecknadelsymbol in der oberen rechten Ecke eines der fünf Diagramme klicken, wird das ausgewählte Diagramm an das Azure-Dashboard angeheftet, das Sie zuletzt angezeigt haben.  Über dem Dashboard können Sie die Größe des Diagramms ändern und es neu positionieren. Die Auswahl des Diagramms über das Dashboard leitet Sie zum Azure Monitor für VMs weiter und lädt den richtigen Bereich und die richtige Ansicht.  

Klicken Sie auf das Symbol links neben dem Stecknadelsymbol in einem der fünf Diagramme, um die Ansicht **Top-N-Liste** zu öffnen.  Hier sehen Sie die Ressourcennutzung für die betreffende Leistungsmetrik nach einzelnen VMs in einer Listenansicht und den Computer mit der tendenziell höchsten Nutzung.  

![Top-N-Listenansicht für eine ausgewählte Leistungsmetrik](media/vminsights-performance/vminsights-performance-topnlist-01.png)

Wenn Sie auf den virtuellen Computer klicken, wird der Bereich **Eigenschaften** auf der rechten Seite erweitert, um die Eigenschaften des ausgewählten Elements anzuzeigen, wie die vom Betriebssystem gemeldeten Systeminformationen, Eigenschaften der Azure-VM usw. Durch Klicken auf eine der Optionen im Abschnitt **Quicklinks** werden Sie direkt von der ausgewählten VM zu dem betreffenden Feature weitergeleitet.  

![Bereich „Eigenschaften von virtuellen Computern“](./media/vminsights-performance/vminsights-properties-pane-01.png)

Wechseln Sie zur Registerkarte **Aggregatdiagramme**, um die Leistungsmetriken nach dem Durchschnitt oder nach Quantilskennzahlen gefiltert anzuzeigen.  

![Aggregatansicht Leistung in VM Insights](./media/vminsights-performance/vminsights-performance-aggview-02.png)

Die folgenden Diagramme zur Kapazitätsauslastung stehen zur Verfügung:

* CPU-Auslastung % – zeigt standardmäßig den Mittelwert und das obere 95. Quantil an 
* Verfügbarer Arbeitsspeicher – zeigt standardmäßig den Mittelwert und das obere 5. und 10. Quantil an 
* Genutzter Speicherplatz auf logischen Datenträgern % – zeigt standardmäßig den Mittelwert und das 95. Quantil an 
* Rate der gesendeten Bytes – zeigt standardmäßig den Mittelwert für gesendete Bytes an 
* Rate der empfangenen Bytes – zeigt standardmäßig den Mittelwert für empfangene Bytes an

Außerdem haben Sie die Möglichkeit, die Granularität der Diagramme innerhalb des Zeitraums durch Auswahl von **Mittelw.** , **Min.** , **Max.** , **50.** , **90.** und **95.** im Quantilselektor zu ändern.

Um die Ressourcennutzung der einzelnen VMs in einer Listenansicht darzustellen und zu sehen, welcher Computer tendenziell die höchste Nutzung aufweist, wählen Sie die Registerkarte **Top-N-Liste** aus.  Auf der Seite **Top-N-Liste** werden die 20 Computer mit der höchsten Nutzung nach dem 95. Quantil für die Metrik *CPU-Auslastung %* angezeigt.  Sie können weitere Computer anzeigen, indem Sie **Weitere laden** auswählen, und die Ergebnisse werden erweitert, um die oberen 500 Computer anzuzeigen. 

>[!NOTE]
>Die Liste kann nicht mehr als jeweils 500 Computer darstellen.  
>

![Beispiel für die Top-N-Liste-Seite](./media/vminsights-performance/vminsights-performance-topnlist-01.png)

Um die Ergebnisse für eine bestimmte VM in der Liste zu filtern, geben Sie ihren Computernamen in das Textfeld **Nach Namen suchen** ein.  

Wenn Sie lieber die Nutzung für eine andere Leistungsmetrik anzeigen möchten, wählen Sie in der Dropdownliste **Metrik** **Verfügbarer Arbeitsspeicher**, **Genutzter Speicherplatz auf logischen Datenträgern %** , **Netzwerk – empfangene Byte/s** oder **Netzwerk – gesendete Byte/s** aus, dann wird die Liste aktualisiert, um die Auslastung im Bereich der betreffenden Metrik darzustellen.  

Durch Auswählen eines virtuellen Computers in der Liste wird der Bereich **Eigenschaften** rechts auf der Seite geöffnet, und hier können Sie **Leistungsdetails** auswählen.  Die Seite **VM-Details** wird mit dem Bereich der betreffenden VM geöffnet; die Benutzeroberfläche beim direkten Zugriff auf VM Insights – Leistung aus der Azure VM ist ähnlich.  

## <a name="view-performance-directly-from-an-azure-vm"></a>Direktes Anzeigen der Leistung in einer Azure-VM

Führen Sie für den direkten Zugriff aus einer VM die folgenden Schritte aus.

1. Wählen Sie im Azure-Portal die Option **Virtual Machines** aus. 
2. Wählen Sie in der Liste eine VM und im Abschnitt **Überwachung** die Option **Insights** aus.  
3. Wählen Sie die Registerkarte **Leistung** aus. 

Diese Seite enthält nicht nur Diagramme zur Leistungsauslastung, sondern auch eine Tabelle, die für jeden ermittelten logischen Datenträger dessen Kapazität, die Auslastung und den Gesamtmittelwert für jede Kennzahl anzeigt.  

Die folgenden Diagramme zur Kapazitätsauslastung stehen zur Verfügung:

* CPU-Auslastung % – zeigt standardmäßig den Mittelwert und das obere 95. Quantil an 
* Verfügbarer Arbeitsspeicher – zeigt standardmäßig den Mittelwert und das obere 5. und 10. Quantil an 
* Genutzter Speicherplatz auf logischen Datenträgern % – zeigt standardmäßig den Mittelwert und das 95. Quantil an 
* Logischer Datenträger – IOPs – zeigt standardmäßig den Mittelwert und das 95. Quantil an
* Logischer Datenträger – MB/s – zeigt standardmäßig den Mittelwert und das 95. Quantil an
* Logischer Datenträger – max. Nutzung – zeigt standardmäßig den Mittelwert und das 95. Quantil an
* Rate der gesendeten Bytes – zeigt standardmäßig den Mittelwert für gesendete Bytes an 
* Rate der empfangenen Bytes – zeigt standardmäßig den Mittelwert für empfangene Bytes an

Wenn Sie auf das Stecknadelsymbol in der oberen rechten Ecke eines der Diagramme klicken, wird das ausgewählte Diagramm an das Azure-Dashboard angeheftet, das Sie zuletzt angezeigt haben. Über dem Dashboard können Sie die Größe des Diagramms ändern und es neu positionieren. Die Auswahl des Diagramms über das Dashboard leitet Sie zum Azure Monitor für VMs weiter und lädt die Leistungsdetailansicht für den virtuellen Computer.  

![Ansicht von VM Insights – Leistung direkt in der VM](./media/vminsights-performance/vminsights-performance-directvm-01.png)

## <a name="view-performance-directly-from-an-azure-virtual-machine-scale-set"></a>Direktes Anzeigen der Leistung über eine Azure-VM-Skalierungsgruppe

Führen Sie für den direkten Zugriff über eine Azure-VM-Skalierungsgruppe die folgenden Schritte durch.

1. Klicken Sie im Azure-Portal auf **VM-Skalierungsgruppen**.
2. Wählen Sie in der Liste eine VM und im Abschnitt **Überwachung** die Option **Insights** aus, um die Registerkarte **Leistung** aufzurufen.

Auf dieser Seite wird die Leistungsansicht für Azure Monitor für die ausgewählte Skalierungsgruppe geladen. Dadurch können Sie die Top-N-Instanzen in der Skalierungsgruppe für die überwachten Metriken, die aggregierte Leistung der Skalierungsgruppe sowie die Trends für die ausgewählten Metriken für die einzelnen Instanzen in der Skalierungsgruppe anzeigen. Wenn Sie eine Instanz aus der Listenansicht auswählen, können Sie deren Übersicht laden oder eine detaillierte Leistungsansicht für diese Instanz aufrufen.

Wenn Sie auf das Stecknadelsymbol in der oberen rechten Ecke eines der Diagramme klicken, wird das ausgewählte Diagramm an das Azure-Dashboard angeheftet, das Sie zuletzt angezeigt haben. Über dem Dashboard können Sie die Größe des Diagramms ändern und es neu positionieren. Die Auswahl des Diagramms über das Dashboard leitet Sie zum Azure Monitor für VMs weiter und lädt die Leistungsdetailansicht für den virtuellen Computer.  

![VM-Leistungsübersicht über die Ansicht der VM-Skalierungsgruppe](./media/vminsights-performance/vminsights-performance-directvmss-01.png)

>[!NOTE]
>Sie können eine detaillierte Leistungsansicht für eine bestimmte Instanz aus der Ansicht „Instanzen“ für Ihre Skalierungsgruppe aufrufen. Navigieren Sie im Abschnitt **Einstellungen** zu **Instanzen**, und wählen Sie dann **Insights** aus.



## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich darüber, wie Sie in Azure Monitor für VMs enthaltene [Arbeitsmappen](vminsights-workbooks.md) verwenden, um Leistungs- und Netzwerkmetriken eingehender zu analysieren.  

- Weitere Informationen zu ermittelten Anwendungsabhängigkeiten finden Sie unter [Anzeigen der Zuordnung in Azure Monitor](vminsights-maps.md).
