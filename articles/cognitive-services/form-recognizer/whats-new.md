---
title: Neuerungen in der Formularerkennung
titleSuffix: Azure Cognitive Services
description: Informieren Sie sich über die neuesten Änderungen an der Formularerkennungs-API.
author: laujan
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: conceptual
ms.date: 04/14/2021
ms.author: lajanuar
ms.openlocfilehash: e952d481daf53b1806dc3cfbb658c8c0c21f6984
ms.sourcegitcommit: db925ea0af071d2c81b7f0ae89464214f8167505
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/15/2021
ms.locfileid: "107516296"
---
<!-- markdownlint-disable MD024 -->
<!-- markdownlint-disable MD036 -->
# <a name="whats-new-in-form-recognizer"></a>Neuerungen in der Formularerkennung

Der Formularerkennungsdienst wird fortlaufend aktualisiert. In diesem Artikel finden Sie aktuelle Informationen zu Featureverbesserungen, Fixes und Dokumentationsaktualisierungen.

## <a name="april-2021"></a>April 2021
<!-- markdownlint-disable MD029 -->

### <a name="sdk-updates-api--version-21-preview3"></a>SDK-Updates (API-Version 2.1-preview.3)

### <a name="c-version-310-beta4"></a>**C#-Version 3.1.0-beta.4**

