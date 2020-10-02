---
title: PBR-Materialien
description: Beschreibt den PBR-Materialtyp.
author: jakrams
ms.author: jakras
ms.date: 02/11/2020
ms.topic: article
ms.openlocfilehash: a1fedb637bee9d98fb09d8fc3fa133b2992ce86e
ms.sourcegitcommit: f845ca2f4b626ef9db73b88ca71279ac80538559
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/09/2020
ms.locfileid: "89613661"
---
# <a name="pbr-materials"></a>PBR-Materialien

*PBR-Materialien* sind einer der unterstützten [Materialtypen](../../concepts/materials.md) in Azure Remote Rendering. Sie werden für [Gittermodelle](../../concepts/meshes.md) verwendet, die eine realistische Beleuchtung erhalten sollen.

PBR steht für **P**hysically **B**ased **R**endering. Dies besagt, dass das Material die visuellen Eigenschaften einer Oberfläche auf physikalisch plausible Weise beschreibt, sodass realistische Ergebnisse unter allen Beleuchtungsbedingungen möglich sind. Die meisten modernen Spiele-Engines und Inhaltserstellungstools unterstützen PBR-Materialien, da sie als die beste Annäherung an realistische Szenarien für Echtzeitrendering angesehen werden.

![Mit ARR gerendertes gITF-Beispielmodell eines Helms](media/helmet.png)

PBR-Materialien sind jedoch keine Universallösung. Es gibt Materialien, die Licht je nach Blickwinkel unterschiedlich reflektieren. Hierzu zählen einige Stoffarten und Autolacke. Diese Arten von Materialien werden vom standardmäßigen PBR-Modell nicht behandelt und von Azure Remote Rendering zurzeit nicht unterstützt. Dies gilt auch für PBR-Erweiterungen, z. B. *Thin-Film* (mehrschichtige Oberflächen) und *Clear-Coat* (für Autolacke).

## <a name="common-material-properties"></a>Allgemeine Materialeigenschaften

Diese Eigenschaften gelten für alle Materialien:

* **albedoColor**: Diese Farbe wird mit anderen Farben multipliziert, z. B. mit *albedoMap* oder *:::no-loc text="vertex ":::-Farben*. Wenn *transparency* für ein Material aktiviert ist, wird der Alphakanal verwendet, um die Deckkraft anzupassen, wobei `1` vollständig undurchsichtig und `0` vollständig transparent bedeutet. Die Standardfarbe ist Weiß.

  > [!NOTE]
  > Wenn ein PBR-Material vollständig transparent ist, z. B. ein vollkommen sauberes Glas, wird die Umgebung immer noch von ihm reflektiert. Helle Stellen wie die Sonne sind in der Reflexion immer noch sichtbar. Dies ist bei [Farbmaterialien](color-materials.md) anders.

* **albedoMap:** Eine [2D-Textur](../../concepts/textures.md) für Albedo-Werte pro Pixel.

* **alphaClipEnabled** und **alphaClipThreshold**: Wenn *alphaClipEnabled* „true“ ist, werden alle Pixel, deren Albedo-Alphawert geringer als *alphaClipThreshold* ist, nicht gezeichnet. Alphaclipping kann auch ohne Aktivierung der Transparenz verwendet werden, und es beschleunigt das Rendern. Materialien, auf die Alphaclipping angewendet wird, werden dennoch langsamer gerendert als vollständig undurchsichtige Materialien. Alphaclipping ist standardmäßig deaktiviert.

* **textureCoordinateScale** und **textureCoordinateOffset**: Die Skalierung wird mit den UV-Texturkoordinaten multipliziert, und der Offset wird addiert. Kann zum Strecken und Verschieben der Texturen verwendet werden. Die Standardskalierung ist (1, 1) und der Standardoffset ist (0, 0).

* **useVertexColor**: Wenn das Gittermodell :::no-loc text="vertex":::-Farben enthält und diese Option aktiviert ist, werden die :::no-loc text="vertex":::-Farben des Gittermodells in *albedoColor* und *albedoMap* multipliziert. Standardmäßig ist *useVertexColor* deaktiviert.

