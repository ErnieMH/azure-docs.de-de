---
author: rothja
ms.service: key-vault
ms.topic: include
ms.date: 04/21/2020
ms.author: jroth
ms.openlocfilehash: 01b3c9584f3ecddbcdcc6938f5eb469510a47a4e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "85838903"
---
### <a name="key-transactions-maximum-transactions-allowed-in-10-seconds-per-vault-per-regionsup1sup"></a>Schlüsseltransaktionen (maximale Transaktionen innerhalb von 10 Sekunden pro Tresor und Region<sup>1</sup>):

|Schlüsseltyp|HSM-Schlüssel<br>Erstellungsschlüssel|HSM-Schlüssel<br>Alle anderen Transaktionen|Softwareschlüssel<br>Erstellungsschlüssel|Softwareschlüssel<br>Alle anderen Transaktionen|
|:---|---:|---:|---:|---:|
|RSA 2.048 Bit|5|1\.000|10|2\.000|
|RSA 3.072 Bit|5|250|10|500|
|RSA 4.096 Bit|5|125|10|250|
|ECC P-256|5|1\.000|10|2\.000|
|ECC P-384|5|1\.000|10|2\.000|
|ECC P-521|5|1\.000|10|2\.000|
|ECC SECP256K1|5|1\.000|10|2\.000|

> [!NOTE]
> In der vorstehenden Tabelle ist zu erkennen, dass für RSA-Softwareschlüssel mit 2.048 Bit 2.000 GET-Transaktionen pro 10 Sekunden zulässig sind. Für RSA-HSM-Schlüssel mit 2.048 Bit sind 1.000 GET-Transaktionen pro 10 Sekunden zulässig.
>
> Die genannten Schwellenwerte für die Drosselung sind gewichtet, und ihre Summe wird erzwungen. Wie in der vorherigen Tabelle gezeigt fällt für das Ausführen von GET-Vorgängen mit RSA-HSM-Schlüsseln bei Verwendung eines 4.096-Bit-Schlüssels ein achtmal größerer Aufwand als mit einem 2.048-Bit-Schlüssel an. Der Grund dafür ist: 1.000/125 = 8.
>
> Innerhalb eines 10-Sekunden-Intervalls kann ein Azure Key Vault-Client *nur einen* der folgenden Vorgänge ausführen, bevor der HTTP-Statuscode `429` zur Drosselung auftritt:
> - 2\.000 GET-Transaktionen für RSA 2.048-Bit-Softwareschlüssel
> - 1\.000 GET-Transaktionen für RSA 2.048-Bit-HSM-Schlüssel
> - 125 GET-Transaktionen für RSA 4.096-Bit-HSM-Schlüssel
> - 124 GET-Transaktionen für RSA 4.096-Bit-HSM-Schlüssel und 8 GET-Transaktionen für RSA 2.048-Bit-HSM-Schlüssel

### <a name="secrets-managed-storage-account-keys-and-vault-transactions"></a>Geheimnisse, Schlüssel für verwaltete Speicherkonten und Tresortransaktionen:

| Transaktionstyp | Maximal zulässige Transaktionen innerhalb von 10 Sekunden pro Tresor und Region<sup>1</sup> |
| --- | --- |
| Alle Transaktionen |2\.000 |

Informationen zur Handhabung der Drosselung bei Überschreiten dieser Grenzwerte finden Sie unter [Anleitung zur Drosselung von Azure Key Vault](../articles/key-vault/key-vault-ovw-throttling.md).

<sup>1</sup> Für alle Transaktionsarten gilt als abonnementweiter Grenzwert das Fünffache des Schlüsseltresorlimits. „HSM – andere Transaktionen“ pro Abonnement ist beispielsweise auf 5.000 Transaktionen in 10 Sekunden pro Abonnent beschränkt.

### <a name="azure-private-link-integration"></a>Azure Private Link-Integration

> [!NOTE]
> Die Anzahl der Schlüsseltresore mit den pro Abonnement aktivierten privaten Endpunkten ist ein anpassbarer Grenzwert. Der gezeigte Grenzwert ist der Standardwert. Wenn Sie eine Erhöhung des Grenzwerts für Ihren Dienst beantragen möchten, senden Sie eine E-Mail an akv-privatelink@microsoft.com. Wir genehmigen diese Anträge je nach Fall.

| Resource | Begrenzung |
| -------- | ----- |
| Private Endpunkte pro Schlüsseltresor | 64 |
| Schlüsseltresore mit privaten Endpunkten pro Abonnement | 400 |
