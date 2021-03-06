---
title: Das aktualisierte .NET Core-Ereignismuster
description: "Erfahren Sie, wie das .NET Core-Ereignismuster die Flexibilität mit Abwärtskompabilität erhöht und wie Sie sichere Ereignisverarbeitung mit asynchronen Abonnenten implementieren können."
keywords: .NET, .NET Core
author: BillWagner
ms.author: wiwagn
ms.date: 06/20/2016
ms.topic: article
ms.prod: .net
ms.technology: devlang-csharp
ms.devlang: csharp
ms.assetid: 9aa627c3-3222-4094-9ca8-7e88e1071e06
ms.translationtype: HT
ms.sourcegitcommit: 306c608dc7f97594ef6f72ae0f5aaba596c936e1
ms.openlocfilehash: cf69cbe0a7adbd274d1cb9e9544dda77d9fa1740
ms.contentlocale: de-de
ms.lasthandoff: 07/28/2017

---

# <a name="the-updated-net-core-event-pattern"></a>Das aktualisierte .NET Core-Ereignismuster

[Vorherige](event-pattern.md)

Im vorherigen Artikel wurden die am häufigsten verwendeten Ereignismuster erläutert. .NET Core ist ein lockereres Muster. Die `EventHandler<TEventArgs>`-Definition hat in dieser Version nicht länger die Einschränkung, dass `TEventArgs` eine von `System.EventArgs` abgeleitete Klasse sein muss.

Dies erhöht die Flexibilität für Sie und ist abwärtskompatibel. Beginnen wir mit der Flexibilität. Die System.EventArgs-Klasse leitet eine Methode ein: `MemberwiseClone()`, die eine flache Kopie des Objekts erstellt.
Diese Methode muss Reflektion verwenden, um ihre Funktionalität für jede von `EventArgs` abgeleitete Klasse zu implementieren. Diese Funktionalität lässt sich in einer bestimmten abgeleiteten Klasse einfacher erstellen. Das bedeutet, dass das Ableiten von System.EventArgs eine Einschränkung ist, die Ihre Entwürfe beschränkt, aber keine zusätzlichen Vorteile bietet.
Tatsächlich können Sie die Definitionen von `FileFoundArgs` und `SearchDirectoryArgs` ändern, sodass sie nicht von `EventArgs` abgeleitet werden.
Das Programm funktioniert genauso.

Sie könnten auch `SearchDirectoryArgs` in eine Struktur ändern, wenn Sie eine weitere Änderung vornehmen:

```csharp  
internal struct SearchDirectoryArgs  
{  
    internal string CurrentSearchDirectory { get; }  
    internal int TotalDirs { get; }  
    internal int CompletedDirs { get; }  
    
    internal SearchDirectoryArgs(string dir, int totalDirs, 
        int completedDirs) : this()  
    {  
        CurrentSearchDirectory = dir;  
        TotalDirs = totalDirs;  
        CompletedDirs = completedDirs;  
    }  
}  
```   

Die zusätzliche Änderung besteht darin, den Standardkonstruktor aufzurufen, bevor der Konstruktor eingegeben wird, der alle Felder initialisiert. Ohne diesen Zusatz würden die Regeln von C# berichten, dass auf die Eigenschaften zugegriffen wird, bevor sie zugewiesen wurden.

Ändern Sie `FileFoundArgs` nicht von einer Klasse (Verweistyp) in eine Struktur (Werttyp). Das Protokoll zur Behandlung von Abbrüchen erfordert, dass die Ereignisargumente als Verweis übergeben werden. Wenn Sie diese Änderung vorgenommen hätten, könnte die Dateisuche-Klasse niemals Änderungen beobachten, die von einem der Ereignisabonnenten vorgenommen wurden. Für jeden Abonnenten würde eine neue Kopie der Struktur verwendet werden, und diese Kopie würde sich von derjenigen unterscheiden, die vom Dateisuche-Objekt gesehen wird.