* **Neue Methoden zum Analysieren von Daten aus Identitätsdokumenten:**

   **[StartRecognizeIdDocumentsFromUriAsync](/dotnet/api/azure.ai.formrecognizer.formrecognizerclient.startrecognizeiddocumentsasync?view=azure-dotnet-preview&preserve-view=true)**

   **[StartRecognizeIdDocumentsAsync](/dotnet/api/azure.ai.formrecognizer.formrecognizerclient.startrecognizeiddocumentsasync?view=azure-dotnet-preview&preserve-view=true)**

   Eine Liste der Feldwerte finden Sie in der Dokumentation zur Formularerkennung unter [Extrahierte Felder](concept-identification-cards.md#fields-extracted).

* Die Gruppe der Dokumentsprachen, die für die Methode **[StartRecognizeContent](/dotnet/api/azure.ai.formrecognizer.formrecognizerclient.startrecognizecontent?view=azure-dotnet-preview&preserve-view=true)** angegeben werden können, wurde erweitert.

* **Neue Eigenschaft `Pages`, die von den folgenden Klassen unterstützt wird:**

   **[RecognizeBusinessCardsOptions](/dotnet/api/azure.ai.formrecognizer.recognizebusinesscardsoptions?view=azure-dotnet-preview&preserve-view=true)**</br>
   **[RecognizeCustomFormsOptions](/dotnet/api/azure.ai.formrecognizer.recognizecustomformsoptions?view=azure-dotnet-preview&preserve-view=true)**</br>
   **[RecognizeInvoicesOptions](/dotnet/api/azure.ai.formrecognizer.recognizeinvoicesoptions?view=azure-dotnet-preview&preserve-view=true)**</br>
   **[RecognizeReceiptsOptions](/dotnet/api/azure.ai.formrecognizer.recognizereceiptsoptions?view=azure-dotnet-preview&preserve-view=true)**</br>

   Mit der Eigenschaft `Pages` können Sie bei PDF- und TIFF-Dokumenten mit mehreren Seiten einzelne Seiten oder einen Seitenbereich auswählen. Geben Sie für einzelne Seiten die Seitenzahl ein (beispielsweise `3`). Geben Sie für einen Seitenbereich (beispielsweise Seite 2 und Seiten 5 bis 7) die Seitennummern und Bereiche in einem kommagetrennten Format ein: `2, 5-7`.    

* **Neue Eigenschaft `ReadingOrder`, die für die folgende Klasse unterstützt wird:**

   **[RecognizeContentOptions](/dotnet/api/azure.ai.formrecognizer.recognizecontentoptions?view=azure-dotnet-preview&preserve-view=true)**

  Die Eigenschaft `ReadingOrder` ist ein optionaler Parameter, mit dem Sie angeben können, welcher Algorithmus für die Lesereihenfolge (`basic` oder `natural`) angewendet werden soll, um die Extraktionsreihenfolge von Textelementen zu bestimmen. Wenn Sie hier nichts angeben, lautet der Standardwert `basic`.

#### <a name="breaking-changes"></a>Breaking Changes

* Der Client verwendet standardmäßig die neueste unterstützte Dienstversion (aktuell **2.1-preview.3**).

* Von der Methode **[StartRecognizeCustomForms](/dotnet/api/azure.ai.formrecognizer.formrecognizerclient.startrecognizecustomforms?view=azure-dotnet-preview&preserve-view=true#Azure_AI_FormRecognizer_FormRecognizerClient_StartRecognizeCustomForms_System_String_System_IO_Stream_Azure_AI_FormRecognizer_RecognizeCustomFormsOptions_System_Threading_CancellationToken_)** wird jetzt eine Ausnahme vom Typ `RequestFailedException()` ausgelöst, wenn eine ungültige Datei übergeben wird.

### <a name="java-version-310-beta3"></a>**Java-Version 3.1.0-beta.3**

* **Neue Methoden zum Analysieren von Daten aus Identitätsdokumenten:**

  **[beginRecognizeIdDocumentsFromUrl](/java/api/com.azure.ai.formrecognizer.formrecognizerclient.beginrecognizeiddocumentsfromurl?view=azure-java-preview&preserve-view=true)**

  **[beginRecognizeIdDocuments](/java/api/com.azure.ai.formrecognizer.formrecognizerclient.beginrecognizeiddocuments?view=azure-java-preview&preserve-view=true)**

   Eine Liste der Feldwerte finden Sie in der Dokumentation zur Formularerkennung unter _Extrahierte Felder_.

* **Unterstützung von Bitmapbilddateien (.bmp) für benutzerdefinierte Formulare und Trainingsmethoden in der Enumeration `FormContentType`:**

* `image/bmp`

* **Neue Eigenschaft `Pages`, die von den folgenden Klassen unterstützt wird:**

   **[RecognizeBusinessCardsOptions](/java/api/com.azure.ai.formrecognizer.models.recognizebusinesscardsoptions?view=azure-java-preview&preserve-view=true)**</br>
   **[RecognizeCustomFormOptions](/java/api/com.azure.ai.formrecognizer.models.recognizecustomformsoptions?view=azure-java-preview&preserve-view=true)**</br>
   **[RecognizeInvoicesOptions](/java/api/com.azure.ai.formrecognizer.models.recognizeinvoicesoptions?view=azure-java-preview&preserve-view=true)**</br>
   **[RecognizeReceiptsOptions](/java/api/com.azure.ai.formrecognizer.models.recognizereceiptsoptions?view=azure-java-preview&preserve-view=true)**</br>

  Mit der Eigenschaft `Pages` können Sie bei PDF- und TIFF-Dokumenten mit mehreren Seiten einzelne Seiten oder einen Seitenbereich auswählen. Geben Sie für einzelne Seiten die Seitenzahl ein (beispielsweise `3`). Geben Sie für einen Seitenbereich (beispielsweise Seite 2 und Seiten 5 bis 7) die Seitennummern und Bereiche in einem kommagetrennten Format ein: `2, 5-7`.

* **Unterstützung von Bitmapbilddateien (.bmp) für benutzerdefinierte Formulare und Trainingsmethoden in den Feldern vom Typ [FormContentType](/java/api/com.azure.ai.formrecognizer.models.formcontenttype?view=azure-java-preview&preserve-view=true#fields):**

  `image/bmp`

* **Neues Schlüsselwortargument `ReadingOrder`, das für die folgenden Methoden unterstützt wird:**

* **[beginRecognizeContent](https://docs.microsoft.com/java/api/com.azure.ai.formrecognizer.formrecognizerclient.beginrecognizecontent?view=azure-java-preview&preserve-view=true)**</br>
**[beginRecognizeContentFromUrl](/java/api/com.azure.ai.formrecognizer.formrecognizerclient.beginrecognizecontentfromurl?view=azure-java-preview&preserve-view=true)**</br>

   Das Schlüsselwortargument `ReadingOrder` ist ein optionaler Parameter, mit dem Sie angeben können, welcher Algorithmus für die Lesereihenfolge (`basic` oder `natural`) angewendet werden soll, um die Extraktionsreihenfolge von Textelementen zu bestimmen. Wenn Sie hier nichts angeben, lautet der Standardwert `basic`.

* Der Client verwendet standardmäßig die neueste unterstützte Dienstversion (aktuell **2.1-preview.3**).

### <a name="javascript-version-310-beta3"></a>**JavaScript-Version 3.1.0-beta.3**

* **Neue Methoden zum Analysieren von Daten aus Identitätsdokumenten:**

    **[beginRecognizeIdDocumentsFromUrl](/javascript/api/@azure/ai-form-recognizer/formrecognizerclient?view=azure-node-preview&preserve-view=true&branch=main#beginRecognizeIdDocumentsFromUrl_string__BeginRecognizeIdDocumentsOptions_)**

    **[beginRecognizeIdDocuments](/javascript/api/@azure/ai-form-recognizer/formrecognizerclient?view=azure-node-preview&preserve-view=true&branch=main#beginRecognizeIdDocuments_FormRecognizerRequestBody__BeginRecognizeIdDocumentsOptions_)**

    Eine Liste der Feldwerte finden Sie in der Dokumentation zur Formularerkennung unter _Extrahierte Felder_.

* **Neue Feldwerte für die FieldValue-Schnittstelle:**

    `gender`: Mögliche Werte sind `M`, `F` oder `X`.</br>
   `country`: Mögliche Werte sind dreistellige Ländercodezeichenfolgen gemäß [ISO Alpha-3](https://www.iso.org/obp/ui/#search).

* **Neue Option `pages`, die von allen Formularerkennungsmethoden unterstützt wird (benutzerdefinierte Formulare und alle vordefinierten Modelle). Mit dem Argument können Sie bei PDF- und TIFF-Dokumenten mit mehreren Seiten einzelne Seiten oder einen Seitenbereich auswählen. Geben Sie für einzelne Seiten die Seitenzahl ein (beispielsweise `3`). Geben Sie für einen Seitenbereich (beispielsweise Seite 2 und Seiten 5 bis 7) die Seitennummern und Bereiche in einem kommagetrennten Format ein: `2, 5-7`.

* Den Inhaltserkennungsmethoden wurde die Unterstützung des Typs **[ReadingOrder](/javascript/api/@azure/ai-form-recognizer/readingorder?view=azure-node-preview&preserve-view=true)** hinzugefügt. Mithilfe dieser Option können Sie den Algorithmus steuern, mit dem der Dienst bestimmt, wie erkannte Textzeilen sortiert werden sollen. Sie können angeben, welcher Algorithmus für die Lesereihenfolge (`basic` oder `natural`) angewendet werden soll, um die Extraktionsreihenfolge von Textelementen zu bestimmen. Wenn Sie hier nichts angeben, lautet der Standardwert `basic`.

* Der Typ **[FormField](/javascript/api/@azure/ai-form-recognizer/formfield?view=azure-node-preview&preserve-view=true)** wurde auf mehrere verschiedene Schnittstellen aufgeteilt. Dies sollte nur in bestimmten Grenzfällen (undefinierter Werttyp) zu API-Kompatibilitätsproblemen führen.

* Für alle REST-API-Aufrufe wurde zur Version **2.1-preview.3** des Formularerkennungsdiensts migriert.

### <a name="python-version--310b4"></a>**Python-Version 3.1.0b4**

* **Neue Methoden zum Analysieren von Daten aus Identitätsdokumenten:**

  **[begin_recognize_id_documents_from_url](/python/api/azure-ai-formrecognizer/azure.ai.formrecognizer.formrecognizerclient?view=azure-python&preserve-view=true)**

  **[begin_recognize_id_documents](/python/api/azure-ai-formrecognizer/azure.ai.formrecognizer.formrecognizerclient?view=azure-python&preserve-view=true)**

  Eine Liste der Feldwerte finden Sie in der Dokumentation zur Formularerkennung unter _Extrahierte Felder_.

* **Neue Feldwerte für die Enumeration [FieldValueType](/python/api/azure-ai-formrecognizer/azure.ai.formrecognizer.fieldvaluetype?view=azure-python-preview&preserve-view=true):**

   gender: Mögliche Werte sind `M`, `F` oder `X`.

  country: Mögliche Werte sind [Ländercodes gemäß ISO Alpha-3](https://www.iso.org/obp/ui/#search).

* **Unterstützung von Bitmapbilddateien (.bmp) für benutzerdefinierte Formulare und Trainingsmethoden in der Enumeration [FormContentType](/python/api/azure-ai-formrecognizer/azure.ai.formrecognizer.formcontenttype?view=azure-python-preview&preserve-view=true):**

    image/bmp

* **Neues Schlüsselwortargument `pages`, das von folgenden Methoden unterstützt wird:**

    **[begin_recognize_receipts](/python/api/azure-ai-formrecognizer/azure.ai.formrecognizer.formrecognizerclient?view=azure-python-preview&preserve-view=true&branch=main#begin-recognize-receipts-receipt----kwargs-)**

    **[begin_recognize_receipts_from_url](/python/api/azure-ai-formrecognizer/azure.ai.formrecognizer.formrecognizerclient?view=azure-python-preview&preserve-view=true#begin-recognize-receipts-from-url-receipt-url----kwargs-)**

   **[begin_recognize_business_cards](/python/api/azure-ai-formrecognizer/azure.ai.formrecognizer.formrecognizerclient?view=azure-python-preview&preserve-view=true#begin-recognize-business-cards-business-card----kwargs-)**

   **[begin_recognize_business_cards_from_url](/python/api/azure-ai-formrecognizer/azure.ai.formrecognizer.formrecognizerclient?view=azure-python-preview&preserve-view=true#begin-recognize-business-cards-from-url-business-card-url----kwargs-)**

   **[begin_recognize_invoices](/python/api/azure-ai-formrecognizer/azure.ai.formrecognizer.formrecognizerclient?view=azure-python-preview&preserve-view=true#begin-recognize-invoices-invoice----kwargs-)**

   **[begin_recognize_invoices_from_url](/python/api/azure-ai-formrecognizer/azure.ai.formrecognizer.formrecognizerclient?view=azure-python-preview&preserve-view=true#begin-recognize-invoices-from-url-invoice-url----kwargs-)**

   **[begin_recognize_content](/python/api/azure-ai-formrecognizer/azure.ai.formrecognizer.formrecognizerclient?view=azure-python-preview&preserve-view=true#begin-recognize-content-form----kwargs-)**

  **[begin_recognize_content_from_url](/python/api/azure-ai-formrecognizer/azure.ai.formrecognizer.formrecognizerclient?view=azure-python-preview&preserve-view=true#begin-recognize-content-from-url-form-url----kwargs-)**

   Mit dem Schlüsselwortargument `pages` können Sie bei PDF- und TIFF-Dokumenten mit mehreren Seiten einzelne Seiten oder einen Seitenbereich auswählen. Geben Sie für einzelne Seiten die Seitenzahl ein (beispielsweise `3`). Geben Sie für einen Seitenbereich (beispielsweise Seite 2 und Seiten 5 bis 7) die Seitennummern und Bereiche in einem kommagetrennten Format ein: `2, 5-7`.

* **Neues Schlüsselwortargument `readingOrder`, das für die folgenden Methoden unterstützt wird:**

   **[begin_recognize_content](/python/api/azure-ai-formrecognizer/azure.ai.formrecognizer.formrecognizerclient?view=azure-python-preview&preserve-view=true#begin-recognize-content-form----kwargs-)**

   **[begin_recognize_content_from_url](/python/api/azure-ai-formrecognizer/azure.ai.formrecognizer.formrecognizerclient?view=azure-python-preview&preserve-view=true#begin-recognize-content-from-url-form-url----kwargs-)**

   Das Schlüsselwortargument `readingOrder` ist ein optionaler Parameter, mit dem Sie angeben können, welcher Algorithmus für die Lesereihenfolge (`basic` oder `natural`) angewendet werden soll, um die Extraktionsreihenfolge von Textelementen zu bestimmen. Wenn Sie hier nichts angeben, lautet der Standardwert `basic`.

## <a name="march-2021"></a>März 2021

**Die öffentliche Vorschauversion 3 der Formularerkennung v2.1 ist jetzt verfügbar.** v2.1-preview.3 wurde veröffentlicht und enthält die folgenden Funktionen:

* **Neues vordefiniertes ID-Modell:** Mit dem neuen vordefinierten ID-Modell können Kunden Ausweispapiere erfassen und strukturierte Daten zurückgeben, um die Verarbeitung zu automatisieren. Dabei werden unsere leistungsstarken Funktionen der optischen Zeichenerkennung (Optical Character Recognition, OCR) mit ID-Erkennungsmodellen kombiniert, mit denen wesentliche Informationen aus Reisepässen und US-Führerscheinen extrahiert werden, z. B. Name, Geburtsdatum, Ausstellungsdatum, Ablaufdatum usw.

  [Weitere Informationen zum vordefinierten ID-Modell](concept-identification-cards.md)

   :::image type="content" source="./media/id-canada-passport-example.png" alt-text="Beispiel eines Reisepasses" lightbox="./media/id-canada-passport-example.png":::

* **Extraktion von Einzelposten im vordefinierten Rechnungsmodell:** Im vordefinierten Rechnungsmodell wird jetzt die Extraktion von Einzelposten unterstützt. Nun können vollständige Posten und deren Bestandteile extrahiert werden, z. B. Beschreibung, Betrag, Menge, Produkt-ID, Datum usw. Mit einem einfachen API-/SDK-Aufruf können Sie nützliche Daten aus Rechnungen extrahieren, z. B. Text, Tabellen, Schlüssel-Wert-Paare und Einzelposten.

   [Weitere Informationen zum vordefinierten Rechnungsmodell](concept-invoices.md)

* **Überwachtes Beschriften und Trainieren von Tabellen, Beschriftung leerer Werte:** Zusätzlich zu den [hochmodernen Deep Learning-Funktionen der Formularerkennung zur automatischen Tabellenextraktion](https://techcommunity.microsoft.com/t5/azure-ai/enhanced-table-extraction-from-documents-with-form-recognizer/ba-p/2058011) können Kunden nun auch Tabellen beschriften und trainieren. Dieses neue Release bietet die Möglichkeit, Einzelposten und Tabellen (dynamisch und fest) zu beschriften und zu trainieren sowie ein benutzerdefiniertes Modell zum Extrahieren von Schlüssel-Wert-Paaren und Einzelposten zu trainieren. Nach dem Trainieren eines Modells werden mit dem Modell Einzelposten als Teil der JSON-Ausgabe im Abschnitt „documentResults“ extrahiert.

    :::image type="content" source="./media/table-labeling.png" alt-text="Tabellenbeschriftung" lightbox="./media/table-labeling.png":::

    Neben Tabellen können nun auch leere Werte beschriftet werden. Wenn Dokumente im Trainingssatz keine Werte für bestimmte Felder enthalten, können Sie sie beschriften, damit Werte ordnungsgemäß aus analysierten Dokumenten extrahiert werden können.

* **Unterstützung für 66 neue Sprachen:** Die Layout-API der Formularerkennung und benutzerdefinierte Modelle unterstützen jetzt 73 Sprachen.

  [Weitere Informationen zur Sprachunterstützung für die Formularerkennung](language-support.md)

* **Natürliche Leserichtung, Handschriftklassifizierung und Seitenauswahl:** Mit diesem Update können Sie festlegen, dass Textzeilen in der natürlichen Leserichtung ausgegeben werden anstatt in der standardmäßigen Leserichtung von links nach rechts und von oben nach unten. Dazu verwenden Sie den neuen Abfrageparameter „readingOrder“ und legen ihn für eine benutzerfreundlichere Ausgabe der Leserichtung auf den Wert „natural“ fest. Für lateinische Sprachen klassifiziert die Formularerkennung außerdem Textzeilen als handschriftlich oder nicht handschriftlich und gibt eine Konfidenzbewertung an.

* **Qualitätsverbesserungen beim vordefinierten Belegmodell:** Dieses Update enthält zahlreiche Qualitätsverbesserungen für das vordefinierte Belegmodell, insbesondere rund um die Extraktion von Einzelposten.

## <a name="november-2020"></a>November 2020

### <a name="new-features"></a>Neue Funktionen

**Die öffentliche Vorschauversion 2 der Formularerkennung v2.1 ist jetzt verfügbar.** v2.1-preview.2 wurde veröffentlicht und enthält die folgenden Funktionen:

- **Neues vordefiniertes Rechnungsmodell:** Mit dem neuen vordefinierten Rechnungsmodell können Kunden Rechnungen in verschiedenen Formaten erstellen und strukturierte Daten zurückgeben, um die Rechnungsverarbeitung zu automatisieren. Es kombiniert unsere leistungsstarken Funktionen zur optischen Zeichenerkennung (Optical Character Recognition, OCR) mit Deep Learning-Modellen zum Rechnungsverständnis, um wichtige Informationen aus Rechnungen in englischer Sprache zu extrahieren. Es extrahiert wichtige Angaben, Tabellen und Informationen wie Kunde, Anbieter, Rechnungs-ID, Fälligkeitsdatum für die Rechnung, Summe, fälliger Betrag, Steuerbetrag, Lieferadresse und Rechnungsadresse.

  > [Weitere Informationen zum vordefinierten Rechnungsmodell](concept-invoices.md)

  :::image type="content" source="./media/invoice-example.jpg" alt-text="Beispiel für Rechnung" lightbox="./media/invoice-example.jpg":::

- **Erweiterte Tabellenextraktion** – Die Formularerkennung bietet jetzt eine verbesserte Tabellenextraktion, die unsere leistungsstarken OCR-Funktionen (Optical Character Recognition, Optische Zeichenerkennung) mit einem Deep Learning-Modell für Tabellenextraktion kombiniert. Die Formularerkennung kann Daten aus Tabellen extrahieren, einschließlich komplexer Tabellen mit zusammengeführten Spalten, Zeilen, ohne Rahmen und mehr.

  :::image type="content" source="./media/tables-example.jpg" alt-text="Beispiel für Tabellen" lightbox="./media/tables-example.jpg":::


  > [Weitere Informationen zur Layoutextraktion](concept-layout.md)

- **Update der Clientbibliothek:** Die aktuelle Version der [Clientbibliotheken](quickstarts/client-library.md) für .NET, Python, Java und JavaScript unterstützt die Formularerkennung 2.1-API.
- **Neue unterstützte Sprache: Japanisch** – Die folgenden neuen Sprachen werden jetzt unterstützt: für `AnalyzeLayout` und `AnalyzeCustomForm`: Japanisch (`ja`). [Sprachunterstützung](language-support.md)
- **Textzeilenstilanzeige (handschriftlich/anders) (nur lateinische Sprachen)**  – Die Formularerkennung gibt jetzt ein `appearance`-Objekt aus, das – zusammen mit einer Zuverlässigkeitsbewertung – klassifiziert, ob jede Textzeile von Hand geschrieben wurde oder nicht. Dieses Feature wird nur für lateinische Sprachen unterstützt.
- **Qualitätsverbesserungen** – Extraktionsverbesserungen, einschließlich Verbesserungen der einstelligen Extraktion.
- **Neues Feature „try-it-out“ im Tool für die Beschriftung von Beispielen für die Formularerkennung** – Möglichkeit zum Ausprobieren von vordefinierten Modellen für Rechnungen, Belege und Visitenkarten sowie der Layout-API mithilfe dieses Tools. Sehen Sie sich an, wie Ihre Daten extrahiert werden, ohne Code schreiben zu müssen.

  > [Ausprobieren des Beispieltools für die Formularerkennung](https://fott-preview.azurewebsites.net/)

  ![Beispiel für FOTT](./media/ui-preview.jpg)

- **Feedbackschleife:** Wenn Sie Dateien über das Tool für die Beschriftung von Beispielen analysieren, können Sie es jetzt auch dem Trainingssatz hinzufügen und die Bezeichnungen bei Bedarf anpassen und trainieren, um das Modell zu verbessern.
- **Automatisches Bezeichnen von Dokumenten:** Bezeichnet zusätzliche Dokumente automatisch basierend auf zuvor bezeichneten Dokumenten im Projekt

## <a name="august-2020"></a>August 2020

### <a name="new-features"></a>Neue Funktionen

**Die öffentliche Vorschau der Formularerkennung v2.1 ist jetzt verfügbar.** V2.1-preview.1 wurde veröffentlicht, einschließlich der folgenden Features:


- **REST-API-Referenz ist verfügbar**: [v2.1-preview.1-Referenz](https://westcentralus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1-preview-1/operations/AnalyzeBusinessCardAsync) anzeigen
- **Zusätzlich zu Englisch werden neue Sprachen unterstützt.** Die folgenden [Sprachen](language-support.md) werden jetzt unterstützt: für `Layout` und `Train Custom Model`: Englisch (`en`), Chinesisch (vereinfacht) (`zh-Hans`), Niederländisch (`nl`), Französisch (`fr`), Deutsch (`de`), Italienisch (`it`), Portugiesisch (`pt`) und Spanisch (`es`).
- **Kontrollkästchen-/Auswahlmarkierungserkennung**: Die Formularerkennung unterstützt das Erkennen und Extrahieren von Auswahlmarkierungen, z. B. von Kontrollkästchen und Optionsfeldern. Auswahlmarkierungen werden in `Layout` extrahiert, und Sie können jetzt auch `Train Custom Model` - _mit Bezeichnungen trainieren_, um Schlüssel-Wert-Paare für Auswahlmarkierungen zu extrahieren.
- **Model Compose** – Ermöglicht das Zusammensetzen und Aufrufen mehrerer Modelle mit einer einzigen Modell-ID. Wenn Sie ein Dokument zur Analyse mit einer zusammengesetzten Modell-ID übermitteln, wird zunächst ein Klassifizierungsschritt durchgeführt, um es an das richtige benutzerdefinierte Modell weiterzuleiten. Model Compose steht für das `Train Custom Model` - _Trainieren mit Bezeichnungen_ zur Verfügung.
- **Modellname** – Fügen Sie Ihren benutzerdefinierten Modellen einen Anzeigenamen zur einfacheren Verwaltung und Nachverfolgung hinzu.
- **[Neues vordefiniertes Modell für Visitenkarten](concept-business-cards.md)** zum Extrahieren allgemeiner Felder auf Visitenkarten in englischer Sprache.
- **[Neue Gebietsschemas für vordefinierte Belege](concept-receipts.md)** : Neben en-US ist jetzt zusätzliche Unterstützung für en-AU, en-CA, en-GB und en-IN verfügbar.
- **Qualitätsverbesserungen** für `Layout`, `Train Custom Model` - _Trainieren ohne Bezeichnungen_ und _Trainieren mit Bezeichnungen_.

**v2.0** enthält das folgende Update:

- Die [Clientbibliotheken](quickstarts/client-library.md) für .NET, Python, Java und JavaScript sind nun allgemein verfügbar.

**Neue Beispiele** sind auf GitHub verfügbar.

- Unter [Knowledge Extraction Recipes - Forms Playbook](https://github.com/microsoft/knowledge-extraction-recipes-forms) werden bewährte Methoden aus echten Kundenengagements mit der Formularerkennung gesammelt und nützliche Codebeispiele, Checklisten und Beispielpipelines für die Entwicklung dieser Projekte bereitgestellt.
- Das [Samplebezeichnungstool](https://github.com/microsoft/OCR-Form-Tools) wurde aktualisiert, um die neue Funktionalität von v2.1 zu unterstützen. Informationen zu den ersten Schritten mit dem Tool finden Sie in diesem [Schnellstart](quickstarts/label-tool.md).
- Im Formularerkennungsbeispiel [Intelligent Kiosk](https://github.com/microsoft/Cognitive-Samples-IntelligentKiosk/blob/master/Documentation/FormRecognizer.md) wird gezeigt, wie `Analyze Receipt` und `Train Custom Model` - _Trainieren ohne Bezeichnungen_ integriert werden.

## <a name="july-2020"></a>Juli 2020

### <a name="new-features"></a>Neue Funktionen
<!-- markdownlint-disable MD004 -->
* **v2.0-Referenz verfügbar** – Mehr dazu finden Sie in der [v2.0 API-Referenz](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/AnalyzeWithCustomForm) sowie in den aktualisierten SDKs für [.NET](/dotnet/api/overview/azure/ai.formrecognizer-readme), [Python](/python/api/overview/azure/), [Java](/java/api/overview/azure/ai-formrecognizer-readme) und [JavaScript](/javascript/api/overview/azure/).
* **Tabellenerweiterungen und Extraktionserweiterungen** – Umfasst Verbesserungen der Genauigkeit und der Tabellenextraktion, insbesondere eine Funktion zum Erlernen von Tabellenheadern und -strukturen beim _benutzerdefinierten Trainieren ohne Bezeichnungen_.

* **Währungsunterstützung** – Erkennung und Extraktion von globalen Währungssymbolen.
* **Azure Gov** – Die Formularerkennung steht jetzt auch in Azure Gov zur Verfügung.
* **Erweiterte Sicherheitsfeatures**:
  * **Bring Your Own Key (BYOK)**  – Die Formularerkennung verschlüsselt Ihre Daten automatisch, wenn sie in der Cloud persistent gespeichert werden, um sie zu schützen und Sie bei der Einhaltung der Sicherheits- und Complianceverpflichtungen Ihrer Organisation zu unterstützen. Standardmäßig verwendet Ihr Abonnement von Microsoft verwaltete Verschlüsselungsschlüssel. Sie können Ihr Abonnement jetzt auch mit eigenen Verschlüsselungsschlüsseln verwalten. [Kundenseitig verwaltete Schlüssel, auch bezeichnet als „Bring Your Own Key“ (BYOK),](./encrypt-data-at-rest.md) bieten eine größere Flexibilität beim Erstellen, Rotieren, Deaktivieren und Widerrufen von Zugriffssteuerungen. Außerdem können Sie die zum Schutz Ihrer Daten verwendeten Verschlüsselungsschlüssel überwachen.
  * **Private Endpunkte:** Ermöglichen in einem virtuellen Netzwerk den [sicheren Zugriff auf Daten über eine private Verbindung](../../private-link/private-link-overview.md).

## <a name="june-2020"></a>Juni 2020

### <a name="new-features"></a>Neue Funktionen

* **CopyModel-API wurde zu Client-SDKs hinzugefügt** – Sie können jetzt Modelle mithilfe der Client-SDKs aus einem Abonnement in ein anderes kopieren. Allgemeine Informationen zu diesem Feature finden Sie unter [Sichern und Wiederherstellen von Modellen](./disaster-recovery.md).
* **Azure Active Directory-Integration** – Sie können jetzt Clientobjekte der Formularerkennung mithilfe Ihrer Azure AD-Anmeldeinformationen in den SDKs authentifizieren.
* **SDK-spezifische Änderungen** – Hierzu zählen sowohl geringfügige Featureergänzungen als auch Breaking Changes. Weitere Informationen finden Sie in den SDK-Änderungsprotokollen.
  * [Änderungsprotokoll zu C# SDK Preview 3](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/formrecognizer/Azure.AI.FormRecognizer/CHANGELOG.md)
  * [Änderungsprotokoll zu Python SDK Preview 3](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/formrecognizer/azure-ai-formrecognizer/CHANGELOG.md)
  * [Änderungsprotokoll zu Java SDK Preview 3](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/formrecognizer/azure-ai-formrecognizer/CHANGELOG.md)
  * [Änderungsprotokoll zu JavaScript SDK Preview 3](https://github.com/Azure/azure-sdk-for-js/blob/%40azure/ai-form-recognizer_1.0.0-preview.3/sdk/formrecognizer/ai-form-recognizer/CHANGELOG.md)

## <a name="april-2020"></a>April 2020

### <a name="new-features"></a>Neue Funktionen

* **SDK-Unterstützung für Version 2.0 der Formularerkennungs-API (Public Preview):**  – In diesem Monat haben wir unsere Dienstunterstützung um ein Vorschau-SDK für Version 2.0 der Formularerkennung (Vorschauversion) erweitert. Verwenden Sie die folgenden Links, um die ersten Schritte mit Ihrer bevorzugten Sprache auszuführen:
  * [.NET SDK](/dotnet/api/overview/azure/ai.formrecognizer-readme)
  * [Java SDK](/java/api/overview/azure/ai-formrecognizer-readme)
  * [Python SDK](/python/api/overview/azure/ai-formrecognizer-readme)
  * [JavaScript SDK](/javascript/api/overview/azure/ai-form-recognizer-readme)

  Das neue SDK unterstützt alle Features von Version 2.0 der REST-API für die Formularerkennung. Sie können beispielsweise ein Modell mit oder ohne Bezeichnungen trainieren und Text, Schlüssel-Wert-Paare und Tabellen aus Ihren Formularen extrahieren, mit dem vorgefertigten Belegdienst Daten aus Belegen extrahieren sowie mit dem Layoutdienst Text und Tabellen aus Ihren Dokumenten extrahieren. Sie können Ihr Feedback zu den SDKs über das [SDK-Feedbackformular](https://aka.ms/FR_SDK_v1_feedback) teilen.

* **Kopieren eines benutzerdefinierten Modells:** Mithilfe der neuen Funktion zum Kopieren benutzerdefinierter Modelle können Sie nun Modelle zwischen Regionen und Abonnements kopieren. Vor dem Aufrufen der API zum Kopieren eines benutzerdefinierten Modells müssen Sie zunächst die Autorisierung zum Kopieren in die Zielressource erhalten, indem Sie den Vorgang für die Kopierautorisierung für den Zielressourcenendpunkt aufrufen.

  * REST-API zum [Generieren einer Kopierautorisierung](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/CopyCustomFormModelAuthorization)
  * REST-API zum [Kopieren eines benutzerdefinierten Modells](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/CopyCustomFormModel)

### <a name="security-improvements"></a>Verbesserungen der Sicherheit

* Kundenseitig verwaltete Schlüssel stehen jetzt für die Formularerkennung zur Verfügung. Weitere Informationen finden Sie unter [Verschlüsselung für ruhende Daten der Formularerkennung](./encrypt-data-at-rest.md).
* Verwenden Sie verwaltete Identitäten für den Zugriff auf Azure-Ressourcen mit Azure Active Directory. Weitere Informationen finden Sie unter [Autorisieren des Zugriffs auf verwaltete Identitäten](../authentication.md#authorize-access-to-managed-identities).

## <a name="march-2020"></a>März 2020

### <a name="new-features"></a>Neue Funktionen

* **Werttypen für die Beschriftung** Sie können nun die Typen der Werte angeben, die Sie mit dem Tool „Formularerkennung“ zur Beschriftung von Beispielen beschriften können. Derzeit werden die folgenden Werttypen und Variationen unterstützt:
  * `string`
    * Standardwert, `no-whitespaces`, `alphanumeric`
  * `number`
    * Standardwert, `currency`
  * `date`
    * Standardwert, `dmy`, `mdy`, `ymd`
  * `time`
  * `integer`

  Weitere Informationen zur Verwendung dieses Features finden Sie in der Anleitung zum [Tool zum Beschriften von Beispielen](./quickstarts/label-tool.md#specify-tag-value-types).


* **Visualisierung von Tabellen** Im Tool für die Beschriftung von Beispielen werden nun im Dokument erkannte Tabellen angezeigt. Mithilfe dieses Features können Sie die im Dokument erkannten und aus dem Dokument extrahierten Tabellen anzeigen, bevor Sie die Beschriftung und Analyse durchführen. Dieses Feature kann mithilfe der Ebenenoption aktiviert bzw. deaktiviert werden.

  Die folgende Abbildung ist ein Beispiel für die Erkennung und Extraktion von Tabellen:

  > [!div class="mx-imgBorder"]
  > ![Tabellenvisualisierung mit dem Tool für die Beschriftung von Beispielen](./media/whats-new/table-viz.png)

    Die extrahierten Tabellen stehen in der JSON-Ausgabe unter `"pageResults"` zur Verfügung.

  > [!IMPORTANT]
  > Das Beschriften von Tabellen wird nicht unterstützt. Nicht automatisch erkannte und extrahierte Tabellen können nur als Schlüssel-Wert-Paare beschriftet werden. Beim Beschriften von Tabellen als Schlüssel-Wert-Paare muss jede Zelle als eindeutiger Wert beschriftet werden.

### <a name="extraction-enhancements"></a>Extraktionsverbesserungen

Dieses Release bietet Verbesserungen bei der Extraktion sowie eine verbesserte Genauigkeit. Hierzu zählt insbesondere die Möglichkeit, mehrere Schlüssel-Wert-Paare in der gleichen Textzeile zu beschriften und zu extrahieren.

### <a name="sample-labeling-tool-is-now-open-source"></a>Tool für die Beschriftung von Beispielen jetzt als Open-Source-Projekt verfügbar

Das Tool „Formularerkennung“ für die Beschriftung von Beispielen ist jetzt als Open-Source-Projekt verfügbar. Sie können es in Ihre Lösungen integrieren und kundenspezifische Änderungen vornehmen, um es an Ihre individuellen Anforderungen anzupassen.

Weitere Informationen zum Tool „Formularerkennung“ für die Beschriftung von Beispielen finden Sie in der Dokumentation auf [GitHub](https://github.com/microsoft/OCR-Form-Tools/blob/master/README.md).

### <a name="tls-12-enforcement"></a>Erzwingen von TLS 1.2

TLS 1.2 wird nun für alle HTTP-Anforderungen erzwungen, die an diesen Dienst gerichtet werden. Weitere Informationen finden Sie unter [Sicherheit von Azure Cognitive Services](../cognitive-services-security.md).

## <a name="january-2020"></a>Januar 2020

In diesem Release wird die Formularerkennung 2.0 (Vorschauversion) eingeführt. In den folgenden Abschnitten finden Sie weitere Informationen zu neuen Features, Verbesserungen und Änderungen.

### <a name="new-features"></a>Neue Funktionen

* **Benutzerdefiniertes Modell**
  * **Trainieren mit Bezeichnungen** Sie können jetzt ein benutzerdefiniertes Modell mit manuell gekennzeichneten Daten trainieren. Diese Methode führt zu Modellen mit besserer Leistung und kann Modelle hervorbringen, die mit komplexen Formularen oder Formularen arbeiten, die Werte ohne Schlüssel enthalten.
  * **Asynchrone API** Sie können asynchrone API-Aufrufe verwenden, um mit großen Datasets und Dateien zu trainieren und diese zu analysieren.
  * **Unterstützung von TIFF-Dateien** Sie können jetzt mit TIFF-Dokumenten trainieren und Daten aus ihnen extrahieren.
  * **Verbesserte Extrahierungsgenauigkeit**

* **Vordefiniertes Belegmodell**
  * **Trinkgeldbeträge** Sie können jetzt Trinkgeldbeträge und andere handschriftliche Werte extrahieren.
  * **Extraktion von Zeilenelementen** Sie können Werte von Zeilenelementen aus Belegen extrahieren.
  * **Zuverlässigkeitswerte** Sie können die Zuverlässigkeit des Modells für jeden extrahierten Wert anzeigen.
  * **Verbesserte Extrahierungsgenauigkeit**

* **Layoutextraktion** Sie können nun mit der Layout-API Textdaten und Tabellendaten aus Ihren Formularen extrahieren.

### <a name="custom-model-api-changes"></a>Änderungen an der benutzerdefinierten Modell-API

Alle APIs für das Trainieren und Verwenden benutzerdefinierter Modelle wurden umbenannt, und einige synchrone Methoden sind jetzt asynchron. Dies sind die wichtigsten Änderungen:

* Der Prozess zum Trainieren eines Modells ist jetzt asynchron. Sie initiieren das Training über den API-Aufruf **/custom/models**. Dieser Aufruf gibt eine Vorgangs-ID zurück, die Sie in **custom/models/{modelID}** übergeben können, um die Trainingsergebnisse zurückzugeben.
* Die Schlüssel-Wert-Extraktion wird jetzt vom API-Aufruf **/custom/models/{modelID}/analyze** initiiert. Dieser Aufruf gibt eine Vorgangs-ID zurück, die Sie in **custom/models/{modelID}/analyzeResults/{resultID}** übergeben können, um die Extraktionsergebnisse zurückzugeben.
* Vorgangs-IDs für das Trainieren finden sich jetzt im Header **Location** von HTTP-Antworten, nicht mehr im Header **Operation-Location**.

### <a name="receipt-api-changes"></a>Änderungen an der Beleg-API

Die APIs zum Lesen von Belegen wurden umbenannt.

* Die Extraktion von Belegdaten wird jetzt vom API-Aufruf **/prebuilt/receipt/analyze** initiiert. Dieser Aufruf gibt eine Vorgangs-ID zurück, die Sie in **/prebuilt/receipt/analyzeResults/{resultID}** übergeben können, um die Extraktionsergebnisse zurückzugeben.

### <a name="output-format-changes"></a>Änderungen am Ausgabeformat

Die JSON-Antworten für alle API-Aufrufe haben neue Formate. Es wurden einige Schlüssel und Werte hinzugefügt, entfernt oder umbenannt. Beispiele für die aktuellen JSON-Formate finden Sie in den Schnellstarts.

## <a name="next-steps"></a>Nächste Schritte

Durchlaufen Sie einen [Schnellstart](quickstarts/client-library.md) zu den ersten Schritten zum Schreiben einer Formularverarbeitungs-App mit der Formularerkennung in der Entwicklungssprache Ihrer Wahl.

## <a name="see-also"></a>Weitere Informationen

* [Was ist die Formularerkennung?](./overview.md)