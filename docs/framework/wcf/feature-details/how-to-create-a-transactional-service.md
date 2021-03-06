---
title: "Vorgehensweise: Erstellen eines Transaktionsdiensts | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 1bd2e4ed-a557-43f9-ba98-4c70cb75c154
caps.latest.revision: 12
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 12
---
# Vorgehensweise: Erstellen eines Transaktionsdiensts
In diesem Beispiel werden diverse Aspekte der Erstellung eines Transaktionsdiensts und die Nutzung einer von einem Client initiierten Transaktion für die Koordinierung von Dienstvorgängen veranschaulicht.  
  
### Erstellen eines Transaktionsdiensts  
  
1.  Erstellen Sie einen Dienstvertrag, und fügen Sie den Vorgängen die gewünschte Einstellung aus der <xref:System.ServiceModel.TransactionFlowOption>\-Enumeration hinzu, um die Anforderungen für eingehende Transaktionen festzulegen.Beachten Sie, dass Sie auch das <xref:System.ServiceModel.TransactionFlowAttribute> auf die zu implementierende Dienstklasse platzieren können.Hierdurch wird die Einzelimplementierung einer Schnittstelle, die diese Transaktionseinstellungen nutzt, anstelle von allen Implementierungen ermöglicht.  
  
    ```  
    [ServiceContract]  
    public interface ICalculator  
    {  
        [OperationContract]  
        // Use this to require an incoming transaction  
        [TransactionFlow(TransactionFlowOption.Mandatory)]  
        double Add(double n1, double n2);  
        [OperationContract]  
        // Use this to permit an incoming transaction  
        [TransactionFlow(TransactionFlowOption.Allowed)]  
        double Subtract(double n1, double n2);  
    }  
    ```  
  
2.  Erstellen Sie eine Implementierungsklasse, und verwenden Sie <xref:System.ServiceModel.ServiceBehaviorAttribute>, um optional ein <xref:System.ServiceModel.ServiceBehaviorAttribute.TransactionIsolationLevel%2A> und einen <xref:System.ServiceModel.ServiceBehaviorAttribute.TransactionTimeout%2A> anzugeben.Beachten Sie, dass in vielen Fällen der standardmäßige <xref:System.ServiceModel.ServiceBehaviorAttribute.TransactionTimeout%2A> von 60 Sekunden und die standardmäßige <xref:System.ServiceModel.ServiceBehaviorAttribute.TransactionIsolationLevel%2A> mit dem Wert `Unspecified` angemessen sind.Für jeden Vorgang können Sie das <xref:System.ServiceModel.OperationBehaviorAttribute>\-Attribut nutzen, um festzulegen, ob die innerhalb der Methode durchgeführte Arbeit innerhalb des Umfangs eines Transaktionsumfangs gemäß dem Wert des <xref:System.ServiceModel.OperationBehaviorAttribute.TransactionScopeRequired%2A>\-Attributs erfolgen soll.In diesem Fall stimmt die für die `Add`\-Methode verwendete Transaktion mit der obligatorischen eingehenden Transaktionen überein, die vom Client kommt, und die für die `Subtract`\-Methode verwendete Transaktion entspricht entweder der eingehenden Transaktion, wenn eine solche vom Client übergeben wurde, oder einer neuen implizit oder lokal erstellten Transaktion.  
  
    ```  
    [ServiceBehavior(  
        TransactionIsolationLevel = System.Transactions.IsolationLevel.Serializable,  
        TransactionTimeout = "00:00:45")]  
    public class CalculatorService : ICalculator  
    {  
        [OperationBehavior(TransactionScopeRequired = true)]  
        public double Add(double n1, double n2)  
        {  
            // Perform transactional operation  
            RecordToLog(String.Format("Adding {0} to {1}", n1, n2));  
            return n1 + n2;  
        }  
  
        [OperationBehavior(TransactionScopeRequired = true)]  
        public double Subtract(double n1, double n2)  
        {  
            // Perform transactional operation  
            RecordToLog(String.Format("Subtracting {0} from {1}", n2, n1));  
            return n1 - n2;  
        }  
  
        private static void RecordToLog(string recordText)  
        {  
            // Database operations omitted for brevity  
            // This is where the transaction provides specific benefit  
            // - changes to the database will be committed only when  
            // the transaction completes.  
        }  
    }  
    ```  
  
