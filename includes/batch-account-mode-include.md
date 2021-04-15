---
title: include file
description: include file
services: batch
author: laurenhughes
ms.service: batch
ms.topic: include
ms.date: 05/04/2018
ms.author: lahugh
ms.custom: include file
ms.openlocfilehash: 51b687dc2a6264eb12362616429746c9b182c3b1
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "68323892"
---
> [!NOTE]
> Beim Erstellen eines Batchkontos können Sie zwischen zwei Modi der *Poolzuordnung* wählen: **Benutzerabonnement** und **Batchdienst**. In den meisten Fällen sollten Sie den standardmäßigen Batchdienstmodus wählen, in dem Pools im Hintergrund von Azure verwalteten Abonnements zugeordnet werden. Im alternativen Benutzerabonnementmodus werden virtuelle Batchcomputer und andere Ressourcen direkt in Ihrem Abonnement erstellt, wenn ein Pool erstellt wird. Der Benutzerabonnementmodus ist erforderlich, wenn Sie mithilfe von [reservierten Azure VM-Instanzen](https://azure.microsoft.com/pricing/reserved-vm-instances/) Batchpools erstellen möchten. Wenn Sie ein Batch-Konto im Benutzerabonnementmodus erstellen möchten, müssen Sie auch Ihr Abonnement in Azure Batch registrieren und das Konto einer Azure Key Vault-Instanz zuordnen.