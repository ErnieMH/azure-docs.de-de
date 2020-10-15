---
title: Hinzufügen von Integritätstests zu Ihren AKS-Pods
description: Dieser Artikel enthält Informationen zum Hinzufügen von Integritätstests (Bereitschafts- und/oder Livetests) zu AKS-Pods mit einem Application Gateway.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: how-to
ms.date: 11/4/2019
ms.author: caya
ms.openlocfilehash: 8c8b8b0090877db7abc8fae0e44f928e8b10dcf5
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "84808004"
---
# <a name="add-health-probes-to-your-service"></a>Hinzufügen von Integritätstests zu Ihrem Dienst
Standardmäßig stellt der Eingangscontroller einen HTTP GET-Test für die verfügbar gemachten Pods bereit.
Die Testeigenschaften können angepasst werden, indem Sie Ihrer `deployment`/`pod`-Spezifikation einen [Bereitschafts- oder Livetest](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) hinzufügen.

## <a name="with-readinessprobe-or-livenessprobe"></a>Mit `readinessProbe` oder `livenessProbe`
```yaml
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: aspnetapp
spec:
  replicas: 3
  template:
    metadata:
      labels:
        service: site
    spec:
      containers:
      - name: aspnetapp
        image: mcr.microsoft.com/dotnet/core/samples:aspnetapp
        imagePullPolicy: IfNotPresent
        ports:
        - containerPort: 80
        readinessProbe:
          httpGet:
            path: /
            port: 80
          periodSeconds: 3
          timeoutSeconds: 1
```

Kubernetes-API-Referenz:
* [Containertests](https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/#container-probes)
* [HttpGet-Aktion](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.18/#httpgetaction-v1-core)

> [!NOTE]
> * `readinessProbe` und `livenessProbe` werden unterstützt, wenn sie mit `httpGet`konfiguriert sind.
> * Das Testen an einem anderen Port als dem, der auf dem Pod verfügbar gemacht wird, wird zurzeit nicht unterstützt.
> * `HttpHeaders`, `InitialDelaySeconds` und `SuccessThreshold` werden nicht unterstützt.

##  <a name="without-readinessprobe-or-livenessprobe"></a>Ohne `readinessProbe` oder `livenessProbe`
Wenn die oben genannten Tests nicht bereitgestellt werden, geht der Eingangscontroller davon aus, dass der Dienst im `Path` erreichbar ist, der für die `backend-path-prefix`-Anmerkung oder den `path` angegeben ist, der in der `ingress`-Definition für den Dienst angegeben wird.

## <a name="default-values-for-health-probe"></a>Standardwerte für den Integritätstest
Für alle Eigenschaften, die nicht vom Bereitschafts-/Livetest abgeleitet werden können, werden Standardwerte festgelegt.

| Application Gateway-Testeigenschaft | Standardwert |
|-|-|
| `Path` | / |
| `Host` | localhost |
| `Protocol` | HTTP |
| `Timeout` | 30 |
| `Interval` | 30 |
| `UnhealthyThreshold` | 3 |