---
title: Beispiele für die Transformation von allgemeinen Ansprüchen für benutzerdefinierte Richtlinien
titleSuffix: Azure AD B2C
description: Beispiele für die Transformation allgemeiner Ansprüche für das Schema des Frameworks für die Identitätsfunktion (Identity Experience Framework, IEF) von Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 02/03/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 52831a1907d5ca8d13b0477c909d0d0358873973
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "85202219"
---
# <a name="general-claims-transformations"></a>Transformationen allgemeiner Ansprüche

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Dieser Artikel enthält Beispiele für die Verwendung von Transformationen allgemeiner Ansprüche für das Identity Experience Framework-Schema in Azure Active Directory B2C (Azure AD B2C). Weitere Informationen finden Sie unter [ClaimsTransformations](claimstransformations.md).

## <a name="copyclaim"></a>CopyClaim

Kopiert den Wert eines Anspruchs in einen anderen. Beide Ansprüche müssen denselben Typ aufweisen.

| Element | TransformationClaimType | Datentyp | Notizen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputClaim | String, Int | Der Anspruchstyp, der kopiert werden soll. |
| OutputClaim | outputClaim | String, Int | Der Anspruchstyp, der erstellt wird, nachdem diese Anspruchstransformation aufgerufen wurde. |

Verwenden Sie diese Anspruchstransformation, um einen Wert von einer Zeichenfolge oder einem numerischen Anspruch in einen anderen Anspruch zu kopieren. Im folgenden Beispiel wird der Wert des externalEmail-Anspruchs in einen email-Anspruch kopiert.

```xml
<ClaimsTransformation Id="CopyEmailAddress" TransformationMethod="CopyClaim">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="externalEmail" TransformationClaimType="inputClaim"/>
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="email" TransformationClaimType="outputClaim"/>
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Beispiel

- Eingabeansprüche:
    - **inputClaim**: bob@contoso.com
- Ausgabeansprüche:
    - **outputClaim**: bob@contoso.com

## <a name="doesclaimexist"></a>DoesClaimExist

Überprüft, ob **inputClaim** vorhanden ist oder nicht, und legt **outputClaim** auf TRUE bzw. FALSE fest.

| Element | TransformationClaimType | Datentyp | Notizen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputClaim |Any | Der Eingabeanspruch, dessen Vorhandensein überprüft werden muss. |
| OutputClaim | outputClaim | boolean | Der Anspruchstyp, der erstellt wird, nachdem diese Anspruchstransformation aufgerufen wurde. |

Mithilfe dieser Anspruchstransformation können Sie überprüfen, ob ein Anspruch vorhanden ist oder einen beliebigen Wert enthält. Der Rückgabewert ist ein boolescher Wert, der angibt, ob der Anspruch vorhanden ist. Im folgenden Beispiel wird geprüft, ob die E-Mail-Adresse vorhanden ist.

```xml
<ClaimsTransformation Id="CheckIfEmailPresent" TransformationMethod="DoesClaimExist">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="inputClaim" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="isEmailPresent" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Beispiel

- Eingabeansprüche:
  - **inputClaim**: someone@contoso.com
- Ausgabeansprüche:
  - **outputClaim**: true

## <a name="hash"></a>Hash

Hash des angegebenen Nur-Texts, der Salt und einen geheimen Schlüssel verwendet. Der verwendete Hashalgorithmus lautet SHA-256.

| Element | TransformationClaimType | Datentyp | Notizen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | plaintext | Zeichenfolge | Der zu verschlüsselnde Eingabeanspruch |
| InputClaim | Salt | Zeichenfolge | Der Salt-Parameter. Sie können mit der `CreateRandomString`-Anspruchstransformation einen Zufallswert erstellen. |
| InputParameter | randomizerSecret | Zeichenfolge | Verweist auf einen vorhandenen Azure AD B2C-**Richtlinienschlüssel**. Gehen Sie wie folgt vor, um einen neuen Richtlinienschlüssel zu erstellen: Wählen Sie in Ihrem Azure AD B2C-Mandanten unter **Verwalten** die Option **Identity Experience Framework** aus. Wählen Sie **Richtlinienschlüssel** aus, um die in Ihrem Mandanten verfügbaren Schlüssel anzuzeigen. Wählen Sie **Hinzufügen**. Wählen Sie unter **Optionen** den Eintrag **Manuell** aus. Geben Sie einen Namen an (das Präfix *B2C_1A_* wird möglicherweise automatisch hinzugefügt). Geben Sie im Textfeld **Geheimnis** einen geheimen Schlüssel ein, der verwendet werden soll, z. B. „1234567890“. Wählen Sie unter **Schlüsselverwendung** **Signatur** aus. Klicken Sie auf **Erstellen**. |
| OutputClaim | hash | Zeichenfolge | Der Anspruchstyp, der erstellt wird, nachdem diese Anspruchstransformation aufgerufen wurde. Der im Eingabeanspruch `plaintext` konfigurierte Anspruch. |

```xml
<ClaimsTransformation Id="HashPasswordWithEmail" TransformationMethod="Hash">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="password" TransformationClaimType="plaintext" />
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="salt" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="randomizerSecret" DataType="string" Value="B2C_1A_AccountTransformSecret" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="hashedPassword" TransformationClaimType="hash" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Beispiel

- Eingabeansprüche:
  - **plaintext**: MyPass@word1
  - **salt**: 487624568
  - **randomizerSecret**: B2C_1A_AccountTransformSecret
- Ausgabeansprüche:
  - **outputClaim:** CdMNb/KTEfsWzh9MR1kQGRZCKjuxGMWhA5YQNihzV6U=
