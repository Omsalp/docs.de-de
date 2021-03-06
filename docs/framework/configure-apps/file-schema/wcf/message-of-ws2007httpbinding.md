---
title: "&lt;message&gt; von &lt;ws2007HttpBinding&gt; | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 9ffd8db6-84a8-4b38-a9fe-2cb1a87a1c97
caps.latest.revision: 9
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 9
---
# &lt;message&gt; von &lt;ws2007HttpBinding&gt;
Definiert die Einstellungen für die Sicherheit des [\<ws2007HttpBinding\>](../../../../../docs/framework/configure-apps/file-schema/wcf/ws2007httpbinding.md)\-Elements auf Nachrichtenebene.  
  
## Syntax  
  
```  
  
<ws2007HttpBinding>  
 <binding >  
  <security>  
   <message clientCredentialType =  
    "None/Windows/UserName/Certificate/IssuedToken"  
    establishSecurityContext="Boolean"  
    negotiateServiceCredential="Boolean"  
    algorithmSuite= Enumeration. See algorithmSuite Attribute below.  
    defaultProtectionLevel="None/Sign/EncryptionAndSign" />  
  </security>  
 </binding>  
</ws2007HttpBinding>  
```  
  
## Typ  
 <xref:System.ServiceModel.NonDualMessageSecurityOverHttp>  
  
## Attribute und Elemente  
 In den folgenden Abschnitten werden Attribute sowie untergeordnete und übergeordnete Elemente beschrieben.  
  
### Attribute  
  
|Attribut|Beschreibung|  
|--------------|------------------|  
|`algorithmSuite`|Legt die Nachrichtenverschlüsselungs\- und Key Wrap\-Algorithmen fest.  Die Algorithmen und die Schlüsselgröße werden durch die <xref:System.ServiceModel.Security.SecurityAlgorithmSuite>\-Klasse ermittelt.  Diese Algorithmen entsprechen den in der Security Policy Language \(WS\-SecurityPolicy\)\-Spezifikation angegebenen Algorithmen.<br /><br /> Der Standardwert ist Basic256.|  
|`clientCredentialType`|Dies ist optional.  Gibt den Typ der Anmeldeinformationen an, die bei der Clientauthentifizierung im `Message`\-Sicherheitsmodus oder im `TransportWithMessageCredentials`\-Sicherheitsmodus verwendet werden.  In der folgenden Tabelle sind die Enumerationswerte aufgeführt.  Der Standardwert ist Windows.<br /><br /> Dieses Attribut ist vom Typ <xref:System.ServiceModel.MessageCredentialType>.|  
|`establishSecurityContext`|Ein Wert, der bestimmt, ob der Sicherheitskanal eine sichere Sitzung aufbaut.  In einer sicheren Sitzung wird vor dem Austausch der Anwendungsnachrichten ein Sicherheitskontexttoken erstellt.  Wenn das Sicherheitskontexttoken erstellt wurde, stellt der Sicherheitskanal eine <xref:System.ServiceModel.Channels.ISession>\-Schnittstelle für die oberen Kanäle bereit.  Weitere Informationen über sichere Sitzungen finden Sie unter [Vorgehensweise: Erstellen einer sicheren Sitzung](../../../../../docs/framework/wcf/feature-details/how-to-create-a-secure-session.md).<br /><br /> Der Standardwert ist `true`.|  
|`negotiateServiceCredential`|Dies ist optional.  Ein Wert, der angibt, ob die Dienstanmeldeinformationen auf dem Client out\-of\-band bereitgestellt oder vom Dienst für den Client über einen Aushandlungsvorgang abgerufen werden.  Eine solche Verhandlung ist Vorläufer zum üblichen Nachrichtenaustausch.<br /><br /> Wenn das `clientCredentialType`\-Attribut None, Username oder Certificate lautet, wird durch Festlegen dieses Attributs auf `false` angegeben, dass das Dienstzertifikat auf dem Client out\-of\-band verfügbar ist und der Client das Dienstzertifikat \(unter Verwendung von [\<serviceCertificate\>](../../../../../docs/framework/configure-apps/file-schema/wcf/servicecertificate-of-servicecredentials.md)\) im [\<serviceCredentials\>](../../../../../docs/framework/configure-apps/file-schema/wcf/servicecredentials.md)\-Dienstverhalten angeben muss.  Dieser Modus ist mit SOAP\-Stapeln interoperabel, die WS\-Trust und WS\-SecureConversation implementieren.<br /><br /> Wenn das `ClientCredentialType`\-Attribut `Windows` lautet, wird durch Festlegen dieses Attributs auf `false` die Kerberos\-basierte Authentifizierung angegeben.  Dies bedeutet, dass Client und Dienst Teil der gleichen Kerberos\-Domäne sein müssen.  Dieser Modus ist mit SOAP\-Stapeln interoperabel, die das Kerberos\-Tokenprofil \(gemäß der Definition in OASIS WSS TC\) sowie WS\-Trust und WS\-SecureConversation implementieren.<br /><br /> Wenn dieses Attribut `true` lautet, wird eine .NET SOAP\-Aushandlung verursacht, die den <xref:System.ServiceModel.Security.Tokens.ServiceModelSecurityTokenTypes.Spnego%2A>\-Austausch über SOAP\-Nachrichten tunnelt.<br /><br /> Die Standardeinstellung ist `true`.|  
  
