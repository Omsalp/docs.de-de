---
title: "Entity SQL-Kurzreferenz | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-ado"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "VB"
  - "CSharp"
  - "C++"
ms.assetid: e53dad9e-5e83-426e-abb4-be3e78e3d6dc
caps.latest.revision: 2
author: "JennieHubbard"
ms.author: "jhubbard"
manager: "jhubbard"
caps.handback.revision: 2
---
# Entity SQL-Kurzreferenz
Dieses Thema enthält eine Kurzreferenz zu [!INCLUDE[esql](../../../../../../includes/esql-md.md)]\-Abfragen.  Die Abfragen in diesem Thema basieren auf dem AdventureWorks Sales\-Modell.  
  
## Literale  
  
### Zeichenfolge  
 Es gibt Zeichenfolgenliterale, die aus Unicode\-, und solche, die aus Nicht\-Unicode\-Zeichen bestehen.  Unicode\-Zeichenfolgen wird "N" vorangestellt.  Beispielsweise `N'hello'`.  
  
 Im Folgenden ist ein Beispiel für ein nicht\-Unicode\-Zeichenfolgenliteral abgebildet:  
  
```  
'hello'  
--same as  
"hello"  
```  
  
 Ausgabe:  
  
|Wert|  
|----------|  
|hello|  
  
### DateTime  
 In **DateTime**\-Literalen sind sowohl das Datum als auch die Uhrzeit erforderlich.  Es gibt keine Standardwerte.  
  
 Beispiel:  
  
```  
DATETIME '2006-12-25 01:01:00.000'   
--same as  
DATETIME '2006-12-25 01:01'  
```  
  
 Ausgabe:  
  
|Wert|  
|----------|  
|12\/25\/2006 1:01:00 AM|  
  
### Ganze Zahl  
 Ganzzahlige Literale können vom Typ Int32 \(123\), UInt32 \(123U\), Int64 \(123L\) und UInt64 \(123UL\) sein.  
  
 Beispiel:  
  
```  
--a collection of integers  
{1, 2, 3}  
```  
  
 Ausgabe:  
  
|Wert|  
|----------|  
|1|  
|2|  
|3|  
  
### Andere  
 Andere von [!INCLUDE[esql](../../../../../../includes/esql-md.md)] unterstützte Literale sind **Guid**, **Binary**, **Float\/Double**, **Decimal** und `null`.  NULL\-Literale werden in [!INCLUDE[esql](../../../../../../includes/esql-md.md)] als mit allen anderen Typen im kozeptionellen Modell kompatibel aufgefasst.  
  
## Typkonstruktoren  
  
### ROW  
 [ROW](../../../../../../docs/framework/data/adonet/ef/language-reference/row-entity-sql.md) Erstellt einen anonymen, strukturell typisierten \(Datensatz\-\)Wert wie im folgenden Beispiel: `ROW(1 AS myNumber, ‘Name’ AS myName).`  
  
 Beispiel:  
  
```  
SELECT VALUE row (product.ProductID as ProductID, product.Name   
    as ProductName) FROM AdventureWorksEntities.Product AS product  
```  
  
 Ausgabe:  
  
|ProductID|Name|  
|---------------|----------|  
|1|Adjustable Race|  
|879|All\-Purpose Bike Stand|  
|712|AWC Logo Cap|  
|...|...|  
  
### MULTISET  
 [MULTISET](../../../../../../docs/framework/data/adonet/ef/language-reference/multiset-entity-sql.md) erstellt Auflistungen, z. B.:  
  
 `MULTISET(1,2,2,3)` `--same as`\-`{1,2,2,3}.`  
  
 Beispiel:  
  
```  
SELECT VALUE product FROM AdventureWorksEntities.Product AS product WHERE product.ListPrice IN MultiSet (125, 300)  
```  
  
 Ausgabe:  
  
|ProductID|Name|ProductNumber|…|  
|---------------|----------|-------------------|-------|  
|842|Touring\-Panniers, Large|PA\-T100|…|  
  
### Objekt  
 [Konstruktoren benannter Typen](../../../../../../docs/framework/data/adonet/ef/language-reference/named-type-constructor-entity-sql.md) erstellt \(benannte\) benutzerdefinierte Objekte, z. B. `person("abc", 12)`.  
  
 Beispiel:  
  
```  
SELECT VALUE AdventureWorksModel.SalesOrderDetail (o.SalesOrderDetailID, o.CarrierTrackingNumber, o.OrderQty,   
o.ProductID, o.SpecialOfferID, o.UnitPrice, o.UnitPriceDiscount,   
o.rowguid, o.ModifiedDate) FROM AdventureWorksEntities.SalesOrderDetail   
AS o  
```  
  
 Ausgabe:  
  
|SalesOrderDetailID|CarrierTrackingNumber|OrderQty|ProductID|...|  
|------------------------|---------------------------|--------------|---------------|---------|  
|1|4911\-403C\-98|1|776|...|  
|2|4911\-403C\-98|3|777|...|  
|...|...|...|...|...|  
  
