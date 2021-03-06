---
title: 'Gewusst wie: Erstellen und Initialisieren von Ablaufverfolgungsquellen'
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology:
- dotnet-clr
ms.tgt_pltfrm: 
ms.topic: article
dev_langs:
- VB
- CSharp
- C++
- jsharp
helpviewer_keywords:
- trace sources
- initializing trace sources
- configuration files [.NET Framework], trace sources
ms.assetid: f88dda6f-5fda-45be-9b3c-745a9b708c4d
caps.latest.revision: 10
author: mairaw
ms.author: mairaw
manager: wpickett
ms.translationtype: HT
ms.sourcegitcommit: 306c608dc7f97594ef6f72ae0f5aaba596c936e1
ms.openlocfilehash: 1df5f06afaf23795a0efd6af763e29193ba82d90
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---
# <a name="how-to-create-and-initialize-trace-sources"></a>Gewusst wie: Erstellen und Initialisieren von Ablaufverfolgungsquellen
Die <xref:System.Diagnostics.TraceSource>-Klasse wird von Anwendungen verwendet, um Ablaufverfolgungen zu erzeugen, die der Anwendung zugeordnet werden können. <xref:System.Diagnostics.TraceSource> stellt Ablaufverfolgungsmethoden bereit, mit denen Sie Ereignisse und Daten einfach verfolgen und Ablaufverfolgungen zu Informationszwecken ausgeben können. Die Ablaufverfolgungsausgabe von <xref:System.Diagnostics.TraceSource> kann mit oder ohne Konfigurationsdateien erstellt und initialisiert werden. Dieses Thema enthält Anweisungen für beide Varianten. Es wird jedoch empfohlen, Konfigurationsdateien zu verwenden, um die Neukonfiguration der durch die Ablaufverfolgungsquellen erzeugten Ablaufverfolgungen zur Laufzeit zu vereinfachen.  
  
### <a name="to-create-and-initialize-a-trace-source-using-a-configuration-file"></a>So erstellen und initialisieren Sie eine Ablaufverfolgungsquelle mit einer Konfigurationsdatei  
  
