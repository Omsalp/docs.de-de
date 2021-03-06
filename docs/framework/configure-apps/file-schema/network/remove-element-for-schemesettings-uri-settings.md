---
title: "&lt;remove&gt;-Element f&#252;r schemeSettings (URI-Einstellungen) | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "VB"
  - "CSharp"
  - "C++"
  - "jsharp"
ms.assetid: 4095ba51-de20-4f87-b562-018abe422c91
caps.latest.revision: 5
author: "mcleblanc"
ms.author: "markl"
manager: "markl"
caps.handback.revision: 5
---
# &lt;remove&gt;-Element f&#252;r schemeSettings (URI-Einstellungen)
Entfernt eine Schemaeinstellung für einen Schemanamen.  
  
## Syntax  
  
```  
  
      <remove   
   <name = "http|https"/>  
/>  
```  
  
## Attribute und Elemente  
 In den folgenden Abschnitten werden Attribute sowie untergeordnete und übergeordnete Elemente beschrieben.  
  
### Attribute  
  
|Attribute|**Beschreibung**|  
|---------------|----------------------|  
|name|Der Schemaname, für den diese Einstellung gültig ist.  Die einzigen unterstützten Werte sind Name \= "http" und Name \= "https".|  
  
### Untergeordnete Elemente  
 Keine.  
  
### Übergeordnete Elemente  
  
|Element|**Beschreibung**|  
|-------------|----------------------|  
|[\<schemeSettings\>\-Element \(Uri\-Einstellungen\)](../../../../../docs/framework/configure-apps/file-schema/network/schemesettings-element-uri-settings.md)|Gibt an, wie ein <xref:System.Uri>\-Objekt für bestimmte Schemen analysiert wird.|  
  
## Hinweise  
 Standardmäßig entfernt die <xref:System.Uri?displayProperty=fullName>\-Klasse Escapezeichen aus als Prozentwert codierte Pfadtrennzeichen vor dem Ausführen der Pfadkomprimierung.  Dies wurde als Sicherheitsmechanismus gegen Angriffe implementiert. Beispiel:  
  
 `http://www.contoso.com/..%2F..%2F/Windows/System32/cmd.exe?/c+dir+c:\`  
  
 Wenn dieser URI an Module übergeben wird und als Prozentwert codierte Zeichen nicht ordnungsgemäß behandelt werden, kann dies dazu führen, dass vom Server der folgende Befehl ausgeführt wird:  
  
 `c:\Windows\System32\cmd.exe /c dir c:\`  
  
 Aus diesem Grund entfernt die <xref:System.Uri?displayProperty=fullName>\-Klasse zuerst Escapezeichen aus Pfadtrennzeichen und wendet dann die Pfadkomprimierung an.  Das Übergeben der böswilligen URL oben an den <xref:System.Uri?displayProperty=fullName>\-Klassenkonstruktor führt zum folgenden URI:  
  
 `http://www.microsoft.com/Windows/System32/cmd.exe?/c+dir+c:\`  
  
 Dieses Standardverhalten kann so abgeändert werden, dass aus prozentcodierten Pfadtrennzeichen, die die schemeSettings\-Konfigurationsoption für ein bestimmtes Schema verwenden, die Escapezeichen nicht gelöscht werden.  
  
## Konfigurationsdateien  
 Dieses Element kann in der Konfigurationsdatei der Anwendung oder in der Konfigurationsdatei des Computers \(Machine.config\) verwendet werden.  
  
## Beispiel  
 Das folgende Codebeispiel zeigt eine Konfiguration, die von der <xref:System.Uri>\-Klasse verwendet wird und alle Schemaeinstellungen für das http\-Schema entfernt.  
  
```  
<configuration>  
  <uri>  
    <schemeSettings>  
      <remove name="http"/>  
    </schemeSettings>  
  </uri>  
</configuration>  
```  
  
## Siehe auch  
 <xref:System.Configuration.SchemeSettingElement?displayProperty=fullName>   
 <xref:System.Configuration.SchemeSettingElementCollection?displayProperty=fullName>   
 <xref:System.Configuration.UriSection?displayProperty=fullName>   
 <xref:System.Configuration.UriSection.SchemeSettings%2A?displayProperty=fullName>   
 <xref:System.GenericUriParserOptions?displayProperty=fullName>   
 <xref:System.Uri?displayProperty=fullName>   
 [Netzwerkeinstellungsschema](../../../../../docs/framework/configure-apps/file-schema/network/index.md)