## algorithmSuite\-Attribut  
  
|Wert|Beschreibung|  
|----------|------------------|  
|Basic128|Verwendet Aes128\-Verschlüsselung, Sha1 für den Nachrichtenhash und Rsa\-oaep\-mgf1p für Key Wrap.|  
|Basic192|Verwendet Aes192\-Verschlüsselung, Sha1 für den Nachrichtenhash und Rsa\-oaep\-mgf1p für Key Wrap.|  
|Basic256|Verwendet Aes256\-Verschlüsselung, Sha1 für den Nachrichtenhash und Rsa\-oaep\-mgf1p für Key Wrap.|  
|Basic256Rsa15|Verwendet Aes256\-Nachrichtenverschlüsselung, Sha1 für den Nachrichtenhash und Rsa15 für Key Wrap.|  
|Basic192Rsa15|Verwendet Aes192 für die Nachrichtenverschlüsselung, Sha1 für den Nachrichtenhash und Rsa15 für Key Wrap.|  
|TripleDes|Verwendet TripleDes\-Verschlüsselung, Sha1 für den Nachrichtenhash und Rsa\-oaep\-mgf1p für Key Wrap.|  
|Basic128Rsa15|Verwendet Aes128 für die Nachrichtenverschlüsselung, Sha1 für den Nachrichtenhash und Rsa15 für Key Wrap.|  
|TripleDesRsa15|Verwendet TripleDes\-Verschlüsselung, Sha1 für den Nachrichtenhash und Rsa15 für Key Wrap.|  
|Basic128Sha256|Verwendet Aes256\-Nachrichtenverschlüsselung, Sha256 für den Nachrichtenhash und Rsa\-oaep\-mgf1p für Key Wrap.|  
|Basic192Sha256|Verwendet Aes192 für die Nachrichtenverschlüsselung, Sha256 für den Nachrichtenhash und Rsa\-oaep\-mgf1p für Key Wrap.|  
|Basic256Sha256|Verwendet Aes256\-Nachrichtenverschlüsselung, Sha256 für den Nachrichtenhash und Rsa\-oaep\-mgf1p für Key Wrap.|  
|TripleDesSha256|Verwendet TripleDes für die Nachrichtenverschlüsselung, Sha256 für den Nachrichtenhash und Rsa\-oaep\-mgf1p für Key Wrap.|  
|Basic128Sha256Rsa15|Verwendet Aes128 für die Nachrichtenverschlüsselung, Sha256 für den Nachrichtenhash und Rsa15 für Key Wrap.|  
|Basic192Sha256Rsa15|Verwendet Aes192 für die Nachrichtenverschlüsselung, Sha256 für den Nachrichtenhash und Rsa15 für Key Wrap.|  
|Basic256Sha256Rsa15|Verwendet Aes256 für die Nachrichtenverschlüsselung, Sha256 für den Nachrichtenhash und Rsa15 für Key Wrap.|  
|TripleDesSha256Rsa15|Verwendet TripleDes für die Nachrichtenverschlüsselung, Sha256 für den Nachrichtenhash und Rsa15 für Key Wrap.|  
  
