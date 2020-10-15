---
title: include file
description: include file
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 03/18/2020
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 10c2b447a3f174afe93f56084827756d24d982cc
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "82792189"
---
## <a name="create-an-image-gallery"></a>Erstellen eines Imagekatalogs 

Ein Imagekatalog ist die primäre Ressource, die zur Ermöglichung des Teilens von Images verwendet wird. Zulässige Zeichen für Katalognamen sind Groß- und Kleinbuchstaben, Zahlen und Punkte. Der Katalogname darf keine Bindestriche enthalten. Katalognamen müssen innerhalb Ihres Abonnements eindeutig sein. 

Erstellen Sie mit [New-AzGallery](https://docs.microsoft.com/powershell/module/az.compute/new-azgallery) einen Imagekatalog. Im folgenden Beispiel wird der Katalog *myGallery* in der Ressourcengruppe *myGalleryRG* erstellt.

```azurepowershell-interactive
$resourceGroup = New-AzResourceGroup `
   -Name 'myGalleryRG' `
   -Location 'West Central US'  
$gallery = New-AzGallery `
   -GalleryName 'myGallery' `
   -ResourceGroupName $resourceGroup.ResourceGroupName `
   -Location $resourceGroup.Location `
   -Description 'Shared Image Gallery for my organization'  
```
   

## <a name="share-the-gallery"></a>Teilen des Katalogs

Wir empfehlen, dass Sie den Zugriff auf der Ebene des Imagekatalogs teilen. Verwenden Sie eine E-Mail-Adresse und das Cmdlet [Get-AzADUser](/powershell/module/az.resources/get-azaduser), um die Objekt-ID für den Benutzer abzurufen, und verwenden Sie dann [New-AzRoleAssignment](/powershell/module/Az.Resources/New-AzRoleAssignment), um ihnen Zugriff auf den Katalog zu gewähren. Ersetzen Sie die Beispiel-E-Mail-Adresse alinne_montes@contoso.com in diesem Beispiel durch Ihre eigenen Informationen.

```azurepowershell-interactive
# Get the object ID for the user
$user = Get-AzADUser -StartsWith alinne_montes@contoso.com
# Grant access to the user for our gallery
New-AzRoleAssignment `
   -ObjectId $user.Id `
   -RoleDefinitionName Reader `
   -ResourceName $gallery.Name `
   -ResourceType Microsoft.Compute/galleries `
   -ResourceGroupName $resourceGroup.ResourceGroupName

```

