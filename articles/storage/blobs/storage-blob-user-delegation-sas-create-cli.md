---
title: Verwenden der Azure CLI zum Erstellen einer SAS für die Benutzerdelegierung für einen Container oder ein Blob
titleSuffix: Azure Storage
description: Hier erfahren Sie, wie Sie mit der Azure CLI eine SAS für die Benutzerdelegierung mit Azure Active Directory-Anmeldeinformationen erstellen.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 12/18/2019
ms.author: tamram
ms.reviewer: dineshm
ms.subservice: blobs
ms.custom: devx-track-azurecli
ms.openlocfilehash: 536cd01fbcf2c5d18a8c12030b709427d9bb91b1
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "98703605"
---
# <a name="create-a-user-delegation-sas-for-a-container-or-blob-with-the-azure-cli"></a>Erstellen einer SAS für die Benutzerdelegierung für einen Container oder ein Blob mit der Azure CLI

[!INCLUDE [storage-auth-sas-intro-include](../../../includes/storage-auth-sas-intro-include.md)]

In diesem Artikel wird beschrieben, wie Sie Azure Active Directory-Anmeldeinformationen (Azure AD) verwenden, um eine SAS für die Benutzerdelegierung für einen Container oder ein Blob mit der Azure CLI zu erstellen.

[!INCLUDE [storage-auth-user-delegation-include](../../../includes/storage-auth-user-delegation-include.md)]

## <a name="install-the-latest-version-of-the-azure-cli"></a>Installieren der aktuellsten Version der Azure CLI

Stellen Sie zunächst sicher, dass Sie die neueste Version der Azure CLI installiert haben, um die Azure CLI zum Sichern einer SAS mit Azure AD-Anmeldeinformationen verwenden zu können. Weitere Informationen zum Installieren der Azure CLI finden Sie unter [Installieren der Azure CLI](/cli/azure/install-azure-cli).

Um eine SAS für die Benutzerdelegierung mit der Azure CLI zu erstellen, vergewissern Sie sich, dass Version 2.0.78 oder höher installiert ist. Verwenden Sie den Befehl `az --version`, um Ihre installierte Version zu überprüfen.

## <a name="sign-in-with-azure-ad-credentials"></a>Anmeldung mit Azure AD-Anmeldeinformationen

Melden Sie sich mit Ihren Azure AD-Anmeldeinformationen bei der Azure CLI an. Weitere Informationen finden Sie unter [Anmelden mit der Azure CLI](/cli/azure/authenticate-azure-cli).

## <a name="assign-permissions-with-azure-rbac"></a>Zuweisen von Berechtigungen mit Azure RBAC

Um eine SAS für die Benutzerdelegierung aus Azure PowerShell zu erstellen, muss dem Azure AD-Konto, mit dem die Anmeldung bei der Azure CLI erfolgt, eine Rolle zugewiesen werden, die die Aktion **Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey** beinhaltet. Diese Berechtigung ermöglicht es diesem Azure AD Konto, den *Benutzerdelegierungsschlüssel* anzufordern. Der Benutzerdelegierungsschlüssel wird zum Signieren der SAS für die Benutzerdelegierung verwendet. Die Rolle, die die Aktion **Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey** bereitstellt, muss auf der Ebene des Speicherkontos, der Ressourcengruppe oder des Abonnements zugewiesen werden.

Wenn Sie nicht über ausreichende Berechtigungen zum Zuweisen von Azure-Rollen zu einem Azure AD-Sicherheitsprinzipal verfügen, müssen Sie möglicherweise den Kontobesitzer oder den Administrator bitten, die erforderlichen Berechtigungen zuzuweisen.

Im folgenden Beispiel wird die Rolle **Storage Blob Data Contributor** (Speicherblob-Datenmitwirkender) zugewiesen, die die Aktion **Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey** beinhaltet. Die Rolle wird auf der Ebene des Speicherkontos festgelegt.

Denken Sie daran, die Platzhalterwerte in eckigen Klammern durch Ihre eigenen Werte zu ersetzen:

```azurecli-interactive
az role assignment create \
    --role "Storage Blob Data Contributor" \
    --assignee <email> \
    --scope "/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>"
```

Weitere Informationen zu den integrierten Rollen, die die Aktion **Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey** enthalten, finden Sie unter [Integrierte Azure-Rollen](../../role-based-access-control/built-in-roles.md).

## <a name="use-azure-ad-credentials-to-secure-a-sas"></a>Verwenden von Azure AD-Anmeldeinformationen zum Sichern einer SAS

Wenn Sie eine SAS für die Benutzerdelegierung mit der Azure CLI erstellen, wird der Benutzerdelegierungsschlüssel, der zum Signieren der SAS verwendet wird, implizit für Sie erstellt. Die Startzeit und die Ablaufzeit, die Sie für die SAS angeben, werden auch als Startzeit und Ablaufzeit für den Benutzerdelegierungsschlüssel verwendet.

Da das maximale Intervall, in dem der Benutzerdelegierungsschlüssel gültig ist, 7 Tage ab dem Startdatum beträgt, sollten Sie eine Ablaufzeit für die SAS angeben, die innerhalb von 7 Tagen der Startzeit liegt. Die SAS ist ungültig, nachdem der Benutzerdelegierungsschlüssel abgelaufen ist, sodass eine SAS mit einer Ablaufzeit von mehr als 7 Tagen trotzdem nur 7 Tage gültig ist.

