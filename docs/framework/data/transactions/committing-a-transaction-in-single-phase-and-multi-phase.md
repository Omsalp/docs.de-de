---
title: "Ausf&#252;hren eines Einphasen- oder Mehrphasencommits f&#252;r eine Transaktion  | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 694ea153-e4db-41ae-96ac-9ac66dcb69a9
caps.latest.revision: 3
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 3
---
# Ausf&#252;hren eines Einphasen- oder Mehrphasencommits f&#252;r eine Transaktion 
Jede in einer Transaktion verwendete Ressource wird von einem Ressourcen\-Manager \(RM\) verwaltet, dessen Aktionen von einem Transaktions\-Manager \(TM\) koordiniert werden.Im Thema [Eintragen von Ressourcen als Teilnehmer an einer Transaktion ](../../../../docs/framework/data/transactions/enlisting-resources-as-participants-in-a-transaction.md) wird beschrieben, wie eine oder mehrere Ressourcen in eine Transaktion eingetragen werden können.In diesem Thema wird erläutert, wie das Ausführen von Commits für Transaktionen unter eingetragenen Ressourcen koordiniert werden kann.  
  
 Am Ende der Transaktion fordert die Anwendung zur Ausführung eines Commit oder eines Rollback für die Transaktion auf.Der Transaktions\-Manager muss Risiken verhindern, wie sie beispielsweise entstehen, wenn einige Ressourcen\-Manager für einen Commit und andere für ein Rollback stimmen.  
  
 Wenn die Transaktion mehr als eine Ressource umfasst, müssen Sie ein Zweiphasencommit \(2PC\) ausführen.Mit dem Zweiphasencommit\-Protokoll \(Vorbereitungs\- und Commitphase\) wird sichergestellt, dass am Ende der Transaktion für alle Änderungen an allen Ressourcen entweder ein vollständiges Commit ausgeführt wurde oder sie vollständig zurückgesetzt wurden.Alle Teilnehmer werden dann über das Endergebnis informiert.Eine detaillierte Beschreibung des Zweiphasencommit\-Protokolls finden Sie im Buch "*Transaction Processing : Concepts and Techniques \(Morgan Kaufmann Series in Data Management Systems\) ISBN:1558601902*" \(englisch\) von Jim Gray.  
  
 Sie können zudem die Leistung der Transaktion optimieren, indem Sie am Einphasencommit\-Protokoll teilnehmen.Weitere Informationen finden Sie unter [Optimierung mit Einphasencommit und heraufstufbarer Einphasenbenachrichtigung ](../../../../docs/framework/data/transactions/optimization-spc-and-promotable-spn.md).  
  
 Wenn Sie lediglich über das Ergebnis einer Transaktion informiert werden, aber am Abstimmen nicht teilnehmen möchten, können Sie sich für das <xref:System.Transactions.Transaction.TransactionCompleted>\-Ereignis anmelden.  
  
