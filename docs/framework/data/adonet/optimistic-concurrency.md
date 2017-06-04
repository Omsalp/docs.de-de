---
title: "Vollst&#228;ndige Parallelit&#228;t | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-ado"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: e380edac-da67-4276-80a5-b64decae4947
caps.latest.revision: 6
author: "JennieHubbard"
ms.author: "jhubbard"
manager: "jhubbard"
caps.handback.revision: 6
---
# Vollst&#228;ndige Parallelit&#228;t
In einer Umgebung mit mehreren Benutzern gibt es zwei Modelle für das Update von Daten in einer Datenbank: das Modell der vollständigen Parallelität und das Modell der eingeschränkten Parallelität.  Das <xref:System.Data.DataSet>\-Objekt unterstützt die Verwendung der vollständigen Parallelität für lange Aktivitäten, wie bei der Datenfernverarbeitung und der Interaktion mit Daten.  
  
 Die eingeschränkte Parallelität umfasst das Sperren von Zeilen an der Datenquelle, damit Benutzer Daten, die andere Benutzer betreffen, nicht ändern können.  Wenn ein Benutzer bei einem eingeschränkten Modell eine Aktion ausführt, durch die eine Sperre angewendet wird, können andere Benutzer so lange keine mit der Sperre in Konflikt stehenden Aktionen ausführen, bis der Eigentümer der Sperre diese aufgehoben hat.  Dieses Modell wird hauptsächlich in Umgebungen verwendet, in denen häufig Datenkonflikte auftreten, oder in denen der Aufwand von Datensperren geringer ist als der Aufwand für Rollbacks von Transaktionen, die aufgrund von Konflikten durch eine Parallelbearbeitung ausgeführt werden müssen.  
  
 Daher erstellt ein Benutzer, der eine Zeile aktualisiert, bei einem Modell für pessimistische Parallelität eine Sperre.  	Die Zeile kann erst wieder von einem anderen Benutzer geändert werden, wenn der Benutzer den Updatevorgang für die Zeile abgeschlossen und die Sperre aufgehoben hat.  Aus diesem Grund eignet sich die eingeschränkte Parallelität am besten bei kurzen Sperrfristen wie bei der programmgesteuerten Verarbeitung von Datensätzen.  Die eingeschränkte Parallelität stellt keine flexible Option dar, wenn Benutzer mit Daten interagieren und Datensätze dadurch für einen relativ langen Zeitraum sperren.  
  
> [!NOTE]
>  Wenn Sie mehrere Zeilen auf einmal aktualisieren müssen, ermöglicht das Erstellen einer Transaktion ein höheres Maß an Skalierung als das Sperren bei eingeschränkter Parallelität.  
  
 Im Gegensatz dazu sperren Benutzer bei der vollständigen Parallelität keine Zeile, während sie sie lesen.  Wenn ein Benutzer eine Zeile aktualisieren möchte, muss durch die Anwendung festgestellt werden, ob ein anderer Benutzer die Zeile seit dem Abruf geändert hat.  Die vollständige Parallelität wird im Allgemeinen in Umgebungen mit wenigen Datenkonflikten verwendet.  Vollständige Parallelität führt zu einer Leistungssteigerung, weil keine Datensätze gesperrt werden müssen, denn für das Sperren von Datensätzen sind zusätzliche Serverressourcen erforderlich.  Außerdem ist für die Verwaltung von Datensatzsperren eine ständige Verbindung zum Datenbankserver notwendig.  Da dies beim Modell der vollständigen Parallelität nicht der Fall ist, können Verbindungen zum Server innerhalb eines kürzeren Zeitraums eine Vielzahl von Clients bedienen.  
  
 Beim Modell der vollständigen Parallelität wird dann von einer Verletzung ausgegangen, wenn, nachdem ein Benutzer einen Wert aus der Datenbank empfangen hat, ein anderer Benutzer den Wert ändert, bevor der erste Benutzer den Wert zu ändern versucht hat.  Wie der Server eine Parallelitätsverletzung auflöst, kann am Besten anhand des folgenden Beispiels erläutert werden.  
  
 Die folgenden Tabellen stellen ein Beispiel für eine vollständige Parallelität dar.  
  
 Um 13:00 Uhr ruft User1 eine Zeile mit den folgenden Werten aus der Datenbank auf:  
  
 **CustID     LastName     FirstName**  
  
 101          Smith             Bob  
  