## Verweise  
  
### REF  
 [REF](../../../../../../docs/framework/data/adonet/ef/language-reference/ref-entity-sql.md) erstellt einen Verweis auf eine Entitätstypinstanz.  Die folgende Abfrage gibt beispielsweise einen Verweis auf jede Order\-Entität in der Orders\-Entitätenmenge zurück:  
  
```  
SELECT REF(o) AS OrderID FROM Orders AS o  
```  
  
 Ausgabe:  
  
|Wert|  
|----------|  
|1|  
|2|  
|3|  
|...|  
  
 Im folgenden Beispiel wird der Eigenschaftsextrahierungsoperator \(.\) für den Zugriff auf eine Eigenschaft einer Entität verwendet.  Bei der Verwendung des Eigenschaftsextraktionsoperators wird der Verweis automatisch dereferenziert.  
  
 Beispiel:  
  
```  
SELECT VALUE REF(p).Name FROM   
    AdventureWorksEntities.Product as p  
```  
  
 Ausgabe:  
  
|Wert|  
|----------|  
|Adjustable Race|  
|All\-Purpose Bike Stand|  
|AWC Logo Cap|  
|...|  
  
### DEREF  
 [DEREF](../../../../../../docs/framework/data/adonet/ef/language-reference/deref-entity-sql.md) dereferenziert einen Verweiswert und erstellt das Ergebnis dieser Dereferenzierung.  Die folgende Abfrage erstellt beispielsweise die Order\-Entitäten für jede Bestellung in der Orders\-Entitätenmenge: `SELECT DEREF(o2.r) FROM (SELECT REF(o) AS r FROM LOB.Orders AS o) AS o2`.  
  
 Beispiel:  
  
```  
SELECT VALUE DEREF(REF(p)).Name FROM   
    AdventureWorksEntities.Product as p  
```  
  
 Ausgabe:  
  
|Wert|  
|----------|  
|Adjustable Race|  
|All\-Purpose Bike Stand|  
|AWC Logo Cap|  
|...|  
  
### CREATEREF UND KEY  
 [CREATEREF](../../../../../../docs/framework/data/adonet/ef/language-reference/createref-entity-sql.md) erstellt einen Verweis, der einen Schlüssel übergibt.  [KEY](../../../../../../docs/framework/data/adonet/ef/language-reference/key-entity-sql.md) extrahiert den Schlüsselteil eines Ausdrucks mit Typverweis.  
  
 Beispiel:  
  
```  
SELECT VALUE Key(CreateRef(AdventureWorksEntities.Product, row(p.ProductID)))   
    FROM AdventureWorksEntities.Product as p  
```  
  
 Ausgabe:  
  
|ProductID|  
|---------------|  
|980|  
|365|  
|771|  
|...|  
  
## Funktionen  
  
### Kanonische Funktionen  
 Der Namespace für [kanonische Funktionen](../../../../../../docs/framework/data/adonet/ef/language-reference/canonical-functions.md) ist Edm, wie in `Edm.Length("string")`.  Der Namespace muss nur dann angegeben werden, wenn ein anderer Namespace importiert wurde, der eine Funktion mit dem gleichen Namen wie eine kanonische Funktion enthält.  Wenn zwei Namespaces über die gleiche Funktion verfügen, sollte der Benutzer den vollständigen Namen angeben.  
  
 Beispiel:  
  
```  
SELECT Length(c. FirstName) As NameLen FROM   
    AdventureWorksEntities.Contact AS c   
    WHERE c.ContactID BETWEEN 10 AND 12  
```  
  
 Ausgabe:  
  
|NameLen|  
|-------------|  
|6|  
|6|  
|5|  
  
### Microsoft\-anbieterspezifische Funktionen  
 [Microsoft\-anbieterspezifische Funktionen](../../../../../../docs/framework/data/adonet/ef/sqlclient-for-ef-functions.md) befinden sich im `SqlServer`\-Namespace.  
  
 Beispiel:  
  
```  
SELECT SqlServer.LEN(c.EmailAddress) As EmailLen FROM   
    AdventureWorksEntities.Contact AS c WHERE   
    c.ContactID BETWEEN 10 AND 12  
```  
  
 Ausgabe:  
  
|EmailLen|  
|--------------|  
|27|  
|27|  
|26|  
  
## Namespaces  
 [USING](../../../../../../docs/framework/data/adonet/ef/language-reference/using-entity-sql.md) gibt in einem Abfrageausdruck verwendete Namespaces an.  
  
 Beispiel:  
  
```  
using SqlServer; LOWER('AA');  
```  
  
 Ausgabe:  
  
|Wert|  
|----------|  
|aa|  
  