Schauen wir uns nun an, wie diese Änderung abwärtskompatibel gemacht werden kann.
Das Entfernen der Einschränkung wirkt sich nicht auf vorhandenen Code aus. Alle vorhandenen Ereignisargumenttypen werden immer noch von `System.EventArgs` abgeleitet.
Abwärtskompatibilität ist ein wichtiger Grund, weshalb sie weiterhin von `System.EventArgs` ableiten. Vorhandene Ereignisabonnenten werden Abonnenten für ein Ereignis, die dem klassische Muster folgen.

Nach derselben Logik würden alle erstellten Ereignisargumenttypen keine Abonnenten in vorhandenen Codebases haben. Neue Ereignistypen, die nicht von `System.EventArgs` abgeleitet sind, unterbrechen diese Codebases nicht.

## <a name="events-with-async-subscribers"></a>Ereignisse mit asynchronen Abonnenten

Das letzte, in diesem Artikel behandelte Muster ist die richtige Schreibweise von Ereignisabonnenten, die asynchronen Code aufrufen. Informationen hierzu finden Sie im Artikel zu [Async und Await](async.md). Asynchrone Methoden können einen Rückgabetyp „Void“ haben, aber davon ist dringend abzuraten. Wenn Ihr Ereignisabonnent eine asynchrone Methode aufruft, haben Sie keine Wahl, außer eine `async void`-Methode zu erstellen. Sie wird von der Signatur des Ereignishandlers benötigt.

Sie müssen diesen entgegengesetzten Leitfaden abstimmen. Sie müssen irgendwie eine sichere `async void`-Methode erstellen. Die Grundlagen des Musters, das Sie implementieren müssen, finden Sie untenan:

```csharp
worker.StartWorking += async (sender, eventArgs) =>
{
    try 
    {
        await DoWorkAsync();
    }
    catch (Exception e)
    {
        //Some form of logging.
        Console.WriteLine($"Async task failure: {e.ToString()}");
        // Consider gracefully, and quickly exiting.
    }
};
```

Beachten Sie, dass der Handler als ein asynchroner Handler markiert ist. Da er einem Ereignishandler-Delegattyp zugewiesen wurde, weist er den Rückgabetyp „Void“ auf. Das bedeutet, dass Sie dem im Handler angezeigten Muster folgen müssen und nicht zulassen dürfen, dass Ausnahmen ausgelöst werden, die nicht zum Kontext des asynchronen Handlers gehören. Da der Handler keine Aufgabe zurückgibt, gibt es keine Aufgabe, die einen Fehler berichten kann, indem sie in den Fehlerzustand tritt. Da die Methode asynchron ist, kann sie die Ausnahme nicht einfach auslösen. (Die aufrufende Methode wird weiterhin ausgeführt, weil sie `async` (asynchron) ist.) Das tatsächliche Laufzeitverhalten wird für unterschiedliche Umgebungen unterschiedlich definiert. Sie kann den Thread oder das Programm beenden oder das Programm in einem unbestimmten Zustand belassen. Keine dieser Verhaltensweisen erzielt gute Ergebnisse.

Deshalb sollten Sie die Await-Anweisung für die asynchrone Aufgabe in Ihrem eigenen try-Block umschließen. Wenn sie einen fehlerhaften Vorgang verursacht, können Sie den Fehler protokollieren. Wenn es sich um einen Fehler handelt, aus dem sich Ihre Anwendung nicht wiederherstellen lässt, können Sie das Programm schnell und problemlos beenden.

Dies sind die wichtigsten Updates für das .NET-Ereignismuster. In den Bibliotheken, mit denen Sie arbeiten, Sie zahlreiche Beispiele für die früheren Versionen. Allerdings sollten Sie auch die neuesten Muster kennenlernen.

Der nächste Artikel dieser Reihe hilft Ihnen dabei, zwischen der Verwendung von `delegates` und `events` in ihren Entwürfen zu unterscheiden. Da sich beide Konzepte ähneln, wird Ihnen dieser Artikel helfen, die beste Entscheidung für Ihre Programme zu treffen.

[Nächste](distinguish-delegates-events.md)

