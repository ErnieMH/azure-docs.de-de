---
author: yegu-ms
ms.service: redis-cache
ms.topic: include
ms.date: 11/09/2018
ms.author: yegu
ms.openlocfilehash: 61e93e3700b9a396d2ac4fdcbb51fc5c874cf9cb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "68286270"
---
.NET-Anwendungen können den **StackExchange.Redis**-Cacheclient verwenden. Dieser kann mithilfe eines NuGet-Pakets in Visual Studio konfiguriert werden, um die Konfiguration von Cacheclientanwendungen zu vereinfachen. 

> [!NOTE]
> Weitere Informationen finden Sie auf der [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis)-Seite auf GitHub und in der Dokumentation für den [StackExchange.Azure Cache for Redis-Client](https://github.com/StackExchange/StackExchange.Redis#documentation).
>
>

Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie **NuGet-Pakete verwalten** aus, um eine Clientanwendung in Visual Studio mithilfe des StackExchange.Redis-NuGet-Pakets zu konfigurieren. 

![NuGet-Pakete verwalten](media/redis-cache-configure-stackexchange-redis-nuget/redis-cache-manage-nuget-menu.png)

Geben Sie **StackExchange.Redis** oder **StackExchange.Redis.StrongName** in das Suchfeld ein, wählen Sie die gewünschte Version aus den Ergebnissen aus, und klicken Sie auf **Installieren**.

> [!NOTE]
> Falls Sie eine Version der **StackExchange.Redis**-Clientbibliothek mit starkem Namen bevorzugen, wählen Sie **StackExchange.Redis.StrongName** aus. Wählen Sie andernfalls **StackExchange.Redis** aus.
>
>

![StackExchange.Redis-NuGet-Paket](media/redis-cache-configure-stackexchange-redis-nuget/redis-cache-stackexchange-redis.png)

Das NuGet-Paket wird heruntergeladen und fügt die benötigten Assemblyverweise zu Ihrer Clientanwendung hinzu, um mithilfe des StackExchange.Azure Cache for Redis-Clients auf Azure Cache for Redis zuzugreifen.

> [!NOTE]
> Wenn Sie das Projekt zuvor für die Verwendung von StackExchange.Redis konfiguriert haben, können Sie mithilfe von **NuGet Package Manager**nach Updates für das Paket suchen. Um nach aktualisierten Versionen des NuGet-Pakets „StackExchange.Redis“ zu suchen und diese zu installieren, klicken Sie im Fenster **NuGet Package Manager** auf **Updates**. Wenn ein Update des NuGet-Pakets „StackExchange.Redis“ verfügbar ist, können Sie Ihr Projekt für die Verwendung der aktualisierten Version aktualisieren.
>
>

Sie können das StackExchange.Redis-NuGet-Paket auch installieren, indem Sie im Menü **Tools** auf **NuGet-Paket-Manager** > **Paket-Manager-Konsole** klicken und im Fenster **Paket-Manager-Konsole** den folgenden Befehl ausführen:

```
Install-Package StackExchange.Redis
```