## clientCredentialType\-Attribut  
  
|Wert|Beschreibung|  
|----------|------------------|  
|`None`|Dies ermöglicht dem Dienst, mit anonymen Clients zu interagieren.  Auf Dienstseite wird dadurch angegeben, dass der Dienst keine Clientanmeldeinformationen erfordert.  Auf Clientseite wird dadurch angegeben, dass der Client keine Clientanmeldeinformationen bereitstellt.|  
|`Certificate`|Ermöglicht dem Dienst, die Forderung zu stellen, dass der Client über ein Zertifikat authentifiziert werden muss.  Wenn der `message`\-Sicherheitsmodus verwendet wird und das `negotiateServiceCredential`\-Attribut auf `false` festgelegt ist, muss dem Client das Dienstzertifikat zur Verfügung gestellt werden.|  
|`IssuedToken`|Gibt ein benutzerdefiniertes Token an, das in der Regel von einem Sicherheitstokendienst ausgegeben wird.|  
|`UserName`|Ermöglicht es dem Dienst, zu fordern, dass der Client unter Verwendung von `UserName`\-Anmeldeinformationen authentifiziert werden muss.  [!INCLUDE[indigo2](../../../../../includes/indigo2-md.md)] unterstützt kein Kennwortdigest und keine ableitenden Schlüssel mit Kennwörtern sowie keine Verwendung solcher Schlüssel für die Nachrichtensicherheit.  [!INCLUDE[indigo2](../../../../../includes/indigo2-md.md)] erzwingt, dass der Transport geschützt wird, wenn `UserName`\-Anmeldeinformationen verwendet werden.  Dieser Modus führt entweder zu einem interoperablen Austausch oder zu einer nicht interoperablen Aushandlung basierend auf dem `negotiateServiceCredential`\-Attribut.|  
|`Windows`|Dies ermöglicht den SOAP\-Austausch im Rahmen des authentifizierten Kontexts von `Windows`\-Anmeldeinformationen.  Wenn das `negotiateServiceCredential`\-Attribut auf `true` festgelegt ist, wird entweder eine SSPI\-Verhandlung oder Kerberos \(ein interoperabler Standard\) ausgeführt.|  
  
### Untergeordnete Elemente  
 Keine  
  
### Übergeordnete Elemente  
  
|Element|Beschreibung|  
|-------------|------------------|  
|[\<Sicherheit\>](../../../../../docs/framework/configure-apps/file-schema/wcf/security-of-ws2007httpbinding.md)|Definiert die Sicherheitseinstellungen für [\<ws2007HttpBinding\>](../../../../../docs/framework/configure-apps/file-schema/wcf/ws2007httpbinding.md).|  
  
## Siehe auch  
 <xref:System.ServiceModel.NonDualMessageSecurityOverHttp>   
 <xref:System.ServiceModel.Configuration.WSHttpSecurityElement.Message%2A>   
 <xref:System.ServiceModel.WSHttpSecurity.Message%2A>   
 <xref:System.ServiceModel.Configuration.NonDualMessageSecurityOverHttpElement>   
 [Sichern von Diensten und Clients](../../../../../docs/framework/wcf/feature-details/securing-services-and-clients.md)   
 [Bindungen](../../../../../docs/framework/wcf/bindings.md)   
 [Konfigurieren der vom System bereitgestellten Bindungen](../../../../../docs/framework/wcf/feature-details/configuring-system-provided-bindings.md)   
 [Using Bindings to Configure Windows Communication Foundation Services and Clients](http://msdn.microsoft.com/de-de/bd8b277b-932f-472f-a42a-b02bb5257dfb)   
 [\<Bindung\>](../../../../../docs/framework/misc/binding.md)