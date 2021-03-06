---
title: "Fehlervertrag | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: b31b140e-dc3b-408b-b3c7-10b6fe769725
caps.latest.revision: 16
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 16
---
# Fehlervertrag
Im Fehlervertragsbeispiel wird veranschaulicht, wie Fehlerinformationen von einem Dienst zu einem Client übermittelt werden.  Das Beispiel basiert auf [Erste Schritte](../../../../docs/framework/wcf/samples/getting-started-sample.md). Dem Dienst wurde dabei zusätzlicher Code hinzugefügt, um eine interne Ausnahme in einen Fehler zu konvertieren.  Der Client versucht, eine Division durch 0 \(null\) auszuführen, um einen Fehlerzustand beim Dienst zu erzwingen.  
  
> [!NOTE]
>  Die Setupprozedur und die Buildanweisungen für dieses Beispiel befinden sich am Ende dieses Themas.  
  
 Der Rechnervertrag wurde geändert, um ein <xref:System.ServiceModel.FaultContractAttribute> einzuschließen, wie im folgenden Beispielcode gezeigt.  
  
```  
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]  
public interface ICalculator  
{  
    [OperationContract]  
    int Add(int n1, int n2);  
    [OperationContract]  
    int Subtract(int n1, int n2);  
    [OperationContract]  
    int Multiply(int n1, int n2);  
    [OperationContract]  
    [FaultContract(typeof(MathFault))]  
    int Divide(int n1, int n2);  
}  
```  
  
 Das <xref:System.ServiceModel.FaultContractAttribute>\-Attribut gibt an, dass der `Divide`\-Vorgang möglicherweise einen Fehler vom Typ `MathFault` zurückgibt.  Ein Fehler kann von jedem Typ sein, der serialisiert werden kann.  In diesem Fall ist der `MathFault` ein Datenvertrag, wie im Folgenden dargestellt:  
  
```  
[DataContract(Namespace="http://Microsoft.ServiceModel.Samples")]  
public class MathFault  
{      
    private string operation;  
    private string problemType;  
  
    [DataMember]  
    public string Operation  
    {  
        get { return operation; }  
        set { operation = value; }  
    }  
  
    [DataMember]          
    public string ProblemType  
    {  
        get { return problemType; }  
        set { problemType = value; }  
    }  
}  
  
```  
  
 Die `Divide`\-Methode löst eine <xref:System.ServiceModel.FaultException%601>\-Ausnahme aus, wenn eine Ausnahme durch Division durch 0 \(null\) auftritt, wie im folgenden Beispielcode dargestellt.  Diese Ausnahme führt dazu, dass ein Fehler an den Client gesendet wird.  
  
```  
public int Divide(int n1, int n2)  
{  
    try  
    {  
        return n1 / n2;  
    }  
    catch (DivideByZeroException)  
    {  
        MathFault mf = new MathFault();  
        mf.operation = "division";  
        mf.problemType = "divide by zero";  
        throw new FaultException<MathFault>(mf);  
    }  
}  
```  
  
 Der Clientcode erzwingt einen Fehler, indem eine Division durch 0 \(null\) angefordert wird.  Wenn Sie das Beispiel ausführen, werden die Anforderungen und Antworten für den Vorgang im Clientkonsolenfenster angezeigt.  Sie sehen, dass die Division durch 0 \(null\) als Fehler gemeldet wird.  Drücken Sie im Clientfenster die EINGABETASTE, um den Client zu schließen.  
  
```  
Add(15,3) = 18  
Subtract(145,76) = 69  
Multiply(9,81) = 729  
FaultException<MathFault>: Math fault while doing division. Problem: divide by zero  
  
Press <ENTER> to terminate client.  
```  
  
 Der Client fängt hierzu die entsprechende `FaultException<MathFault>`\-Ausnahme ab:  
  