3.  Konfigurieren Sie die Bindungen in der Konfigurationsdatei, wobei Sie angeben, dass der Transaktionskontext übergeben werden sollte, und die dafür zu verwendenden Protokolle festlegen.[!INCLUDE[crdefault](../../../../includes/crdefault-md.md)][ServiceModel\-Transaktionskonfiguration](../../../../docs/framework/wcf/feature-details/servicemodel-transaction-configuration.md).Der Bindungstyp wird im `binding`\-Attribut des Endpunktelements angegeben.Das [\<endpoint\>](http://msdn.microsoft.com/de-de/13aa23b7-2f08-4add-8dbf-a99f8127c017)\-Element enthält ein `bindingConfiguration`\-Attribut, das auf eine Bindungskonfiguration mit dem Namen `transactionalOleTransactionsTcpBinding` verweist \(wie in folgender Beispielkonfiguration gezeigt\).  
  
    ```  
    <service name="CalculatorService">  
      <endpoint address="net.tcp://localhost:8008/CalcService"  
        binding="netTcpBinding"  
        bindingConfiguration="transactionalOleTransactionsTcpBinding"  
        contract="ICalculator"  
        name="OleTransactions_endpoint" />  
    </service>  
    ```  
  
     Der Transaktionsfluss wird auf Konfigurationsebene mithilfe des `transactionFlow`\-Attributs aktiviert, und das Transaktionsprotokoll wird über das `transactionProtocol`\-Attribut angegeben \(wie in folgender Konfiguration gezeigt\).  
  
    ```  
    <bindings>  
      <netTcpBinding>  
        <binding name="transactionalOleTransactionsTcpBinding"  
          transactionFlow="true"  
          transactionProtocol="OleTransactions"/>  
      </netTcpBinding>  
    </bindings>  
    ```  
  
### Unterstützung mehrerer Transaktionsprotokolle  
  
1.  Um eine optimale Leistung zu erhalten, sollten Sie für Szenarien, die einen Client und einen Dienst umfassen, die mit [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] geschrieben wurden, das OleTransactions\-Protokoll verwenden.Allerdings ist das WS\-AtomicTransaction \(WS\-AT\)\-Protokoll für Szenarien nützlich, bei denen Interoperabilität mit Protokollstapeln eines Drittanbieters erforderlich ist.Sie können [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)]\-Dienste so konfigurieren, dass sie beide Protokolle unterstützen, indem Sie mehrere Endpunkte mit den entsprechenden protokollspezifischen Bindungen bereitstellen \(wie in folgender Beispielkonfiguration gezeigt\).  
  
    ```  
    <service name="CalculatorService">  
      <endpoint address="http://localhost:8000/CalcService"  
        binding="wsHttpBinding"  
        bindingConfiguration="transactionalWsatHttpBinding"  
        contract="ICalculator"  
        name="WSAtomicTransaction_endpoint" />  
      <endpoint address="net.tcp://localhost:8008/CalcService"  
        binding="netTcpBinding"  
        bindingConfiguration="transactionalOleTransactionsTcpBinding"  
        contract="ICalculator"  
        name="OleTransactions_endpoint" />  
    </service>  
    ```  
  
     Das Transaktionsprotokoll wird über das `transactionProtocol`\-Attribut festgelegt.Bei der vom System bereitgestellten `wsHttpBinding` steht dieses Attribut nicht zur Verfügung, da diese Bindung nur das WS\-AT\-Protokoll nutzen kann.  
  
    ```  
    <bindings>  
      <wsHttpBinding>  
        <binding name="transactionalWsatHttpBinding"  
          transactionFlow="true" />  
      </wsHttpBinding>  
      <netTcpBinding>  
        <binding name="transactionalOleTransactionsTcpBinding"  
          transactionFlow="true"  
          transactionProtocol="OleTransactions"/>  
      </netTcpBinding>  
    </bindings>  
    ```  
  
### Kontrolle des Abschlusses einer Transaktion  
  
1.  Standardmäßig schließen [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)]\-Vorgänge automatisch Transaktionen ab, wenn keine nicht behandelten Ausnahmen ausgelöst werden.Sie können dieses Verhalten ändern, indem Sie die <xref:System.ServiceModel.OperationBehaviorAttribute.TransactionAutoComplete%2A>\-Eigenschaft und die <xref:System.ServiceModel.OperationContext.SetTransactionComplete%2A>\-Methode verwenden.Wenn es erforderlich ist, dass ein Vorgang innerhalb derselben Transaktion erfolgt wie ein anderer Vorgang \(Beispiel: ein Debit\- und Kredit\-Vorgang\), können Sie das automatische Verhalten für den Abschluss deaktivieren, indem Sie die <xref:System.ServiceModel.OperationBehaviorAttribute.TransactionAutoComplete%2A>\-Eigenschaft auf `false` festlegen \(wie in folgendem `Debit`\-Vorgangsbeispiel gezeigt\).Die Transaktion, die der `Debit`\-Vorgang nutzt, wird nicht abgeschlossen, bevor eine Methode mit der <xref:System.ServiceModel.OperationBehaviorAttribute.TransactionAutoComplete%2A>\-Eigenschaft auf `true` aufgerufen wird \(wie in Vorgang `Credit1` gezeigt\) oder wenn die <xref:System.ServiceModel.OperationContext.SetTransactionComplete%2A>\-Methode aufgerufen wird, um explizit die Transaktion als abgeschlossen zu kennzeichnen \(wie in Vorgang `Credit2` gezeigt\).Beachten Sie, dass die beiden Kreditvorgänge zu Illustrationszwecken angeführt werden und dass ein einzelner Kreditvorgang üblicher wäre.  
  
    ```  
    [ServiceBehavior]  
    public class CalculatorService : IAccount  
    {  
        [OperationBehavior(  
            TransactionScopeRequired = true, TransactionAutoComplete = false)]  
        public void Debit(double n)  
        {  
            // Perform debit operation  
  
            return;  
        }  
  
        [OperationBehavior(  
            TransactionScopeRequired = true, TransactionAutoComplete = true)]  
        public void Credit1(double n)  
        {  
            // Perform credit operation  
  
            return;  
        }  
  
        [OperationBehavior(  
            TransactionScopeRequired = true, TransactionAutoComplete = false)]  
        public void Credit2(double n)  
        {  
            // Perform alternate credit operation  
  
            OperationContext.Current.SetTransactionComplete();  
            return;  
        }  
    }  
    ```  
  
