---
title: Konvertieren von Sitzungstokenformaten im .NET SDK – Azure Cosmos DB
description: Informationen zum Konvertieren von Sitzungstokenformaten, um die Kompatibilität zwischen verschiedenen .NET SDK-Versionen sicherzustellen
author: vinhms
ms.service: cosmos-db
ms.topic: how-to
ms.date: 04/30/2020
ms.author: vitrinh
ms.custom: devx-track-csharp
ms.openlocfilehash: 787c39681d0e9aff25d205c7b195be00b8c0bc9c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89020013"
---
# <a name="convert-session-token-formats-in-net-sdk"></a>Konvertieren von Sitzungstokenformaten im .NET SDK

In diesem Artikel wird erläutert, wie Sie verschiedene Sitzungstokenformate konvertieren, um die Kompatibilität zwischen den SDK-Versionen sicherzustellen.

> [!NOTE]
> Standardmäßig verfolgt das SDK das Sitzungstoken automatisch und verwendet das aktuelle Sitzungstoken.  Weitere Informationen finden Sie unter [Verwenden von Sitzungstoken](how-to-manage-consistency.md#utilize-session-tokens). Die Anweisungen in diesem Artikel gelten nur unter den folgenden Bedingungen:
> * Ihr Azure Cosmos DB-Konto verwendet die Sitzungskonsistenz.
> * Sie verwalten die Sitzungstoken manuell.
> * Sie verwenden mehrere SDK-Versionen gleichzeitig.

## <a name="session-token-formats"></a>Sitzungstokenformate

Es gibt zwei Sitzungstokenformate: **Einfach** und **Vektor**.  Diese zwei Formate sind nicht austauschbar, sodass das Format bei der Übergabe an die Clientanwendung mit unterschiedlichen Versionen konvertiert werden muss.
- Das Sitzungstokenformat **Einfach** wird von .NET SDK V1 (Microsoft.Azure.DocumentDB-Version 1.x) verwendet.
- Das Sitzungstokenformat **Vektor** wird von .NET SDK V2 (Microsoft.Azure.DocumentDB-Version 2.x) verwendet.

### <a name="simple-session-token"></a>Einfaches Sitzungstoken

Ein einfaches Sitzungstoken weist das folgende Format auf: `{pkrangeid}:{globalLSN}`

### <a name="vector-session-token"></a>Vektorsitzungstoken

Ein Vektorsitzungstoken weist das folgende Format auf: `{pkrangeid}:{Version}#{GlobalLSN}#{RegionId1}={LocalLsn1}#{RegionId2}={LocalLsn2}....#{RegionIdN}={LocalLsnN}`

## <a name="convert-to-simple-session-token"></a>Konvertieren in ein einfaches Sitzungstoken

Um ein Sitzungstoken mit .NET SDK V1 an den Client zu übergeben, verwenden Sie das Sitzungstokenformat **Einfach**.  Verwenden Sie z. B. den folgenden Beispielcode, um das Format zu konvertieren.

```csharp
private static readonly char[] SegmentSeparator = (new[] { '#' });
private static readonly char[] PkRangeSeparator = (new[] { ':' });

// sessionTokenToConvert = session token from previous response
string[] items = sessionTokenToConvert.Split(PkRangeSeparator, StringSplitOptions.RemoveEmptyEntries);
string[] sessionTokenSegments = items[1].Split(SessionTokenHelpers.SegmentSeparator, StringSplitOptions.RemoveEmptyEntries);

string sessionTokenInSimpleFormat;

if (sessionTokenSegments.Length == 1)
{
    // returning the same token since it already has the correct format
    sessionTokenInSimpleFormat = sessionTokenToConvert;
}
else
{
    long version = 0;
    long globalLSN = 0;

    if (!long.TryParse(sessionTokenSegments[0], out version)
        || !long.TryParse(sessionTokenSegments[1], out globalLSN))
    {
        throw new ArgumentException("Invalid session token format", sessionTokenToConvert);
    }

    sessionTokenInSimpleFormat = string.Format("{0}:{1}", items[0], globalLSN);
}
```

## <a name="convert-to-vector-session-token"></a>Konvertieren in ein Vektorsitzungstoken

Um ein Sitzungstoken mit .NET SDK V2 an den Client zu übergeben, verwenden Sie das Sitzungstokenformat **Vektor**.  Verwenden Sie z. B. den folgenden Beispielcode, um das Format zu konvertieren.

```csharp

private static readonly char[] SegmentSeparator = (new[] { '#' });
private static readonly char[] PkRangeSeparator = (new[] { ':' });

// sessionTokenToConvert = session token from previous response
string[] items = sessionTokenToConvert.Split(PkRangeSeparator, StringSplitOptions.RemoveEmptyEntries);
string[] sessionTokenSegments = items[1].Split(SegmentSeparator, StringSplitOptions.RemoveEmptyEntries);

string sessionTokenInVectorFormat;

if (sessionTokenSegments.Length == 1)
{
    long globalLSN = 0;
    if (long.TryParse(sessionTokenSegments[0], out globalLSN))
    {
        sessionTokenInVectorFormat = string.Format("{0}:-2#{1}", items[0], globalLSN);
    }
    else
    {
        throw new ArgumentException("Invalid session token format", sessionTokenToConvert);
    }
}
else
{
    // returning the same token since it already has the correct format
    sessionTokenInVectorFormat = sessionTokenToConvert;
}
```

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die folgenden Artikel:

* [Verwalten von Konsistenzebenen in Azure Cosmos DB](how-to-manage-consistency.md#utilize-session-tokens)
* [Auswählen der richtigen Konsistenzebene](consistency-levels-choosing.md)
* [Kompromisse in Bezug auf Konsistenz, Verfügbarkeit und Leistung](consistency-levels-tradeoffs.md)
* [Kompromisse in Bezug auf Verfügbarkeit und Leistung für verschiedene Konsistenzebenen](consistency-levels-tradeoffs.md)
