---
title: "&lt;transport&gt; von &lt;wsHttpBinding&gt; | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 21e38acf-450a-4bda-82b6-de305e1f7cd8
caps.latest.revision: 11
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 11
---
# &lt;transport&gt; von &lt;wsHttpBinding&gt;
Definiert die Authentifizierungseinstellungen für den HTTP\-Transport.  
  
## Syntax  
  
```  
  
<wsHttpBinding>  
    <binding>  
        <security mode=”None|Transport|TransportWithMessageCredential|TransportCredentialOnly”>  
            <transport  
            clientCredentialType="Basic|Certificate|Digest|None|Ntlm|Windows"  
            proxyCredentialType="Basic|Digest|None|Ntlm|Windows"  
            realm="string" />  
                <extendedProtectionPolicy policyEnforcement=”Never|WhenSupported|Always” protectionScenario=”TransportSelected|TrustedProxy”>  
                    <customServiceNames></customServiceNames>  
                </extendedProtecutionPolicy>  
            </transport>  
        </security>  
    </binding>  
</wsHttpBinding>  
```  
  
## Typ  
 <xref:System.ServiceModel.HttpTransportSecurity>  
  
## Attribute und Elemente  
 In den folgenden Abschnitten werden Attribute sowie untergeordnete und übergeordnete Elemente beschrieben.  
  
### Attribute  
  
|Attribut|Beschreibung|  
|--------------|------------------|  
|`clientCredentialType`|Gibt die zum Authentifizieren des Clients beim Dienst verwendeten Anmeldeinformationen an.  Dieses Attribut ist vom Typ <xref:System.ServiceModel.HttpClientCredentialType>.|  
|`proxyCredentialType`|Gibt die Anmeldeinformationen an, die verwendet werden, um den Client bei einem Domänenproxy zu authentifizieren.  Dieses Attribut ist vom Typ <xref:System.ServiceModel.HttpProxyCredentialType>.|  
|`realm`|Eine Zeichenfolge, die den Authentifizierungsbereich für die Digest\- oder Standardauthentifizierung angibt.  Der Standardwert ist eine leere Zeichenfolge.<br /><br /> Ein Authentifizierungsbereich gibt mindestens den Namen des Hosts an, der die Authentifizierung durchführt.  Er kann auch eine Auflistung von Benutzern angeben, die Zugriff haben.  Ein Benutzer kann den Authentifizierungsbereich abfragen, um zu erfahren, welche der verschiedenen möglichen Benutzernamen und Kennwörter verwendet werden können.|  
|`policyEnforcement`|Diese Enumeration gibt an, wann die <xref:System.Security.Authentication.ExtendedProtectionPolicy> erzwungen werden soll.<br /><br /> 1.  Never – die Richtlinie wird nie erzwungen \(erweiterter Schutz ist deaktiviert\).<br />2.  WhenSupported – die Richtlinie wird nur erzwungen, wenn der Client erweiterten Schutz unterstützt.<br />3.  Always – die Richtlinie wird immer erzwungen.  Clients, die erweiterten Schutz nicht unterstützen, werden nicht authentifiziert.|  
  
## clientCredentialType\-Attribut  
  
|Wert|Beschreibung|  
|----------|------------------|  
|`None`|Die Sicherheitsfunktionen sind deaktiviert.|  
|`Basic`|Verwendet die Standardauthentifizierung.|  
|`Digest`|Verwendet Digestauthentifizierung.|  
|`Ntlm`|Verwendet als Ausweichlösung die NTLM\-Authentifizierung für eine Windows\-Domäne.|  
|`Windows`|Verwendet die integrierte Windows\-Authentifizierung.|  
|`Certificate`|Verwendet X.509\-Zertifikate zum Authentifizieren des Clients.|  
  
## proxyCredentialType\-Attribut  
  
|Wert|Beschreibung|  
|----------|------------------|  
|`None`|Die Sicherheitsfunktionen sind deaktiviert.|  
|`Basic`|Verwendet die Standardauthentifizierung.|  
|`Digest`|Verwendet Digestauthentifizierung.|  
|`Ntlm`|Verwendet als Ausweichlösung NTLM für eine Windows\-Domäne.|  
|`Windows`|Verwendet die integrierte Windows\-Authentifizierung.|  
|`Certificate`|Verwendet X.509\-Zertifikate zum Authentifizieren des Clients.|  
  
### Untergeordnete Elemente  
 Keine  
  
### Übergeordnete Elemente  
  
|Element|Beschreibung|  
|-------------|------------------|  
|[\<Sicherheit\>](../../../../../docs/framework/configure-apps/file-schema/wcf/security-of-wshttpbinding.md)|Gibt die Sicherheitsfunktionen von [\<wsHttpBinding\>](../../../../../docs/framework/configure-apps/file-schema/wcf/wshttpbinding.md) wieder.|  
  
## Siehe auch  
 <xref:System.ServiceModel.HttpTransportSecurity>   
 <xref:System.ServiceModel.WSHttpSecurity.Transport%2A>   
 <xref:System.ServiceModel.Configuration.WSHttpSecurityElement.Transport%2A>   
 <xref:System.ServiceModel.Configuration.HttpTransportSecurityElement>   
 [Sichern von Diensten und Clients](../../../../../docs/framework/wcf/feature-details/securing-services-and-clients.md)   
 [Bindungen](../../../../../docs/framework/wcf/bindings.md)   
 [Konfigurieren der vom System bereitgestellten Bindungen](../../../../../docs/framework/wcf/feature-details/configuring-system-provided-bindings.md)   
 [Using Bindings to Configure Windows Communication Foundation Services and Clients](http://msdn.microsoft.com/de-de/bd8b277b-932f-472f-a42a-b02bb5257dfb)   
 [\<Bindung\>](../../../../../docs/framework/misc/binding.md)