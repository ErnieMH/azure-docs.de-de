---
title: Übersicht über Consul
description: Hier finden Sie eine Übersicht über Consul.
author: paulbouwer
ms.topic: article
ms.date: 10/09/2019
ms.author: pabouwer
ms.openlocfilehash: 77cb6a693c5a73d0498b0acf9bc4ad8cc9f8f32f
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "83774000"
---
# <a name="consul"></a>Consul

## <a name="overview"></a>Übersicht

[Consul][consul] ist eine Dienstnetzwerklösung für mehreren Rechenzentren, mit der Sie Dienste über Runtimeplattformen hinweg verbinden und schützen können. [Connect][consul-features] ist die Komponente, die Dienstmesh-Funktionen bereitstellt.

## <a name="architecture"></a>Aufbau

Consul stellt eine Datenebene bereit, die standardmäßig aus [Envoy][envoy-proxy]-basierten [Sidecars][consul-sidecar] besteht. Consul besitzt eine austauschbare Proxyarchitektur. Diese intelligenten Proxys steuern den gesamten ein- und ausgehenden Netzwerkdatenverkehr Ihrer Mesh-Apps und -Workloads.

Die Steuerungsebene verwaltet die Konfiguration und die Richtlinie über die folgenden [Komponenten][consul-architecture]:

- **Server:** Ein im Servermodus ausgeführter Consul-Agent zur Verwaltung des Consul-Clusterzustands.

- **Client:** Ein im einfachen Clientmodus ausgeführter Consul-Agent. Auf jedem Computeknoten muss ein Client-Agent ausgeführt werden. Dieser Client handelt die Konfiguration und die Richtlinie zwischen den Workloads und der Consul-Konfiguration aus. 

Das folgende Architekturdiagramm veranschaulicht, wie die verschiedenen Komponenten auf der Daten- und Steuerungsebene interagieren.

![Übersicht über die Consul-Komponenten und die Architektur](media/servicemesh/consul/about-architecture.png)


## <a name="selection-criteria"></a>Auswahlkriterien

Beim Auswerten von Consul für Ihre Workloads ist es wichtig, die folgenden Bereiche zu verstehen und zu berücksichtigen:

- [Consul-Prinzipien](#consul-principles)
- [Capabilities](#capabilities)
- [Szenarios](#scenarios)


### <a name="consul-principles"></a>Consul-Prinzipien

Das [Consul-Projekt][consul-principles] basiert auf folgenden Prinzipien:

- **API-gesteuert:** Schreiben Sie sämtliche Aspekte der Konfiguration und der Richtlinie als Code.

- **Ortsunabhängige Ausführung und Verbindungsherstellung:** Verbinden Sie Workloads über Runtimeplattformen hinweg (Kubernetes, virtuelle Computer, serverlose Lösungen).

- **Erweiterung und Integration:** Stellen Sie infrastrukturübergreifend eine sichere Verbindung zwischen Workloads her.


### <a name="capabilities"></a>Funktionen

Consul bietet folgende Funktionen:

- **Mesh:** Gateway (für mehrere Rechenzentren), virtuelle Computer (über Clusterknoten), Dienstsynchronisierung, integrierte Debugoption

- **Proxys:** Envoy, integrierter Proxy, austauschbar, verfügbarer L4-Proxy für Windows-Workloads

- **Datenverkehrsverwaltung:** Routing, Aufteilung, Auflösung

- **Richtlinie:** Absichten, ACLs

- **Sicherheit:** Autorisierung, Authentifizierung, Verschlüsselung, SPIFFE-basierte Identitäten, externe Zertifizierungsstelle (Tresor), Zertifikatverwaltung und Rotation

- **Überwachung:** Metriken, UI-Dashboard, Prometheus, Grafana


### <a name="scenarios"></a>Szenarien

Consul wird für folgende Szenarien empfohlen:

- Erweitern vorhandener, mit Consul verbundener Workloads

- Complianceanforderungen im Zusammenhang mit der Zertifikat verwaltung

- Dienstmesh mit mehreren Clustern

- VM-basierte Workloads, die in das Dienstmesh integriert werden sollen



## <a name="next-steps"></a>Nächste Schritte

In der folgenden Dokumentation erfahren Sie, wie Sie Consul in Azure Kubernetes Service (AKS) installieren:

> [!div class="nextstepaction"]
> [Installieren und Verwenden von Consul in Azure Kubernetes Service (AKS)][consul-install]

Weitere Informationen zu Consul-Features und zur Consul-Architektur finden Sie hier:

- [Tutorials zu den ersten Schritten mit Consul][consul-getting-started]
- [Consul-Features][consul-features]
- [Consul-Architektur][consul-architecture]
- [Consul: Funktionsweise von Connect][consul-how-connect-works]

<!-- LINKS - external -->
[consul]: https://www.consul.io/mesh.html
[consul-features]: https://www.consul.io/docs/connect/index.html
[consul-architecture]: https://www.consul.io/docs/internals/architecture.html
[consul-sidecar]: https://www.consul.io/docs/connect/proxies.html
[consul-how-connect-works]: https://www.consul.io/docs/connect/connect-internals.html
[consul-principles]: https://www.consul.io/
[consul-getting-started]:https://learn.hashicorp.com/consul?track=gs-consul-service-mesh#gs-consul-service-mesh

[envoy-proxy]: https://www.envoyproxy.io/
[grafana]: https://grafana.com/
[prometheus]: https://prometheus.io/

<!-- LINKS - internal -->
[consul-install]: ./servicemesh-consul-install.md
