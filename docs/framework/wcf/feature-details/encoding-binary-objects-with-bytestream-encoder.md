---
title: "Codieren von bin&#228;ren Objekten mit dem ByteStream-Encoder | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 020ee981-c889-4b12-a3ea-91823ef46444
caps.latest.revision: 8
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 8
---
# Codieren von bin&#228;ren Objekten mit dem ByteStream-Encoder
Das Senden und Empfangen von unformatierten Binärdaten mit [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] wird mithilfe von <xref:System.ServiceModel.Channels.ByteStreamMessageEncodingBindingElement> konfiguriert.  
  
## Bytestream\-Nachrichtenencoder\-Architektur  
 Der von [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] verwendete binäre Nachrichtenencoder weist keine Funktionen zum Verarbeiten, Überprüfen oder Identifizieren der zugrunde liegenden Daten in der Nachricht auf.Das Datenpaket wird in XML codiert, gesendet, empfangen und decodiert.Der Encoder verarbeitet die Daten, nachdem diese an den Transport übergeben wurden und bevor die Nachricht an die Nachrichtenwarteschlange gesendet wird.Der Funktion nach schließt der binäre Encoder die Nachrichtendaten zum Senden in `<binary>`\-Elemente ein und entfernt die Elemente, nachdem die Nachricht empfangen wurde.  
  
## Verwenden des Bytestream\-Nachrichtenencoders  
 Das folgende Beispiel zeigt einen Dienstvertrag, der den Bytestream\-Nachrichtenencoder implementiert.  
  
```csharp  
[OperationContract]  
Void Myfunction(Stream stream);  
```  
  
 Das folgende Beispiel zeigt, wie der Dienst aufgerufen wird.  
  
```csharp  
proxy.MyFunction(stream);  
```  
  
 Falls Sie einen Dienst verwenden, der eine Nachrichteninfrastruktur implementiert \(zum Beispiel ein Router\), wird die Nachricht verarbeitet, ohne dass diese untersucht bzw. überprüft oder dass auf andere Art damit interagiert wird. Dies wird im folgenden Beispiel veranschaulicht.  
  
```csharp  
[OperationContract]  
void ProcessMessage(Message message) ;  
```  
  
## Szenarien  
 Der Bytestreamencoder ist in folgenden Szenarios nützlich.  
  
-   Übertragen eines JPEG\-Bilds zwischen Computern mit [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)].In diesem Szenario wird das Bild über den Transport von einer externen Quelle empfangen. Bei den gesendeten Daten handelt es sich um die unformatierten Bytes, aus denen das Bild besteht.Ein Dienst empfängt die Binärdaten und zeigt das Bild an.  
  
-   Lesen von Informationen aus einer Meldungswarteschlange und Verarbeiten der Informationen.Die Meldung wird von einem Meldungswarteschlangen\-Manager gelesen und zur Verarbeitung an den Meldungswarteschlangenkanal übergeben.Der Meldungswarteschlangenkanal fungiert im [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)]\-Kanalstapel als Warteschlangen\-Manager.  
  
 Wenn eine Meldung über einen Meldungswarteschlangenkanal gesendet wird, hat der Absender keine Kontrolle über die vom Warteschlangen\-Manger empfangenen Bytes.Wenn im Empfängerprozess keine unformatierten Bytes gelesen werden können, wird die Meldung als falsch formatierte Meldung empfangen und nicht verarbeitet. Es wird davon ausgegangen, dass der Empfängerprozess die empfangenen Bytes in ein akzeptables Format übersetzen kann.