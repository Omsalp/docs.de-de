---
title: "Gewusst wie: Registrieren und Konfigurieren eines Dienstmonikers | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "VB"
  - "CSharp"
helpviewer_keywords: 
  - "COM [WCF], Konfigurieren von Dienstmonikern"
  - "COM [WCF], Registrieren von Dienstmonikern"
ms.assetid: e5e16c80-8a8e-4eef-af53-564933b651ef
caps.latest.revision: 20
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 20
---
# Gewusst wie: Registrieren und Konfigurieren eines Dienstmonikers
Vor der Verwendung eines [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)]\-Dienstmonikers in einer COM\-Anwendung mit einem typisierten Vertrag müssen die erforderlichen attributierten Typen bei COM registriert werden. Des Weiteren müssen die COM\-Anwendung sowie der Moniker mit der erforderlichen Bindungskonfiguration konfiguriert werden.  
  
### So registrieren Sie die erforderlichen attributierten Typen bei COM  
  
1.  Verwenden Sie das [ServiceModel Metadata Utility\-Tool \(Svcutil.exe\)](../../../../docs/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md)\-Tool, um den Metadatenvertrag vom [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)]\-Dienst abzurufen.  Dadurch werden der Quellcode für eine [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)]\-Clientassembly sowie eine Konfigurationsdatei für die Clientanwendung generiert.  
  
2.  Stellen Sie sicher, dass die Typen in der Assembly als `ComVisible` markiert sind.  Fügen Sie hierzu der Datei AssemblyInfo.cs in Ihrem Visual Studio\-Projekt das folgende Attribut hinzu:  
  
    ```  
    [assembly: ComVisible(true)]  
    ```  
  
3.  Kompilieren Sie den verwalteten [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)]\-Client als Assembly mit starkem Namen.  Dies erfordert die Signierung mit einem kryptografischen Schlüsselpaar.  [!INCLUDE[crdefault](../../../../includes/crdefault-md.md)] [Signieren einer Assembly mit einem starken Namen](http://go.microsoft.com/fwlink/?LinkId=94874) im .NET\-Anwendungshandbuch.  
  
4.  Verwenden Sie das Assemblyregistrierungstool \(Regasm.exe\) mit der `/tlb`\-Option, um die Typen in der Assembly bei COM zu registrieren.  
  
5.  Fügen Sie die Assembly mithilfe des Tools für den globalen Assemblycache \(Gacutil.exe\) dem globalen Assemblycache hinzu.  
  
    > [!NOTE]
    >  Das Signieren sowie das Hinzufügen der Assembly zum globalen Assemblycache sind optionale Schritte, sie dienen jedoch zum Vereinfachen des Prozesses zum Laden der Assembly aus dem korrekten Speicherort im Laufzeitmodus.  
  
### So konfigurieren Sie die COM\-Anwendung und den Moniker mit der erforderlichen Bindungskonfiguration  
  
-   Platzieren Sie die Bindungsdefinitionen \(die vom [ServiceModel Metadata Utility\-Tool \(Svcutil.exe\)](../../../../docs/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md) in der generierten Konfigurationsdatei der Clientanwendung generiert wurden\) in der Konfigurationsdatei der entsprechenden Clientanwendung.  Beispiel: Für eine ausführbare Visual Basic 6.0\-Datei mit dem Namen CallCenterClient.exe muss die Konfiguration in eine Datei mit dem Namen CallCenterConfig.exe.config platziert werden, und diese Datei muss sich im gleichen Verzeichnis befinden wie die ausführbare Datei.  Der Moniker kann nun von der Clientanwendung verwendet werden.  Hinweis: Die Bindungskonfiguration ist nicht erforderlich, wenn einer der von [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] bereitgestellten Standardbindungstypen verwendet wird.  
  
     Der folgende Typ wird registriert:  
  
    ```  
    using System.ServiceModel;  
  
    ...  
  
    [ServiceContract]   
    public interface IMathService   
    {  
    [OperationContract]  
    public int Add(int x, int y);  
    [OperationContract]  
    public int Subtract(int x, int y);  
    }  
    ```  
  
     Die Anwendung wird mit einer `wsHttpBinding`\-Bindung verfügbar gemacht.  Für den angegebenen Typ und die Anwendungskonfiguration werden die folgenden Beispielmonikerzeichenfolgen verwendet:  
  
    ```  
    service4:address=http://localhost/MathService, binding=wsHttpBinding, bindingConfiguration=Binding1  
  
    ```  
  
     `or`  
  
    ```  
    service4:address=http://localhost/MathService, binding=wsHttpBinding, bindingConfiguration=Binding1, contract={36ADAD5A-A944-4d5c-9B7C-967E4F00A090}  
  
    ```  
  
     Nachdem ein Verweis auf die Assembly mit den `IMathService`\-Typen hinzugefügt wurde \(siehe folgendes Codebeispiel\), kann jede dieser Monikerzeichenfolgen in einer Visual Basic 6.0\-Anwendung verwendet werden.  
  
    ```  
    Dim MathProxy As IMathService  
    Dim result As Integer  
  
    Set MathProxy = GetObject( _  
            "service4:address=http://localhost/MathService, _  
            binding=wsHttpBinding, _  
            bindingConfiguration=Binding1")  
  
    result = MathProxy.Add(3, 5)  
  
    ```  
  
     In diesem Beispiel wird die Definition für die Bindungskonfiguration `Binding1` in einer Konfigurationsdatei für die Clientanwendung mit geeignetem Namen \(wie vb6appname.exe.config\) gespeichert.  
  
    > [!NOTE]
    >  Für C\#\-, C\+\+\- oder andere .NET\-Anwendung kann ein ähnlicher Code verwendet werden.  
  
    > [!NOTE]
    >  : Ist der Moniker nicht ordnungsgemäß formatiert oder der Dienst nicht verfügbar, wird nach dem `GetObject`\-Aufruf ein Syntaxfehler zurückgegeben.  Vergewissern Sie sich bei Auftreten dieses Fehlers, dass der verwendete Moniker korrekt und der Dienst verfügbar ist.  
  
     Obgleich in diesem Thema hauptsächlich die Verwendung des Dienstmonikers in VB 6.0\-Code behandelt wird, können Dienstmoniker auch in anderen Sprachen verwendet werden.  Bei Verwendung eines Monikers in C\+\+\-Code muss die von Svcutil.exe generierte Assembly gemäß dem folgenden Beispiel mit "no\_namespace named\_guids raw\_interfaces\_only" importiert werden:  
  
    ```  
    #import "ComTestProxy.tlb" no_namespace named_guids  
    ```  
  
     Dadurch wird die importierte Schnittstellendefinitionen geändert, sodass von allen Methoden `HResult` zurückgegeben wird.  Alle anderen Rückgabewerte werden zu out\-Parametern umgewandelt.  An der Ausführung der Methoden ändert das nichts.  Dies ermöglicht es Ihnen, beim Aufrufen einer Methode auf dem Proxy die Ursache einer Ausnahme zu bestimmen.  Diese Funktion ist ausschließlich in C\+\+\-Code verfügbar.  
  
## Siehe auch  
 [ServiceModel Metadata Utility\-Tool \(Svcutil.exe\)](../../../../docs/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md)