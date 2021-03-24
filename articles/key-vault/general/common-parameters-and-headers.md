---
title: Allgemeine Parameter und Header
description: Die Parameter und Header, die für alle Vorgänge gelten, die Sie ggf. im Zusammenhang mit Key Vault-Ressourcen ausführen
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: mbaldwin
ms.openlocfilehash: d1d93bcd84fd9460e658b221089a4b24d46b0429
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "83005817"
---
# <a name="common-parameters-and-headers"></a>Allgemeine Parameter und Header

Die folgenden Informationen gelten für alle Vorgänge, die Sie ggf. im Zusammenhang mit Key Vault-Ressourcen ausführen:

- Der HTTP-Header `Host` muss immer vorhanden sein, und der Hostname des Tresors muss angegeben werden. Beispiel: `Host: contoso.vault.azure.net`. Beachten Sie, dass die meisten Clienttechnologien den `Host`-Header aus dem URI mit Daten auffüllen. Beispielsweise legt `GET https://contoso.vault.azure.net/secrets/mysecret{...}` die Angabe `Host` auf `contoso.vault.azure.net` fest. Dies bedeutet Folgendes: Wenn Sie auf Key Vault mit einer unformatierten IP-Adresse wie `GET https://10.0.0.23/secrets/mysecret{...}` zugreifen, ist der automatische Wert des `Host`-Headers falsch, und Sie müssen manuell sicherstellen, dass der `Host`-Header den Hostnamen des Tresors enthält.
- Ersetzen Sie `{api-version}` durch die API-Version im URI.
- Ersetzen Sie `{subscription-id}` durch Ihren Abonnementbezeichner im URI.
- Ersetzen Sie `{resource-group-name}` durch die Ressourcengruppe. Weitere Informationen finden Sie unter „Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen“.
- Ersetzen Sie `{vault-name}` durch Ihren Schlüsseltresornamen im URI.
- Legen Sie den Inhaltstyp auf „application/json“ fest.
- Legen Sie den Autorisierungsheader auf ein JSON-Webtoken fest, das Sie aus Azure Active Directory (AAD) abgerufen haben. Weitere Informationen finden Sie unter [Authentifizieren von Anforderungen des Azure-Ressourcen-Managers](authentication-requests-and-responses.md).

## <a name="common-error-response"></a>Allgemeine Fehlerantwort
Der Dienst gibt anhand von HTTP-Statuscodes an, ob der Vorgang erfolgreich oder fehlerhaft war. Außerdem enthalten Fehler eine Antwort im folgenden Format:

```
   {  
     "error": {  
     "code": "BadRequest",  
     "message": "The key vault sku is invalid."  
     }  
   }  
```

|Elementname | type | BESCHREIBUNG |
|---|---|---|
| code | Zeichenfolge | Der Typ des aufgetretenen Fehlers.|
| message | Zeichenfolge | Eine Beschreibung der Ursache des Fehlers. |



## <a name="see-also"></a>Weitere Informationen
 [Referenz für die Azure Key Vault-REST-API](/rest/api/keyvault/)
