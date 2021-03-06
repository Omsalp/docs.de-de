---
title: "Mathematische Funktionen | Microsoft Docs"
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
ms.assetid: b040c7cb-156d-40f2-9152-61065b18148c
caps.latest.revision: 2
author: "JennieHubbard"
ms.author: "jhubbard"
manager: "jhubbard"
caps.handback.revision: 2
---
# Mathematische Funktionen
Der .NET Framework\-Datenanbieter für SQL Server \(SqlClient\) stellt mathematische Funktionen bereit, die Berechnungen für als Argumente bereitgestellte Eingabewerte durchführen und einen numerischen Wert als Ergebnis zurückgeben.  Diese Funktionen befinden sich im SQLServer\-Namespace, der bei der Verwendung von SqlClient verfügbar ist.  Mit der Namespace\-Eigenschaft eines Anbieters kann Entity Framework ermitteln, welches Präfix von diesem Anbieter für bestimmte Konstrukte verwendet wird, z. B. Typen und Funktionen. In der folgenden Tabelle werden die mathematischen Funktionen von SqlClient beschrieben.  
  
|Funktion|Beschreibung|  
|--------------|------------------|  
|`ABS(` `expression` `)`|Führt die Absolutwertfunktion aus.<br /><br /> **Argumente**<br /><br /> `expression`: `Int32`, `Int64`, `Double` oder `Decimal`.<br /><br /> **Rückgabewert**<br /><br /> Der Absolutwert des angegebenen Ausdrucks.<br /><br /> **Beispiel**<br /><br /> `SqlServer.ABS(-2)`|  
|`ACOS(` `expression` `)`|Gibt den Arkuskosinuswert des angegebenen Ausdrucks zurück.<br /><br /> **Argumente**<br /><br /> `expression`: `Double`<br /><br /> **Rückgabewert**<br /><br /> Ein `Double`<br /><br /> **Beispiel**<br /><br /> `SqlServer.ACOS(.9)`|  
|`ASIN(` `expression` `)`|Gibt den Arkussinuswert des angegebenen Ausdrucks zurück.<br /><br /> **Argumente**<br /><br /> `expression`: `Double`<br /><br /> **Rückgabewert**<br /><br /> Ein `Double`<br /><br /> **Beispiel**<br /><br /> `SqlServer.ASIN(.9)`|  
|`ATAN(` `expression` `)`|Gibt den Arkustangens\-Wert des angegebenen numerischen Ausdrucks zurück.<br /><br /> **Argumente**<br /><br /> `expression`: `Double`<br /><br /> **Rückgabewert**<br /><br /> Ein `Double`<br /><br /> **Beispiel**<br /><br /> `SqlServer.ATAN(9)`|  
|`ATN2(` `expression`, `expression``)`|Gibt den Winkel im Bogenmaß \(Radiant\) zurück, dessen Tangens zwischen den beiden angegebenen numerischen Ausdrücken liegt.<br /><br /> **Argumente**<br /><br /> `expression`: `Double`<br /><br /> **Rückgabewert**<br /><br /> Ein `Double`<br /><br /> **Beispiel**<br /><br /> `SqlServer.ATN2(9, 8)`|  
|`CEILING(` `expression` `)`|Konvertiert den angegebenen Ausdruck zur kleinsten Ganzzahl, die größer als oder gleich dem Ausdruck ist.<br /><br /> **Argumente**<br /><br /> `expression`: `Int32`, `Int64`, `Double` oder `Decimal`.<br /><br /> **Rückgabewert**<br /><br /> `Int32`, `Int64`, `Double` oder `Decimal`.<br /><br /> **Beispiel**<br /><br /> [!code-csharp[DP EntityServices Concepts#SQLSERVER_CEILING](../../../../../samples/snippets/csharp/VS_Snippets_Data/dp entityservices concepts/cs/entitysql.cs#sqlserver_ceiling)]
 [!code-sql[DP EntityServices Concepts#SQLSERVER_CEILING](../../../../../samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#sqlserver_ceiling)]|  
|`COS(` `expression` `)`|Berechnet den trigonometrischen Kosinus des im Bogenmaß angegebenen Winkels.<br /><br /> **Argumente**<br /><br /> `expression`: `Double`<br /><br /> **Rückgabewert**<br /><br /> Ein `Double`<br /><br /> **Beispiel**<br /><br /> `SqlServer.COS(45)`|  
|`COT(` `expression` `)`|Berechnet den trigonometrischen Kotangens des im Bogenmaß angegebenen Winkels.<br /><br /> **Argumente**<br /><br /> `expression`: `Double`<br /><br /> **Rückgabewert**<br /><br /> Ein `Double`<br /><br /> **Beispiel**<br /><br /> `SqlServer.COT(60)`|  
|`DEGREES(` `radians` `)`|Gibt den entsprechenden Winkel in Grad zurück.<br /><br /> **Argumente**<br /><br /> `expression`: `Int32`, `Int64`, `Double` oder `Decimal`.<br /><br /> **Rückgabewert**<br /><br /> `Int32`, `Int64`, `Double` oder `Decimal`.<br /><br /> **Beispiel**<br /><br /> `SqlServer.DEGREES(3.1)`|  
|`EXP(` `expression` `)`|Berechnet den Exponentialwert des angegebenen numerischen Ausdrucks.<br /><br /> **Argumente**<br /><br /> `expression`: `Double`<br /><br /> **Rückgabewert**<br /><br /> Ein `Double`<br /><br /> **Beispiel**<br /><br /> `SqlServer.EXP(1)`|  
|`FLOOR(` `expression` `)`|Konvertiert den angegebenen Ausdruck zur größten Ganzzahl, die kleiner als oder gleich dem angegebenen numerischen Ausdruck ist.<br /><br /> **Argumente**<br /><br /> `expression`: `Double`<br /><br /> **Rückgabewert**<br /><br /> Ein `Double`<br /><br /> **Beispiel**<br /><br /> [!code-csharp[DP EntityServices Concepts#SQLSERVER_FLOOR](../../../../../samples/snippets/csharp/VS_Snippets_Data/dp entityservices concepts/cs/entitysql.cs#sqlserver_floor)]
 [!code-sql[DP EntityServices Concepts#SQLSERVER_FLOOR](../../../../../samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#sqlserver_floor)]|  
|`LOG(` `expression` `)`|Berechnet den natürlichen Logarithmus des angegebenen `float`\-Ausdrucks.<br /><br /> **Argumente**<br /><br /> `expression`: `Double`<br /><br /> **Rückgabewert**<br /><br /> Ein `Double`<br /><br /> **Beispiel**<br /><br /> `SqlServer.LOG(100)`|  
|`LOG10(` `expression` `)`|Gibt den Logarithmus zur Basis 10 des angegebenen `Double`\-Ausdrucks zurück.<br /><br /> **Argumente**<br /><br /> `expression`: `Double`<br /><br /> **Rückgabewert**<br /><br /> Ein `Double`<br /><br /> **Beispiel**<br /><br /> `SqlServer.LOG10(100)`|  
|`PI()`|Gibt den konstanten Wert von Pi als `Double`\-Typ zurück.<br /><br /> **Rückgabewert**<br /><br /> Ein `Double`<br /><br /> **Beispiel**<br /><br /> `SqlServer.PI()`|  
|`POWER(` `numeric_expression, power_expression` `)`|Berechnet den Wert eines angegebenen Ausdrucks in einer angegebenen Potenz.<br /><br /> **Argumente**<br /><br /> `numeric_expression`:  `Int32`, `Int64`, `Double` oder `Decimal`.<br /><br /> `power_expression`: Ein `Double`\-Wert, der die Potenz darstellt, die auf den `numeric_expression` angewendet wird.<br /><br /> **Rückgabewert**<br /><br /> Der Wert des angegebenen `numeric_expression` zur angegebenen `power_expression`.<br /><br /> **Beispiel**<br /><br /> `SqlServer.POWER(2,7)`|  
|`RADIANS(` `expression` `)`|Konvertiert Grad\- in Radiantwerte.<br /><br /> **Argumente**<br /><br /> `expression`: `Int32`, `Int64`, `Double` oder `Decimal`.<br /><br /> **Rückgabewert**<br /><br /> `Int32`, `Int64`,<br /><br /> `Double` oder<br /><br /> `Decimal`.<br /><br /> **Beispiel**<br /><br /> `SqlServer.RADIANS(360.0)`|  
|`RAND(`\[seed\]`)`|Gibt einen Zufallswert zwischen 0 und 1 zurück.<br /><br /> **Argumente**<br /><br /> Gibt den seed\-Wert als `Int32` zurück.  Ist der seed\-Wert nicht angegeben, fügt das Datenbankmodul von SQL Server einen Zufallsstartwert hinzu.  Für einen angegebenen Startwert wird immer dasselbe Ergebnis zurückgegeben.<br /><br /> **Rückgabewert**<br /><br /> Ein zufälliger `Double`\-Wert zwischen 0 \(null\) und 1.<br /><br /> **Beispiel**<br /><br /> `SqlServer.RAND()`|  
|`ROUND(` `numeric_expression, length` \[ ,`function` \]`)`|Gibt einen numerischen Ausdruck zurück, der auf die angegebene Länge oder Genauigkeit gerundet wurde.<br /><br /> **Argumente**<br /><br /> `numeric_expression`:  `Int32`, `Int64`, `Double` oder `Decimal`.<br /><br /> `length`: Ein `Int32`\-Wert, der die Rundungsgenauigkeit für `numeric_expression` angibt.  Wenn `length` eine positive Zahl ist, wird `numeric_expression` auf die Anzahl der mit `length` angegebenen Dezimalstellen gerundet.  Wenn `length` eine negative Zahl ist, wird `numeric_expression` auf der linken Seite des Dezimaltrennzeichens gemäß der Angabe von `length` gerundet.<br /><br /> `function`: `` \(optional\) Ein `Int32`\-Wert, der den auszuführenden Vorgangstyp darstellt.  Wird die Funktion ausgelassen oder weist sie einen Wert von 0 \(Standard\) auf, wird `numeric_expression` gerundet.  Wird nicht der Wert 0 angegeben, wird `numeric_expression` abgeschnitten.<br /><br /> **Rückgabewert**<br /><br /> Der Wert des angegebenen `numeric_expression` zur angegebenen `power_expression`.<br /><br /> **Beispiel**<br /><br /> `SqlServer.ROUND(748.58, -3)`|  
|`SIGN(` `expression` `)`|Gibt das positive \(\+1\) oder negative Vorzeichen \(\-1\) oder das Vorzeichen 0 \(null\) des angegebenen Ausdrucks zurück.<br /><br /> **Argumente**<br /><br /> `expression`: `Int32`, `Int64`, `Double` oder `Decimal`<br /><br /> **Rückgabewert**<br /><br /> `Int32`, `Int64`, `Double` oder `Decimal`.<br /><br /> **Beispiel**<br /><br /> `SqlServer.SIGN(-10)`|  
|`SIN(` `expression` `)`|Berechnet den trigonometrischen Sinus des angegebenen Winkels im Bogenmaß, und gibt einen `Double`\-Ausdruck zurück.<br /><br /> **Argumente**<br /><br /> `expression`: `Double`<br /><br /> **Rückgabewert**<br /><br /> Ein `Double`<br /><br /> **Beispiel**<br /><br /> `SqlServer.SIN(20)`|  
|`SQRT(` `expression` `)`|Gibt die Quadratwurzel des angegebenen Ausdrucks zurück.<br /><br /> **Argumente**<br /><br /> `expression`: `Double`<br /><br /> **Rückgabewert**<br /><br /> Ein `Double`<br /><br /> **Beispiel**<br /><br /> `SqlServer.SQRT(3600)`|  
|`SQUARE(` `expression` `)`|Gibt den quadratischen Wert des angegebenen Ausdrucks zurück.<br /><br /> **Argumente**<br /><br /> `expression`: `Double`<br /><br /> **Rückgabewert**<br /><br /> Ein `Double`<br /><br /> **Beispiel**<br /><br /> `SqlServer.SQUARE(25)`|  
|`TAN(` `expression` `)`|Berechnet den Tangens eines angegebenen Ausdrucks.<br /><br /> **Argumente**<br /><br /> `expression`: `Double`<br /><br /> **Rückgabewert**<br /><br /> `Double`<br /><br /> **Beispiel**<br /><br /> `SqlServer.TAN(45.0)`|  
  
 Weitere Informationen zu den von SqlClient unterstützten mathematischen Funktionen finden Sie in der Dokumentation für die SQL Server\-Version, die im SqlClient\-Anbietermanifest angegeben wurde:  
  
|SQL Server 2000|SQL Server 2005|SQL Server 2008|  
|---------------------|---------------------|---------------------|  
|[Mathematische Funktionen \(Transact\-SQL\)](http://go.microsoft.com/fwlink/?LinkId=115913)|[Mathematische Funktionen \(Transact\-SQL\)](http://go.microsoft.com/fwlink/?LinkId=115911)|[Mathematische Funktionen \(Transact\-SQL\)](http://go.microsoft.com/fwlink/?LinkId=115912)|  
  
## Siehe auch  
 [SqlClient für Entity Framework\-Funktionen](../../../../../docs/framework/data/adonet/ef/sqlclient-for-ef-functions.md)