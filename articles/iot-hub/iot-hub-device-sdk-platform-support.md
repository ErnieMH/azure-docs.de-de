---
title: Azure IoT-Geräte-SDKs – Plattformunterstützung | Microsoft-Dokumentation
description: Open-Source-Geräte stehen auf GitHub in C, .NET (C#), Java, Node.js und Python zur Verfügung, um Geräte mit Azure IoT Hub und dem Device Provisioning Service (DPS) zu verbinden.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 10/08/2019
ms.author: robinsh
ms.openlocfilehash: ab450c6ec6ae32cafcf07e88940736f209973d8d
ms.sourcegitcommit: d23602c57d797fb89a470288fcf94c63546b1314
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/01/2021
ms.locfileid: "106168041"
---
# <a name="azure-iot-device-sdks-platform-support"></a>Azure IoT-Geräte-SDKs – Plattformunterstützung

Microsoft ist bestrebt, das Universum von Azure IoT Hub-fähigen Geräten kontinuierlich zu erweitern. Microsoft veröffentlicht Open-Source-Geräte-SDKs auf GitHub als Hilfe beim Verbinden von Geräten mit Azure IoT Hub und dem Device Provisioning Service. Die Geräte-SDKs stehen für C, .NET (C#), Java, Node.js und Python zur Verfügung. Microsoft testet jedes SDK, um sicherzustellen, dass es auf den unterstützten Konfigurationen ausgeführt wird, deren Details im Abschnitt [Microsoft-Unterstützung von SDKs und Geräteplattformen](#microsoft-sdks-and-device-platform-support) aufgeführt sind.

Zusätzlich zu den Geräte-SDKs bietet Microsoft mehrere andere Wege, um Kunden und Entwicklern eine Verbindung ihrer Geräte mit Azure IoT zu ermöglichen:

* Microsoft arbeitet mit mehreren Partnerunternehmen zusammen, um ihnen bei der Veröffentlichung von Development Kits auf der Grundlage des Azure IoT C-SDK für ihre eigenen Hardwareplattformen zu helfen.

* Microsoft arbeitet mit vertrauenswürdigen Microsoft-Partnern zusammen, um eine stetig erweiterte Gruppe von Geräten bereitzustellen, die für Azure IoT getestet und zertifiziert wurden. Eine aktuelle Liste dieser Geräte finden Sie im [Azure Certified for IoT-Gerätekatalog](https://devicecatalog.azure.com/).

* Microsoft stellt im Azure IoT Hub-Geräte-C SDK eine Plattformabstraktionsschicht (Platform Abstraction Layer, PAL) bereit, die Entwickler zum einfachen Portieren des SDKs auf ihre jeweilige Plattform verhilft. Weitere Informationen finden Sie im [C SDK porting guidance](https://github.com/Azure/azure-c-shared-utility/blob/master/devdoc/porting_guide.md) (Leitfaden für C SDK-Portierung).

Dieses Thema bietet Informationen zu den Microsoft-SDKs und den von ihnen unterstützten Plattformkonfigurationen sowie zu jeder der oben aufgelisteten anderen Optionen.

## <a name="microsoft-sdks-and-device-platform-support"></a>Microsoft-Unterstützung von SDKs und Geräteplattformen

Microsoft veröffentlicht Open-Source-SDKs auf GitHub für die folgenden Sprachen: C, .NET (C#), Node.js, Java und Python. Die SDKs und deren Abhängigkeiten sind in diesem Abschnitt aufgelistet. Die SDKs werden auf jeder Geräteplattform unterstützt, die diese Abhängigkeiten erfüllt.

Bei jedem der aufgelisteten SDKs sorgt Microsoft für Folgendes:

* Erstellt kontinuierlich Builds und führt End-to-End-Tests für den Mainbranch des relevanten SDKs in GitHub auf mehreren beliebten Plattformen durch.  Um eine Testabdeckung in verschiedenen Compilerversionen bereitzustellen, führen wir Tests in der Regel für die neueste LTS-Version und die am häufigsten verwendete Version durch.

* Stellt ggf. Installationsleitfäden oder Installationspakete bereit.

* Bietet vollständige Unterstützung für die SDKs auf GitHub – mit Open-Source-Code, einem Pfad für Kundenbeiträge und Einbindung des Produktteams bei GitHub-Problemen.

### <a name="c-sdk"></a>C-SDK

Das [Azure IoT Hub C-Geräte-SDK](https://github.com/Azure/azure-iot-sdk-c) wird mit den folgenden Konfigurationen getestet und unterstützt sie.

| OS                  | TLS-Bibliothek                  | Zusätzliche Anforderungen                                                                     |
|---------------------|------------------------------|---------------------------------------------------------------------------------------------|
| Linux               | OpenSSL, WolfSSL oder BearSSL | Berkeley-Sockets</br></br>Portable Operating System Interface (POSIX)                       |
| iOS 12.2            | OpenSSL                      | XCode, in OSX 10.13.4 emuliert                                                               |
| Windows 10-Familie   | SChannel                     |                                                                                             |
| Mbed OS 5.4         | Mbed TLS 2                   | [MXChip IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/)                  |
| Azure Sphere-Betriebssystem     | WolfSSL                      | [Azure Sphere MT3620](https://azure.microsoft.com/services/azure-sphere/get-started/) |
| Arduino             | BearSSL                      | [ESP32 oder ESP8266](https://github.com/Azure/azure-iot-arduino#simple-sample-instructions) 

### <a name="python-sdk"></a>Python SDK

Das [Azure IoT Hub Python-Geräte-SDK](https://github.com/Azure/azure-iot-sdk-python) wird mit den folgenden Konfigurationen getestet und unterstützt sie.

| OS                  | Compiler                          |
|---------------------|-----------------------------------|
| Linux               | Python 2.7.*, 3.6 oder höher |
| macOS High Sierra   | Python 2.7.*, 3.6 oder höher |
| Windows 10-Familie   | Python 2.7.*, 3.6 oder höher |

Die asynchronen APIs werden nur von Python-Version 3.5.3 oder höher unterstützt. Wir empfehlen Ihnen, mindestens Version 3.7 zu verwenden.

### <a name="net-sdk"></a>.NET SDK

Das [Azure IoT Hub .NET (C#)-Geräte-SDK](https://github.com/Azure/azure-iot-sdk-csharp) wird mit den folgenden Konfigurationen getestet und unterstützt sie.

| OS                                   | Standard                                                   |
|--------------------------------------|------------------------------------------------------------|
| Linux                                | .NET Core 2.1                                              |
| Windows 10 Desktop- und Server-SKUs   | .NET Core 2.1, .NET Framework 4.5.1 oder .NET Framework 4.7 |

Das .NET SDK kann auch mit Windows IoT Core mit dem [Azure-Geräte-Agent](https://github.com/ms-iot/azure-client-tools/blob/master/docs/device-agent/device-agent.md) oder [einem benutzerdefinierten NT-Dienst, der per RPC mit UWP-Anwendungen kommunizieren kann](/samples/microsoft/windows-iotcore-samples/ntservice-rpc/), verwendet werden.

### <a name="nodejs-sdk"></a>Node.js SDK

Das [Azure IoT Hub Node.js-Geräte-SDK](https://github.com/Azure/azure-iot-sdk-node) wird mit den folgenden Konfigurationen getestet und unterstützt sie.

| OS                  | Node-Version    |
|---------------------|-----------------|
| Linux               | LTS und aktuell |
| Windows 10-Familie   | LTS und aktuell |

### <a name="java-sdk"></a>Java-SDK

Das [Azure IoT Hub Java-Geräte-SDK](https://github.com/Azure/azure-iot-sdk-java) wird mit den folgenden Konfigurationen getestet und unterstützt sie.

| OS                     | Java-Version |
|------------------------|--------------|
| Android API 28         | Java 8       |
| Linux x64             | Java 8       |
| Windows 10-Familie x64  | Java 8       |

## <a name="partner-supported-development-kits"></a>Unterstützte Partner-Development Kits

Microsoft arbeitet mit verschiedenen Partnern zusammen, um Development Kits für mehrere Mikroprozessorarchitekturen bereitzustellen. Diese Partner haben das Azure IoT-SDK auf ihre Plattform portiert. Partner erstellen und verwalten die Plattformabstraktionsschicht (Platform Abstraction Layer, PAL) des SDK. Microsoft arbeitet mit diesen Partnern zusammen, um erweiterte Unterstützung zu bieten.

| Partner             | Geräte                            | Link                     | Support |
|---------------------|------------------------------------|--------------------------|---------|
| Espressif           | ESP32 <br/> ESP8266                              | [Esp-azure](https://github.com/espressif/esp-azure)                | [GitHub](https://github.com/espressif/esp-azure)  
| Qualcomm            | Qualcomm MDM9206 LTE IoT Modem     | [Qualcomm LTE für IoT-SDK](https://developer.qualcomm.com/software/lte-iot-sdk) | [Forum](https://developer.qualcomm.com/forums/software/lte-iot-sdk)   |
| ST Microelectronics | STM32L4-Serie <br/> STM32F4-Serie <br/>  STM32F7-Serie <br/>  STM32L4 Discovery Kit für IoT-Knoten    | [X-CUBE-AZURE](https://www.st.com/en/embedded-software/x-cube-azure.html) <br/>  <br/> [P-NUCLEO-AZURE](https://www.st.com/content/st_com/en/products/evaluation-tools/solution-evaluation-tools/communication-and-connectivity-solution-eval-boards/p-nucleo-azure1.html) <br/> [FP-CLD-AZURE](https://www.st.com/content/st_com/en/products/embedded-software/mcus-embedded-software/stm32-embedded-software/stm32-ode-function-pack-sw/fp-cld-azure1.html)            | [Unterstützung](https://www.st.com/content/st_com/en/support/support-home.html)
| Texas Instruments   | CC3220SF LaunchPad </br> CC3220S LaunchPad </br> CC3235SF LaunchPad </br> CC3235S LaunchPad </br> MSP432E4 LaunchPad | [Azure IoT-Plug-In für SimpleLink](https://github.com/TexasInstruments/azure-iot-pal-simplelink) | [TI E2- Forum](https://e2e.ti.com) <br/> [TI E2E-Forum für CC3220](https://e2e.ti.com/support/wireless_connectivity/simplelink_wifi_cc31xx_cc32xx/) <br/> [TI E2E-Forum für MSP432E4](https://e2e.ti.com/support/microcontrollers/msp430/) |

## <a name="porting-the-microsoft-azure-iot-c-sdk"></a>Portieren des Microsoft Azure IoT C SDK

Wenn Ihre Geräteplattform in keinem der vorstehenden Abschnitte beschrieben wird, können Sie eine Portierung des Azure IoT C SDK in Erwägung ziehen. Dieser Vorgang umfasst in erster Linie die Implementierung der Plattformabstraktionsschicht (Platform Abstraction Layer, PAL) des SDK. Die PAL definiert Grundtypen, die für eine Verbindung zwischen Ihrem Gerät und übergeordneten Funktionen im SDK sorgen. Weitere Informationen finden Sie im [Porting Guidance](https://github.com/Azure/azure-c-shared-utility/blob/master/devdoc/porting_guide.md) (Leitfaden für Portierung).

## <a name="microsoft-partners-and-certified-azure-iot-devices"></a>Microsoft-Partner und zertifizierte Azure IoT-Geräte

Microsoft arbeitet mit einer Reihe von Partnern zusammen, um das Azure IoT-Universum mit getesteten und zertifizierten Azure IoT-Geräten kontinuierlich zu erweitern.

* Informationen zum Navigieren zu zertifizierten Azure IoT-Geräten finden Sie im [Microsoft Azure Certified for IoT-Gerätekatalog](https://devicecatalog.azure.com/).

* Weitere Informationen zum Azure Certified for IoT-Ökosystem finden Sie unter [Treten Sie dem Certified für IoT-Ökosystem bei](../certification/overview.md).

## <a name="connecting-to-iot-hub-without-an-sdk"></a>Herstellen einer Verbindung mit IoT Hub ohne ein SDK

Wenn Ihnen die Verwendung eines der IoT Hub-Geräte-SDKs nicht möglich ist, können Sie mithilfe eines unterstützten Kommunikationsprotokolls eine direkte Verbindung mit IoT Hub herstellen. Weitere Informationen finden Sie unter [Entwickeln ohne ein Azure IoT SDK](iot-hub-devguide-no-sdk.md).

## <a name="support-and-other-resources"></a>Support und weitere Ressourcen

Wenn bei der Verwendung der Azure IoT-Geräte-SDKs Probleme auftreten, gibt es mehrere Möglichkeiten, um Support zu erhalten. Diese werden im Folgenden vorgestellt. Vollständige Informationen zu allen Supportoptionen finden Sie unter [Support- und Hilfeoptionen für Azure IoT](../iot-fundamentals/iot-support-help.md). 

**Melden von Fehlern** – Fehler in den Geräte-SDKs können auf der Problemseite des relevanten GitHub-Projekts gemeldet werden. Fehlerbehebungen aus dem Projekt werden zeitnah in Produktupdates integriert.

* [Probleme beim Azure IoT Hub C SDK](https://github.com/Azure/azure-iot-sdk-c/issues)

* [Probleme beim Azure IoT Hub .NET (C#) SDK](https://github.com/Azure/azure-iot-sdk-csharp/issues)

* [Probleme beim Azure IoT Hub Java SDK](https://github.com/Azure/azure-iot-sdk-java/issues)

* [Probleme beim Azure IoT Hub Node.js SDK](https://github.com/Azure/azure-iot-sdk-node/issues)

* [Probleme beim Azure IoT Hub Python SDK](https://github.com/Azure/azure-iot-sdk-python/issues)

**Technische Fragen**: Sie können technische Fragen sowohl auf der Seite [Microsoft Q&A](/answers/topics/azure-iot-sdk.html) als auch im [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-iot-sdk)-Forum stellen. Verwenden Sie dafür das Tag *azure-iot-sdk*.

**Microsoft-Kundensupportteam** – Benutzer mit einem [Supportplan](https://azure.microsoft.com/support/plans/) können direkt über das [Azure-Portal](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) eine neue Supportanfrage erstellen, um sich an das Microsoft-Kundensupportteam zu wenden.

**Funktionsanfragen** – Azure IoT-Funktionsanfragen werden über die [User Voice-Seite](https://feedback.azure.com/forums/321918-azure-iot) für das Produkt nachverfolgt.

## <a name="next-steps"></a>Nächste Schritte

* [Geräte- und Dienst-SDKs](iot-hub-devguide-sdks.md)
* [Porting Guidance](https://github.com/Azure/azure-c-shared-utility/blob/master/devdoc/porting_guide.md) (Leitfaden für Portierung)
