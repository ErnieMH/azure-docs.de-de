---
title: Ausführen von Befehlen in einer ausgeführten Containerinstanz
description: Erfahren Sie, wie ein Befehl in einem Container ausgeführt wird, der zurzeit in Azure Container Instances ausgeführt wird
ms.topic: article
ms.date: 03/30/2018
ms.openlocfilehash: de48e6ac246e2b0751561b4c60bb63d88b599bdf
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "79225846"
---
# <a name="execute-a-command-in-a-running-azure-container-instance"></a>Ausführen eines Befehls in einer ausgeführten Azure Container Instances-Instanz

Azure Container Instances unterstützt die Ausführung eines Befehls in einem ausgeführten Container. Die Ausführung eines Befehls in einem bereits gestarteten Container ist besonders bei der Anwendungsentwicklung und Problembehandlung von großem Nutzen. Am häufigsten wird dieses Features zum Starten einer interaktiven Befehlsshell verwendet, sodass Probleme in einem ausgeführten Container behoben werden können.

## <a name="run-a-command-with-azure-cli"></a>Ausführen eines Befehls mit der Azure CLI

Führen Sie mit [az container exec][az-container-exec] einen Befehl in einem ausgeführten Container in der [Azure CLI][azure-cli] aus:

```azurecli
az container exec --resource-group <group-name> --name <container-group-name> --exec-command "<command>"
```

Gehen Sie beispielsweise zum Starten einer Bash-Shell in einem Nginx-Container wie folgt vor:

```azurecli
az container exec --resource-group myResourceGroup --name mynginx --exec-command "/bin/bash"
```

In der folgenden Beispielausgabe wird die Bash-Shell in einem ausgeführten Linux-Container gestartet, der einen Terminalserver bereitstellt, in dem `ls` ausgeführt wird:

```output
root@caas-83e6c883014b427f9b277a2bba3b7b5f-708716530-2qv47:/# ls
bin   dev  home  lib64  mnt  proc  run   srv  tmp  var
boot  etc  lib   media  opt  root  sbin  sys  usr
root@caas-83e6c883014b427f9b277a2bba3b7b5f-708716530-2qv47:/# exit
exit
Bye.
```

In diesem Beispiel wird die Eingabeaufforderung in einem ausgeführten Nano Server-Container gestartet:

```azurecli
az container exec --resource-group myResourceGroup --name myiis --exec-command "cmd.exe"
```

```output
Microsoft Windows [Version 10.0.14393]
(c) 2016 Microsoft Corporation. All rights reserved.

C:\>dir
 Volume in drive C has no label.
 Volume Serial Number is 76E0-C852

 Directory of C:\

03/23/2018  09:13 PM    <DIR>          inetpub
11/20/2016  11:32 AM             1,894 License.txt
03/23/2018  09:13 PM    <DIR>          Program Files
07/16/2016  12:09 PM    <DIR>          Program Files (x86)
03/13/2018  08:50 PM           171,616 ServiceMonitor.exe
03/23/2018  09:13 PM    <DIR>          Users
03/23/2018  09:12 PM    <DIR>          var
03/23/2018  09:22 PM    <DIR>          Windows
               2 File(s)        173,510 bytes
               6 Dir(s)  21,171,609,600 bytes free

C:\>exit
Bye.
```

## <a name="multi-container-groups"></a>Gruppen mit mehreren Containern

Wenn Ihre [Containergruppe](container-instances-container-groups.md) mehrere Container enthält, z.B. einen Anwendungscontainer und eine Protokollierungssidecardatei, geben Sie mit `--container-name` den Namen des Containers an, in dem der Befehl ausgeführt werden soll.

In der Containergruppe *mynginx* befinden sich beispielsweise zwei Container: *nginx-app* und *logger*. Hiermit starten Sie eine Shell für den Container *nginx-app*:

```azurecli
az container exec --resource-group myResourceGroup --name mynginx --container-name nginx-app --exec-command "/bin/bash"
```

## <a name="restrictions"></a>Beschränkungen

Azure Container Instances unterstützt derzeit den Start eines einzelnen Prozesses mit [az container exec][az-container-exec], und Sie können keine Befehlsargumente übergeben. Sie können beispielsweise keine Befehle wie in `sh -c "echo FOO && echo BAR"` verketten oder `echo FOO` ausführen.

## <a name="next-steps"></a>Nächste Schritte

Informationen zu anderen Tools zur Problembehandlung und häufige Bereitstellungsprobleme finden Sie unter [Beheben von Bereitstellungsproblemen für Azure Container Instances](container-instances-troubleshooting.md).

<!-- LINKS - internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-exec]: /cli/azure/container#az-container-exec
[azure-cli]: /cli/azure