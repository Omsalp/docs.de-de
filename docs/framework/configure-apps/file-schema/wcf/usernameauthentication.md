---
title: "&lt;userNameAuthentication&gt; | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 24d8b398-770f-418f-ba23-c4325419cfa6
caps.latest.revision: 10
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 10
---
# &lt;userNameAuthentication&gt;
Gibt die Anmeldeinformationen eines Diensts basierend auf Benutzername und Kennwort an.  
  
## Syntax  
  
```  
  
<userNameAuthentication  
   cacheLogonTokenLifetime="TimeSpan"  
   cacheLogonTokens="Boolean"   
   customUserNamePasswordValidatorType="String"  
   includeWindowsGroups="Boolean"   
   maxCacheLogonTokens="Integer"  
   membershipProviderName="String"  
   userNamePasswordValidationMode="Windows/MembershipProvider/Custom" />  
```  
  
## Attribute und Elemente  
 In den folgenden Abschnitten werden Attribute sowie untergeordnete und übergeordnete Elemente beschrieben.  
  
### Attribute  
  
|Attribut|Beschreibung|  
|--------------|------------------|  
|`cacheLogonTokenLifetime`|Eine <xref:System.Timespan>, die angibt, wie lange ein Token maximal zwischengespeichert wird.  Der Standardwert ist 00:15:00.|  
|`cacheLogonTokens`|Ein boolescher Wert, der angibt, ob Anmeldetoken zwischengespeichert werden.  Die Standardeinstellung ist `false`.|  
|`customUserNamePasswordValidatorType`|Eine Zeichenfolge, die angibt, welche benutzerdefinierte Prüfung für Benutzername und Kennwort verwendet werden soll.  Der Standardwert ist eine leere Zeichenfolge.|  
|`includeWindowsGroups`|Ein boolescher Wert, der angibt, ob Windows\-Gruppen im Sicherheitskontext enthalten sind.  Die Standardeinstellung ist `true`.<br /><br /> Wird dieses Attribut auf `true` festgelegt, hat dies Auswirkungen auf die Leistung, da dabei eine vollständige Gruppenerweiterung durchgeführt wird.  Legen Sie diese Eigenschaft auf `false` fest, wenn Sie die Liste der Gruppen, zu denen ein Benutzer gehört, nicht einrichten müssen.|  
|`maxCacheLogonTokens`|Eine ganze Zahl, die die maximale Anzahl an Anmeldetoken angibt, die zwischengespeichert werden können.  Dieser Wert muss größer als null sein.  Der Standard ist 128.|  
|`membershipProviderName`|Wenn das `clientCredentialType`\-Attribut einer Bindung auf `username` festgelegt ist, wird der Benutzername Windows\-Konten zugewiesen.  Sie können dieses Verhalten mit diesem Attribut überschreiben. Bei dem Attribut handelt es sich um eine Zeichenfolge mit dem Namen des <xref:System.Web.Security.MembershipProvider>\-Werts, der den relevanten Mechanismus zur Kennwortprüfung bereitstellt.|  
|`userNamePasswordValidationMode`|Gibt die Art und Weise an, in der der Benutzername und das Kennwort überprüft werden.  Gültige Werte sind:<br /><br /> -   Windows<br />-   MembershipProvider<br />-   Benutzerdefiniert<br /><br /> Der Standardwert ist Windows.  Dieses Attribut ist vom Typ <xref:System.ServiceModel.Security.UserNamePasswordValidationMode>.|  
  
### Untergeordnete Elemente  
 Keine  
  
### Übergeordnete Elemente  
  
|Element|Beschreibung|  
|-------------|------------------|  
|[\<serviceCredentials\>](../../../../../docs/framework/configure-apps/file-schema/wcf/servicecredentials.md)|Gibt die Anmeldeinformationen an, die beim Authentifizieren des Diensts verwendet werden sollen, sowie die Einstellungen für die Überprüfung der Clientanmeldeinformationen.|  
  
## Hinweise  
 Wenn keine der von einem Dienst verwendeten Bindungen für die Benutzername\/Kennwort\-basierte Authentifizierung konfiguriert ist, werden die Attribute für dieses Element ignoriert.  Dazu gehören `customUserNamePasswordValidatorType`, `includeWindowsGroups`, `membershipProviderName` und `userNamePasswordValidationMode`.  
  
 Wenn keine der von einem Dienst verwendeten Bindungen für die Windows\-Authentifizierung von Benutzername\/Kennwort konfiguriert ist, werden die mit der Zwischenspeicherung von Anmeldetoken zusammenhängenden Eigenschaften ignoriert.  Dazu gehören `cacheLogonTokenLifetime`, `cacheLogonTokens` und `maxCacheLogonTokens`.  
  
## Siehe auch  
 <xref:System.ServiceModel.Configuration.UserNameServiceElement>   
 <xref:System.ServiceModel.Description.ServiceCredentials.UserNameAuthentication%2A>   
 <xref:System.ServiceModel.Security.UserNamePasswordServiceCredential>   
 <xref:System.ServiceModel.Configuration.ServiceCredentialsElement.UserNameAuthentication%2A>