---
title: Azure Analysis Services-Aliasservernamen | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Aliase für Azure Analysis Services-Servernamen erstellen. Benutzer können dann mit einem kürzeren Aliasnamen anstelle des Servernamens eine Verbindung mit Ihrem Server herstellen.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 06/16/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 753ac449418b78fe02640f1b70b94b5b3d7f2be5
ms.sourcegitcommit: 2c586a0fbec6968205f3dc2af20e89e01f1b74b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2020
ms.locfileid: "92019034"
---
# <a name="alias-server-names"></a>Aliasservernamen

Mit einem Servernamenalias können Benutzer beim Herstellen der Verbindung mit Ihrem Azure Analysis Services-Server anstelle des Servernamens ein kürzeres *Alias* verwenden. Wenn die Verbindung über eine Clientanwendung hergestellt wird, wird der Alias unter Verwendung des Protokollformats **link://** als Endpunkt angegeben. Der Endpunkt gibt dann den tatsächlichen Servernamen für die Verbindungsherstellung zurück.

Aliasservernamen ermöglichen Folgendes:

- Modellmigration zwischen Servern ohne Beeinträchtigung der Benutzer 
- Verwendung leichter zu merkender Servernamen 
- Weiterleitung von Benutzern an unterschiedliche Server (abhängig von der Tageszeit) 
- Weiterleitung von Benutzern in verschiedenen Regionen an Instanzen, die jeweils geografisch näher liegen (wie bei Verwendung von Azure Traffic Manager) 

Als Alias kann jeder HTTPS-Endpunkt fungieren, der einen gültigen Azure Analysis Services-Servernamen zurückgibt. Der Endpunkt muss HTTPS über Port 443 unterstützen, und der Port darf nicht im URI angegeben werden.

![Alias mit Linkformat](media/analysis-services-alias/aas-alias-browser.png)

Bei der Verbindungsherstellung über einen Client wird der Aliasservername im Protokollformat **link://** eingegeben. In Power BI Desktop sieht das beispielsweise wie folgt aus:

![Power BI Desktop-Verbindung](media/analysis-services-alias/aas-alias-connect-pbid.png)

## <a name="create-an-alias"></a>Erstellen eines Alias

Für die Erstellung eines Aliasendpunkts können Sie eine beliebige Methode verwenden, die einen gültigen Azure Analysis Services-Servernamen zurückgibt. Beispiele wären etwa ein Verweis auf eine Datei in Azure Blob Storage mit dem tatsächlichen Servernamen oder die Erstellung und Veröffentlichung einer ASP.NET Web Forms-Anwendung.

In diesem Beispiel wird eine ASP.NET Web Forms-Anwendung in Visual Studio erstellt. Der Seitenverweis und das Benutzersteuerelement werden von der Seite „Default.aspx“ entfernt. „Default.aspx“ enthält einfach nur die folgende Page-Direktive:

```
<%@ Page Title="Home Page" Language="C#" AutoEventWireup="true" CodeBehind="Default.aspx.cs" Inherits="FriendlyRedirect._Default" %>
```

Das Ereignis „Page_Load“ in der Datei „Default.aspx.cs“ verwendet die Methode „Response.Write()“, um den Azure Analysis Services-Servernamen zurückzugeben.

```
protected void Page_Load(object sender, EventArgs e)
{
    this.Response.Write("asazure://<region>.asazure.windows.net/<servername>");
}
```

## <a name="see-also"></a>Weitere Informationen

[Clientbibliotheken](/analysis-services/client-libraries?view=azure-analysis-services-current)   
[Herstellen einer Verbindung mit Power BI](analysis-services-connect-pbi.md)