|Spaltenname|Ursprünglicher Wert|Aktueller Wert|Wert in Datenbank|  
|-----------------|-------------------------|--------------------|-----------------------|  
|CustID|101|101|101|  
|LastName|Smith|Smith|Smith|  
|FirstName|Bob|Bob|Bob|  
  
 Um 13:01 Uhr ruft User2 dieselbe Zeile ab.  
  
 Um 13:03 Uhr ändert User2 **FirstName** von **Bob** in **Robert** und aktualisiert die Datenbank.  
  
|Spaltenname|Ursprünglicher Wert|Aktueller Wert|Wert in Datenbank|  
|-----------------|-------------------------|--------------------|-----------------------|  
|CustID|101|101|101|  
|LastName|Smith|Smith|Smith|  
|FirstName|Bob|Robert|Bob|  
  
 Das Update ist erfolgreich, weil die Werte in der Datenbank zur Zeit des Updates mit den ursprünglichen Werten übereinstimmen, die User2 vorliegen.  
  
 Um 13:05 Uhr ändert User1 **FirstName** von **Bob** in **James** und versucht, die Zeile zu aktualisieren.  
  
|Spaltenname|Ursprünglicher Wert|Aktueller Wert|Wert in Datenbank|  
|-----------------|-------------------------|--------------------|-----------------------|  
|CustID|101|101|101|  
|LastName|Smith|Smith|Smith|  
|FirstName|Bob|James|Robert|  
  
 Zu diesem Zeitpunkt stellt User1 eine Verletzung der optimistischen Parallelität fest, weil der Wert in der Datenbank \("Robert"\) nicht mit dem ursprünglichen Wert übereinstimmt, den User1 erwartet hat \("Bob"\).  Die Parallelitätsverletzung sagt Ihnen einfach nur, dass das Update fehlgeschlagen ist.  Nun muss entschieden werden, ob die von User2 vorgenommenen Änderungen mit den Änderungen von User1 überschrieben oder die Änderungen von User1 verworfen werden sollen.  
  
## Testen auf Verletzung der vollständigen Parallelität  
 Es gibt mehrere Testverfahren, mit denen eine Verletzung der vollständigen Parallelität festgestellt werden kann.  Eine Methode besteht im Einfügen einer Timestamp\-Spalte in die Tabelle.  Datenbanken stellen im Allgemeinen Timestamp\-Funktionen zur Verfügung, mit denen der Updatezeitpunkt \(Datum und Uhrzeit\) des Datensatzes gekennzeichnet werden kann.  Bei dieser Methode wird eine Timestamp\-Spalte in die Tabellendefinition eingefügt.  Bei jedem Update des Datensatzes wird der Timestamp aktualisiert, sodass er den aktuellen Zeitpunkt \(Datum und Uhrzeit\) angibt.  Bei einem Test auf eine Verletzung der vollständigen Parallelität wird die Timestamp\-Spalte bei jeder Abfrage des Tabelleninhalts zurückgegeben.  Bei einem Updateversuch wird der Timestamp\-Wert in der Datenbank mit dem ursprünglichen Timestamp\-Wert verglichen, der in der geänderten Zeile enthalten ist.  Wenn die Werte identisch sind, wird die Timestamp\-Spalte mit der aktuellen Uhrzeit aktualisiert, um das Update zu kennzeichnen.  Wenn die Werte nicht identisch sind, wurde die vollständige Parallelität verletzt.  
  
 Beim zweiten Testverfahren, mit dem eine Verletzung der vollständigen Parallelität festgestellt werden kann, wird überprüft, ob alle ursprünglichen Spaltenwerte einer Zeile mit denen in der Datenbank übereinstimmen.  Betrachten Sie z. B. folgende Abfrage:  
  
