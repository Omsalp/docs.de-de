---
title: "Gewusst wie: Ändern der Computerkonfigurationsdatei zum Aktivieren der IPv6-Unterstützung"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.tgt_pltfrm: 
ms.topic: article
dev_langs:
- VB
- CSharp
- C++
- jsharp
ms.assetid: 5611b677-b9cc-43b8-a434-60e18d89aada
caps.latest.revision: 18
author: mcleblanc
ms.author: markl
manager: markl
ms.translationtype: HT
ms.sourcegitcommit: 306c608dc7f97594ef6f72ae0f5aaba596c936e1
ms.openlocfilehash: 6a23a2690b0df30eaa815d0b321200af3a3403e5
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---
# <a name="how-to-modify-the-computer-configuration-file-to-enable-ipv6-support"></a>Gewusst wie: Ändern der Computerkonfigurationsdatei zum Aktivieren der IPv6-Unterstützung
Das folgende Codebeispiel zeigt, wie Sie die Computerkonfigurationsdatei *machine.config* ändern, um die IPv6-Unterstützung zu aktivieren. Die Datei *machine.config* ist im Ordner *%Windir%\Microsoft.NET\Framework* in dem Verzeichnis gespeichert, in dem Windows installiert wurde. Es gibt für jede Version von .NET Framework, die auf dem Computer installiert ist, eine eigene Datei *machine.config* in den Unterordnern von *%Windir%\Microsoft.NET\Framework* (z.B. *C:\WINDOWS\Microsoft.NET\Framework\v2.0.50727\machine.config*).  
  
 Diese Einstellungen können auch in der Konfigurationsdatei für die Anwendung vorgenommen werden. Die Anwendungskonfigurationsdatei hat Vorrang vor der Computerkonfigurationsdatei.  
  
 Für .NET Framework Version 1.1 und früher gibt der Wert des Konfigurationsschalters **ipv6 enabled** an, ob Member der Klasse <xref:System.Net.Dns?displayProperty=fullName> IPv6-Adressen zurückgeben.  
  
 Für .NET Framework Version 2.0 und höher geben alle Member der Klasse <xref:System.Net.Dns?displayProperty=fullName> (z.B. die <xref:System.Net.Dns.GetHostEntry%2A?displayProperty=fullName>-Methode), sofern Windows IPv6 unterstützt, IPv6-Adressen mit einer Einschränkung zurück. Veraltete Member der Klasse <xref:System.Net.Dns?displayProperty=fullName> (z.B. die <xref:System.Net.Dns.Resolve%2A?displayProperty=fullName>-Methode) lesen und erkennen den Wert in der Konfigurationsdatei.  
  
> [!NOTE]
>  Für .NET Framework Version 2.0 und höher ist IPv6 standardmäßig aktiviert. Für .NET Framework Version 1.1 und früher ist IPv6 standardmäßig deaktiviert.  
  
## <a name="example"></a>Beispiel  
  
```xml  
<system.net>  
    …………  
    <settings>  
        …………  
        <ipv6 enabled="true"/>   
    ……………  
    </settings>  
    ………………  
<system.net>  
```  
  
## <a name="see-also"></a>Siehe auch  
 [IPv6 Addressing (IPv6-Adressierung)](../../../docs/framework/network-programming/ipv6-addressing.md)   
 [Network Settings Schema (Netzwerkeinstellungsschema)](../../../docs/framework/configure-apps/file-schema/network/index.md)   
 [\<IPv6>-Element (Netzwerkeinstellungen)](../../../docs/framework/configure-apps/file-schema/network/ipv6-element-network-settings.md)

