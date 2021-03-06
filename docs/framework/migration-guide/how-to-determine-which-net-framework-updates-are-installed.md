---
title: 'Gewusst wie: Bestimmen der installierten .NET Framework-Updates'
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology:
- dotnet-clr
ms.tgt_pltfrm: 
ms.topic: article
helpviewer_keywords:
- updates, determining for .NET Framework
- .NET Framework, determining updates
ms.assetid: 53c7b5f7-d47a-402a-b194-7244a696a88b
caps.latest.revision: 6
author: mairaw
ms.author: mairaw
manager: wpickett
ms.translationtype: HT
ms.sourcegitcommit: 306c608dc7f97594ef6f72ae0f5aaba596c936e1
ms.openlocfilehash: b29b402e859688dcced6bd4429b18298070fb5e4
ms.contentlocale: de-de
ms.lasthandoff: 09/19/2017

---
# <a name="how-to-determine-which-net-framework-updates-are-installed"></a>Gewusst wie: Bestimmen der installierten .NET Framework-Updates
Die installierten Updates für jede auf einem Computer installierten Version von .NET Framework werden in der Windows-Registrierung aufgeführt. Sie können diese Informationen mit dem Registrierungs-Editor (regedit.exe) anzeigen.  
  
 Im Registrierungs-Editor werden die .NET Framework-Versionen und die installierten Updates für jede Version in verschiedenen Unterschlüsseln gespeichert. Informationen zum Ermitteln der installierten Versionsnummern finden Sie unter [Gewusst wie: Bestimmen der installierten .NET Framework-Versionen](../../../docs/framework/migration-guide/how-to-determine-which-versions-are-installed.md). Informationen zum Installieren von .NET Framework finden Sie unter [Install the .NET Framework for developers (Installieren von .NET Framework für Entwickler)](../../../docs/framework/install/guide-for-developers.md).  
  
### <a name="to-find-installed-updates"></a>So suchen Sie installierte Updates  
  
1.  Öffnen Sie das Programm **regedit.exe**. Öffnen Sie in Windows 8 und höher die Startseite, und geben Sie den Namen ein. In früheren Versionen von Windows wählen Sie im **Startmenü** **Ausführen** aus, und geben Sie dann in das Feld **Öffnen** den Wert **regedit.exe** ein.  
  
     Sie müssen über Administratorrechte verfügen, um regedit.exe ausführen zu können.  
  
2.  Öffnen Sie im Registrierungs-Editor den folgenden Unterschlüssel:  
  
     HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Updates  
  
     Die installierten Updates werden unter Unterschlüsseln aufgeführt, die die für sie geltende .NET Framework-Version identifizieren. Jedes Update wird durch eine Knowledge Base (KB)-Nummer identifiziert.  
  
## <a name="example"></a>Beispiel  
 Der folgende Code ermittelt programmgesteuert die auf einem Computer installierten .NET Framework-Updates. Sie müssen über Administratorrechte verfügen, um dieses Beispiel ausführen zu können.  
  
 [!code-csharp[ListUpdates#1](../../../samples/snippets/csharp/VS_Snippets_CLR/listupdates/cs/program.cs#1)]
 [!code-vb[ListUpdates#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/listupdates/vb/program.vb#1)]  
  
 Das Beispiel führt zu einer Ausgabe, die der folgenden Ausgabe ähnelt:  
  
```  
Microsoft .NET Framework 3.5 SP1  
  KB953595  Hotfix for Microsoft .NET Framework 3.5 SP1 (KB953595)  
  SP1  
    KB2657424  Security Update for Microsoft .NET Framework 3.5 SP1 (KB2657424)  
    KB958484  Hotfix for Microsoft .NET Framework 3.5 SP1 (KB958484)  
    KB963707  Update for Microsoft .NET Framework 3.5 SP1 (KB963707)  
Microsoft .NET Framework 4 Client Profile  
  KB2160841  Security Update for Microsoft .NET Framework 4 Client Profile (KB2160841)  
  KB2446708  Security Update for Microsoft .NET Framework 4 Client Profile (KB2446708)  
  KB2468871  Update for Microsoft .NET Framework 4 Client Profile (KB2468871)  
  KB2478663  Security Update for Microsoft .NET Framework 4 Client Profile (KB2478663)  
  KB2518870  Security Update for Microsoft .NET Framework 4 Client Profile (KB2518870)  
  KB2533523  Update for Microsoft .NET Framework 4 Client Profile (KB2533523)  
  KB2539636  Security Update for Microsoft .NET Framework 4 Client Profile (KB2539636)  
  KB2572078  Security Update for Microsoft .NET Framework 4 Client Profile (KB2572078)  
  KB2633870  Security Update for Microsoft .NET Framework 4 Client Profile (KB2633870)  
  KB2656351  Security Update for Microsoft .NET Framework 4 Client Profile (KB2656351)  
Microsoft .NET Framework 4 Extended  
  KB2416472  Security Update for Microsoft .NET Framework 4 Extended (KB2416472)  
  KB2468871  Update for Microsoft .NET Framework 4 Extended (KB2468871)  
  KB2487367  Security Update for Microsoft .NET Framework 4 Extended (KB2487367)  
  KB2533523  Update for Microsoft .NET Framework 4 Extended (KB2533523)  
  KB2656351  Security Update for Microsoft .NET Framework 4 Extended (KB2656351)  
```  
  
## <a name="see-also"></a>Siehe auch

[Gewusst wie: Bestimmen der installierten .NET Framework-Versionen](../../../docs/framework/migration-guide/how-to-determine-which-versions-are-installed.md)   
[Installieren von .NET Framework](../../../docs/framework/install/guide-for-developers.md)   
[Versionen und Abhängigkeiten](../../../docs/framework/migration-guide/versions-and-dependencies.md)

