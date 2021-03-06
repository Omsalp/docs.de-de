---
title: "Tabellenwertparameter | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-ado"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 370c16d5-db7b-43e3-945b-ccaab35b739b
caps.latest.revision: 5
author: "JennieHubbard"
ms.author: "jhubbard"
manager: "jhubbard"
caps.handback.revision: 5
---
# Tabellenwertparameter
Tabellenwertparameter bieten eine gute Möglichkeit, mehrere Datenzeilen aus einer Clientanwendung an [!INCLUDE[ssNoVersion](../../../../../includes/ssnoversion-md.md)] zu marshallen, ohne dass mehrere Roundtrips oder eine besondere serverseitige Logik für die Verarbeitung der Daten notwendig sind.  Sie können Tabellenwertparameter verwenden, um Datenzeilen in einer Clientanwendung zu kapseln und diese Daten in einem einzelnen parametrisierten Befehl an den Server zu senden.  Die eingehenden Datenzeilen werden in einer Tabellenvariablen gespeichert, die anschließend mit [!INCLUDE[tsql](../../../../../includes/tsql-md.md)] bearbeitet werden kann.  
  
 Auf Spaltenwerte in Tabellenwertparametern kann mit SELECT\-Standardanweisungen in [!INCLUDE[tsql](../../../../../includes/tsql-md.md)] zugegriffen werden.  Tabellenwertparameter sind stark typisiert, und die Überprüfung ihrer Struktur erfolgt automatisch.  Die Größe der Tabellenwertparameter wird nur vom Serverarbeitsspeicher beschränkt.  
  
> [!NOTE]
>  Daten in einem Tabellenwertparameter können nicht zurückgegeben werden.  Tabellenwertparameter sind reine Eingabeparameter; das OUTPUT\-Schlüsselwort wird nicht unterstützt.  
  
 Weitere Informationen über Tabellenwertparameter finden Sie in den folgenden Ressourcen.  
  
