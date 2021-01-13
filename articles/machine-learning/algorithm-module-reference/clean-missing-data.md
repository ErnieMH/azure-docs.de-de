---
title: 'Clean Missing Data: Modulreferenz'
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie mit dem Modul Clean Missing Data in Azure Machine Learning fehlende Werte entfernen, ersetzen oder ableiten.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/11/2020
ms.openlocfilehash: 5b7943b2026d640ae7e5d119e165bd752ae2fe7f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90898820"
---
# <a name="clean-missing-data-module"></a>Modul „Clean Missing Data“

In diesem Artikel wird ein Modul im Azure Machine Learning-Designer beschrieben.

Mit diesem Modul können Sie fehlende Werte entfernen, ersetzen oder ableiten. 

Datenanalysten überprüfen Daten oft auf fehlende Werte und führen dann verschiedene Vorgänge zum Korrigieren der Daten oder Einfügen neuer Werte aus. Durch solche Bereinigungsvorgänge sollen Probleme aufgrund von fehlenden Daten verhindert werden, die beim Trainieren eines Modells auftreten können. 

Dieses Modul unterstützt mehrere Typen von Vorgängen zum „Bereinigen“ fehlender Werte, darunter:

+ Ersetzen von fehlenden Werten durch einen Platzhalter, einen Mittelwert oder einen anderen Wert
+ Vollständiges Entfernen von Zeilen und Spalten, in denen Werte fehlen
+ Ableiten von Werten basierend auf statistischen Methoden


Durch Verwendung dieses Moduls wird Ihr Quelldataset nicht geändert. Stattdessen erstellt es ein neues Dataset in Ihrem Arbeitsbereich, das Sie im nachfolgenden Workflow verwenden können. Sie können das neue, bereinigte Dataset auch zur Wiederverwendung speichern.

Dieses Modul gibt außerdem eine Definition der Transformation aus, die zum Bereinigen der fehlenden Werte verwendet wird. Sie können diese Transformation bei anderen Datasets mit demselben Schema wiederverwenden, indem Sie das Modul [Apply Transformation](./apply-transformation.md) (Transformation anwenden) einsetzen.  

## <a name="how-to-use-clean-missing-data"></a>Verwenden des Moduls „Clean Missing Data“

Mit diesem Modul können Sie einen Bereinigungsvorgang definieren. Sie können den Bereinigungsvorgang auch speichern, damit Sie ihn zu einem späteren Zeitpunkt auf neue Daten anwenden können. In den folgenden Abschnitten erfahren Sie, wie Sie einen Bereinigungsprozess erstellen und speichern können: 
 
