---
title: "Entschärfung: X509CertificateClaimSet.FindClaims-Methode"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology:
- dotnet-clr
ms.tgt_pltfrm: 
ms.topic: article
ms.assetid: ee356e3b-f932-48f5-875a-5e42340bee63
caps.latest.revision: 7
author: rpetrusha
ms.author: ronpet
manager: wpickett
ms.translationtype: HT
ms.sourcegitcommit: 306c608dc7f97594ef6f72ae0f5aaba596c936e1
ms.openlocfilehash: d19bf73e36061729c0c57439f4e4144669787d1a
ms.contentlocale: de-de
ms.lasthandoff: 07/28/2017

---
# <a name="mitigation-x509certificateclaimsetfindclaims-method"></a>Entschärfung: X509CertificateClaimSet.FindClaims-Methode
Beginnend mit Apps, die auf [!INCLUDE[net_v461](../../../includes/net-v461-md.md)] ausgerichtet sind, versucht die <xref:System.IdentityModel.Claims.X509CertificateClaimSet.FindClaims%2A?displayProperty=fullName>-Methode das `claimType`-Argument mit allen DNS-Einträgen in seinem SAN-Feld zu vergleichen.  
  
## <a name="impact"></a>Auswirkungen  
 Diese Änderung betrifft nur Apps, die auf Versionen von .NET Framework ausgerichtet sind, die mit [!INCLUDE[net_v461](../../../includes/net-v461-md.md)] beginnen.  
  
 Bei Apps, die auf frühere Versionen von .NET Framework ausgerichtet sind, versucht die <xref:System.IdentityModel.Claims.X509CertificateClaimSet.FindClaims%2A?displayProperty=fullName>-Methode, das `claimType`-Argument nur mit dem letzten DNS-Eintrag abzustimmen.  
  
## <a name="mitigation"></a>Problemumgehung  
 Ist diese Änderung nicht erwünscht, kann sie für Apps, die auf Versionen von .NET Framework ausgerichtet sind, die mit [!INCLUDE[net_v461](../../../includes/net-v461-md.md)] beginnen, abgelehnt werden. Dazu muss dem [\<runtime>](../../../docs/framework/configure-apps/file-schema/runtime/runtime-element.md)-Abschnitt der Konfigurationsdatei der App die folgende Konfigurationseinstellung hinzugefügt werden:  
  
```xml  
<runtime>  
   <AppContextSwitchOverrides value="Switch.System.IdentityModel.DisableMultipleDNSEntriesInSANCertificate=true" />   
</runtime>  
```  
  
 Für Apps, die für frühere Versionen von .NET Framework vorgesehen sind, aber unter [!INCLUDE[net_v461](../../../includes/net-v461-md.md)] und höheren Versionen ausgeführt werden, kann dieses Verhalten übernommen werden, indem dem [\<runtime>](../../../docs/framework/configure-apps/file-schema/runtime/runtime-element.md)-Abschnitt der Konfigurationsdatei der Anwendung die folgende Konfigurationseinstellung hinzugefügt wird:  
  
```xml  
<runtime>  
    <AppContextSwitchOverrides value="Switch.System.IdentityModel.DisableMultipleDNSEntriesInSANCertificate=false" />   
</runtime>  
```  
  
## <a name="see-also"></a>Siehe auch  
 [Neuausrichtungsänderungen](../../../docs/framework/migration-guide/retargeting-changes-in-the-net-framework-4-6-1.md)