```  
SELECT Col1, Col2, Col3 FROM Table1  
```  
  
 Um beim Aktualisieren einer Zeile in **Table1** einen Test auf eine Verletzung der vollständigen Parallelität durchzuführen, geben Sie folgende UPDATE\-Anweisung an:  
  
```  
UPDATE Table1 Set Col1 = @NewCol1Value,  
              Set Col2 = @NewCol2Value,  
              Set Col3 = @NewCol3Value  
WHERE Col1 = @OldCol1Value AND  
      Col2 = @OldCol2Value AND  
      Col3 = @OldCol3Value  
```  
  
 Wenn die ursprünglichen Werte mit den Werten in der Datenbank übereinstimmen, wird das Update durchgeführt.  Wenn ein Wert geändert wurde, wird die Zeile von der UPDATE\-Anweisung nicht aktualisiert, weil die WHERE\-Klausel keine Übereinstimmung findet.  
  
 Es empfiehlt sich, in einer Abfrage stets einen eindeutigen Primärschlüsselwert zurückzugeben.  Andernfalls könnte die vorhergehende UPDATE\-Anweisung mehr als eine Zeile aktualisieren, was möglicherweise nicht von Ihnen beabsichtigt ist.  
  
 Wenn in einer Spalte Ihrer Datenquelle NULL\-Werte zulässig sind, müssen Sie Ihre WHERE\-Klausel erweitern, um nach einem entsprechenden NULL\-Verweis in Ihrer lokalen Tabelle und in der Datenquelle zu suchen.  Die folgende UPDATE\-Anweisung prüft z. B., ob ein NULL\-Verweis in der lokalen Zeile noch mit einem NULL\-Verweis in der Datenquelle übereinstimmt oder ob der Wert in der lokalen Zeile noch mit dem Wert in der Datenquelle übereinstimmt.  
  
```  
UPDATE Table1 Set Col1 = @NewVal1  
  WHERE (@OldVal1 IS NULL AND Col1 IS NULL) OR Col1 = @OldVal1  
```  
  
 Sie können bei einem Modell der vollständigen Parallelität auch weniger strenge Kriterien anwenden.  Wenn Sie z. B. nur die Primärschlüsselspalten in der WHERE\-Klausel verwenden, werden die Daten unabhängig davon überschrieben, ob die anderen Spalten seit der letzten Abfrage aktualisiert wurden oder nicht.  Zudem besteht die Möglichkeit, eine WHERE\-Klausel auf spezifische Spalten anzuwenden, sodass die Daten überschrieben werden, sofern nicht bestimmte Felder seit deren letzten Abfrage aktualisiert wurden.  
  