1.  Erstellen Sie ein Visual Studio-Konsolenanwendungsprojekt, und ersetzen Sie den angegebenen Code durch den folgenden Code. Dieser Code protokolliert Fehler und Warnungen und gibt einige davon in der Konsole aus und einige davon in der myListener-Datei, die von den Einträgen in der Konfigurationsdatei erstellt wird.  
  
     [!code-csharp[TraceSourceExample1#1](../../../samples/snippets/csharp/VS_Snippets_CLR/tracesourceexample1/cs/program.cs#1)] [!code-vb[TraceSourceExample1#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/tracesourceexample1/vb/program.vb#1)]  
  
2.  Fügen Sie dem Projekt eine Anwendungskonfigurationsdatei hinzu, falls keine vorhanden ist, um die Ablaufverfolgungsquelle mit dem Namen `TraceSourceApp` aus Schritt 1 des Codebeispiels zu initialisieren.  
  
3.  Ersetzen Sie den Standardinhalt der Konfigurationsdatei durch die folgenden Einstellungen, um einen Konsolen-Ablaufverfolgungslistener und einen Textwriter-Ablaufverfolgungslistener für die Ablaufverfolgungsquelle zu initialisieren, die in Schritt 1 erstellt wurde.  
  
    ```xml  
    <configuration>  
      <system.diagnostics>  
        <sources>  
          <source name="TraceSourceApp"   
            switchName="sourceSwitch"   
            switchType="System.Diagnostics.SourceSwitch">  
            <listeners>  
              <add name="console"   
                type="System.Diagnostics.ConsoleTraceListener">  
                <filter type="System.Diagnostics.EventTypeFilter"   
                  initializeData="Error"/>  
              </add>  
              <add name="myListener"/>  
              <remove name="Default"/>  
            </listeners>  
          </source>  
        </sources>  
        <switches>  
          <add name="sourceSwitch" value="Error"/>  
        </switches>  
        <sharedListeners>  
          <add name="myListener"   
            type="System.Diagnostics.TextWriterTraceListener"   
            initializeData="myListener.log">  
            <filter type="System.Diagnostics.EventTypeFilter"   
              initializeData="Error"/>  
          </add>  
        </sharedListeners>  
      </system.diagnostics>  
    </configuration>  
    ```  
  
     Neben der Konfiguration für die Ablaufverfolgungslistener werden in der Konfigurationsdatei Filter für beide Listener sowie ein Quellschalter für die Ablaufverfolgungsquelle erstellt. Es werden zwei Verfahren zum Hinzufügen der Ablaufverfolgungslistener gezeigt: das direkte Hinzufügen des listeners zur Ablaufverfolgungsquelle sowie das Hinzufügen eines Listeners zur gemeinsam genutzten Auflistung der Listener und das anschließende Hinzufügen zur Ablaufverfolgungsquelle nach Namen. Die für die beiden Listener angegebenen Filter werden mit unterschiedlichen Stufen für die Quelle initialisiert. Dies führt dazu, dass einige Meldungen nur von einem der beiden Listener geschrieben werden.  
  
     Die Konfigurationsdatei initialisiert die Einstellungen für die Ablaufverfolgungsquelle, wenn die Anwendung initialisiert wird. Die Anwendung kann die durch die Konfigurationsdatei festgelegten Eigenschaften dynamisch ändern, um die vom Benutzer angegebenen Einstellungen zu überschreiben. So können Sie beispielsweise sicherstellen, dass Meldungen für kritische Fehler unabhängig von den aktuellen Konfigurationseinstellungen immer in eine Textdatei geschrieben werden. Im Beispielcode wird veranschaulicht, wie Einstellungen in der Konfigurationsdatei überschrieben werden können, um sicherzustellen, dass Meldungen für schwerwiegende Fehler an die Ablaufverfolgungslistener ausgegeben werden.  
  
     Wenn Sie die Einstellungen aus der Konfigurationsdatei ändern, während die Anwendung ausgeführt wird, werden die ursprünglichen Einstellungen nicht geändert. Um die Einstellungen zu ändern, müssen Sie entweder die Anwendung neu starten oder die Anwendung programmgesteuert aktualisieren, indem Sie die <xref:System.Diagnostics.Trace.Refresh%2A?displayProperty=fullName>-Methode verwenden.  
  
### <a name="to-initialize-trace-sources-listeners-and-filters-without-a-configuration-file"></a>So initialisieren Sie Ablaufverfolgungsquellen, Listener und Filter ohne eine Konfigurationsdatei  
  
-   Verwenden Sie den folgenden Beispielcode, um die Ablaufverfolgung mit einer Ablaufverfolgungsquelle zu aktivieren, ohne eine Konfigurationsdatei zu verwenden. Dieses Verfahren wird nicht empfohlen. Möglicherweise gibt es jedoch Situationen, in denen Sie sich nicht auf Konfigurationsdateien verlassen möchten, um die Ablaufverfolgung sicherzustellen.  
  
     [!code-csharp[TraceSourceExample2#1](../../../samples/snippets/csharp/VS_Snippets_CLR/tracesourceexample2/cs/program.cs#1)] [!code-vb[TraceSourceExample2#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/tracesourceexample2/vb/program.vb#1)]  
  
## <a name="see-also"></a>Siehe auch  
 <xref:System.Diagnostics.TraceSource>   
 <xref:System.Diagnostics.TextWriterTraceListener>   
 <xref:System.Diagnostics.ConsoleTraceListener>   
 <xref:System.Diagnostics.EventTypeFilter>   
 [Ablaufverfolgung und Instrumentieren von Anwendungen](../../../docs/framework/debug-trace-profile/tracing-and-instrumenting-applications.md)