## Zweiphasencommit \(2PC\)  
 In der ersten Transaktionsphase fragt der Transaktions\-Manager jede Ressource ab, um zu ermitteln, ob ein Commit oder ein Rollback für eine Transaktion ausgeführt werden sollte.In der zweiten Transaktionsphase informiert der Transaktions\-Manager alle Ressourcen über das Ergebnis seiner Abfragen und ermöglicht ihnen so die Durchführung eventuell erforderlicher Bereinigungen.  
  
 Um an einer solchen Transaktion teilnehmen zu können, muss ein Ressourcen\-Manager die <xref:System.Transactions.IEnlistmentNotification>\-Schnittstelle implementieren, die Methoden bereitstellt, die vom TM während eines 2PC als Benachrichtigungen aufgerufen werden.Es folgt ein Beispiel für eine solche Implementierung.  
  
 [!code-csharp[Tx_Enlist#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/tx_enlist/cs/enlist.cs#2)]
 [!code-vb[Tx_Enlist#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/tx_enlist/vb/enlist.vb#2)]  
  
### Vorbereitungsphase \(Phase 1\)  
 Bei Erhalt einer <xref:System.Transactions.CommittableTransaction.Commit%2A>\-Anforderung von einer Anwendung initiiert der Transaktions\-Manager die Vorbereitungsphase aller eingetragenen Teilnehmer, indem er die <xref:System.Transactions.IEnlistmentNotification.Prepare%2A>\-Methode für jede eingetragene Ressource aufruft, um von jeder Ressource ein Abstimmungsergebnis zur Transaktion zu erhalten.  
  
 Der Ressourcen\-Manager, der die <xref:System.Transactions.IEnlistmentNotification>\-Schnittstelle implementiert, sollte zuerst die <xref:System.Transactions.IEnlistmentNotification.Prepare%28System.Transactions.PreparingEnlistment%29>\-Methode implementieren, wie das folgende einfache Beispiel zeigt.  
  
```  
public void Prepare(PreparingEnlistment preparingEnlistment)  
{  
     Console.WriteLine("Prepare notification received");  
     //Perform work  
  
     Console.Write("reply with prepared? [Y|N] ");  
     c = Console.ReadKey();  
     Console.WriteLine();  
  
     //If work finished correctly, reply with prepared  
     if ((c.KeyChar == 'Y') || (c.KeyChar == 'y'))  
     {  
          preparingEnlistment.Prepared();  
          break;  
     }  
  
     // otherwise, do a ForceRollback  
     else if ((c.KeyChar == 'N') || (c.KeyChar == 'n'))  
     {  
          preparingEnlistment.ForceRollback();  
          break;  
     }  
}  
  
```  
  
 Wenn der Manager für dauerhafte Ressourcen diesen Aufruf erhält, muss er die Wiederherstellungsinformationen der Transaktion \(die nach Abruf der <xref:System.Transactions.PreparingEnlistment.RecoveryInformation%2A>\-Eigenschaft verfügbar sind\) sowie eventuelle weitere Informationen protokollieren, die zur Ausführung der vom Commit betroffenen Transaktion erforderlich sind.Dies muss nicht innerhalb der <xref:System.Transactions.IEnlistmentNotification.Prepare%2A>\-Methode geschehen, da der RM dies als Arbeitsthread ausführen kann.  
  
 Wenn der RM seine Vorbereitung abgeschlossen hat, muss er entscheiden, ob ein Commit oder ein Rollback ausgeführt wird, indem er die <xref:System.Transactions.PreparingEnlistment.Prepared%2A>\-Methode oder die <xref:System.Transactions.PreparingEnlistment.ForceRollback%2A>\-Methode aufruft.Beachten Sie, dass die <xref:System.Transactions.PreparingEnlistment>\-Klasse eine <xref:System.Transactions.Enlistment.Done%2A>\-Methode von der <xref:System.Transactions.Enlistment>\-Klasse erbt.Wenn Sie diese Methode beim <xref:System.Transactions.PreparingEnlistment>\-Rückruf während der Vorbereitungsphase aufrufen, informiert sie den TM, dass es sich um eine schreibgeschützte Eintragung handelt \(das bedeutet, die Ressourcen\-Manager können die transaktionsgeschützten Daten lesen, aber nicht aktualisieren\), und der RM erhält keine weiteren Benachrichtigungen vom TM, was das Ergebnis der Transaktion in Phase 2 betrifft.  
  
 Nachdem alle Ressourcen\-Manager <xref:System.Transactions.PreparingEnlistment.Prepared%2A> gestimmt haben, wird der Anwendung mitgeteilt, dass das Commit der Transaktion erfolgreich war.  
  
### Commitphase \(Phase 2\)  
 Wenn der TM in der zweiten Phase der Transaktion erfolgreiche Vorbereitungsergebnisse von allen RMs erhält \(alle RMs haben am Ende von Phase 1 <xref:System.Transactions.PreparingEnlistment.Prepared%2A> aufgerufen\), ruft er die <xref:System.Transactions.IEnlistmentNotification.Commit%2A>\-Methode für jeden Ressourcen\-Manager auf.Die Ressourcen\-Manager können dann die Änderungen dauerhaft übernehmen und den Commit abschließen.  
  
 Informiert einer der RMs über das Fehlschlagen der Vorbereitung in Phase 1, ruft der TM die <xref:System.Transactions.IEnlistmentNotification.Rollback%2A>\-Methode für jeden RM auf und gibt der Anwendung das Fehlschlagen des Commit bekannt.  
  
 Ihr Ressourcen\-Manager sollte daher die folgenden Methoden implementieren.  
  
```  
public void Commit (Enlistment enlistment)  
{  
     // Do any work necessary when commit notification is received  
  
     // Declare done on the enlistment  
     enlistment.Done();  
}  
  
public void Rollback (Enlistment enlistment)  
{  
     // Do any work necessary when rollback notification is received  
  
     // Declare done on the enlistment    
     enlistment.Done();    
}  
```  
  
 Der RM muss auf der Grundlage des Benachrichtigungstyps alle zum Abschließen der Transaktion erforderlichen Aktionen ausführen und den TM durch Aufrufen der <xref:System.Transactions.Enlistment.Done%2A>\-Methode für den <xref:System.Transactions.Enlistment>\-Parameter darüber informieren, dass er bereit ist.Diese Aufgabe kann als Arbeitsthread ausgeführt werden.Die Benachrichtigungen der Phase 2 können inline im gleichen Thread erfolgen, mit dem die <xref:System.Transactions.PreparingEnlistment.Prepared%2A>\-Methode in Phase 1 aufgerufen wurde.Daher sollten Sie nach dem <xref:System.Transactions.PreparingEnlistment.Prepared%2A>\-Aufruf keine Aufgaben mehr ausführen \(z. B. Freigeben von Sperren\), die vor dem Empfang der Benachrichtigungen aus Phase 2 abgeschlossen sein müssen.  
  
### Implementieren von InDoubt  
 Schließlich sollten Sie die <xref:System.Transactions.IEnlistmentNotification.InDoubt%2A>\-Methode für den flüchtigen Ressourcen\-Manager implementieren.Diese Methode wird aufgerufen, wenn der Transaktions\-Manager den Kontakt zu einem oder mehreren Teilnehmern verliert und ihren Status deshalb nicht kennt.Wenn dieser Fall eintritt, sollten Sie ihn protokollieren, damit Sie später prüfen können, ob einer der Transaktionsteilnehmer in einem inkonsistenten Zustand geblieben ist.  
  
```  
public void InDoubt (Enlistment enlistment)  
{  
     // log this  
     enlistment.Done();  
}  
```  
  
## Optimierung des Einphasencommit  
 Das Einphasencommit\-Protokoll ist zur Laufzeit effizienter, da alle Updates ohne eine explizite Koordination ausgeführt werden.Weitere Informationen zu diesem Protokoll finden Sie unter [Optimierung mit Einphasencommit und heraufstufbarer Einphasenbenachrichtigung ](../../../../docs/framework/data/transactions/optimization-spc-and-promotable-spn.md).  
  
## Siehe auch  
 [Optimierung mit Einphasencommit und heraufstufbarer Einphasenbenachrichtigung ](../../../../docs/framework/data/transactions/optimization-spc-and-promotable-spn.md)   
 [Eintragen von Ressourcen als Teilnehmer an einer Transaktion ](../../../../docs/framework/data/transactions/enlisting-resources-as-participants-in-a-transaction.md)