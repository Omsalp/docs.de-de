---
title: "Verwenden der Nachverfolgung zum Beheben von Anwendungsfehlern | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 8851adde-c3c2-4391-9523-d8eb831490af
caps.latest.revision: 7
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 7
---
# Verwenden der Nachverfolgung zum Beheben von Anwendungsfehlern
[!INCLUDE[wf](../../../includes/wf-md.md)]ermöglicht es Ihnen, workflowbezogene Informationen zu verfolgen, um Details zur Ausführung einer [!INCLUDE[wf2](../../../includes/wf2-md.md)]\-Anwendung oder eines WF\-Diensts zu erhalten.[!INCLUDE[wf2](../../../includes/wf2-md.md)]\-Hosts können Workflowereignisse während der Ausführung einer Workflowinstanz erfassen.Wenn der Workflow Fehler oder Ausnahmen generiert, können Sie mithilfe der Details aus der [!INCLUDE[wf2](../../../includes/wf2-md.md)]\-Überwachung Probleme bei der Verarbeitung behandeln.  
  
## Problembehandlung von WF mit der WF\-Überwachung  
 Um Fehler bei der Verarbeitung einer [!INCLUDE[wf2](../../../includes/wf2-md.md)]\-Aktivität zu erkennen, können Sie die Überwachung mit einem Überwachungsprofil aktivieren, das eine Abfrage nach einem <xref:System.Activities.Tracking.ActivityStateRecord> mit dem Status "Faulted" durchführt.Die entsprechende Abfrage ist im folgenden Code angegeben.  
  
```  
<activityStateQueries>  
              <activityStateQuery activityName="*">  
                <states>  
                  <state name="Faulted" />  
                </states>  
              </activityStateQuery>  
 </activityStateQueries>  
```  
  
 Wenn ein Fehler weitergegeben und in einem Fehlerhandler behandelt wird \(z. B. eine <xref:System.Activities.Statements.TryCatch>\-Aktivität\), kann dies mit einem <xref:System.Activities.Tracking.FaultPropagationRecord> erkannt werden kann.Der <xref:System.Activities.Tracking.FaultPropagationRecord> zeigt die Quellaktivität des Fehlers und den Namen des Fehlerhandlers an.Der <xref:System.Activities.Tracking.FaultPropagationRecord> enthält Fehlerdetails in Form des Ausnahmestapels für den Fehler. Die Abfrage zum Abonnieren eines <xref:System.Activities.Tracking.FaultPropagationRecord> lautet:  
  
```  
<faultPropagationQueries>  
              <faultPropagationQuery faultSourceActivityName ="*" faultHandlerActivityName="*"/>  
 </faultPropagationQueries>  
  
```  
  
 Wenn ein Fehler nicht innerhalb des Workflows behandelt wird, führt er zu einer nicht behandelten Ausnahme der Workflowinstanz, und die Workflowinstanz wird abgebrochen.Um die Details der nicht behandelten Ausnahme zu verstehen, muss das Überwachungsprofil eine Abfrage des Workflowinstanz\-Datensatzes nach `state name=”UnhandledException”` ausführen, wie im folgenden Beispiel angegeben.  
  
```  
<workflowInstanceQueries>  
              <workflowInstanceQuery>  
                <states>  
                  <state name="UnhandledException" />  
                </states>  
              </workflowInstanceQuery>  
</workflowInstanceQueries>  
```  
  
 Wenn in einer Workflowinstanz eine nicht behandelte Ausnahme auftritt, wird ein <xref:System.Activities.Tracking.WorkflowInstanceUnhandledExceptionRecord>\-Objekt ausgegeben, wenn die [!INCLUDE[wf2](../../../includes/wf2-md.md)]\-Überwachung aktiviert wurde.  
  
 Dieser Überwachungsdatensatz enthält die Fehlerdetails in Form des Ausnahmestapels.Er weist Details der Fehlerquelle auf \(z. B. die Aktivität\), die zur nicht behandelten Ausnahme geführt hat. Um Fehlerereignisse von [!INCLUDE[wf2](../../../includes/wf2-md.md)] zu abonnieren, aktivieren Sie das Überwachen durch Hinzufügen eines Überwachungsteilnehmers.Konfigurieren Sie diesen Teilnehmer mit einem Überwachungsprofil, das `ActivityStateQuery (state=”Faulted”)`, <xref:System.Activities.Tracking.FaultPropagationRecord> und `WorkflowInstanceQuery (state=”UnhandledException”)` abfragt.  
  
 Wenn die Überwachung mit dem ETW\-Überwachungsteilnehmer aktiviert wurde, werden die Fehlerereignisse an eine ETW\-Sitzung ausgegeben.Die Ereignisse können mit der Ereignisanzeige angezeigt werden.Diese befindet sich unter dem Knoten **Ereignisanzeige\-\>Anwendungen und Dienstprotokolle\-\>Microsoft\-\>Windows\-\>Anwendungsserver\-Anwendungen** im analytischen Kanal.  
  
## Siehe auch  
 [Überwachung mit Windows Server AppFabric](http://go.microsoft.com/fwlink/?LinkId=201273)   
 [Überwachen von Anwendungen mit AppFabric](http://go.microsoft.com/fwlink/?LinkId=201275)