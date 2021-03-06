---
title: "Benutzerdefinierte Nachverfolgung | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 2d191c9f-62f4-4c63-92dd-cda917fcf254
caps.latest.revision: 16
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 16
---
# Benutzerdefinierte Nachverfolgung
Anhand dieses Beispiels wird veranschaulicht, wie eine benutzerdefinierte Nachverfolgungskomponente erstellt und der Inhalt der Nachverfolgungsdaten in die Konsole geschrieben wird.Außerdem wird veranschaulicht, wie mit benutzerdefinierten Daten aufgefüllte <xref:System.Activities.Tracking.CustomTrackingRecord>\-Objekte ausgegeben werden.Die konsolenbasierte Nachverfolgungskomponente filtert die vom Workflow ausgegebenen <xref:System.Activities.Tracking.TrackingRecord>\-Objekte mit einem im Code erstellten Nachverfolgungsprofilobjekt.  
  
## Beispieldetails  
 [!INCLUDE[wf](../../../../includes/wf-md.md)] stellt eine Infrastruktur zur Nachverfolgung der Ausführung einer Workflowinstanz bereit.Die Nachverfolgungslaufzeit implementiert eine Workflowinstanz, um Ereignisse in Verbindung mit dem Workflowlebenszyklus, Ereignisse aus den Workflowaktivitäten sowie benutzerdefinierte Nachverfolgungsereignisse auszugeben.In der folgenden Tabelle sind die primären Komponenten der Nachverfolgungsinfrastruktur aufgeführt.  
  
|Komponente|Beschreibung|  
|----------------|------------------|  
|Überwachungslaufzeit|Stellt die Infrastruktur zur Ausgabe von Nachverfolgungsdatensätzen bereit.|  
|Nachverfolgungskomponente|Verwendet die Nachverfolgungsdatensätze.[!INCLUDE[netfx40_short](../../../../includes/netfx40-short-md.md)] wird mit einem Nachverfolgungsteilnehmer geliefert, der Nachverfolgungsdatensätze als Ereignisse der Ereignisablaufverfolgung für Windows \(ETW\) schreibt.|  
|Überwachungsprofil|Ein Filtermechanismus, der einem Überwachungsteilnehmer das Abonnieren einer Teilmenge der Überwachungsdatensätze ermöglicht, die von einer Workflowinstanz ausgegeben werden.|  
  
 In der folgenden Tabelle werden die Nachverfolgungsdatensätze aufgeführt, die von der Workflowlaufzeit ausgegeben werden.  
  
|Nachverfolgungsdatensatz|Beschreibung|  
|------------------------------|------------------|  
|Nachverfolgungsdatensätze zur Workflowinstanz.|Beschreiben den Lebenszyklus der Workflowinstanz.Wenn der Workflow gestartet oder abgeschlossen wird, wird beispielsweise ein Instanzdatensatz ausgegeben.|  
|Nachverfolgungsdatensätze zum Aktivitätszustand.|Führen Einzelheiten zur Aktivitätsausführung auf.Diese Datensätze geben den Zustand einer Workflowaktivität an, z. B. wenn eine Aktivität geplant oder abgeschlossen wird oder wenn ein Fehler ausgelöst wird.|  
|Datensatz zur Wiederaufnahme von Lesezeichen.|Wird immer dann ausgegeben, wenn ein Lesezeichen in einer Workflowinstanz wieder aufgenommen wird.|  
|Benutzerdefinierte Nachverfolgungsdatensätze.|Ein Workflowautor kann benutzerdefinierte Nachverfolgungsdatensätze erstellen und in einer benutzerdefinierten Aktivität ausgeben.|  
  
 Die Nachverfolgungskomponente abonniert eine Teilmenge der ausgegebenen <xref:System.Activities.Tracking.TrackingRecord>\-Objekte mit Nachverfolgungsprofilen.Ein Nachverfolgungsprofil enthält Nachverfolgungsabfragen, die das Abonnieren eines bestimmten Typs von Nachverfolgungsdatensätzen ermöglichen.Nachverfolgungsprofile können im Code oder in der Konfiguration angegeben werden.  
  
### Benutzerdefinierte Nachverfolgungskomponente  
 Die API der Nachverfolgungskomponente ermöglicht eine Erweiterung der Nachverfolgungslaufzeit mit einer vom Benutzer bereitgestellten Nachverfolgungskomponente. Diese kann benutzerdefinierte Logik enthalten, mit der von der Workflowlaufzeit ausgegebene <xref:System.Activities.Tracking.TrackingRecord>\-Objekte behandelt werden.  
  
 Zum Schreiben einer Nachverfolgungskomponente muss der Benutzer <xref:System.Activities.Tracking.TrackingParticipant> implementieren.Die <xref:System.Activities.Tracking.TrackingParticipant.Track%2A>\-Methode muss von der benutzerdefinierten Komponente implementiert werden.Diese Methode wird aufgerufen, wenn ein <xref:System.Activities.Tracking.TrackingRecord>\-Objekt von der Workflowlaufzeit ausgegeben wird.  
  
```csharp  
public abstract class TrackingParticipant  
{  
    protected TrackingParticipant();  
  
    public virtual TrackingProfile TrackingProfile { get; set; }  
    public abstract void Track(TrackingRecord record, TimeSpan timeout);  
}  
  
```  
  
 Die vollständige Nachverfolgungskomponente wird in der Datei "ConsoleTrackingParticipant.cs" implementiert. Das folgende Codebeispiel enthält die <xref:System.Activities.Tracking.TrackingParticipant.Track%2A>\-Methode für die benutzerdefinierte Nachverfolgungskomponente.  
  
