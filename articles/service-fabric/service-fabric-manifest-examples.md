---
title: Beispiele für Azure Service Fabric-Anwendungsmanifeste
description: Erfahren Sie, wie Sie die Einstellungen für das Anwendungs- und Dienstmanifest für eine Service Fabric-Anwendung konfigurieren.
author: peterpogorski
ms.topic: conceptual
ms.date: 06/11/2018
ms.author: pepogors
ms.openlocfilehash: 6f44d2ddaac11545a3a3dc52fd20e02263c6ad67
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "75451643"
---
# <a name="service-fabric-application-and-service-manifest-examples"></a>Beispiele für Service Fabric-Anwendungs- und -Dienstmanifeste
Dieser Abschnitt enthält Beispiele für Anwendungs- und Dienstmanifeste. Diese Beispiele sollen keine wichtigen Szenarien zeigen, sondern die verschiedenen verfügbaren Einstellungen und deren Verwendung. 

Im Folgenden finden Sie einen Index der gezeigten Features und die Beispielmanifeste, zu denen sie gehören.

|Funktion|Manifest|
|---|---|
|[Ressourcenkontrolle](service-fabric-resource-governance.md)|[Reliable Services-Anwendungsmanifest](service-fabric-manifest-example-reliable-services-app.md#application-manifest), [Containeranwendungsmanifest](service-fabric-manifest-example-container-app.md#application-manifest)|
|[Ausführen eines Diensts als lokales Administratorkonto](service-fabric-application-runas-security.md)|[Reliable Services-Anwendungsmanifest](service-fabric-manifest-example-reliable-services-app.md#application-manifest)|
|[Anwenden einer Standardrichtlinie auf alle Dienstcodepakete](service-fabric-application-runas-security.md#apply-a-default-policy-to-all-service-code-packages)|[Reliable Services-Anwendungsmanifest](service-fabric-manifest-example-reliable-services-app.md#application-manifest)|
|[Erstellen von Benutzer- und Gruppenprinzipalen](service-fabric-application-runas-security.md)|[Reliable Services-Anwendungsmanifest](service-fabric-manifest-example-reliable-services-app.md#application-manifest)|
|Freigeben eines Datenpakets für Dienstinstanzen|[Reliable Services-Anwendungsmanifest](service-fabric-manifest-example-reliable-services-app.md#application-manifest)|
|[Überschreiben von Dienstendpunkten](service-fabric-service-manifest-resources.md#overriding-endpoints-in-servicemanifestxml)|[Reliable Services-Anwendungsmanifest](service-fabric-manifest-example-reliable-services-app.md#application-manifest)|
|[Ausführen eines Skripts beim Dienststart](service-fabric-run-script-at-service-startup.md)|[VotingWeb-Dienstmanifest](service-fabric-manifest-example-reliable-services-app.md#votingweb-service-manifest)|
|[Definieren eines HTTPS-Endpunkts](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md#define-an-https-endpoint-in-the-service-manifest)|[VotingWeb-Dienstmanifest](service-fabric-manifest-example-reliable-services-app.md#votingweb-service-manifest)|
|[Deklarieren eines Konfigurationspakets](service-fabric-application-and-service-manifests.md)|[VotingData-Dienstmanifest](service-fabric-manifest-example-reliable-services-app.md#votingdata-service-manifest)|
|[Deklarieren eines Datenpakets](service-fabric-application-and-service-manifests.md)|[VotingData-Dienstmanifest](service-fabric-manifest-example-reliable-services-app.md#votingdata-service-manifest)|
|[Überschreiben von Umgebungsvariablen](service-fabric-get-started-containers.md#configure-and-set-environment-variables)|[Containeranwendungsmanifest](service-fabric-manifest-example-container-app.md#application-manifest)|
|[Konfigurieren der Port-zu-Host-Zuordnung des Containers](service-fabric-get-started-containers.md#configure-container-port-to-host-port-mapping-and-container-to-container-discovery)| [Containeranwendungsmanifest](service-fabric-manifest-example-container-app.md#application-manifest)|
|[Konfigurieren der Authentifizierung der Containerregistrierung](service-fabric-get-started-containers.md#configure-container-repository-authentication)|[Containeranwendungsmanifest](service-fabric-manifest-example-container-app.md#application-manifest)|
|[Festlegen des Isolationsmodus](service-fabric-get-started-containers.md#configure-isolation-mode)|[Containeranwendungsmanifest](service-fabric-manifest-example-container-app.md#application-manifest)|
|[Angeben buildspezifischer Containerimages für das Betriebssystem](service-fabric-get-started-containers.md#specify-os-build-specific-container-images)|[Containeranwendungsmanifest](service-fabric-manifest-example-container-app.md#application-manifest)|
|[Festlegen von Umgebungsvariablen](service-fabric-get-started-containers.md#configure-and-set-environment-variables)|[FrontEndService Dienstmanifest des Containers](service-fabric-manifest-example-container-app.md#frontendservice-service-manifest), [BackEndService-Dienstmanifest des Containers](service-fabric-manifest-example-container-app.md#backendservice-service-manifest)|
|[Konfigurieren eines Endpunkts](service-fabric-get-started-containers.md#configure-communication)|[FrontEndService Dienstmanifest des Containers](service-fabric-manifest-example-container-app.md#frontendservice-service-manifest), [BackEndService-Dienstmanifest des Containers](service-fabric-manifest-example-container-app.md#backendservice-service-manifest), [VotingData-Dienstmanifest](service-fabric-manifest-example-reliable-services-app.md#votingdata-service-manifest)|
|Übergeben von Befehlen an den Container|[FrontEndService-Dienstmanifest des Containers](service-fabric-manifest-example-container-app.md#frontendservice-service-manifest)|
|[Importieren eines Zertifikats in einen Container](service-fabric-securing-containers.md)|[FrontEndService-Dienstmanifest des Containers](service-fabric-manifest-example-container-app.md#frontendservice-service-manifest)|
|[Konfigurieren des Volumetreibers](service-fabric-containers-volume-logging-drivers.md)|[BackEndService-Dienstmanifest des Containers](service-fabric-manifest-example-container-app.md#backendservice-service-manifest)|

