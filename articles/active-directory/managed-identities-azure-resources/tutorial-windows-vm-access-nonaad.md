---
title: Tutorial`:` Verwenden einer verwalteten Identität für den Zugriff auf Azure Key Vault – Windows – Azure AD
description: In diesem Tutorial erfahren Sie, wie Sie eine systemseitig zugewiesene verwaltete Identität eines virtuellen Windows-Computers verwenden, um auf Azure Key Vault zuzugreifen.
services: active-directory
documentationcenter: ''
author: barclayn
manager: daveba
editor: daveba
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/10/2020
ms.author: barclayn
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2890eb2211ac0a105363742a0e900e52a577ed27
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89255824"
---
# <a name="tutorial-use-a-windows-vm-system-assigned-managed-identity-to-access-azure-key-vault"></a>Tutorial: Verwenden der systemseitig zugewiesenen verwalteten Identität eines virtuellen Windows-Computers für den Zugriff auf Azure Key Vault 

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

In diesem Tutorial erfahren Sie, wie Sie eine systemseitig zugewiesene verwaltete Identität für einen virtuellen Windows-Computer verwenden, um auf Azure Key Vault zuzugreifen. Key Vault dient als Bootstrap und ermöglicht es Ihrer Clientanwendung, mithilfe des Geheimnisses auf Ressourcen zuzugreifen, die nicht von Azure Active Directory (AD) geschützt sind. Verwaltete Dienstidentitäten werden von Azure automatisch verwaltet und ermöglichen Ihnen die Authentifizierung für Dienste, die die Azure AD-Authentifizierung unterstützen, ohne dass Sie Anmeldeinformationen in Ihren Code einfügen müssen. 

Folgendes wird vermittelt:


> [!div class="checklist"]
> * Gewähren des Zugriffs auf ein in einer Key Vault gespeicherten Geheimnisses für den virtuellen Computer 
> * Abrufen eines Zugriffstokens mithilfe der VM-Identität und Verwenden dieses Zugriffstokens zum Abrufen des Geheimnisses aus der Key Vault 

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]


## <a name="enable"></a>Aktivieren

[!INCLUDE [msi-tut-enable](../../../includes/active-directory-msi-tut-enable.md)]



## <a name="grant-access"></a>Gewähren von Zugriff  
 
In diesem Abschnitt wird gezeigt, wie Sie einem virtuellen Computer den Zugriff auf ein in einer Key Vault-Instanz gespeichertes Geheimnis gewähren. Mithilfe von verwalteten Identitäten für Azure-Ressourcen kann der Code Zugriffstoken zur Authentifizierung von Ressourcen abrufen, die die Azure AD-Authentifizierung unterstützen.  Allerdings unterstützen nicht alle Azure-Dienste die Azure AD-Authentifizierung. Um verwaltete Identitäten für Azure-Ressourcen mit diesen Diensten zu verwenden, speichern Sie die Dienstanmeldeinformationen in Azure Key Vault, und greifen Sie mit der verwalteten Identität des virtuellen Computers auf Key Vault zu, um die Anmeldeinformationen abzurufen. 

Zunächst müssen Sie eine Key Vault-Instanz erstellen und der systemseitig zugewiesenen verwalteten Identität des virtuellen Computers den Zugriff darauf erteilen.   

1. Wählen Sie oben in der linken Navigationsleiste **Ressource erstellen** > **Sicherheit + Identität** > **Key Vault** aus.  
2. Geben Sie unter **Name** einen Namen für die neue Key Vault an. 
3. Suchen Sie die Key Vault in dem Abonnement und in der Ressourcengruppe, in der sich auch der zuvor erstellte virtuelle Computer befindet. 
4. Wählen Sie **Zugriffsrichtlinien** aus, und klicken Sie auf **Neue hinzufügen**. 
5. Wählen Sie unter „Anhand einer Vorlage konfigurieren“ die Option **Verwaltung von Geheimnissen** aus. 
6. Wählen Sie **Prinzipal auswählen**, und geben Sie im Suchfeld den Namen des zuvor erstellten virtuellen Computers ein.  Wählen Sie in der Ergebnisliste den virtuellen Computer aus, und klicken Sie auf **Auswählen** . 
7. Klicken Sie auf **OK**, um die neue Zugriffsrichtlinie hinzuzufügen. Klicken Sie dann erneut auf **OK**, um die Auswahl der Zugriffsrichtlinie abzuschließen. 
8. Klicken Sie auf **Erstellen**, um die Key Vault zu erstellen. 

    ![Alternativer Bildtext](./media/msi-tutorial-windows-vm-access-nonaad/msi-blade.png)