### Das "DataAdapter.RowUpdated"\-Ereignis  
 Das **RowUpdated**\-Ereignis des <xref:System.Data.Common.DataAdapter>\-Objekts kann in Verbindung mit den oben beschriebenen Methoden eingesetzt werden, um Ihre Anwendung über eine Verletzung der vollständigen Parallelität zu benachrichtigen.  **RowUpdated** tritt nach jedem Versuch auf, eine **Modified**\-Zeile eines **DataSets** zu aktualisieren.  Damit können Sie spezifischen Behandlungscode hinzufügen, einschließlich Verarbeitung bei Ausnahmen, Einfügen von benutzerdefinierten Fehlerinformationen, Hinzufügen einer Wiederholungslogik usw.  Das <xref:System.Data.Common.RowUpdatedEventArgs>\-Objekt gibt eine **RecordsAffected**\-Eigenschaft mit der Anzahl Zeilen zurück, die von einem bestimmten Updatebefehl für eine bestimmte geänderte Zeile in einer Tabelle betroffen sind.  Wenn Sie den Updatebefehl auf einen Test auf vollständige Parallelität festlegen, gibt die **RecordsAffected**\-Eigenschaft bei einer Verletzung der vollständigen Parallelität den Wert 0 als Ergebnis zurück, weil keine Datensätze aktualisiert wurden.  Wenn dies der Fall ist, wird eine Ausnahme ausgelöst.  Mit dem **RowUpdated**\-Ereignis können Sie dieser Situation begegnen und die Ausnahme verhindern, indem Sie einen entsprechenden **RowUpdatedEventArgs.Status**\-Wert festlegen, z. B. **UpdateStatus.SkipCurrentRow**.  Weitere Informationen zum **RowUpdated**\-Ereignis finden Sie unter [Umgang mit 'DataAdapter'\-Ereignissen](../../../../docs/framework/data/adonet/handling-dataadapter-events.md).  
  
 Optional können Sie **DataAdapter.ContinueUpdateOnError** auf **True** festlegen, bevor Sie **Update** aufrufen und auf die in der **RowError**\-Eigenschaft einer bestimmten Zeile gespeicherten Fehlerinformationen reagieren, wenn die **Update**\-Operation abgeschlossen ist.  Weitere Informationen finden Sie unter [Zeilenfehlerinformationen](../../../../docs/framework/data/adonet/dataset-datatable-dataview/row-error-information.md).  
  
## Beispiel für eine vollständige Parallelität  
 Im folgenden einfachen Beispiel wird der **UpdateCommand** eines **DataAdapter** darauf festgelegt, einen Test auf vollständige Parallelität auszuführen. Außerdem wird mit dem **RowUpdated**\-Ereignis getestet, ob die vollständige Parallelität verletzt wurde.  Wenn die vollständige Parallelität verletzt wurde, legt die Anwendung für die Zeile, für die das Update angegeben wurde, **RowError** fest, sodass eine Verletzung der vollständigen Parallelität angezeigt wird.  
  
 Beachten Sie, dass die Parameterwerte, die an die WHERE\-Klausel des UPDATE\-Befehls übergeben wurden, den **Original**\-Werten der entsprechenden Spalten zugeordnet werden.  
  
```vb  
' Assumes connection is a valid SqlConnection.  
Dim adapter As SqlDataAdapter = New SqlDataAdapter( _  
  "SELECT CustomerID, CompanyName FROM Customers ORDER BY CustomerID", _  
  connection)  
  
' The Update command checks for optimistic concurrency violations  
' in the WHERE clause.  
adapter.UpdateCommand = New SqlCommand("UPDATE Customers " &  
  "(CustomerID, CompanyName) VALUES(@CustomerID, @CompanyName) " & _  
  "WHERE CustomerID = @oldCustomerID AND CompanyName = " &  
  "@oldCompanyName", connection)  
adapter.UpdateCommand.Parameters.Add( _  
  "@CustomerID", SqlDbType.NChar, 5, "CustomerID")  
adapter.UpdateCommand.Parameters.Add( _  
  "@CompanyName", SqlDbType.NVarChar, 30, "CompanyName")  
  
' Pass the original values to the WHERE clause parameters.  
Dim parameter As SqlParameter = dataSet.UpdateCommand.Parameters.Add( _  
  "@oldCustomerID", SqlDbType.NChar, 5, "CustomerID")  
parameter.SourceVersion = DataRowVersion.Original  
parameter = adapter.UpdateCommand.Parameters.Add( _  
  "@oldCompanyName", SqlDbType.NVarChar, 30, "CompanyName")  
parameter.SourceVersion = DataRowVersion.Original  
  
' Add the RowUpdated event handler.  
AddHandler adapter.RowUpdated, New SqlRowUpdatedEventHandler( _  
  AddressOf OnRowUpdated)  
  
Dim dataSet As DataSet = New DataSet()  
adapter.Fill(dataSet, "Customers")  
  
' Modify the DataSet contents.  
adapter.Update(dataSet, "Customers")  
  
Dim dataRow As DataRow  
  
For Each dataRow In dataSet.Tables("Customers").Rows  
    If dataRow.HasErrors Then   
       Console.WriteLine(dataRow (0) & vbCrLf & dataRow.RowError)  
    End If  
Next  
  
Private Shared Sub OnRowUpdated( _  
  sender As object, args As SqlRowUpdatedEventArgs)  
   If args.RecordsAffected = 0  
      args.Row.RowError = "Optimistic Concurrency Violation!"  
      args.Status = UpdateStatus.SkipCurrentRow  
   End If  
End Sub  
```  
  
