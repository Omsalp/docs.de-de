---
title: -noconfig (C#-Compileroptionen)
ms.date: 2015-07-20
ms.prod: .net
ms.technology:
- devlang-csharp
ms.topic: article
f1_keywords:
- /noconfig
dev_langs:
- CSharp
helpviewer_keywords:
- /noconfig compiler option [C#]
- csc.rsp
- -noconfig compiler option [C#]
- noconfig compiler option [C#]
ms.assetid: cd26967e-e494-4c8c-b5c9-af13b2f78b2e
caps.latest.revision: 11
author: BillWagner
ms.author: wiwagn
translation.priority.ht:
- cs-cz
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pl-pl
- pt-br
- ru-ru
- tr-tr
- zh-cn
- zh-tw
ms.translationtype: HT
ms.sourcegitcommit: 306c608dc7f97594ef6f72ae0f5aaba596c936e1
ms.openlocfilehash: 594e972dc834ab74412e30a48428f850ae02b5ac
ms.contentlocale: de-de
ms.lasthandoff: 07/28/2017

---
# <a name="noconfig-c-compiler-options"></a>/noconfig (C#-Compileroptionen)
Die Option **/noconfig** weist den Compiler an, nicht mit der Datei „csc.rsp“ zu kompilieren, die sich im gleichen Verzeichnis wie die Datei „csc.exe“ befindet und daraus geladen wird.  
  
## <a name="syntax"></a>Syntax  
  
```console  
/noconfig  
```  
  
## <a name="remarks"></a>Hinweise  
 Die Datei „csc.rsp“ verweist auf alle Assemblys, die im Lieferumfang von .NET Framework enthalten sind. Die tatsächlichen Verweise, die die .NET-Entwicklungsumgebung von Visual Studio enthält, hängen vom Projekttyp ab.  
  
 Sie können die Datei „csc.rsp“ ändern und zusätzliche Compileroptionen angeben, die in jeder Kompilierung über die Befehlszeile mit „csc.exe“ enthalten sein sollen (mit Ausnahme der Option **/noconfig**).  
  
 Der Compiler verarbeitet die an den Befehl **csc** übergebenen Optionen zuletzt. Aus diesem Grund überschreibt jede Option in der Befehlszeile die Einstellung für die gleiche Option in der Datei „csc.rsp“.  
  
 Wenn der Compiler nach den Einstellungen in der Datei „csc.rsp“ suchen und diese verwenden soll, geben Sie **/noconfig** an.  
  
 Diese Compileroption steht in Visual Studio nicht zur Verfügung und kann auch nicht programmgesteuert angepasst werden.  
  
## <a name="see-also"></a>Siehe auch  
 [C#-Compileroptionen](../../../csharp/language-reference/compiler-options/index.md)   
 [Verwalten von Projekt- und Projektmappeneigenschaften](/visualstudio/ide/managing-project-and-solution-properties)