Beim Erstellen einer SAS für die Benutzerdelegierung sind `--auth-mode login` und `--as-user parameters` erforderlich. Geben Sie *login* für den Parameter `--auth-mode` an, damit Anforderungen an Azure Storage mit Ihren Azure AD-Anmeldeinformationen autorisiert werden. Geben Sie den `--as-user`-Parameter an, um festzulegen, dass die zurückgegebene SAS eine SAS für die Benutzerdelegierung sein sollte.

### <a name="create-a-user-delegation-sas-for-a-container"></a>Erstellen einer SAS für die Benutzerdelegierung für einen Container

Um eine SAS für die Benutzerdelegierung für einen Container mit der Azure CLI zu erstellen, verwenden Sie den Befehl [az storage container generate-sas](/cli/azure/storage/container#az-storage-container-generate-sas).

Zu den unterstützten Berechtigungen für eine SAS für die Benutzerdelegierung für einen Container zählen „Add“ (Hinzufügen), „Create“ (Erstellen), „Delete“ (Löschen), „List (Auflisten), „Read“ (Lesen) und „Write“ (Schreiben). Berechtigungen können einzeln oder kombiniert angegeben werden. Weitere Informationen zu diesen Berechtigungen finden Sie unter [Erstellen einer SAS für die Benutzerdelegierung](/rest/api/storageservices/create-user-delegation-sas).

Im folgenden Beispiel wird eine SAS für die Benutzerdelegierung für einen Container zurückgegeben. Denken Sie daran, die Platzhalterwerte in Klammern durch Ihre eigenen Werte zu ersetzen:

```azurecli-interactive
az storage container generate-sas \
    --account-name <storage-account> \
    --name <container> \
    --permissions acdlrw \
    --expiry <date-time> \
    --auth-mode login \
    --as-user
```

Das zurückgegebene SAS-Token für die Benutzerdelegierung ähnelt dem folgenden Token:

```
se=2019-07-27&sp=r&sv=2018-11-09&sr=c&skoid=<skoid>&sktid=<sktid>&skt=2019-07-26T18%3A01%3A22Z&ske=2019-07-27T00%3A00%3A00Z&sks=b&skv=2018-11-09&sig=<signature>
```

### <a name="create-a-user-delegation-sas-for-a-blob"></a>Erstellen einer SAS für die Benutzerdelegierung für ein Blob

Um eine SAS für die Benutzerdelegierung für ein Blob mit der Azure CLI zu erstellen, verwenden Sie den Befehl [az storage blob generate-sas](/cli/azure/storage/blob#az-storage-blob-generate-sas).

Zu den unterstützten Berechtigungen für eine SAS für die Benutzerdelegierung für ein Blob zählen „Add“ (Hinzufügen), „Create“ (Erstellen), „Delete“ (Löschen), „Read“ (Lesen) und „Write“ (Schreiben). Berechtigungen können einzeln oder kombiniert angegeben werden. Weitere Informationen zu diesen Berechtigungen finden Sie unter [Erstellen einer SAS für die Benutzerdelegierung](/rest/api/storageservices/create-user-delegation-sas).

Die folgende Syntax gibt eine SAS für die Benutzerdelegierung für ein Blob zurück. Im Beispiel wird der `--full-uri`-Parameter angegeben, der den Blob-URI mit angefügtem SAS-Token zurückgibt. Denken Sie daran, die Platzhalterwerte in Klammern durch Ihre eigenen Werte zu ersetzen:

```azurecli-interactive
az storage blob generate-sas \
    --account-name <storage-account> \
    --container-name <container> \
    --name <blob> \
    --permissions acdrw \
    --expiry <date-time> \
    --auth-mode login \
    --as-user \
    --full-uri
```

Der zurückgegebene SAS-URI für die Benutzerdelegierung ähnelt dem folgenden URI:

```
https://storagesamples.blob.core.windows.net/sample-container/blob1.txt?se=2019-08-03&sp=rw&sv=2018-11-09&sr=b&skoid=<skoid>&sktid=<sktid>&skt=2019-08-02T2
2%3A32%3A01Z&ske=2019-08-03T00%3A00%3A00Z&sks=b&skv=2018-11-09&sig=<signature>
```

> [!NOTE]
> Eine SAS für die Benutzerdelegierung unterstützt nicht das Definieren von Berechtigungen mit einer gespeicherten Zugriffsrichtlinie.

## <a name="revoke-a-user-delegation-sas"></a>Widerrufen einer SAS für die Benutzerdelegierung

Um eine SAS für die Benutzerdelegierung über die Azure CLI zu widerrufen, verwenden Sie den Befehl [az storage account revoke-delegation-keys](/cli/azure/storage/account#az-storage-account-revoke-delegation-keys). Mit diesem Befehl werden alle Benutzerdelegierungsschlüssel widerrufen, die dem angegebenen Speicherkonto zugeordnet sind. Alle mit diesen Schlüsseln verknüpften Shared Access Signatures werden für ungültig erklärt.

Denken Sie daran, die Platzhalterwerte in eckigen Klammern durch Ihre eigenen Werte zu ersetzen:

```azurecli-interactive
az storage account revoke-delegation-keys \
    --name <storage-account> \
    --resource-group <resource-group>
```

> [!IMPORTANT]
> Sowohl der Benutzerdelegierungsschlüssel als auch die Azure-Rollenzuweisungen werden von Azure Storage zwischengespeichert. Daher kann es zu einer Verzögerung zwischen der Initiierung des Sperrprozesses und dem Zeitpunkt kommen, zu dem eine SAS für die Benutzerdelegierung ungültig wird.

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen einer SAS für die Benutzerdelegierung (REST-API)](/rest/api/storageservices/create-user-delegation-sas)
- [Vorgang zum Abrufen eines Benutzerdelegierungsschlüssels](/rest/api/storageservices/get-user-delegation-key)
