---
title: Sicherheitsempfehlungen für Azure Marketplace-Images | Microsoft-Dokumentation
description: Dieser Artikel enthält Empfehlungen für Images im Marketplace.
services: security
documentationcenter: na
author: terrylanfear
manager: barbkess
ms.assetid: ''
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.date: 01/11/2019
ms.author: terrylan
ms.openlocfilehash: 7c317a0b4fea0c981b227bace00c1b8924fd582c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89536381"
---
# <a name="security-recommendations-for-azure-marketplace-images"></a>Sicherheitsempfehlungen für Azure Marketplace-Images

Ihr Image muss diese Empfehlungen zur Sicherheitskonfiguration erfüllen. Dadurch wird ein hohes Maß an Sicherheit für Images von Partnerlösungen im Azure Marketplace gewährleistet.

Führen Sie vor der Übermittlung immer eine Sicherheitsrisikoerkennung für Ihr Image aus. Wenn Sie ein Sicherheitsrisiko in Ihrem eigenen veröffentlichten Image feststellen, müssen Sie Ihre Kunden unverzüglich über das Sicherheitsrisiko informieren und ihnen mitteilen, wie es behoben werden kann.

## <a name="open-source-based-images"></a>Open-Source-basierte Images

| Category | Prüfen |
| -------- | ----- |
| Sicherheit                                                     | Installieren Sie alle aktuellen Sicherheitspatches für die Linux-Distribution.                                                                                                                                                                                                              |
| Sicherheit                                                     | Befolgen Sie Branchenrichtlinien zum Schutz des VM-Images für die jeweilige Linux-Distribution.                                                                                                                                                                                     |
| Sicherheit                                                     | Die Angriffsfläche wurde minimiert, indem nur die absolut erforderlichen Windows Server-Rollen, -Features, -Dienste und Netzwerkports aktiviert wurden.                                                                                                                                               |
| Sicherheit                                                     | Quellcode und resultierendes VM-Image wurden auf Schadsoftware gescannt.                                                                                                                                                                                                                                   |
| Sicherheit                                                     | Das VHD-Image enthält nur erforderliche gesperrte Konten, die über keine Standardkennwörter verfügen, die eine interaktive Anmeldung zulassen würden; keine Hintertüren.                                                                                                                                           |
| Sicherheit                                                     | Deaktivieren Sie alle Firewallregeln, die nicht für die Funktionalität der Anwendung benötigt werden, z.B. für eine Firewall-Appliance.                                                                                                                                                                             |
| Sicherheit                                                     | Entfernen Sie alle vertraulichen Informationen aus dem VHD-Image – z.B. SSH-Testschlüssel, Datei der bekannten Hosts, Protokolldateien und nicht benötigte Zertifikate.                                                                                                                                       |
| Sicherheit                                                     | Vermeiden Sie die Verwendung von LVM.                                                                                                                                                                                                                                            |
| Sicherheit                                                     | Fügen Sie die neuesten Versionen der erforderlichen Bibliotheken hinzu: </br> - OpenSSL v1.0 oder höher </br> - Python 2.5 oder höher (Python 2.6+ wird dringend empfohlen) </br> - Python-Paket pyasn1, falls nicht bereits installiert </br> - d.OpenSSL v1.0 oder höher                                                                |
| Sicherheit                                                     | Löschen Sie Bash/Shell-Verlaufseinträge.                                                                                                                                                                                                                                             |
| Netzwerk                                                   | Beziehen Sie den SSH Server standardmäßig ein. Legen Sie SSH-Keep-Alive mit der folgenden Option auf „sshd config“ fest: ClientAliveInterval 180.                                                                                                                                                        |
| Netzwerk                                                   | Entfernen Sie alle benutzerdefinierten Netzwerkkonfigurationen aus dem Image. Löschen Sie die Datei „resolv.conf“: `rm /etc/resolv.conf`.                                                                                                                                                                                |
| Bereitstellung                                                   | Installieren Sie den neuesten Azure Linux Agent.</br> – Installation mit dem RPM- oder Deb-Paket.  </br> - Sie können auch den manuellen Installationsprozess verwenden, aber die Installationspakete werden empfohlen und bevorzugt. </br> - Wenn der Agent manuell aus dem GitHub-Repository installiert wird, kopieren Sie zunächst die Datei `waagent` nach `/usr/sbin`, und führen Sie (als root) Folgendes aus: </br>`# chmod 755 /usr/sbin/waagent` </br>`# /usr/sbin/waagent -install` </br>Die Agent-Konfigurationsdatei wird unter `/etc/waagent.conf` gespeichert. |
| Bereitstellung                                                   | Stellen Sie sicher, dass der Azure-Support unseren Partnern bei Bedarf die serielle Konsolenausgabe übermitteln und ein geeignetes Timeout für die Einbindung von Betriebssystem-Datenträgern aus Cloudspeicher bereitstellen kann. Fügen Sie die folgenden Parameter zur Startzeile für den Kernel hinzu: `console=ttyS0 earlyprintk=ttyS0 rootdelay=300` |
| Bereitstellung                                                   | Keine swap-Partition auf dem Betriebssystem-Datenträger. Swap kann durch den Linux-Agent für die Erstellung auf dem Datenträger der lokalen Ressource angefordert werden.         |
| Bereitstellung                                                   | Erstellen Sie eine einzelne Stammpartition für den Betriebssystemdatenträger.      |
| Bereitstellung                                                   | Nur 64-Bit-Gastbetriebssysteme.                                                                                                                                                                                                                                                          |