```csharp  
// Assumes connection is a valid SqlConnection.  
SqlDataAdapter adapter = new SqlDataAdapter(  
  "SELECT CustomerID, CompanyName FROM Customers ORDER BY CustomerID",  
  connection);  
  
// The Update command checks for optimistic concurrency violations  
// in the WHERE clause.  
adapter.UpdateCommand = new SqlCommand("UPDATE Customers Set CustomerID = @CustomerID, CompanyName = @CompanyName " +  
   "WHERE CustomerID = @oldCustomerID AND CompanyName = @oldCompanyName, connection);  
adapter.UpdateCommand.Parameters.Add(  
  "@CustomerID", SqlDbType.NChar, 5, "CustomerID");  
adapter.UpdateCommand.Parameters.Add(  
  "@CompanyName", SqlDbType.NVarChar, 30, "CompanyName");  
  
// Pass the original values to the WHERE clause parameters.  
SqlParameter parameter = adapter.UpdateCommand.Parameters.Add(  
  "@oldCustomerID", SqlDbType.NChar, 5, "CustomerID");  
parameter.SourceVersion = DataRowVersion.Original;  
parameter = adapter.UpdateCommand.Parameters.Add(  
  "@oldCompanyName", SqlDbType.NVarChar, 30, "CompanyName");  
parameter.SourceVersion = DataRowVersion.Original;  
  
// Add the RowUpdated event handler.  
adapter.RowUpdated += new SqlRowUpdatedEventHandler(OnRowUpdated);  
  
DataSet dataSet = new DataSet();  
adapter.Fill(dataSet, "Customers");  
  
// Modify the DataSet contents.  
  
adapter.Update(dataSet, "Customers");  
  
foreach (DataRow dataRow in dataSet.Tables["Customers"].Rows)  
{  
    if (dataRow.HasErrors)  
       Console.WriteLine(dataRow [0] + "\n" + dataRow.RowError);  
}  
  
protected static void OnRowUpdated(object sender, SqlRowUpdatedEventArgs args)  
{  
  if (args.RecordsAffected == 0)   
  {  
    args.Row.RowError = "Optimistic Concurrency Violation Encountered";  
    args.Status = UpdateStatus.SkipCurrentRow;  
  }  
}  
```  
  
## Siehe auch  
 [Abrufen und Ändern von Daten in ADO.NET](../../../../docs/framework/data/adonet/retrieving-and-modifying-data.md)   
 [Aktualisieren von Datenquellen mit DataAdapters](../../../../docs/framework/data/adonet/updating-data-sources-with-dataadapters.md)   
 [Zeilenfehlerinformationen](../../../../docs/framework/data/adonet/dataset-datatable-dataview/row-error-information.md)   
 [Transaktionen und Parallelität](../../../../docs/framework/data/adonet/transactions-and-concurrency.md)   
 [ADO.NET Verwaltete Anbieter und DataSet\-Entwicklercenter](http://go.microsoft.com/fwlink/?LinkId=217917)