+ [So ersetzen Sie fehlende Werte](#replace-missing-values)
  
+ [So wenden Sie eine Bereinigungstransformation auf neue Daten an](#apply-a-saved-cleaning-operation-to-new-data)
 
> [!IMPORTANT]
> Die Bereinigungsmethode, die Sie zur Behandlung fehlender Werte verwenden, kann sich auf Ihre Ergebnisse erheblich auswirken. Wir empfehlen, dass Sie mit verschiedenen Methoden experimentieren. Berücksichtigen Sie sowohl die Begründung zur Verwendung einer bestimmten Methode als auch die Qualität der Ergebnisse.

### <a name="replace-missing-values"></a>Ersetzen fehlender Werte  

Immer wenn Sie das Modul [Clean Missing Data](./clean-missing-data.md) auf eine Gruppe von Daten anwenden, wird derselbe Bereinigungsvorgang auf alle von Ihnen ausgewählten Spalten angewendet. Wenn Sie verschiedene Spalten mithilfe verschiedener Methoden bereinigen müssen, verwenden Sie deshalb separate Instanzen des Moduls.

1.  Fügen Sie Ihrer Pipeline das Modul [Clean Missing Data](./clean-missing-data.md) hinzu, und verbinden Sie das Dataset, in dem Werte fehlen.  
  
2.  Wählen Sie bei **Columns to be cleaned** (Zu bereinigende Spalten) die Spalten mit den fehlenden Werten aus, die Sie ändern möchten. Sie können mehrere Spalten auswählen, müssen aber dieselbe Ersetzungsmethode in allen ausgewählten Spalten verwenden. Daher müssen Sie Zeichenfolgenspalten und numerische Spalten in der Regel getrennt bereinigen.

    Um beispielsweise auf fehlende Werte in allen numerischen Spalten zu überprüfen, führen Sie die folgenden Schritte aus:

    1. Wählen Sie das Modul **Clean Missing Data** (Fehlende Daten bereinigen) aus, und klicken Sie im rechten Bereich des Moduls auf **Spalte bearbeiten**.

    3. Wählen Sie für **Include** (Einschließen) den **Column Type** (Spaltentyp) aus der Dropdownliste und anschließend **Numeric** (Numerisch) aus. 
  
    Jede von Ihnen ausgewählte Bereinigungs- oder Ersetzungsmethode muss auf **alle** Spalten in der Auswahl anwendbar sein. Wenn die Daten in einer Spalte mit dem angegebenen Vorgang inkompatibel sind, gibt das Modul einen Fehler zurück und beendet die Pipeline.
  
3.  Geben Sie für **Minimum missing value ratio** (Mindestverhältnis für fehlende Werte) die Mindestanzahl von fehlenden Werten an, die für den auszuführenden Vorgang erforderlich sind.  
  
    Sie verwenden diese Option in Kombination mit **Maximum missing value ratio** (Höchstverhältnis für fehlende Werte) zum Definieren der Bedingungen, unter denen ein Bereinigungsvorgang am Dataset ausgeführt wird. Wenn es zu viele oder zu wenige Zeilen gibt, in denen Werte fehlen, kann der Vorgang nicht ausgeführt werden. 
  
    Die eingegebene Anzahl stellt das **Verhältnis** von fehlenden Werten zu allen Werten in der Spalte dar. Für die Eigenschaft **Minimum missing value ratio** ist standardmäßig „0“ festgelegt. Dies bedeutet: Fehlende Werte werden selbst dann bereinigt, wenn nur ein einziger Wert fehlt. 

    > [!WARNING]
    > Diese Bedingung muss von jeder einzelnen Spalte erfüllt werden, damit der angegebene Vorgang angewendet werden kann. Angenommen beispielsweise, Sie haben drei Spalten ausgewählt und dann als Mindestverhältnis für fehlende Werte „0,2“ (20%) festgelegt, doch nur in einer einzigen Spalte fehlen tatsächlich 20% der Werte. In diesem Fall würde der Bereinigungsvorgang nur auf die Spalte angewendet, in der mehr als 20% der Werte fehlen. Die anderen Spalten würden unverändert beibehalten.
    > 
    > Wenn Sie unsicher sind, ob fehlende Werte geändert wurden, wählen Sie die Option **Generate missing value indicator column** (Indikatorspalte für fehlende Werte generieren) aus. Dann wird eine Spalte an das Dataset angefügt, um anzugeben, ob jede Spalte die festgelegten Kriterien für den minimalen und maximalen Bereich erfüllt hat oder nicht.  
  
4. Geben Sie für **Maximum missing value ratio** die maximale Anzahl von Werten an, die fehlen können, damit der Vorgang ausgeführt wird.   
  
    So möchten Sie eine Ersetzung fehlender Werte beispielsweise nur dann durchführen, wenn in 30% oder weniger der Zeilen Werte fehlen, möchten die Werte aber unverändert beibehalten, wenn in mehr als 30% der Zeilen Werte fehlen.  
  
    Sie definieren die Anzahl als Verhältnis der fehlenden Werte zu allen Werten in der Spalte. Standardmäßig ist für **Maximum missing value ratio** „1“ festgelegt. Dies bedeutet: Fehlende Werte werden selbst dann bereinigt, wenn 100% der Werte in der Spalte fehlen.  
  
   
  
5. Wählen Sie für **Cleaning Mode** (Bereinigungsmodus) eine der folgenden Optionen zum Ersetzen oder Entfernen fehlender Werte aus:  
  
  
    + **Custom substitution value** (Benutzerdefinierter Ersatzwert): Verwenden Sie diese Option zur Angabe eines Platzhalterwerts (z.B. „0“ oder „N/V“), der für alle fehlenden Werte gilt. Der Wert, den Sie als Ersatz angeben, muss mit dem Datentyp der Spalte kompatibel sein.
  
    + **Replace with mean** (Durch Mittelwert ersetzen): Berechnet den Mittelwert der Spalte und verwendet ihn als Ersatzwert für jeden fehlenden Wert in der Spalte.  
  
        Gilt nur für Spalten mit den Datentypen „Integer“, „Double“ oder „Boolean“.  
  
    + **Replace with median** (Durch Median ersetzen): Berechnet den Median der Spalte und verwendet ihn als Ersatz für jeden fehlenden Wert in der Spalte.  
  
        Gilt nur für Spalten mit den Datentypen „Integer“ oder „Double“. 
  
    + **Replace with mode** (Durch Modus ersetzen): Berechnet den Modus für die Spalte und verwendet ihn als Ersatzwert für jeden fehlenden Wert in der Spalte.  
  
        Gilt für Spalten mit den Datentypen „Integer“, „Double“, „Boolean“ oder „Categorical“. 
  
    + **Remove entire row** (Ganze Zeile entfernen): Entfernt jede Zeile im Dataset vollständig, in der mindestens ein Wert fehlt. Dies ist hilfreich, wenn der fehlende Wert als zufällig fehlend betrachtet werden kann.  
  
    + **Remove entire column** (Ganze Spalte entfernen): Entfernt jede Spalte im Dataset vollständig, in der mindestens ein Wert fehlt.  
  
    
  
6. Die Option **Replacement value** (Ersatzwert) steht zur Verfügung, wenn Sie die Option **Custom substitution value** (Benutzerdefinierter Ersatzwert) ausgewählt haben. Geben Sie den neuen Wert ein, der als Ersatzwert für alle fehlenden Werte in der Spalte verwendet werden soll.  
  
    Beachten Sie, dass Sie diese Option nur in Spalten mit den Datentypen „Integer“, „Double“, „Boolean“ oder „String“ verwenden können.
  
7. **Generate missing value indicator column** (Indikatorspalte für fehlende Werte generieren): Wählen Sie diese Option aus, wenn Sie einen Hinweis darauf ausgeben möchten, dass die Werte in der Spalte die Kriterien für eine Bereinigung von fehlenden Werten erfüllt haben. Diese Option ist besonders hilfreich, wenn Sie beim Einrichten eines neuen Bereinigungsvorgangs sicherstellen möchten, dass er wie vorgesehen funktioniert.
  
8. Übermitteln Sie die Pipeline.

### <a name="results"></a>Ergebnisse

Das Modul gibt zwei Ausgaben zurück:  

-   **Cleaned dataset** (Bereinigtes Dataset): Ein Dataset, das besteht aus den ausgewählten Spalten, in denen fehlende Werte wie angegeben behandelt werden, und einer Indikatorspalte, wenn Sie diese Option ausgewählt haben.  

    Spalten, die nicht zur Bereinigung ausgewählt wurden, werden auch „per Pass-Through übergeben“.  
  
-  **Cleaning transformation** (Bereinigungstransformation): Eine für die Bereinigung verwendete Datentransformation, die in Ihrem Arbeitsbereich gespeichert und zu einem späteren Zeitpunkt auf neue Daten angewendet werden kann.

### <a name="apply-a-saved-cleaning-operation-to-new-data"></a>Anwenden eines gespeicherten Bereinigungsvorgang auf neue Daten  

Wenn Sie Bereinigungsvorgänge oft wiederholen müssen, empfehlen wir, dass Sie Ihr „Rezept“ für die Datenbereinigung als eine *Transformation* speichern, um es bei demselben Dataset wiederverwenden zu können. Das Speichern einer Bereinigungstransformation ist besonders hilfreich, wenn Sie Daten mit demselben Schema häufig erneut importieren und dann bereinigen müssen.  
      
1.  Fügen Sie das Modul [Apply Transformation](./apply-transformation.md) (Transformation anwenden) Ihrer Pipeline hinzu.  
  
2.  Fügen Sie dann das zu bereinigende Dataset hinzu, und verbinden Sie es mit dem rechten Eingangsport.  
  
3.  Erweitern Sie die Gruppe **Transforms** im linken Bereich des Designers. Suchen Sie nach der gespeicherten Transformation, und ziehen Sie sie in die Pipeline.  

4.  Verbinden Sie die gespeicherte Transformation mit dem linken Eingangsport von [Apply Transformation](./apply-transformation.md). 

    Wenn Sie eine gespeicherte Transformation anwenden, können Sie die Spalten, auf die die Transformation angewendet wird, nicht auswählen. Der Grund: Die Transformation wurde bereits definiert und gilt automatisch für die Spalten, die im ursprünglichen Vorgang angegeben wurden.

    Allerdings: Nehmen Sie einmal an, Sie hätten eine Transformation für eine Teilmenge numerischer Spalten erstellt. Diese Transformation können Sie auf ein Dataset von gemischten Spaltentypen anwenden, ohne dass ein Fehler ausgelöst wird, weil die fehlenden Werte nur in den übereinstimmenden numerischen Spalten geändert werden.

6.  Übermitteln Sie die Pipeline.  

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich den [Satz der verfügbaren Module](module-reference.md) für Azure Machine Learning an. 