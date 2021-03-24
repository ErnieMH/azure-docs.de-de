---
title: Ändern der KVSActorStateProvider-Einstellungen
description: Erfahren Sie mehr über das Konfigurieren von statusbehafteten Azure Service Fabric Actors vom Typ „KVSActorStateProvider“
author: sumukhs
ms.topic: conceptual
ms.date: 10/2/2017
ms.author: sumukhs
ms.openlocfilehash: cdb115bd57cf3d5af4388f4efa03c2522feef9ca
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "75609773"
---
# <a name="configuring-reliable-actors--kvsactorstateprovider"></a>Konfigurieren von Reliable Actors – KVSActorStateProvider
Sie können die Standardkonfiguration von KVSActorStateProvider ändern, indem Sie die Datei „settings.xml“, die im Stammverzeichnis des Visual Studio-Pakets im Ordner „Config“ generiert wurde, für den betreffenden Actor ändern.

Standardmäßig sucht die Azure Service Fabric-Laufzeit in der Datei „settings.xml“ nach vordefinierten Abschnittsnamen und nutzt die Konfigurationswerte beim Erstellen der zugrunde liegenden Laufzeitkomponenten.

> [!NOTE]
> Löschen oder ändern Sie **nicht** die Abschnittsnamen der folgenden Konfigurationen in der Datei „settings.xml“, die in der Visual Studio-Projektmappe generiert wird.
> 
> 

## <a name="replicator-security-configuration"></a>Replicator-Sicherheitskonfiguration
Replicator-Sicherheitskonfigurationen werden verwendet, um den während der Replikation verwendeten Kommunikationskanal zu schützen. Dies bedeutet, dass Dienste ihren gegenseitigen Replikationsdatenverkehr nicht erkennen können. Dadurch wird sichergestellt, dass die Daten nicht nur hochverfügbar, sondern auch sicher sind.
Standardmäßig wird die Replikationssicherheit durch einen leeren Sicherheitskonfigurationsabschnitt verhindert.

> [!IMPORTANT]
> Auf Linux-Knoten müssen Zertifikate das PEM-Format aufweisen. Weitere Informationen zum Ermitteln und Konfigurieren von Zertifikaten für Linux finden Sie unter [Konfigurieren von Zertifikaten in Linux](./service-fabric-configure-certificates-linux.md). 
> 

### <a name="section-name"></a>Name des Abschnitts
&lt;ActorName&gt;ServiceReplicatorSecurityConfig

## <a name="replicator-configuration"></a>Replicator-Konfiguration
Replicator-Konfigurationen konfigurieren den Replicator, der dafür verantwortlich ist, den Status des Actor-Status-Anbieters hochverfügbar zu machen.
Die Standardkonfiguration wird von der Visual Studio-Vorlage generiert und sollte ausreichen. Dieser Abschnitt befasst sich mit zusätzlichen Konfigurationen, die zum Optimieren des Replicators verfügbar sind.

### <a name="section-name"></a>Name des Abschnitts
&lt;ActorName&gt;ServiceReplicatorConfig

