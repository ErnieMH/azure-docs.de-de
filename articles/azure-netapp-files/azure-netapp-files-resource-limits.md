---
title: Ressourceneinschränkungen für Azure NetApp Files | Microsoft-Dokumentation
description: Beschreibt Limits für Azure NetApp Files-Ressourcen und wie eine Erhöhung des Ressourcenlimits angefordert wird.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 9/16/2020
ms.author: b-juche
ms.openlocfilehash: 0ddb9998c1e1b9b70303aeb4608bc0b53bc103ae
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91325486"
---
# <a name="resource-limits-for-azure-netapp-files"></a>Ressourcenlimits für Azure NetApp Files

Grundlegende Informationen zu Ressourceneinschränkungen für Azure NetApp Files unterstützen Sie bei der Verwaltung Ihrer Volumes.

## <a name="resource-limits"></a>Ressourceneinschränkungen

In der folgenden Tabelle werden die Ressourcengrenzwerte für Azure NetApp Files beschrieben:

|  Resource  |  Standardlimit  |  Über Supportanfrage anpassbar  |
|----------------|---------------------|--------------------------------------|
|  Anzahl der NetApp-Konten pro Azure-Region   |  10    |  Ja   |
|  Anzahl der Kapazitätspools pro NetApp-Konto   |    25     |   Ja   |
|  Anzahl von Volumes pro Abonnement   |    500     |   Ja   |
|  Anzahl der Volumes pro Kapazitätspool     |    500   |    Ja     |
|  Anzahl von Momentaufnahmen pro Volume       |    255     |    Nein        |
|  Anzahl der an Azure NetApp Files (Microsoft.NetApp/volumes) delegierten Subnetze pro virtuellem Azure-Netzwerk    |   1   |    Nein    |
|  Anzahl verwendeter IP-Adressen innerhalb eines VNets (einschließlich VNets mit sofortigem Peering) mit Azure NetApp Files   |    1000   |    Nein   |
|  Mindestgröße eines einzelnen Kapazitätspools   |  4 TiB     |    Nein  |
|  Maximale Größe eines einzelnen Kapazitätspools    |  500 TiB   |   Nein   |
|  Mindestgröße eines einzelnen Volumes    |    100 GB    |    Nein    |
|  Maximale Größe eines einzelnen Volumes     |    ca. 100 TiB    |    Nein    |
|  Maximale Größe einer einzelnen Datei     |    16 TiB    |    Nein    |    
|  Maximale Größe der Verzeichnismetadaten in einem einzelnen Verzeichnis      |    320 MB    |    Nein    |    
|  Maximale Anzahl von Dateien ([maxfiles](#maxfiles)) pro Volume     |    100 Mio.    |    Ja    |    
|  Minimaler zugewiesener Durchsatz für ein Volume für manuelle QoS     |    1 MiB/s   |    Nein    |    
|  Maximaler zugewiesener Durchsatz für ein Volume für manuelle QoS     |    4\.500 MiB/s    |    Nein    |    
|  Anzahl der regionsübergreifenden Datenschutzvolumes für die regionsübergreifende Replikation (Zielvolumes)     |    5    |    Ja    |     

Weitere Informationen finden Sie unter [Häufig gestellte Fragen zur Kapazitätsverwaltung](azure-netapp-files-faqs.md#capacity-management-faqs).

## <a name="maxfiles-limits"></a>Maxfiles-Limits <a name="maxfiles"></a> 

Azure NetApp Files-Volumes besitzen ein Limit namens *maxfiles*. Das maxfiles-Limit ist die Anzahl von Dateien, die ein Volume enthalten kann. Das maxfiles-Limit für ein Azure NetApp Files-Volume wird basierend auf der Größe (dem Kontingent) des Volumes indiziert. Das maxfiles-Limit für ein Volume erhöht oder verringert sich mit einer Rate von 20 Millionen Dateien pro TiB bereitgestellter Volumegröße. 

Der Dienst passt das maxfiles-Limit für ein Volume basierend auf seiner bereitgestellten Größe dynamisch an. Beispielsweise hätte ein Volume, das anfänglich mit einer Größe von 1 TiB konfiguriert wurde, ein maxfiles-Limit von 20 Millionen. Nachfolgende Änderungen an der Größe des Volumes führten zu einer automatischen Neuanpassung des maxfiles-Limits basierend auf den folgenden Regeln: 

|    Volumegröße (Kontingent)     |  Automatische Neuanpassung des maxfiles-Limits    |
|----------------------------|-------------------|
|    <= 1 TiB                |    20 Mio.     |
|    > 1 TiB, aber <= 2 TiB    |    40 Mio.     |
|    > 2 TiB, aber <= 3 TiB    |    60 Mio.     |
|    > 3 TiB, aber <= 4 TiB    |    80 Mio.     |
|    > 4 TiB                 |    100 Mio.    |

Wenn Sie für ein Volume bereits ein Kontingent von mindestens 4 TiB zugeordnet haben, können Sie eine [Supportanfrage](#limit_increase) initiieren, um das maxfiles-Limit auf über 100 Millionen zu erhöhen.

## <a name="request-limit-increase"></a>Anfordern einer Limiterhöhung <a name="limit_increase"></a> 

Sie können eine Supportanfrage an den Azure-Support stellen, um die anpassbaren Grenzwerte aus der obigen Tabelle zu erhöhen. 

Über den Navigationsbereich im Azure-Portal: 

1. Klicken Sie auf **Hilfe und Support**.
2. Klicken Sie auf **+ Neue Supportanfrage**.
3. Geben Sie die folgenden Informationen auf der Registerkarte „Grundeinstellungen“ an: 
    1. Problemtyp: Wählen Sie **Grenzwerte für Dienste und Abonnements (Kontingente)** aus.
    2. Abonnements: Wählen Sie das Abonnement für die Ressource aus, deren Kontingente erhöht werden sollen.
    3. Kontingenttyp: Wählen Sie **Storage: Azure NetApp Files limits** (Speicher: Azure NetApp Files-Grenzwerte) aus.
    4. Klicken Sie auf **Weiter: Lösungen**.
4. Auf der Registerkarte „Details“:
    1. Geben Sie die folgenden Informationen für den entsprechenden Ressourcentyp im Feld „Beschreibung“ an:

        |  Resource  |    Übergeordnete Ressourcen      |    Angeforderte neue Grenzwerte     |    Grund für die Kontingenterhöhung       |
        |----------------|------------------------------|---------------------------------|------------------------------------------|
        |  Konto |  *Abonnement-ID*   |  *Angeforderte neue maximale Anzahl an **Konten***    |  *Szenario oder Anwendungsfall, das zur Anforderung geführt hat*  |
        |  Pool    |  *Abonnement-ID, NetApp-Konto-URI*  |  *Angeforderte neue maximale Anzahl an **Pools***   |  *Szenario oder Anwendungsfall, das zur Anforderung geführt hat*  |
        |  Volume  |  *Abonnement-ID, NetApp-Konto-URI, Kapazitätspool-URI*   |  *Angeforderte neue maximale Anzahl an **Volumes***     |  *Szenario oder Anwendungsfall, das zur Anforderung geführt hat*  |
        |  Maxfiles  |  *Abonnement-ID, NetApp-Konto-URI, Kapazitätspool-URI, Volume-URI*   |  *Angeforderter neuer maximaler **maxfiles**-Wert*     |  *Szenario oder Anwendungsfall, das zur Anforderung geführt hat*  |    
        |  Datenschutzvolumes für die regionsübergreifende Replikation  |  *Abonnement-ID, NetApp-Zielkonto-URI, Zielkapazitätspool-URI, NetApp-Quellkonto-URI, Quellkapazitätspool-URI, Quellvolume-URI*   |  *Angeforderte neue maximale Anzahl der **regionsübergreifenden Datenschutzvolumes für die regionsübergreifende Replikation (Zielvolumes)***     |  *Szenario oder Anwendungsfall, das zur Anforderung geführt hat*  |    

    2. Legen Sie die entsprechende Supportmethode fest, und geben Sie Ihre Kontaktinformationen an.

    3. Klicken Sie auf **Weiter: Überprüfen + erstellen**, um die Anforderung zu erstellen. 


## <a name="next-steps"></a>Nächste Schritte  

- [Grundlegendes zur Speicherhierarchie von Azure NetApp Files](azure-netapp-files-understand-storage-hierarchy.md)
- [Cost model for Azure NetApp Files (Kostenmodell für Azure NetApp Files)](azure-netapp-files-cost-model.md)
