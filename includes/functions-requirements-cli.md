---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/25/2020
ms.author: glenga
ms.openlocfilehash: e969520e226999bc2b104fb4f757619832eaa165
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "88944462"
---
## <a name="configure-your-local-environment"></a>Konfigurieren Ihrer lokalen Umgebung

Bevor Sie mit diesem Lernprogramm beginnen können, benötigen Sie Folgendes:

+ Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

::: zone pivot="programming-language-csharp,programming-language-javascript,programming-language-typescript,programming-language-powershell,programming-language-java"  
+ [Azure Functions Core Tools](../articles/azure-functions/functions-run-local.md#v2), Version 2.7.1846 oder höhere 2.x-Version.
::: zone-end  
::: zone pivot="programming-language-python"
+ Die Azure Functions Core Tools-Version, die Ihrer installierten Python-Version entspricht:

   | Python-Version | Core Tools-Version |
   | -------------- | ------------------ |
   | Python 3.8     | [Version 3.x](../articles/azure-functions/functions-run-local.md#v2) |
   | Python 3.6<br/>Python 3.7 | [Version 2.7.1846 oder höher](../articles/azure-functions/functions-run-local.md#v2) |
  
::: zone-end

+ [Azure-Befehlszeilenschnittstelle](/cli/azure/install-azure-cli) ab Version 2.4 
::: zone pivot="programming-language-javascript,programming-language-typescript"
+ [Node.js](https://nodejs.org/), Active LTS- und Maintenance LTS-Versionen (8.11.1 und 10.14.1 empfohlen)
::: zone-end

::: zone pivot="programming-language-python"
+ [Python 3.8 (64 Bit)](https://www.python.org/downloads/release/python-382/), [Python 3.7 (64 Bit)](https://www.python.org/downloads/release/python-375/), [Python 3.6 (64 Bit)](https://www.python.org/downloads/release/python-368/). Diese Versionen werden von Azure Functions unterstützt. 
::: zone-end
::: zone pivot="programming-language-powershell"
+ [.NET Core SDK 3.1](https://www.microsoft.com/net/download)
::: zone-end
::: zone pivot="programming-language-java"  
+ [Java Developer Kit](https://aka.ms/azure-jdks), Version 8 

    > [!IMPORTANT]
    > + Die Functions-Unterstützung für Java 11 befindet sich derzeit in der Vorschauphase, und mit dem Maven-Archetyp wird standardmäßig eine Java 8-Bereitstellung erstellt. Wenn Sie Ihre Funktions-App stattdessen unter Java 11 ausführen möchten, müssen Sie die Datei „pom.xml“ manuell mit Java 11-Werten aktualisieren. Weitere Informationen finden Sie unter [Java-Versionen](../articles/azure-functions/functions-reference-java.md#java-versions). 
    > + Damit Sie diesen Schnellstart durchführen können, muss die Umgebungsvariable `JAVA_HOME` auf den Installationsspeicherort der richtigen Version des JDK festgelegt sein.

+ [Apache Maven](https://maven.apache.org), Version 3.0 oder höher

::: zone-end
