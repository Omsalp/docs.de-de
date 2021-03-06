---
title: "How to: Specify a Task Scheduler in a Dataflow Block | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-standard"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "TPL dataflow library, linking to task scheduler in TPL"
  - "Task Parallel Library, dataflows"
  - "task scheduler, linking from TPL"
ms.assetid: 27ece374-ed5b-49ef-9cec-b20db34a65e8
caps.latest.revision: 7
author: "rpetrusha"
ms.author: "ronpet"
manager: "wpickett"
caps.handback.revision: 7
---
# How to: Specify a Task Scheduler in a Dataflow Block
Dieses Dokument veranschaulicht, wie Sie einen bestimmten Taskplaner zuweisen, wenn Sie Datenfluss in Ihrer Anwendung verwenden.  Das Beispiel verwendet die <xref:System.Threading.Tasks.ConcurrentExclusiveSchedulerPair?displayProperty=fullName>\-Klasse in einer Windows Forms\-Anwendung, um anzuzeigen, wann die Readeraufgaben aktiv sind und wann eine Writeraufgabe aktiv ist.  Darüber hinaus verwendet das Beispiel die <xref:System.Threading.Tasks.TaskScheduler.FromCurrentSynchronizationContext%2A?displayProperty=fullName>\-Methode, um zu ermögliche, dass ein Datenflussblock im Benutzeroberflächenthread ausgeführt wird.  
  
> [!TIP]
>  Die TPL\-Datenflussbibliothek \(<xref:System.Threading.Tasks.Dataflow?displayProperty=fullName>\-Namespace\) ist nicht in [!INCLUDE[net_v45](../../../includes/net-v45-md.md)] enthalten.  Öffnen Sie zum Installieren des <xref:System.Threading.Tasks.Dataflow>\-Namespace das Projekt in [!INCLUDE[vs_dev11_long](../../../includes/vs-dev11-long-md.md)], wählen Sie im Menü "Projekt" die Option **NuGet\-Pakete verwalten** aus, und suchen Sie online nach dem `Microsoft.Tpl.Dataflow`\-Paket.  
  
### Erstellen der Windows Forms\-Anwendung  
  
1.  Erstellen Sie ein [!INCLUDE[csprcs](../../../includes/csprcs-md.md)]\-Projekt oder ein Visual Basic **Windows Forms\-Anwendungsprojekt**.  In den folgenden Schritten wird das Projekt `WriterReadersWinForms` benannt.  
  
2.  Fügen Sie im Formular\-Designer für das Hauptformular „Form1.cs“ \(„Form1.vb“ in [!INCLUDE[vbprvb](../../../includes/vbprvb-md.md)]\) vier <xref:System.Windows.Forms.CheckBox>\-Steuerelemente hinzu.  Legen Sie die <xref:System.Windows.Forms.Control.Text%2A>\-Eigenschaft auf „Reader 1“ für `checkBox1`, „Reader 2“ für `checkBox2`, „Reader 3“ für `checkBox3` und „Writer“ für `checkBox4` fest.  Legen Sie die <xref:System.Windows.Forms.Control.Enabled%2A>\-Eigenschaft für jedes Steuerelement auf `False` fest.  
  
3.  Fügen Sie dem Formular ein <xref:System.Windows.Forms.Timer>\-Steuerelement hinzu.  Legen Sie die <xref:System.Windows.Forms.Timer.Interval%2A>\-Eigenschaft auf `2500` fest.  
  
## Hinzufügen der Datenflussfunktionalität  
 In diesem Abschnitt wird beschrieben, wie Sie die Datenflussblöcke erstellen, die Teil der Anwendung sind, und wie Sie die einzelnen Blöcke einem Taskplaner zuordnen.  
  
#### Hinzufügen von Datenflussfunktionalität zur Anwendung  
  
1.  Fügen Sie dem Projekt einen Verweis auf „System.Threading.Tasks.Dataflow.dll“ hinzu.  
  