## Paging  
 Paging kann durch Deklarieren einer [SKIP](../../../../../../docs/framework/data/adonet/ef/language-reference/skip-entity-sql.md)\-Unterklausel und einer [LIMIT](../../../../../../docs/framework/data/adonet/ef/language-reference/limit-entity-sql.md)\-Unterklausel in der [ORDER BY](../../../../../../docs/framework/data/adonet/ef/language-reference/order-by-entity-sql.md)\-Klausel erreicht werden.  
  
 Beispiel:  
  
```  
SELECT c.ContactID as ID, c.LastName as Name FROM   
    AdventureWorks.Contact AS c ORDER BY c.ContactID SKIP 9 LIMIT 3;  
```  
  
 Ausgabe:  
  
|ID|Name|  
|--------|----------|  
|10|Adina|  
|11|Agcaoili|  
|12|Aguilar|  
  
## Gruppierung  
 [GROUPING BY](../../../../../../docs/framework/data/adonet/ef/language-reference/group-by-entity-sql.md) gibt Gruppen an, in die von einem Abfrageausdruck \([SELECT](../../../../../../docs/framework/data/adonet/ef/language-reference/select-entity-sql.md)\) zurückgegebene Objekte platziert werden sollen.  
  
 Beispiel:  
  
```  
SELECT VALUE name FROM AdventureWorksEntities.Product as P   
    GROUP BY P.Name HAVING MAX(P.ListPrice) > 5  
```  
  
 Ausgabe:  
  
|Name|  
|----------|  
|LL Mountain Seat Assembly|  
|ML Mountain Seat Assembly|  
|HL Mountain Seat Assembly|  
|...|  
  
## Navigation  
 Der Beziehungsnavigationsoperator ermöglicht die Navigation der Beziehung von einer Entität \(an einem Ende\) zu einer anderen Entität \(am anderen Ende\).  [NAVIGATE](../../../../../../docs/framework/data/adonet/ef/language-reference/navigate-entity-sql.md) wird dem Beziehungstyp als \<Namespace\>.\<Beziehungstypname\> qualifiziert übergeben.  Navigate gibt Ref\<T\> zurück, wenn die Kardinalität des anderen Endes "1" ist.  Wenn die Kardinalität des "to"\-Endes "n" beträgt, ist der Rückgabewert Collection\<Ref\<T\>\>.  
  
 Beispiel:  
  
```  
SELECT a.AddressID, (SELECT VALUE DEREF(v) FROM   
    NAVIGATE(a, AdventureWorksModel.FK_SalesOrderHeader_Address_BillToAddressID) AS v)   
    FROM AdventureWorksEntities.Address AS a  
```  
  
 Ausgabe:  
  
|AddressID|  
|---------------|  
|1|  
|2|  
|3|  
|...|  
  
## SELECT VALUE UND SELECT  
  
### SELECT VALUE  
 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] stellt die **SELECT VALUE**\-Klausel bereit, um die implizite Zeilenkonstruktion zu überspringen.  In einer **SELECT VALUE**\-Klausel kann nur ein Element angegeben werden.  Wenn diese Klausel verwendet wird, wird kein Zeilen\-Wrapper für die Elemente in der **SELECT**\-Klausel erstellt, und eine Auflistung der gewünschten Form kann erstellt werden, beispielsweise `SELECT VALUE a`.  
  
 Beispiel:  
  
```  
SELECT VALUE p.Name FROM AdventureWorksEntities.Product as p  
```  
  
 Ausgabe:  
  
|Name|  
|----------|  
|Adjustable Race|  
|All\-Purpose Bike Stand|  
|AWC Logo Cap|  
|...|  
  
### SELECT  
 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] stellt auch den Zeilenkonstruktor zum Erstellen beliebiger Zeilen bereit.  **SELECT** werden ein oder mehrere Elemente in der Projektion übergeben, und das Ergebnis ist ein Datensatz mit Feldern, z. B.: `SELECT a, b, c`.  
  
 Beispiel:  
  
 SELECT p.Name, p.ProductID FROM AdventureWorksEntities.Product as p Ausgabe:  
  
|Name|ProductID|  
|----------|---------------|  
|Adjustable Race|1|  
|All\-Purpose Bike Stand|879|  
|AWC Logo Cap|712|  
|...|...|  
  
## CASE EXPRESSION  
 [case expression](../../../../../../docs/framework/data/adonet/ef/language-reference/case-entity-sql.md) wertet eine Reihe von booleschen Ausdrücken aus, um das Ergebnis zu ermitteln.  
  
 Beispiel:  
  
```  
CASE WHEN AVG({25,12,11}) < 100 THEN TRUE ELSE FALSE END  
```  
  
 Ausgabe:  
  
|Wert|  
|----------|  
|TRUE|  
  
## Siehe auch  
 [Entity SQL\-Referenz](../../../../../../docs/framework/data/adonet/ef/language-reference/entity-sql-reference.md)   
 [Übersicht über Entity SQL](../../../../../../docs/framework/data/adonet/ef/language-reference/entity-sql-overview.md)