```  
catch (FaultException<MathFault> e)  
{  
    Console.WriteLine("FaultException<MathFault>: Math fault while doing " + e.Detail.operation + ". Problem: " + e.Detail.problemType);  
    client.Abort();  
}  
```  
  
 In der Standardeinstellung werden Details zu unerwarteten Ausnahmen nicht an den Client gesendet, damit verhindert wird, dass Informationen zur Dienstimplementierung die sichere Begrenzung des Diensts verlassen.  `FaultContract` stellt eine Möglichkeit bereit, um Fehler in einem Vertrag so zu beschreiben und bestimmte Arten von Ausnahmen so zu kennzeichnen, dass sie für die Übertragung an den Client geeignet sind.  `FaultException<T>` stellt einen Laufzeitmechanismus zum Senden von Fehlern an den Consumer bereit.  
  
 Es ist jedoch hilfreich, die internen Details eines Dienstfehlers beim Debuggen anzuzeigen.  Zum Deaktivieren des oben beschriebenen sicheren Verhaltens können Sie angeben, dass die Details zu allen unbehandelten Ausnahmen auf dem Server in den Fehler eingeschlossen werden sollen, der an den Client gesendet wird.  Dies wird durch Festlegen von <xref:System.ServiceModel.ServiceBehaviorAttribute.IncludeExceptionDetailInFaults%2A> auf `true` erreicht.  Sie können dies im Code festlegen oder in der Konfiguration, wie im folgenden Beispiel gezeigt.  
  
```  
<behaviors>  
  <serviceBehaviors>  
    <behavior name="CalculatorServiceBehavior">  
      <serviceMetadata httpGetEnabled="True"/>  
      <serviceDebug includeExceptionDetailInFaults="True" />  
    </behavior>  
  </serviceBehaviors>  
</behaviors>  
```  
  
 Darüber hinaus muss das Verhalten dem Dienst zugeordnet werden, indem das `behaviorConfiguration`\-Attribut des Diensts in der Konfigurationsdatei auf "CalculatorServiceBehavior" festgelegt wird.  
  
 Damit solche Fehler auf dem Client abgefangen werden, muss die nicht generische <xref:System.ServiceModel.FaultException> abgefangen werden.  
  
 Dieses Verhalten sollte nur für Debuggingzwecke verwendet und nie in der Produktion aktiviert werden.  
  
### So können Sie das Beispiel einrichten, erstellen und ausführen  
  
1.  Stellen Sie sicher, dass Sie die [Einmaliges Setupverfahren für Windows Communication Foundation\-Beispiele](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md) ausgeführt haben.  
  
2.  Zum Erstellen der C\#\- oder Visual Basic .NET\-Edition der Projektmappe befolgen Sie die unter [Erstellen der Windows Communication Foundation\-Beispiele](../../../../docs/framework/wcf/samples/building-the-samples.md) aufgeführten Anweisungen.  
  
3.  Um das Beispiel in einer Konfiguration mit einem Computer oder computerübergreifend auszuführen, befolgen Sie die Anweisungen unter [Durchführen der Windows Communication Foundation\-Beispiele](../../../../docs/framework/wcf/samples/running-the-samples.md).  
  
> [!IMPORTANT]
>  Die Beispiele sind möglicherweise bereits auf dem Computer installiert.  Suchen Sie nach dem folgenden Verzeichnis \(Standardverzeichnis\), bevor Sie fortfahren.  
>   
>  `<Installationslaufwerk>:\WF_WCF_Samples`  
>   
>  Wenn dieses Verzeichnis nicht vorhanden ist, rufen Sie [Windows Communication Foundation \(WCF\) and Windows Workflow Foundation \(WF\) Samples for .NET Framework 4](http://go.microsoft.com/fwlink/?LinkId=150780) auf, um alle [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)]\- und [!INCLUDE[wf1](../../../../includes/wf1-md.md)]\-Beispiele herunterzuladen.  Dieses Beispiel befindet sich im folgenden Verzeichnis.  
>   
>  `<Installationslaufwerk>:\WF_WCF_Samples\WCF\Basic\Contract\Service\Faults`  
  
## Siehe auch