Fügen Sie der Key Vault nun ein Geheimnis hinzu, das Sie später mithilfe von Code abrufen können, der auf dem virtuellen Computer ausgeführt wird: 

1. Wählen Sie **Alle Ressourcen** aus, suchen Sie den erstellten Schlüsseltresor, und wählen Sie diesen aus. 
2. Wählen Sie **Geheimnisse** aus, und klicken Sie auf **Hinzufügen**. 
3. Wählen Sie unter **Uploadoptionen** die Option **Manuell** aus. 
4. Geben Sie einen Namen und einen Wert für das Geheimnis ein.  Dabei kann es sich um einen beliebigen Wert handeln. 
5. Lassen Sie das Aktivierungs- und das Ablaufdatum leer, und übernehmen Sie für **Aktiviert** die ausgewählte Option **Ja**. 
6. Klicken Sie auf **Erstellen**, um das Geheimnis zu erstellen. 
 
## <a name="access-data"></a>Zugreifen auf Daten  

In diesem Abschnitt wird gezeigt, wie Sie mithilfe der VM-Identität ein Zugriffstoken abrufen und es zum Abrufen des Geheimnisses aus der Key Vault-Instanz verwenden. Wenn bei Ihnen nicht PowerShell 4.3.1 oder höher installiert ist, müssen Sie [die neueste Version herunterladen und installieren](/powershell/azure/).

Zuerst wird mit der systemseitig zugewiesenen verwalteten Identität des virtuellen Computers ein Zugriffstoken für die Authentifizierung bei Key Vault abgerufen:
 
1. Navigieren Sie im Portal zu **Virtuelle Computer**, wechseln Sie dann zu Ihrem virtuellen Windows-Computer, und klicken Sie in der **Übersicht** auf **Verbinden**.
2. Geben Sie Ihren **Benutzernamen** und Ihr **Kennwort** ein, das Sie beim Erstellen des **rs** hinzugefügt haben.  
3. Sie haben nun eine **Remotedesktopverbindung** mit dem virtuellen Computer erstellt. Öffnen Sie jetzt PowerShell in der Remotesitzung.  
4. Rufen Sie in PowerShell die Webanforderung für den Mandanten auf, um das Token für den lokalen Host am spezifischen Port des virtuellen Computers abzurufen.  

    Die PowerShell-Anforderung:
    
    ```powershell
    $response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net' -Method GET -Headers @{Metadata="true"} 
    ```
    
    Extrahieren Sie als Nächstes die vollständige Antwort, die als JSON-formatierte Zeichenfolge (JavaScript Object Notation) im „$response“-Objekt gespeichert ist.  
    
    ```powershell
    $content = $response.Content | ConvertFrom-Json 
    ```
    
    Extrahieren Sie dann das Zugriffstoken aus der Antwort.  
    
    ```powershell
    $KeyVaultToken = $content.access_token 
    ```
    
    Verwenden Sie abschließend den PowerShell-Befehl „Invoke-WebRequest“, um das zuvor in der Key Vault erstellte Geheimnis abzurufen. Übergeben Sie dabei das Zugriffstoken im Autorisierungsheader.  Sie benötigen die URL Ihrer Key Vaults. Diese befindet sich im Abschnitt **Zusammenfassung** der Seite **Übersicht** der Key Vault.  
    
    ```powershell
    (Invoke-WebRequest -Uri https://<your-key-vault-URL>/secrets/<secret-name>?api-version=2016-10-01 -Method GET -Headers @{Authorization="Bearer $KeyVaultToken"}).content 
    ```
    
    Die Antwort sieht so aus: 
    
    ```powershell
    {"value":"p@ssw0rd!","id":"https://mytestkeyvault.vault.azure.net/secrets/MyTestSecret/7c2204c6093c4d859bc5b9eff8f29050","attributes":{"enabled":true,"created":1505088747,"updated":1505088747,"recoveryLevel":"Purgeable"}} 
    ```
    
Nachdem Sie das Geheimnis aus der Key Vault abgerufen haben, können Sie es für die Authentifizierung bei einem Dienst verwenden, für den ein Name und ein Kennwort angegeben werden müssen. 


## <a name="disable"></a>Disable

[!INCLUDE [msi-tut-disable](../../../includes/active-directory-msi-tut-disable.md)]



## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie eine systemseitig zugewiesene verwaltete Identität eines virtuellen Windows-Computers verwenden, um auf Azure Key Vault zuzugreifen.  Weitere Informationen zu Azure Key Vault finden Sie in folgendem Artikel:

> [!div class="nextstepaction"]
>[Azure Key Vault](../../key-vault/general/overview.md)