### <a name="configuration-names"></a>Konfigurationsnamen
| Name | Einheit | Standardwert | Bemerkungen |
| --- | --- | --- | --- |
| BatchAcknowledgementInterval |Sekunden |0,015 |So lange wartet der Replicator auf dem sekundären Replicator nach dem Empfang eines Vorgangs, bevor er eine Bestätigung an den primären Replicator sendet. Alle anderen Bestätigungen, die für innerhalb dieses Intervalls verarbeitete Vorgänge gesendet werden, werden als eine einzelne Antwort gesendet. |
| ReplicatorEndpoint |– |Kein Standardwert – Erforderlicher Parameter |Die IP-Adresse und der Port, die der primäre/sekundäre Replicator für die Kommunikation mit anderen Replicatoren in der Replikatgruppe verwendet. Dabei sollte im Dienstmanifest auf einen TCP-Ressourcenendpunkt verwiesen werden. Weitere Informationen zum Definieren von Endpunktressourcen im Dienstmanifest finden Sie unter [Dienstmanifestressourcen](service-fabric-service-manifest-resources.md) . |
| RetryInterval |Sekunden |5 |Der Zeitraum, nach dem der Replicator eine Nachricht erneut überträgt, wenn er keine Bestätigung für einen Vorgang erhält. |
| MaxReplicationMessageSize |Byte |50 MB |Die maximale Größe der Replikationsdaten, die in einer einzelnen Nachricht übertragen werden können. |
| MaxPrimaryReplicationQueueSize |Anzahl der Vorgänge |1024 |Die maximale Anzahl der Vorgänge in der primären Warteschlange. Ein Vorgang wird freigegeben, nachdem der primäre Replicator eine Bestätigung von allen sekundären Replicators empfangen hat. Dieser Wert muss größer als 64 und eine Potenz von 2 sein. |
| MaxSecondaryReplicationQueueSize |Anzahl der Vorgänge |2048 |Die maximale Anzahl der Vorgänge in der sekundären Warteschlange. Ein Vorgang wird freigegeben, nachdem sein Zustand durch Persistenz hochverfügbar gemacht wurde. Dieser Wert muss größer als 64 und eine Potenz von 2 sein. |

## <a name="store-configuration"></a>Speicherkonfiguration
Speicherkonfigurationen werden zum Konfigurieren des lokalen Speichers verwendet, der zum Beibehalten des zu replizierenden Status verwendet wird.
Die Standardkonfiguration wird von der Visual Studio-Vorlage generiert und sollte ausreichen. Dieser Abschnitt befasst sich mit zusätzlichen Konfigurationen, die zum Optimieren des lokalen Speichers verfügbar sind.

### <a name="section-name"></a>Name des Abschnitts
&lt;ActorName&gt;ServiceLocalStoreConfig

### <a name="configuration-names"></a>Konfigurationsnamen
| Name | Einheit | Standardwert | Bemerkungen |
| --- | --- | --- | --- |
| MaxAsyncCommitDelayInMilliseconds |Millisekunden |200 |Legt das maximale Batchverarbeitungsintervall für permanente Commits des lokalen Speichers fest. |
| MaxVerPages |Anzahl von Seiten |16384 |Die maximale Anzahl von Versionsseiten in der lokalen Speicherdatenbank. Sie bestimmt die maximale Anzahl von ausstehenden Transaktionen. |

## <a name="sample-configuration-file"></a>Beispiel für eine Konfigurationsdatei
```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="MyActorServiceReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="MyActorServiceReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
   </Section>
   <Section Name="MyActorServiceLocalStoreConfig">
      <Parameter Name="MaxVerPages" Value="8192" />
   </Section>
   <Section Name="MyActorServiceReplicatorSecurityConfig">
      <Parameter Name="CredentialType" Value="X509" />
      <Parameter Name="FindType" Value="FindByThumbprint" />
      <Parameter Name="FindValue" Value="9d c9 06 b1 69 dc 4f af fd 16 97 ac 78 1e 80 67 90 74 9d 2f" />
      <Parameter Name="StoreLocation" Value="LocalMachine" />
      <Parameter Name="StoreName" Value="My" />
      <Parameter Name="ProtectionLevel" Value="EncryptAndSign" />
      <Parameter Name="AllowedCommonNames" Value="My-Test-SAN1-Alice,My-Test-SAN1-Bob" />
   </Section>
</Settings>
```
## <a name="remarks"></a>Bemerkungen
Der Parameter „BatchAcknowledgementInterval“ steuert die Replikationslatenz. Der Wert "0" ergibt die geringstmögliche Latenz, allerdings auf Kosten des Durchsatzes (da eine größer Anzahl von Bestätigungsnachrichten gesendet und verarbeitet werden muss, von denen jede weniger Bestätigungen enthält).
Je größer der Wert für "BatchAcknowledgementInterval" ist, um so höher ist der Gesamtdurchsatz der Replikation, zu Lasten einer höheren Vorgangslatenz. Daraus ergibt sich direkt die Latenz von Transaktions-Commits.