2.  Stellen Sie sicher, dass „Form1.cs“ \(„Form1.vb“ bei [!INCLUDE[vbprvb](../../../includes/vbprvb-md.md)]\) die folgenden `using`\-Anweisungen \(`Imports` in [!INCLUDE[vbprvb](../../../includes/vbprvb-md.md)]\) enthält.  
  
     [!code-csharp[TPLDataflow_WriterReadersWinForms#1](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_writerreaderswinforms/cs/writerreaderswinforms/form1.cs#1)]
     [!code-vb[TPLDataflow_WriterReadersWinForms#1](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_writerreaderswinforms/vb/writerreaderswinforms/form1.vb#1)]  
  
3.  Fügen Sie ein <xref:System.Threading.Tasks.Dataflow.BroadcastBlock%601>\-Datenmember zur `Form1`\-Klasse hinzu.  
  
     [!code-csharp[TPLDataflow_WriterReadersWinForms#2](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_writerreaderswinforms/cs/writerreaderswinforms/form1.cs#2)]
     [!code-vb[TPLDataflow_WriterReadersWinForms#2](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_writerreaderswinforms/vb/writerreaderswinforms/form1.vb#2)]  
  
4.  Erstellen Sie im `Form1`\-Konstuktor nach dem Aufruf von `InitializeComponent` ein <xref:System.Threading.Tasks.Dataflow.ActionBlock%601>\-Objekt, das den Status der <xref:System.Windows.Forms.CheckBox>\-Objekte wechselt.  
  
     [!code-csharp[TPLDataflow_WriterReadersWinForms#3](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_writerreaderswinforms/cs/writerreaderswinforms/form1.cs#3)]
     [!code-vb[TPLDataflow_WriterReadersWinForms#3](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_writerreaderswinforms/vb/writerreaderswinforms/form1.vb#3)]  
  
5.  Erstellen Sie im `Form1`\-Konstruktor ein <xref:System.Threading.Tasks.ConcurrentExclusiveSchedulerPair>\-Objekt und vier <xref:System.Threading.Tasks.Dataflow.ActionBlock%601>\-Objekte, ein <xref:System.Threading.Tasks.Dataflow.ActionBlock%601>\-Objekt für jedes <xref:System.Windows.Forms.CheckBox>\-Objekt.  Geben Sie für jedes <xref:System.Threading.Tasks.Dataflow.ActionBlock%601>\-Objekt ein <xref:System.Threading.Tasks.Dataflow.ExecutionDataflowBlockOptions>\-Objekt an, für das die <xref:System.Threading.Tasks.Dataflow.DataflowBlockOptions.TaskScheduler%2A>\-Eigenschaft auf die <xref:System.Threading.Tasks.ConcurrentExclusiveSchedulerPair.ConcurrentScheduler%2A>\-Eigenschaft der Reader und die <xref:System.Threading.Tasks.ConcurrentExclusiveSchedulerPair.ExclusiveScheduler%2A>\-Eigenschaft für den Writer festgelegt ist.  
  
     [!code-csharp[TPLDataflow_WriterReadersWinForms#4](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_writerreaderswinforms/cs/writerreaderswinforms/form1.cs#4)]
     [!code-vb[TPLDataflow_WriterReadersWinForms#4](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_writerreaderswinforms/vb/writerreaderswinforms/form1.vb#4)]  
  
6.  Starten Sie im `Form1`\-Konstruktor das <xref:System.Windows.Forms.Timer>\-Objekt.  
  
     [!code-csharp[TPLDataflow_WriterReadersWinForms#5](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_writerreaderswinforms/cs/writerreaderswinforms/form1.cs#5)]
     [!code-vb[TPLDataflow_WriterReadersWinForms#5](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_writerreaderswinforms/vb/writerreaderswinforms/form1.vb#5)]  
  
7.  Erstellen Sie im Formular\-Designer für das Hauptformular einen Ereignishandler für das <xref:System.Windows.Forms.Timer.Tick>\-Ereignis des Timers.  
  
8.  Implementieren Sie das <xref:System.Windows.Forms.Timer.Tick>\-Ereignis für den Timer.  
  
     [!code-csharp[TPLDataflow_WriterReadersWinForms#6](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_writerreaderswinforms/cs/writerreaderswinforms/form1.cs#6)]
     [!code-vb[TPLDataflow_WriterReadersWinForms#6](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_writerreaderswinforms/vb/writerreaderswinforms/form1.vb#6)]  
  
 Da der `toggleCheckBox`\-Datenflussblock als Benutzeroberfläche agiert, ist es wichtig, dass diese Aktion im Benutzeroberflächenthread erfolgt.  Um dies zu erreichen, stellt dieses Objekt während der Erstellung ein <xref:System.Threading.Tasks.Dataflow.ExecutionDataflowBlockOptions>\-Objekt bereit, für das die <xref:System.Threading.Tasks.Dataflow.DataflowBlockOptions.TaskScheduler%2A>\-Eigenschaft auf <xref:System.Threading.Tasks.TaskScheduler.FromCurrentSynchronizationContext%2A?displayProperty=fullName> festgelegt ist.  Die <xref:System.Threading.Tasks.TaskScheduler.FromCurrentSynchronizationContext%2A>\-Methode erstellt ein <xref:System.Threading.Tasks.TaskScheduler>\-Objekt, das Arbeiten im aktuellen Synchronisierungskontext durchführt.  Da der `Form1`\-Konstruktor über den Benutzeroberflächenthread aufgerufen wird, wird die Aktion für den `toggleCheckBox`\-Datenflussblock ebenfalls im Benutzeroberflächenthread ausgeführt.  
  
 Dieses Beispiel verwendet außerdem die <xref:System.Threading.Tasks.ConcurrentExclusiveSchedulerPair>\-Klasse, um die gleichzeitige Ausführung einiger Datenflussblöcke zu aktivieren. Außerdem ermöglicht sie, dass ein anderer Datenflussblock exklusiv für alle anderen Datenflussblöcke ausgeführt wird, die auf dem gleichen <xref:System.Threading.Tasks.ConcurrentExclusiveSchedulerPair>\-Objekt ausgeführt werden.  Dieses Verfahren ist nützlich, wenn mehrere Datenflussblöcke eine Ressource gemeinsam nutzen und einige davon exklusiven Zugriff auf die Ressource benötigen, da es aufgrund dieses Verfahrens nicht mehr notwendig ist, den Zugriff auf diese Ressource manuell zu synchronisieren.  Dadurch, dass die manuelle Synchronisierung entfällt, kann Code effizienter gestaltet werden.  
  
## Beispiel  
 Das folgende Beispiel zeigt den vollständigen Code für „Form1.cs“ \(„Form1.vb“ in [!INCLUDE[vbprvb](../../../includes/vbprvb-md.md)]\).  
  
 [!code-csharp[TPLDataflow_WriterReadersWinForms#100](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_writerreaderswinforms/cs/writerreaderswinforms/form1.cs#100)]
 [!code-vb[TPLDataflow_WriterReadersWinForms#100](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_writerreaderswinforms/vb/writerreaderswinforms/form1.vb#100)]  
  
## Siehe auch  
 [Datenfluss](../../../docs/standard/parallel-programming/dataflow-task-parallel-library.md)