* **isDoubleSided**: Wenn diese Eigenschaft auf „true“ festgelegt ist, werden Dreiecke mit diesem Material auch dann gerendert, wenn die Kamera auf ihre Rückseite gerichtet ist. Für die PBR-Materialien wird die Beleuchtung auch für die Rückseite korrekt berechnet. Diese Option ist standardmäßig deaktiviert. Weitere Informationen finden Sie unter [:::no-loc text="Single-sided":::-Rendering](single-sided-rendering.md).

## <a name="pbr-material-properties"></a>PBR-Materialeigenschaften

Physically Based Rendering beinhaltet im Wesentlichen die Verwendung der Eigenschaften *BaseColor*, *Metalness* und *Roughness* zum Emulieren einer breite Palette realistischer Materialien. Eine ausführliche Beschreibung von PBR würde den Rahmen dieses Artikels sprengen. Ausführlichere Informationen zu PBR finden Sie in [weiteren Quellen](http://www.pbr-book.org). Die folgenden Eigenschaften gelten speziell für PBR-Materialien:

* **baseColor:** In PBR-Materialien wird die *Albedo-Farbe* als *Basisfarbe* bezeichnet. In Azure Remote Rendering ist die Eigenschaft *Albedo-Farbe* bereits über die allgemeinen Materialeigenschaften vorhanden, sodass es keine zusätzliche Eigenschaft für die Basisfarbe gibt.

* **roughness** und **roughnessMap:** „roughness“ definiert die Glätte oder Rauheit der Oberfläche. Raue Oberflächen streuen das Licht in mehr Richtungen als glatte Oberflächen, wodurch die Reflexionen unscharf werden. Der Wertebereich liegt zwischen `0.0` und `1.0`. Wenn `roughness` gleich `0.0` ist, werden die Reflexionen scharf dargestellt. Wenn `roughness` gleich `0.5` ist, werden die Reflexionen unscharf dargestellt.

  Wenn ein roughness-Wert und eine „roughnessMap“ angegeben werden, ist der endgültige Wert das Produkt der beiden.

* **metalness** und **metalnessMap:** In der Physik entspricht diese Eigenschaft der Angabe, ob eine Oberfläche eine leitende oder eine dielektrische Oberfläche ist. Leitende Materialien verfügen über andere Reflexionseigenschaften und sind in der Regel ohne Albedo-Farbe reflektierend. In PBR-Materialien beeinflusst diese Eigenschaft den Grad, in dem eine Oberfläche die Umgebung reflektiert. Der Wertebereich liegt zwischen `0.0` und `1.0`. Wenn „metalness“ den Wert `0.0` hat, ist die Albedo-Farbe vollständig sichtbar, und das Material sieht wie Kunststoff oder Keramik aus. Wenn „metalness“ den Wert `0.5` hat, sieht das Material wie lackiertes Metall aus. Wenn „metalness“ den Wert `1.0` hat, weist die Oberfläche kaum noch Albedo-Farbe auf und reflektiert nur die Umgebung. Wenn z. B. `metalness` den Wert `1.0` und `roughness` den Wert `0.0` aufweist, sieht die Oberfläche wie ein Spiegel aus.

  Wenn ein metalness-Wert und eine „metalnessMap“ angegeben werden, ist der endgültige Wert das Produkt der beiden.

  ![Mit verschiedenen Werten für „metalness“ und „roughness“ gerenderte Werte](./media/metalness-roughness.png)

  In der obigen Abbildung sieht die Kugel in der rechten unteren Ecke wie Metall und die Kugel in der linken unteren Ecke wie Keramik oder Kunststoff aus. Die Albedo-Farbe ändert sich auch gemäß den physischen Eigenschaften. Wenn sich die Rauheit erhöht, wird die Reflexion des Materials weniger scharf.

* **normalMap:** Um differenzierte Details zu simulieren, kann [Normal Mapping](https://en.wikipedia.org/wiki/Normal_mapping) bereitgestellt werden.

* **occlusionMap** und **aoScale:** Durch [Umgebungsverdeckung](https://en.wikipedia.org/wiki/Ambient_occlusion) werden Objekte mit Spalten realistischer dargestellt, indem verdeckten Bereichen Schatten hinzugefügt werden. Der Verdeckungswertebereich liegt zwischen `0.0` und `1.0`, wobei `0.0` Dunkelheit (verdeckt) und `1.0` keine Verdeckung bedeutet. Wenn für „occlusionMap“ eine 2D-Textur bereitgestellt wird, wird der Effekt aktiviert, und *aoScale* fungiert als Multiplikator.

  ![Ein mit und ohne Umgebungsverdeckung gerendertes Objekt](./media/boom-box-ao2.gif)

* **transparent:** Für PBR-Materialien gibt es nur eine Transparenzeinstellung: Sie ist aktiviert oder deaktiviert. Die Deckkraft wird durch den Alphakanal der Albedo-Farbe definiert. Wenn diese Option aktiviert ist, wird eine komplexere Renderingpipeline aufgerufen, um semitransparente Oberflächen zu zeichnen. Azure Remote Rendering implementiert echte [reihenfolgenunabhängige Transparenz](https://en.wikipedia.org/wiki/Order-independent_transparency) (Order-Independent Transparency, OIT).

  Transparente Geometrie ist aufwendig zu Rendern. Wenn Sie nur Lücken in einer Oberfläche benötigen, z. B. für die Blätter eines Baums, empfiehlt es sich, stattdessen Alpha-Clipping zu verwenden.

  ![Mit von null bis vollständig reichender Transparenz gerenderte Kugeln](./media/transparency.png) Beachten Sie in der obigen Abbildung, dass die äußerste rechte Kugel vollständig transparent ist und die Reflexion dennoch sichtbar ist.

  > [!IMPORTANT]
  > Wenn ein Material zur Laufzeit von undurchsichtig in transparent geändert werden soll, muss der Renderer den *TileBasedComposition*-[Renderingmodus](../../concepts/rendering-modes.md) verwenden. Diese Einschränkung gilt nicht für Materialien, die zunächst als transparente Materialien konvertiert werden.

## <a name="technical-details"></a>Technische Details

Azure Remote Rendering verwendet die auf dem Cook-Torrance-Modell basierende Mikrofacetten-BRDF. Diese beinhaltet GGX-NDF, die Schlick-Fresnel-Gleichung und eine mit GGX korrelierte Smith-Sichtbarkeitsfunktion mit einer Abschwächungsfunktion nach dem Lambert-Beer'schen Gesetz. Dieses Modell ist derzeit der Industriestandard. Ausführlichere Informationen finden Sie in diesem Artikel: [Physically based Rendering – Cook-Torrance](http://www.codinglabs.net/article_physically_based_rendering_cook_torrance.aspx) (in englischer Sprache)

 Eine Alternative zu dem in Azure Remote Rendering verwendeten *Metalness-Roughness*-PBR-Modell ist das *Specular-Glossiness*-PBR-Modell. Dieses Modell kann einen breiteren Bereich von Materialien darstellen. Es ist jedoch teurer und eignet sich in der Regel nicht gut für die Anwendung in Echtzeit.
Eine Konvertierung von *Specular-Glossiness* in *Metalness-Roughness* ist nicht immer möglich, da *(Diffuse, Specular)* -Wertepaare vorhanden sind, die nicht in *(BaseColor, Metalness)* konvertiert werden können. Die Konvertierung in die andere Richtung ist einfacher und präziser, da alle *(BaseColor, Metalness)* -Paare wohldefinierten *(Diffuse, Specular)* -Paaren entsprechen.

## <a name="api-documentation"></a>API-Dokumentation

* [C# PbrMaterial-Klasse](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering.pbrmaterial)
* [C# RemoteManager.CreateMaterial()](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering.remotemanager.creatematerial)
* [C++ PbrMaterial-Klasse](https://docs.microsoft.com/cpp/api/remote-rendering/pbrmaterial)
* [C++ RemoteManager::CreateMaterial()](https://docs.microsoft.com/cpp/api/remote-rendering/remotemanager#creatematerial)

## <a name="next-steps"></a>Nächste Schritte

* [Farbmaterialien](color-materials.md)
* [Texturen](../../concepts/textures.md)
* [Gittermodelle](../../concepts/meshes.md)
