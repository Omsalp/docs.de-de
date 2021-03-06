---
title: "Gewusst wie: Wiedergabe von Sound in Windows Forms | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-winforms"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "jsharp"
helpviewer_keywords: 
  - "Wiedergabe von Tönen, Windows Forms"
  - "Wiedergabe von Sounds"
  - "SoundPlayer-Klasse"
  - "Sounds"
  - "My.Computer.Audio-Objekt, die Wiedergabe von Tönen"
  - "Beispiele [Windows Forms], sounds"
ms.assetid: 3d3350b7-1ebd-4e05-a738-48ca1160a19d
caps.latest.revision: 15
author: "dotnet-bot"
ms.author: "dotnetcontent"
manager: "wpickett"
caps.handback.revision: 15
---
# Gewusst wie: Wiedergabe von Sound in Windows Forms
In diesem Beispiel wird ein Sound in einem bestimmten Pfad zur Laufzeit wiedergegeben.  
  
## <a name="example"></a>Beispiel  
  
```vb  
Sub PlaySimpleSound()  
    My.Computer.Audio.Play("c:\Windows\Media\chimes.wav")  
End Sub  
```  
  
```csharp  
private void playSimpleSound()  
{  
    SoundPlayer simpleSound = new SoundPlayer(@"c:\Windows\Media\chimes.wav");  
    simpleSound.Play();  
}  
```  
  
## <a name="compiling-the-code"></a>Kompilieren des Codes  
 Für dieses Beispiel benötigen Sie Folgendes:  
  
-   Ersetzen Sie den Dateinamen `"c:\Windows\Media\chimes.wav"` durch einen gültigen Dateinamen.  
  
-   (C#) Ein Verweis auf die <xref:System.Media?displayProperty=fullName> Namespace.  
  
## <a name="robust-programming"></a>Stabile Programmierung  
 Dateioperationen sollten in entsprechende strukturierte Ausnahmebehandlungsblöcke eingeschlossen sein.  
  
 Die folgenden Bedingungen können einen Ausnahmefehler verursachen:  
  
-   Der Pfadname ist falsch formatiert. Angenommen, sie enthält ungültige Zeichen oder besteht nur aus Leerzeichen (<xref:System.ArgumentException> Klasse).  
  
-   Der Pfad ist schreibgeschützt (<xref:System.IO.IOException> Klasse).  
  
-   Der Pfadname ist `null` (<xref:System.ArgumentNullException> Klasse).  
  
-   Der Pfadname ist zu lang (<xref:System.IO.PathTooLongException> Klasse).  
  
-   Der Pfad ist ungültig (<xref:System.IO.DirectoryNotFoundException> Klasse).  
  
-   Der Pfad besteht nur aus einem Doppelpunkt ":" (<xref:System.NotSupportedException> Klasse).  
  
## <a name="net-framework-security"></a>.NET Framework-Sicherheit  
 Beurteilen Sie den Inhalt der Datei nicht anhand des Dateinamens. Bei der Datei `Form1.vb` handelt es sich zum Beispiel nicht unbedingt um eine Visual Basic-Quelldatei. Überprüfen Sie alle Eingaben, bevor Sie die Daten in der Anwendung verwenden.  
  
## <a name="see-also"></a>Siehe auch  
 <xref:System.Media.SoundPlayer>   
 [Gewusst wie: Laden eines Sounds asynchron in einem Windows Form](../../../../docs/framework/winforms/controls/how-to-load-a-sound-asynchronously-within-a-windows-form.md)   
 