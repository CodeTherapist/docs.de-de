---
title: Mit .NET-Containern angezieltes Betriebssystem
description: .NET-Microservicesarchitektur für .NET-Containeranwendungen | Mit .NET-Containern angezieltes Betriebssystem
author: CESARDELATORRE
ms.author: wiwagn
ms.date: 09/11/2018
ms.openlocfilehash: b2ae1d2e732f152133dd8a8757b955e05cdd88eb
ms.sourcegitcommit: 5bbfe34a9a14e4ccb22367e57b57585c208cf757
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2018
ms.locfileid: "45970824"
---
# <a name="what-os-to-target-with-net-containers"></a>Mit .NET-Containern angezieltes Betriebssystem

Durch die Vielzahl der von Docker unterstützten Betriebssysteme und die Unterschiede zwischen .NET Framework und .NET Core sollten Sie abhängig von dem Framework, das Sie verwenden, ein bestimmtes Betriebssystem und bestimmte Versionen anzielen.

Für Windows können Sie Windows Server Core oder Windows Nano Server verwenden. Diese Windows-Versionen bieten unterschiedliche Eigenschaften (IIS in Windows Server Core im Vergleich zu einem selbstgehosteten Webserver wie Kestrel in Nano Server), die jeweils für .NET Framework oder .NET Core erforderlich sind.

Für Linux sind mehrere Distributionen (z.B. Debian) verfügbar und werden in offiziellen .NET Docker-Images unterstützt.

In Abbildung 3-1 wird die mögliche Betriebssystemversion in Abhängigkeit vom verwendeten .NET Framework dargestellt.

![Bei der Bereitstellung von .NET Framework-Legacyanwendungen muss Windows Server Core angezielt werden, das bei Kompatibilität mit Legacyanwendungen und IIS ein größeres Image aufweist. Bei der Bereitstellung von .NET Core-Anwendungen kann Windows Nano Server als Zielplattform verwendet werden, das für die Cloud optimiert ist, Kestrel verwendet, kleiner ist und schneller startet. Auch Linux ist als Zielplattform geeignet; Debian, Alpine und andere werden unterstützt. Verwendet ebenfalls Kestrel, ist kleiner und startet schneller.](./media/image1.png)

**Abbildung 3-1.** In Abhängigkeit der Version von .NET Framework anzuzielende Betriebssysteme

Sie können ebenfalls Ihr eigenes Docker-Image erstellen, wenn Sie eine andere Linux-Distribution verwenden oder wenn Sie ein Image mit nicht von Microsoft bereitgestellten Versionen verwenden möchten. Sie könnten beispielsweise ein Image erstellen, bei dem ASP.NET Core unter dem herkömmlichen .NET Framework und Windows Server Core ausgeführt wird. Dabei handelt es sich um ein für Docker ungewöhnliches Szenario.

Wenn Sie den Namen des Images zu Ihrer Dockerfile-Datei hinzufügen, können Sie das Betriebssystem und die Version wie in den folgenden Beispielen dargestellt in Abhängigkeit vom verwendeten Tag auswählen:

<table>
<thead>
<tr class="header">
<th>Bild</th>
<th>Kommentare</th>
</tr>
</thead>
<tbody>
<tr>
<td>microsoft/dotnet:2.1-runtime</td>
<td>Mehrere .NET Core 2.1 Architekturen: Unterstützt Linux und Windows Nano Server, abhängig vom Docker-Host.</td>
</tr>
<tr class="odd">
<td>microsoft/dotnet:2.1-aspnetcore-runtime</td>
<td><p>Mehrere ASP.NET Core 2.1 Architekturen: Unterstützt Linux und Windows Nano Server, abhängig vom Docker-Host.</p>
<p>Das aspnetcore-Image enthält einige Optimierungen für ASP.NET Core.</p></td>
</tr>
<tr class="even">
<td>microsoft/dotnet:2.1-aspnetcore-runtime-alpine</td>
<td>Unter Linux Alpine-Distribution nur .NET Core 2.1-Runtime</td>
</tr>
<tr class="odd">
<td>microsoft/dotnet:2.1-aspnetcore-runtime-nanoserver-1803</td>
<td>Nur .NET Core 2.1-Runtime unter Windows Nano Server (Windows Server, Version 1803)</td>
</tr>
</tbody>
</table>

>[!div class="step-by-step"]
[Zurück](container-framework-choice-factors.md)
[Weiter](official-net-docker-images.md)
