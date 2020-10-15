---
author: ramonarguelles
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 08/23/2019
ms.author: rgarcia
ms.openlocfilehash: 9a02dfbf3f2976489c10ccfeb935915a65d8dc84
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "71180050"
---
Nachdem Ihr Watcher erstellt wurde, wird das `AnchorLocated`-Ereignis für jeden angeforderten Anker ausgelöst. Dieses Ereignis wird sogar dann ausgelöst, wenn ein Anker gefunden wird, oder der Anker nicht gefunden werden kann. Wenn dies geschieht, wird die Ursache im Status angegeben. Nachdem alle Anker für einen Watcher – ob gefunden oder nicht gefunden – verarbeitet wurden, wird das `LocateAnchorsCompleted`-Ereignis ausgelöst. Es besteht eine Beschränkung von 35 Bezeichner pro Watcher. 