2.  Zum Zwecke der Transaktionskorrelation erfordert ein Festlegen der <xref:System.ServiceModel.OperationBehaviorAttribute.TransactionAutoComplete%2A>\-Eigenschaft auf `false` die Verwendung einer sitzungsbasierten Bindung.Die Anforderung wird mit der `SessionMode`\-Eigenschaft auf <xref:System.ServiceModel.ServiceContractAttribute> festgelegt.  
  
    ```  
    [ServiceContract(SessionMode = SessionMode.Required)]  
    public interface IAccount  
    {  
        [OperationContract]  
        [TransactionFlow(TransactionFlowOption.Allowed)]  
        void Debit(double n);  
        [OperationContract]  
        [TransactionFlow(TransactionFlowOption.Allowed)]  
        void Credit1(double n);  
        [OperationContract]  
        [TransactionFlow(TransactionFlowOption.Allowed)]  
        void Credit2(double n);  
    }  
    ```  
  
### Kontrolle der Lebensdauer einer Transaktionsdienstinstanz  
  
1.  [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] verwendet die <xref:System.ServiceModel.ServiceBehaviorAttribute.ReleaseServiceInstanceOnTransactionComplete%2A>\-Eigenschaft, um anzugeben, ob die zugrunde liegende Dienstinstanz bei Abschluss einer Transaktion freigegeben wird.Da hier der Standardwert, auch wenn dies anders konfiguriert wurde, `true` ist, weist [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] ein effizientes und vorhersehbares Just\-In\-Time\-Verhalten auf.Aufrufe an einen Dienst bei einer nachfolgenden Transaktion erhalten ohne Auswirkung des Status der vorherigen Transaktion garantiert eine neue Dienstinstanz.Obwohl dies oft nützlich sein kann, möchten Sie eventuell den Status innerhalb der Dienstinstanz bis über den Transaktionsabschluss hinaus beibehalten.Beispielsweise könnte dies der Fall sein, wenn erforderliche Status oder Handle zu Ressourcen in der Anschaffung oder Wiederherstellung teuer sind.Sie können dies vornehmen, indem Sie die <xref:System.ServiceModel.ServiceBehaviorAttribute.ReleaseServiceInstanceOnTransactionComplete%2A>\-Eigenschaft auf `false` festlegen.Mit dieser Einstellung stehen die Instanz und alle zugehörigen Status bei folgenden Aufrufen zur Verfügung.Achten Sie hierbei insbesondere darauf, wann und wie der Zustand und die Transaktion gelöscht und abgeschlossen werden.Das folgende Beispiel veranschaulicht die Vorgehensweise, indem die Instanz mit der `runningTotal`\-Variablen beibehalten wird.  
  
    ```  
    [ServiceBehavior(TransactionIsolationLevel = [ServiceBehavior(  
        ReleaseServiceInstanceOnTransactionComplete = false)]  
    public class CalculatorService : ICalculator  
    {  
        double runningTotal = 0;  
  
        [OperationBehavior(TransactionScopeRequired = true)]  
        public double Add(double n)  
        {  
            // Perform transactional operation  
            RecordToLog(String.Format("Adding {0} to {1}", n, runningTotal));  
            runningTotal = runningTotal + n;  
            return runningTotal;  
        }  
  
        [OperationBehavior(TransactionScopeRequired = true)]  
        public double Subtract(double n)  
        {  
            // Perform transactional operation  
            RecordToLog(String.Format("Subtracting {0} from {1}", n, runningTotal));  
            runningTotal = runningTotal - n;  
            return runningTotal;  
        }  
  
        private static void RecordToLog(string recordText)  
        {  
            // Database operations omitted for brevity  
        }  
    }  
    ```  
  
    > [!NOTE]
    >  Da die Lebensdauer der Instanz ein dienstinternes Verhalten ist und durch die <xref:System.ServiceModel.ServiceBehaviorAttribute>\-Eigenschaft kontrolliert wird, sind keine Änderungen an der Dienstkonfiguration oder am Dienstvertrag erforderlich.Darüber hinaus enthält die Übertragung keine Darstellung hierzu.