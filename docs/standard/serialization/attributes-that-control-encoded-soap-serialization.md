---
title: Attribute zur Steuerung der Serialisierung von codiertem SOAP
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.tgt_pltfrm: 
ms.topic: article
dev_langs:
- VB
- CSharp
- C++
- jsharp
helpviewer_keywords:
- SOAP, XML serialization
- XML serialization, SOAP
- XML serialization, attributes
- attributes [.NET Framework], XML serialization
- serialization, attributes
ms.assetid: 93ee258c-9c0f-4a08-897c-c10db7a00f91
caps.latest.revision: 8
author: Erikre
ms.author: erikre
manager: erikre
ms.translationtype: HT
ms.sourcegitcommit: 717bcb6f9f72a728d77e2847096ea558a9c50902
ms.openlocfilehash: a6af8cd2560bb9c39657d8e5b088954996ba2a04
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---
# <a name="attributes-that-control-encoded-soap-serialization"></a>Attribute zur Steuerung der Serialisierung von codiertem SOAP 
Das vom World Wide Web Consortium (www.w3.org) herausgegebene Dokument mit dem Titel „Simple Object Access Protocol (SOAP) 1.1“ enthält einen optionalen Abschnitt (Abschnitt 5), in dem die Codierung von SOAP-Parametern beschrieben wird. Um dem Abschnitt 5 dieser Spezifikation zu entsprechen, müssen Sie spezielle Attribute verwenden, die im <xref:System.Xml.Serialization>-Namespace enthalten sind. Wenden Sie diese Attribute auf die entsprechenden Klassen und Member der Klassen an, und verwenden Sie dann <xref:System.Xml.Serialization.XmlSerializer>, um Instanzen dieser Klasse oder Klassen zu serialisieren.  
  
 In der folgenden Tabelle sind die Attribute, ihr Anwendungsbereich und ihre Funktion aufgeführt. Weitere Informationen zum Einsatz dieser Attribute zur Steuerung der XML-Serialisierung finden Sie unter [Vorgehensweise: Serialisieren eines Objekts als SOAP-codierter XML-Stream](../../../docs/standard/serialization/how-to-serialize-an-object-as-a-soap-encoded-xml-stream.md) und [Vorgehensweise: Überschreiben von codierter SOAP-XML-Serialisierung](../../../docs/standard/serialization/how-to-override-encoded-soap-xml-serialization.md).  
  
 Weitere Informationen zu Attributen finden Sie unter [Attribute](../../../docs/standard/attributes/index.md).  
  
|Attribut|Betrifft|Bedeutung|  
|---------------|----------------|---------------|  
|<xref:System.Xml.Serialization.SoapAttributeAttribute>|Öffentliches Feld, Eigenschaft, Parameter oder Rückgabewert.|Der Klassenmember wird als XML-Attribut serialisiert.|  
|<xref:System.Xml.Serialization.SoapElementAttribute>|Öffentliches Feld, Eigenschaft, Parameter oder Rückgabewert.|Die Klasse wird als XML-Element serialisiert.|  
|<xref:System.Xml.Serialization.SoapEnumAttribute>|Öffentliches Feld, das ein Enumerationsbezeichner ist.|Der Elementname eines Enumerationsmembers.|  
|<xref:System.Xml.Serialization.SoapIgnoreAttribute>|Öffentliche Eigenschaften und Felder.|Die Eigenschaft oder das Feld wird beim Serialisieren der Klasse, in dem sie bzw. es enthalten ist, ignoriert.|  
|<xref:System.Xml.Serialization.SoapIncludeAttribute>|Öffentliche abgeleitete Klassendeklarationen und öffentliche Methoden für WSDL-Dokumente (Web Services Description Language).|Der Typ wird beim Generieren von Schemas eingeschlossen (und daher bei der Serialisierung erkannt).|  
|<xref:System.Xml.Serialization.SoapTypeAttribute>|Deklarationen öffentlicher Klassen.|Die Klasse wird als XML-Typ serialisiert.|  
  
## <a name="see-also"></a>Siehe auch  
 [XML- und SOAP-Serialisierung](../../../docs/standard/serialization/xml-and-soap-serialization.md)   
 [Vorgehensweise: Serialisieren eines Objekts als SOAP-codierter XML-Stream](../../../docs/standard/serialization/how-to-serialize-an-object-as-a-soap-encoded-xml-stream.md)   
 [Vorgehensweise: Überschreiben von codierter SOAP-XML-Serialisierung](../../../docs/standard/serialization/how-to-override-encoded-soap-xml-serialization.md)   
 [Attribute](../../../docs/standard/attributes/index.md)   
 <xref:System.Xml.Serialization.XmlSerializer>   
 [How to: Serialize an Object (Vorgehensweise: Serialisieren eines Objekts)](../../../docs/standard/serialization/how-to-serialize-an-object.md)   
 [Vorgehensweise: Deserialisieren eines Objekts](../../../docs/standard/serialization/how-to-deserialize-an-object.md)

