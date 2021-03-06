---
title: "Verschl&#252;sselung von digitalen Signaturen | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "Digitale Signaturen [WCF]"
  - "Digitale Signaturen [WCF], Verschlüsselung"
  - "Verschlüsselung von digitalen Signaturen [WCF]"
ms.assetid: 0868866d-40b4-4341-8e42-eee3b7f15b69
caps.latest.revision: 9
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 9
---
# Verschl&#252;sselung von digitalen Signaturen
Standardmäßig wird eine Nachricht signiert und verschlüsselt, und die Signatur wird digital verschlüsselt.Dies können Sie steuern, indem Sie eine benutzerdefinierte Bindung mit einer Instanz von <xref:System.ServiceModel.Channels.AsymmetricSecurityBindingElement> oder <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> erstellen und dann die `MessageProtectionOrder`\-Eigenschaft der Klasse auf einen <xref:System.ServiceModel.Security.MessageProtectionOrder>\-Enumerationswert festlegen.Die Standardeinstellung ist <xref:System.ServiceModel.Security.MessageProtectionOrder>.Im Vergleich zum einfachen Signieren und Verschlüsseln dauert dieser Prozess 10 bis 40 Prozent länger.Wenn Sie die Verschlüsselung der Signatur jedoch deaktivieren, kann ein Angreifer eventuell den Inhalt der Nachricht erraten.Dies ist möglich, da das Signaturelement den Hashcode des reinen Texts des signierten Teils der Nachricht enthält.Obwohl der Nachrichtentext zwar standardmäßig verschlüsselt ist, enthält die unverschlüsselte Signatur z. B. den Hashcode des Nachrichtentexts.Wenn die Nachricht klein ist, könnte ein Angreifer den Inhalt herleiten.Diese Möglichkeit wird durch das Verschlüsseln der Signatur reduziert oder ausgeschlossen.  
  
 Deshalb sollten Sie die Verschlüsselung der Signatur nur deaktivieren, wenn der Inhalt für Sie von niedrigem Wert und die Leistungssteigerung erheblich ist, z. B. wenn große Binärdateien gesendet werden, die keinerlei Sicherheitsauswirkungen haben.  
  
### So deaktivieren Sie das digitale Signieren  
  
1.  Erstellen Sie eine <xref:System.ServiceModel.Channels.CustomBinding>.[!INCLUDE[crdefault](../../../../includes/crdefault-md.md)][Vorgehensweise: Erstellen einer benutzerdefinierten Bindung mit dem SecurityBindingElement](../../../../docs/framework/wcf/feature-details/how-to-create-a-custom-binding-using-the-securitybindingelement.md).  
  
2.  Fügen Sie der Bindungsauflistung entweder ein <xref:System.ServiceModel.Channels.AsymmetricSecurityBindingElement> oder ein <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> hinzu.  
  
3.  Legen Sie die <xref:System.ServiceModel.Channels.AsymmetricSecurityBindingElement.MessageProtectionOrder%2A?displayProperty=fullName>\-Eigenschaft auf <xref:System.ServiceModel.Security.MessageProtectionOrder> oder die <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement.MessageProtectionOrder%2A?displayProperty=fullName>\-Eigenschaft auf <xref:System.ServiceModel.Security.MessageProtectionOrder> fest.  
  
 [!INCLUDE[crabout](../../../../includes/crabout-md.md)] zur Erstellung benutzerdefinierten Bindungen finden Sie unter [Erstellen benutzerdefinierter Bindungen](../../../../docs/framework/wcf/extending/creating-user-defined-bindings.md).[!INCLUDE[crabout](../../../../includes/crabout-md.md)] zum Erstellen einer benutzerdefinierten Bindung für einen bestimmten Authentifizierungsmodus finden Sie unter [Vorgehensweise: Erstellen eines SecurityBindingElement für einen angegebenen Authentifizierungsmodus](../../../../docs/framework/wcf/feature-details/how-to-create-a-securitybindingelement-for-a-specified-authentication-mode.md).  
  
## Siehe auch  
 <xref:System.ServiceModel.Security.MessageProtectionOrder>   
 <xref:System.ServiceModel.Channels.AsymmetricSecurityBindingElement>   
 <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement>   
 [Vorgehensweise: Erstellen einer benutzerdefinierten Bindung mit dem SecurityBindingElement](../../../../docs/framework/wcf/feature-details/how-to-create-a-custom-binding-using-the-securitybindingelement.md)   
 [Erstellen benutzerdefinierter Bindungen](../../../../docs/framework/wcf/extending/creating-user-defined-bindings.md)   
 [Vorgehensweise: Erstellen eines SecurityBindingElement für einen angegebenen Authentifizierungsmodus](../../../../docs/framework/wcf/feature-details/how-to-create-a-securitybindingelement-for-a-specified-authentication-mode.md)