|Ressource|Beschreibung|  
|---------------|------------------|  
|[Tabellenwertparameter \(Datenbankmodul\)](http://go.microsoft.com/fwlink/?LinkId=98363) in der [!INCLUDE[ssNoVersion](../../../../../includes/ssnoversion-md.md)]\-Onlinedokumentation|Beschreibt das Erstellen und Verwenden von Tabellenwertparametern.|  
|[Benutzerdefinierte Tabellentypen](http://go.microsoft.com/fwlink/?LinkId=98364) in der [!INCLUDE[ssNoVersion](../../../../../includes/ssnoversion-md.md)]\-Onlinedokumentation|Beschreibt benutzerdefinierte Tabellentypen, die zum Deklarieren von Tabellenwertparametern verwendet werden.|  
|Der Abschnitt [Microsoft SQL Server\-Datenbankmodul](http://go.microsoft.com/fwlink/?LinkId=120507) von [CodePlex](http://go.microsoft.com/fwlink/?LinkID=67739)|Enthält Beispiele, in denen die Verwendung von [!INCLUDE[ssNoVersion](../../../../../includes/ssnoversion-md.md)]\-Funktionen veranschaulicht wird.|  
  
## Übergeben von mehreren Zeilen in älteren Versionen von SQL Server  
 Vor der Einführung von Tabellenwertparametern in [!INCLUDE[ssNoVersion](../../../../../includes/ssnoversion-md.md)] 2008 bestanden nur eingeschränkte Möglichkeiten zum Übergeben mehrerer Datenzeilen an eine gespeicherte Prozedur oder einen parametrisierten SQL\-Befehl.  Ein Entwickler konnte aus den folgenden Optionen zum Übergeben mehrerer Zeilen an den Server auswählen:  
  
-   Er hatte die Möglichkeit, eine Reihe einzelner Parameter zu verwenden, um die Werte in mehreren Spalten und Zeilen von Daten darzustellen.  Die Datenmenge, die mithilfe dieser Methode übergeben werden kann, ist durch die Anzahl der zulässigen Parameter beschränkt.  [!INCLUDE[ssNoVersion](../../../../../includes/ssnoversion-md.md)]\-Prozeduren können höchstens über 2100 Parameter verfügen.  Eine Zusammenstellung zur Verarbeitung dieser einzelnen Werte in einer Tabellenvariablen oder einer temporären Tabelle erfordert serverseitige Logik.  
  
-   Ein Entwickler hatte weiterhin die Möglichkeit, mehrere Datenwerte in voneinander getrennten Zeichenfolgen oder in XML\-Dokumenten zu bündeln und diese Textwerte anschließend an eine Prozedur oder Anweisung zu übergeben.  Bei dieser Methode muss die Prozedur oder Anweisung die erforderliche Logik zum Überprüfen der Datenstrukturen sowie zum Entbündeln der Werte beinhalten.  
  
-   Ein Entwickler konnte weiterhin eine Reihe einzelner SQL\-Datenänderungsanweisungen erstellen, die mehrere Zeilen betreffen, wie beispielsweise Anweisungen, die durch den Aufruf der `Update`\-Methode eines <xref:System.Data.SqlClient.SqlDataAdapter> erstellt werden.  Die Änderungen können dabei einzeln oder in Gruppen gestapelt an den Server gesendet werden.  Jede Anweisung wird jedoch auf dem Server einzeln ausgeführt, auch wenn die Anweisungen in Stapeln übermittelt werden.  
  
-   Das `bcp`\-Hilfsprogramm und das <xref:System.Data.SqlClient.SqlBulkCopy>\-Objekt bieten ebenfalls die Möglichkeit, mehrere Datenzeilen in eine Tabelle zu laden.  Obwohl diese Technik sehr effizient ist, bietet sie keine Unterstützung für serverseitige Verarbeitung, wenn die Daten nicht in eine temporäre Tabelle oder Tabellenvariable geladen werden.  
  
## Erstellen von Tabellenwertparameter\-Typen  
 Tabellenwertparameter basieren auf stark typisierten Tabellenstrukturen, die mit CREATE TYPE\-Anweisungen in [!INCLUDE[tsql](../../../../../includes/tsql-md.md)] definiert werden.  Sie müssen einen Tabellentyp erstellen und die Struktur in [!INCLUDE[ssNoVersion](../../../../../includes/ssnoversion-md.md)] definieren, bevor Sie Tabellenwertparameter in Ihren Clientanwendungen verwenden können.  Weitere Informationen über das Erstellen von Tabellentypen finden Sie unter [Benutzerdefinierte Tabellentypen](http://go.microsoft.com/fwlink/?LinkID=98364) in der [!INCLUDE[ssNoVersion](../../../../../includes/ssnoversion-md.md)]\-Onlinedokumentation.  
  
 Die folgende Anweisung erstellt einen Tabellentyp mit dem Namen \<legacyBold\>CategoryTableType\<\/legacyBold\>, der aus den Spalten \<legacyBold\>CategoryID\<\/legacyBold\> und \<legacyBold\>CategoryName\<\/legacyBold\> besteht:  
  
```  
CREATE TYPE dbo.CategoryTableType AS TABLE  
    ( CategoryID int, CategoryName nvarchar(50) )  
```  
  
 Nach der Erstellung eines Tabellentyps können Tabellenwertparameter auf Grundlage dieses Typs deklariert werden.  Das folgende [!INCLUDE[tsql](../../../../../includes/tsql-md.md)]\-Fragment veranschaulicht, wie ein Tabellenwertparameter in der Definition einer gespeicherten Prozedur deklariert wird.  Beachten Sie, dass das READONLY\-Schlüsselwort zum Deklarieren eines Tabellenwertparameters erforderlich ist.  
  
```  
CREATE PROCEDURE usp_UpdateCategories   
    (@tvpNewCategories dbo.CategoryTableType READONLY)  
```  
  
## Ändern von Daten mit Tabellenwertparametern \(Transact\-SQL\)  
 Tabellenwertparameter können in mengenbasierten Datenänderungen verwendet werden, die mehrere Zeilen mit der Ausführung einer einzelnen Anweisung beeinflussen können.  So können Sie z. B. alle Zeilen in einem Tabellenwertparameter auswählen und diese in eine Datenbanktabelle einfügen, oder Sie können eine Updateanweisung erstellen, indem Sie einen Tabellenwertparameter mit der zu aktualisierenden Tabelle verknüpfen.  
  
 Mit der folgenden UPDATE\-Anweisung in [!INCLUDE[tsql](../../../../../includes/tsql-md.md)] wird veranschaulicht, wie ein Tabellenwertparameter durch einen Join mit der Categories\-Tabelle verwendet wird.  Wenn Sie einen Tabellenwertparameter mit einem JOIN in einer FROM\-Klausel verwenden, müssen Sie diesen ebenfalls mit einem Alias versehen. Dies ist im folgenden Beispiel dargestellt, in dem der Tabellenwertparameter mit dem Alias "ec" versehen ist:  
  
```  
UPDATE dbo.Categories  
    SET Categories.CategoryName = ec.CategoryName  
    FROM dbo.Categories INNER JOIN @tvpEditedCategories AS ec  
    ON dbo.Categories.CategoryID = ec.CategoryID;  
```  
  
 Dieses [!INCLUDE[tsql](../../../../../includes/tsql-md.md)]\-Beispiel veranschaulicht, wie Zeilen aus einem Tabellenwertparameter ausgewählt werden, um einen INSERT\-Vorgang in einem einzelnen mengenbasierten Vorgang auszuführen.  
  
```  
INSERT INTO dbo.Categories (CategoryID, CategoryName)  
    SELECT nc.CategoryID, nc.CategoryName FROM @tvpNewCategories AS nc;  
```  
  
## Einschränkungen von Tabellenwertparametern  
 Es gibt mehrere Einschränkungen bei Tabellenwertparametern:  
  
-   Tabellenwertparameter können nicht an [benutzerdefinierte CLR\-Funktionen](http://msdn.microsoft.com/library/ms131077.aspx) übergeben werden.  
  
-   Tabellenwertparameter können nur zur Unterstützung von UNIQUE\- und PRIMARY KEY\-Einschränkungen indiziert werden.  [!INCLUDE[ssNoVersion](../../../../../includes/ssnoversion-md.md)] führt keine Statistik zu Tabellenwertparametern.  
  
-   Tabellenwertparameter sind in [!INCLUDE[tsql](../../../../../includes/tsql-md.md)]\-Code schreibgeschützt.  Die Spaltenwerte in den Zeilen eines Tabellenwertparameters können nicht aktualisiert werden, und Zeilen können nicht eingefügt oder gelöscht werden.  Um die Daten zu ändern, die in einem Tabellenwertparameter an eine gespeicherte Prozedur oder eine parametrisierte Anweisung übergeben werden, müssen die Daten in eine temporäre Tabelle oder eine Tabellenvariable eingefügt werden.  
  
-   Es können keine ALTER TABLE\-Anweisungen zum Ändern des Entwurfs von Tabellenwertparametern verwendet werden.  
  
## Konfigurieren eines SqlParameter\-Beispiels  
 <xref:System.Data.SqlClient> unterstützt das Auffüllen von Tabellenwertparametern aus Objekten vom Typ <xref:System.Data.DataTable>, <xref:System.Data.Common.DbDataReader> oder System.Collections.Generic.IEnumerable\<<xref:Microsoft.SqlServer.Server.SqlDataRecord>\> \(<xref:System.Collections.Generic.IEnumerable%601>?qualifyHint=False&autoUpgrade=True).  Mithilfe der <xref:System.Data.SqlClient.SqlParameter.TypeName%2A>\-Eigenschaft eines <xref:System.Data.SqlClient.SqlParameter> muss ein Typname für den Tabellenwertparameter angegeben werden.  Der `TypeName` muss dem Namen eines kompatiblen Typs entsprechen, der zuvor auf dem Server erstellt wurde.  Das folgende Codefragment zeigt, wie <xref:System.Data.SqlClient.SqlParameter> konfiguriert werden kann, um Daten einzufügen:  
  
```csharp  
// Configure the command and parameter.  
SqlCommand insertCommand = new SqlCommand(  
    sqlInsert, connection);  
SqlParameter tvpParam = insertCommand.Parameters.AddWithValue(  
    "@tvpNewCategories", addedCategories);  
tvpParam.SqlDbType = SqlDbType.Structured;  
tvpParam.TypeName = "dbo.CategoryTableType";  
```  
  
```vb  
' Configure the command and parameter.  
Dim insertCommand As New SqlCommand(sqlInsert, connection)  
Dim tvpParam As SqlParameter = _  
   insertCommand.Parameters.AddWithValue( _  
  "@tvpNewCategories", addedCategories)  
tvpParam.SqlDbType = SqlDbType.Structured  
tvpParam.TypeName = "dbo.CategoryTableType"  
  
```  
  
 Sie können auch ein von <xref:System.Data.Common.DbDataReader> abgeleitetes Objekt verwenden, um Datenzeilen als Stream in einen Tabellenwertparameter zu übertragen. Dies ist im folgenden Fragment dargestellt:  
  
```csharp  
// Configure the SqlCommand and table-valued parameter.  
SqlCommand insertCommand = new SqlCommand(  
  "usp_InsertCategories", connection);  
insertCommand.CommandType = CommandType.StoredProcedure;  
SqlParameter tvpParam =   
   insertCommand.Parameters.AddWithValue(  
   "@tvpNewCategories", dataReader);  
tvpParam.SqlDbType = SqlDbType.Structured;  
```  
  
```vb  
' Configure the SqlCommand and table-valued parameter.  
Dim insertCommand As New SqlCommand("usp_InsertCategories", connection)  
insertCommand.CommandType = CommandType.StoredProcedure  
Dim tvpParam As SqlParameter = _  
  insertCommand.Parameters.AddWithValue("@tvpNewCategories", _  
  dataReader)  
tvpParam.SqlDbType = SqlDbType.Structured  
  
```  
  
## Übergeben eines Tabellenwertparameters an eine gespeicherte Prozedur  
 In diesem Beispiel wird veranschaulicht, wie Daten eines Tabellenwertparameters an eine gespeicherte Prozedur übergeben werden.  Der Code ruft hinzugefügte Zeilen mithilfe der <xref:System.Data.DataTable.GetChanges%2A>\-Methode in eine neue <xref:System.Data.DataTable> ab.  Anschließend definiert der Code einen <xref:System.Data.SqlClient.SqlCommand>, mit dem die <xref:System.Data.SqlClient.SqlCommand.CommandType%2A>\-Eigenschaft auf <xref:System.Data.CommandType> festgelegt wird.  Der <xref:System.Data.SqlClient.SqlParameter> wird mit der <xref:System.Data.SqlClient.SqlParameterCollection.AddWithValue%2A>\-Methode aufgefüllt, und der <xref:System.Data.SqlClient.SqlParameter.SqlDbType%2A> wird auf `Structured` festgelegt.  Der <xref:System.Data.SqlClient.SqlCommand> wird dann mithilfe der <xref:System.Data.SqlClient.SqlCommand.ExecuteNonQuery%2A>\-Methode ausgeführt.  
  
```csharp  
// Assumes connection is an open SqlConnection object.  
using (connection)  
{  
// Create a DataTable with the modified rows.  
DataTable addedCategories =  
  CategoriesDataTable.GetChanges(DataRowState.Added);  
  
// Configure the SqlCommand and SqlParameter.  
SqlCommand insertCommand = new SqlCommand(  
    "usp_InsertCategories", connection);  
insertCommand.CommandType = CommandType.StoredProcedure;  
SqlParameter tvpParam = insertCommand.Parameters.AddWithValue(  
    "@tvpNewCategories", addedCategories);  
tvpParam.SqlDbType = SqlDbType.Structured;  
  
// Execute the command.  
insertCommand.ExecuteNonQuery();  
}  
```  
  
```vb  
' Assumes connection is an open SqlConnection object.  
Using connection  
   '  Create a DataTable with the modified rows.  
   Dim addedCategories As DataTable = _  
     CategoriesDataTable.GetChanges(DataRowState.Added)  
  
  ' Configure the SqlCommand and SqlParameter.  
   Dim insertCommand As New SqlCommand( _  
     "usp_InsertCategories", connection)  
   insertCommand.CommandType = CommandType.StoredProcedure  
   Dim tvpParam As SqlParameter = _  
     insertCommand.Parameters.AddWithValue( _  
     "@tvpNewCategories", addedCategories)  
   tvpParam.SqlDbType = SqlDbType.Structured  
  
   '  Execute the command.  
   insertCommand.ExecuteNonQuery()  
End Using  
```  
  
### Übergeben eines Tabellenwertparameters an eine parametrisierte SQL\-Anweisung  
 Im folgenden Beispiel wird veranschaulicht, wie Daten unter Verwendung einer INSERT\-Anweisung mit einer SELECT\-Unterabfrage, die über einen Tabellenwertparameter als Datenquelle verfügt, in die \<legacyBold\>dbo.Categories\<\/legacyBold\>\-Tabelle eingefügt werden.  Beim Übergeben eines Tabellenwertparameters an eine parametrisierte SQL\-Anweisung muss ein Typname für den Tabellenwertparameter angegeben werden, indem die neue <xref:System.Data.SqlClient.SqlParameter.TypeName%2A>\-Eigenschaft eines <xref:System.Data.SqlClient.SqlParameter> verwendet wird.  Dieser `TypeName` muss dem Namen eines kompatiblen Typs entsprechen, der zuvor auf dem Server erstellt wurde.  Im Code in diesem Beispiel wird die `TypeName`\-Eigenschaft verwendet, um auf die in \<legacyBold\>dbo.CategoryTableType\<\/legacyBold\> definierte Typstruktur zu verweisen.  
  
> [!NOTE]
>  Wenn ein Wert für eine Identitätsspalte in einem Tabellenwertparameter angegeben wird, muss die SET IDENTITY\_INSERT\-Anweisung für die Sitzung ausgegeben werden.  
  
```csharp  
// Assumes connection is an open SqlConnection.  
using (connection)  
{  
// Create a DataTable with the modified rows.  
DataTable addedCategories = CategoriesDataTable.GetChanges(  
    DataRowState.Added);  
  
// Define the INSERT-SELECT statement.  
string sqlInsert =   
    "INSERT INTO dbo.Categories (CategoryID, CategoryName)"  
    + " SELECT nc.CategoryID, nc.CategoryName"  
    + " FROM @tvpNewCategories AS nc;"  
  
// Configure the command and parameter.  
SqlCommand insertCommand = new SqlCommand(  
    sqlInsert, connection);  
SqlParameter tvpParam = insertCommand.Parameters.AddWithValue(  
    "@tvpNewCategories", addedCategories);  
tvpParam.SqlDbType = SqlDbType.Structured;  
tvpParam.TypeName = "dbo.CategoryTableType";  
  
// Execute the command.  
insertCommand.ExecuteNonQuery();  
}  
```  
  
```vb  
' Assumes connection is an open SqlConnection.  
Using connection  
  ' Create a DataTable with the modified rows.  
  Dim addedCategories As DataTable = _  
    CategoriesDataTable.GetChanges(DataRowState.Added)  
  
  ' Define the INSERT-SELECT statement.  
  Dim sqlInsert As String = _  
  "INSERT INTO dbo.Categories (CategoryID, CategoryName)" _  
  & " SELECT nc.CategoryID, nc.CategoryName" _  
  & " FROM @tvpNewCategories AS nc;"  
  
  ' Configure the command and parameter.  
  Dim insertCommand As New SqlCommand(sqlInsert, connection)  
  Dim tvpParam As SqlParameter = _  
     insertCommand.Parameters.AddWithValue( _  
    "@tvpNewCategories", addedCategories)  
  tvpParam.SqlDbType = SqlDbType.Structured  
  tvpParam.TypeName = "dbo.CategoryTableType"  
  
  ' Execute the query  
  insertCommand.ExecuteNonQuery()  
End Using  
```  
  
## Streaming von Zeilen mit einem DataReader  
 Sie können auch ein von <xref:System.Data.Common.DbDataReader> abgeleitetes Objekt verwenden, um Datenzeilen als Stream in einen Tabellenwertparameter zu übertragen.  Im folgenden Codefragment wird veranschaulicht, wie Daten mithilfe eines <xref:System.Data.OracleClient.OracleCommand> und eines <xref:System.Data.OracleClient.OracleDataReader> aus einer Oracle\-Datenbank abgerufen werden.  Im Code wird dann ein <xref:System.Data.SqlClient.SqlCommand> konfiguriert, um eine gespeicherte Prozedur mit einem einzelnen Eingabeparameter aufzurufen.  Die <xref:System.Data.SqlClient.SqlParameter.SqlDbType%2A>\-Eigenschaft von <xref:System.Data.SqlClient.SqlParameter> wird auf `Structured` festgelegt.  Die <xref:System.Data.SqlClient.SqlParameterCollection.AddWithValue%2A>\-Methode übergibt das `OracleDataReader`\-Resultset als Tabellenwertparameter an die gespeicherte Prozedur.  
  
```csharp  
// Assumes connection is an open SqlConnection.  
// Retrieve data from Oracle.  
OracleCommand selectCommand = new OracleCommand(  
   "Select CategoryID, CategoryName FROM Categories;",  
   oracleConnection);  
OracleDataReader oracleReader = selectCommand.ExecuteReader(  
   CommandBehavior.CloseConnection);  
  
 // Configure the SqlCommand and table-valued parameter.  
 SqlCommand insertCommand = new SqlCommand(  
   "usp_InsertCategories", connection);  
 insertCommand.CommandType = CommandType.StoredProcedure;  
 SqlParameter tvpParam =   
    insertCommand.Parameters.AddWithValue(  
    "@tvpNewCategories", oracleReader);  
 tvpParam.SqlDbType = SqlDbType.Structured;  
  
 // Execute the command.  
 insertCommand.ExecuteNonQuery();  
```  
  
```vb  
' Assumes connection is an open SqlConnection.  
' Retrieve data from Oracle.  
Dim selectCommand As New OracleCommand( _  
  "Select CategoryID, CategoryName FROM Categories;", _  
  oracleConnection)  
Dim oracleReader As OracleDataReader = _  
  selectCommand.ExecuteReader(CommandBehavior.CloseConnection)  
  
' Configure SqlCommand and table-valued parameter.  
Dim insertCommand As New SqlCommand("usp_InsertCategories", connection)  
insertCommand.CommandType = CommandType.StoredProcedure  
Dim tvpParam As SqlParameter = _  
  insertCommand.Parameters.AddWithValue("@tvpNewCategories", _  
  oracleReader)  
tvpParam.SqlDbType = SqlDbType.Structured  
  
' Execute the command.  
insertCommand.ExecuteNonQuery()  
```  
  
## Siehe auch  
 [Konfigurieren von Parametern und Parameterdatentypen](../../../../../docs/framework/data/adonet/configuring-parameters-and-parameter-data-types.md)   
 [Befehle und Parameter](../../../../../docs/framework/data/adonet/commands-and-parameters.md)   
 ['DataAdapter'\-Parameter](../../../../../docs/framework/data/adonet/dataadapter-parameters.md)   
 [SQL Server\-Datenoperationen in ADO.NET](../../../../../docs/framework/data/adonet/sql/sql-server-data-operations.md)   
 [ADO.NET Verwaltete Anbieter und DataSet\-Entwicklercenter](http://go.microsoft.com/fwlink/?LinkId=217917)