## <a name="windows-server-based-images"></a>Windows Server-basierte Images

| Category | Prüfen |
|--------- | ----- |
| Sicherheit                                                         | Es muss ein sicheres Basisimage für das Betriebssystem verwendet werden. Die für die Quelle von Images auf Basis von Windows Server verwendete virtuelle Festplatte (VHD) muss mit den Windows Server-Betriebssystemimages, die über Microsoft Azure bereitgestellt werden, erstellt werden. |
| Sicherheit                                                         | Es müssen alle aktuellen Sicherheitsupdates installiert sein.                                                                                                                                     |
| Sicherheit                                                         | Anwendungen sollten nicht von eingeschränkten Benutzernamen wie „administrator“, „root“ oder „admin“ abhängig sein.                                                                |
| Sicherheit                                                         | Aktivieren Sie die BitLocker-Laufwerkverschlüsselung sowohl für Betriebssystem-Festplatten als auch für Datenfestplatten.                                                             |
| Sicherheit                                                         | Die Angriffsfläche wurde minimiert, indem nur die absolut erforderlichen Windows Server-Rollen, -Features, -Dienste und Netzwerkports aktiviert wurden.                         |
| Sicherheit                                                         | Quellcode und resultierendes VM-Image wurden auf Schadsoftware gescannt.                                                                                                                     |
| Sicherheit                                                         | Für die Aktualisierung der Sicherheit von Windows Server-Images sollten automatische Updates festgelegt werden.                                                                                                                |
| Sicherheit                                                         | Das VHD-Image enthält nur erforderliche gesperrte Konten, die über keine Standardkennwörter verfügen, die eine interaktive Anmeldung zulassen würden; keine Hintertüren.                             |
| Sicherheit                                                         | Deaktivieren Sie alle Firewallregeln, die nicht für die Funktionalität der Anwendung benötigt werden, z.B. für eine Firewall-Appliance.                                                               |
| Sicherheit                                                         | Entfernen Sie alle vertraulichen Informationen aus dem VHD-Image – z.B. HOST-Dateien, Protokolldateien und nicht benötigte Zertifikate.                                              |
| Bereitstellung                                                       | Nur 64-Bit-Gastbetriebssysteme.                            |

Auch wenn Ihre Organisation über keine Images im Azure Marketplace verfügt, sollten Sie die Konfiguration Ihrer Windows- und Linux-Images anhand dieser Empfehlungen überprüfen.