```csharp  
protected override void Track(TrackingRecord record, TimeSpan timeout)  
{  
    ...             
    WorkflowInstanceRecord workflowInstanceRecord = record as WorkflowInstanceRecord;  
    if (workflowInstanceRecord != null)  
    {  
        Console.WriteLine(String.Format(CultureInfo.InvariantCulture,  
            " Workflow InstanceID: {0} Workflow instance state: {1}",  
            record.InstanceId, workflowInstanceRecord.State));  
    }  
  
    ActivityStateRecord activityStateRecord = record as ActivityStateRecord;  
    if (activityStateRecord != null)  
    {  
        IDictionary<String, object> variables = activityStateRecord.Variables;  
        StringBuilder vars = new StringBuilder();  
  
        if (variables.Count > 0)  
        {  
            vars.AppendLine("\n\tVariables:");  
            foreach (KeyValuePair<string, object> variable in variables)  
            {     
                vars.AppendLine(String.Format(  
                    "\t\tName: {0} Value: {1}", variable.Key, variable.Value));  
            }  
        }  
        Console.WriteLine(String.Format(CultureInfo.InvariantCulture,  
            " :Activity DisplayName: {0} :ActivityInstanceState: {1} {2}",  
                activityStateRecord.Activity.Name, activityStateRecord.State,  
            ((variables.Count > 0) ? vars.ToString() : String.Empty)));  
    }  
  
    CustomTrackingRecord customTrackingRecord = record as CustomTrackingRecord;  
  
    if ((customTrackingRecord != null) && (customTrackingRecord.Data.Count > 0))  
    {  
        ...  
    }  
    Console.WriteLine();  
  
}  
  
```  
  
 Im folgenden Codebeispiel wird die Konsolenkomponente der aufrufenden Instanz für den Workflowvorgang hinzugefügt.  
  
```csharp  
ConsoleTrackingParticipant customTrackingParticipant = new ConsoleTrackingParticipant()  
{  
    ...  
    // The tracking profile is set here, refer to Program.CS  
...  
}  
  
WorkflowInvoker invoker = new WorkflowInvoker(BuildSampleWorkflow());  
invoker.Extensions.Add(customTrackingParticipant);  
  
```  
  
### Ausgeben von benutzerdefinierten Nachverfolgungsdatensätzen  
 In diesem Beispiel wird auch die Fähigkeit zur Ausgabe von <xref:System.Activities.Tracking.CustomTrackingRecord>\-Objekten aus einer benutzerdefinierten Workflowaktivität veranschaulicht:  
  
-   Die <xref:System.Activities.Tracking.CustomTrackingRecord>\-Objekte werden erstellt und mit benutzerdefinierten Daten aufgefüllt, die mit dem Datensatz ausgegeben werden sollen.  
  
-   Das <xref:System.Activities.Tracking.CustomTrackingRecord>\-Objekt wird durch Aufrufen der Nachverfolgungsmethode des <xref:System.Activities.ActivityContext> ausgegeben.  
  
 Im folgenden Beispiel wird veranschaulicht, wie <xref:System.Activities.Tracking.CustomTrackingRecord>\-Objekte innerhalb einer benutzerdefinierten Aktivität ausgegeben werden.  
  
```csharp  
// Create the Custom Tracking Record  
CustomTrackingRecord customRecord = new CustomTrackingRecord("OrderIn")  
{  
    Data =   
    {  
        {"OrderId", 200},  
        {"OrderDate", "20 Aug 2001"}  
    }  
};  
  
// Emit custom tracking record  
context.Track(customRecord);  
  
```  
  
#### So verwenden Sie dieses Beispiel  
  
1.  Öffnen Sie in [!INCLUDE[vs2010](../../../../includes/vs2010-md.md)] die Projektmappendatei "CustomTrackingSample.sln".  
  
2.  Drücken Sie STRG\+UMSCHALT\+B, um die Projektmappe zu erstellen.  
  
3.  Drücken Sie STRG\+F5, um die Projektmappe auszuführen.  
  
> [!IMPORTANT]
>  Die Beispiele sind möglicherweise bereits auf dem Computer installiert.Suchen Sie nach dem folgenden Verzeichnis \(Standardverzeichnis\), bevor Sie fortfahren.  
>   
>  `<Installationslaufwerk>:\WF_WCF_Samples`  
>   
>  Wenn dieses Verzeichnis nicht vorhanden ist, rufen Sie [Windows Communication Foundation \(WCF\) and Windows Workflow Foundation \(WF\) Samples for .NET Framework 4](http://go.microsoft.com/fwlink/?LinkId=150780) auf, um alle [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)]\- und [!INCLUDE[wf1](../../../../includes/wf1-md.md)]\-Beispiele herunterzuladen.Dieses Beispiel befindet sich im folgenden Verzeichnis.  
>   
>  `<InstallDrive>:\WF_WCF_Samples\WF\Basic\Tracking\CustomTracking`  
  
## Siehe auch  
 [AppFabric\-Überwachungsbeispiele](http://go.microsoft.com/fwlink/?LinkId=193959)