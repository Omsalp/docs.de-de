---
title: "Gew&#228;hren zeilenspezifischer Berechtigungen in SQL Server | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-ado"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: a55aaa12-34ab-41cd-9dec-fd255b29258c
caps.latest.revision: 5
author: "JennieHubbard"
ms.author: "jhubbard"
manager: "jhubbard"
caps.handback.revision: 5
---
# Gew&#228;hren zeilenspezifischer Berechtigungen in SQL Server
Es gibt Szenarien, in denen der Zugriff auf Daten genauer gesteuert werden muss, als dies durch einfaches Gewähren, Widerrufen oder Ablehnen von Berechtigungen möglich ist. Eine Anwendung für eine Krankenhausdatenbank kann z. B. für einzelne Ärzte erfordern, dass diese jeweils nur auf Daten der eigenen Patienten zugreifen dürfen. Ähnliche Anforderungen gibt es in vielen Umgebungen, vom Finanzwesen über die Justiz und die Behörden bis hin zum militärischen Bereich. SQL Server 2016 bietet ein Feature für die [zeilenbasierte Sicherheit](https://msdn.microsoft.com/library/dn765131.aspx), die die zeilenbasierte Zugriffslogik in einer Sicherheitsrichtlinie vereinfacht und zentralisiert, um diese Szenarien besser zu unterstützen. Frühere Versionen von SQL Server können mithilfe von Ansichten eine ähnliche Funktionalität erreichen, um die Filterung auf Zeilenebene zu ermöglichen.  
  
## Implementieren der zeilenbasierter Filterung  
 Die zeilenbasierte Filterung wird für Anwendungen verwendet, die Informationen in einer einzelnen Tabelle wie im obigen Krankenhausbeispiel speichern. Um die zeilenbasierte Filterung zu implementieren, besitzt jede Zeile eine Spalte, die ein Unterscheidungsmerkmal \(Differenzierungsparameter\) definiert, z. B. den Benutzernamen, die Menge oder ein anderes Identifizierungsmerkmal. Sie erstellen entweder eine Sicherheitsrichtlinie oder eine Sicht auf die Tabelle, die die Zeilen filtert, auf die der Benutzer zugreifen kann. Sie können dann parametrisierte gespeicherte Prozeduren erstellen, die die Typen von Abfragen steuern, die vom Benutzer ausgeführt werden können.  
  
 Das folgende Beispiel beschreibt, wie die zeilenbasierte Filterung auf der Grundlage eines Benutzer\- oder Anmeldenamens konfiguriert werden kann.  
  
-   Erstellen Sie die Tabelle, und fügen Sie eine Spalte zum Speichern des Namens hinzu.  
  
-   Aktivieren der zeilenbasierten Filterung:  
  
    -   Bei Verwendung von SQL Server 2016 oder höher oder [Azure SQL\-Datenbank](https://azure.microsoft.com/documentation/services/sql-database/) erstellen Sie eine Sicherheitsrichtlinie, die der Tabelle ein Prädikat hinzufügt, das die Zeilen auf diejenigen einschränkt, die entweder mit dem aktuellen Datenbankbenutzer \(mithilfe der integrierten CURRENT\_USER\(\)\-Funktion\) oder mit dem aktuellen Anmeldenamen \(mithilfe der integrierten SUSER\_SNAME\(\)\-Funktion\) übereinstimmen:  
  
        ```tsql  
        CREATE SCHEMA Security GO CREATE FUNCTION Security.userAccessPredicate(@UserName sysname) RETURNS TABLE WITH SCHEMABINDING AS RETURN SELECT 1 AS accessResult WHERE @UserName = SUSER_SNAME() GO CREATE SECURITY POLICY Security.userAccessPolicy ADD FILTER PREDICATE Security.userAccessPredicate(UserName) ON dbo.MyTable, ADD BLOCK PREDICATE Security.userAccessPredicate(UserName) ON dbo.MyTable GO  
        ```  
  
    -   Wenn Sie eine Vorgängerversion von SQL Server 2016 verwenden, können Sie eine ähnliche Funktionalität mithilfe einer Sicht erzielen:  
  
        ```tsql  
        CREATE VIEW vw_MyTable AS RETURN SELECT * FROM MyTable WHERE UserName = SUSER_SNAME() GO  
  
        ```  
  
-   Erstellen Sie gespeicherte Prozeduren, um Daten auszuwählen, einzufügen, zu aktualisieren und zu löschen. Wenn die Filterung mithilfe einer Sicherheitsrichtlinie durchgeführt wird, sollten die gespeicherten Prozeduren diese Vorgänge direkt mit der Basistabelle durchführen. Wenn die Filterung mithilfe einer Sicht durchgeführt wird, sollten die gespeicherten Prozeduren stattdessen mit der Sicht arbeiten. Die Sicherheitsrichtlinie oder Sicht filtert die durch Benutzerabfragen zurückgegebenen oder geänderten Zeilen automatisch, und die gespeicherte Prozedur bietet eine stärkere Sicherheitsgrenze, um zu verhindern, dass Benutzer mit direktem Zugriff auf die Abfragen erfolgreich Abfragen ausführen, die die Existenz gefilterter Daten ableiten können.  
  
-   Erfassen Sie bei gespeicherten Prozeduren, die Daten einfügen, den Benutzernamen mit der in der Sicherheitsrichtlinie oder Sicht angegebenen Funktion, und fügen Sie diesen Wert in die Spalte „UserName“ ein.  
  
-   Verweigern Sie der Rolle `public` alle Berechtigungen für die Tabellen \(und Sichten, falls zutreffend\). Die Benutzer können keine Berechtigungen aus anderen Datenbankrollen erben, weil das Filterprädikat nicht auf Rollen, sondern auf Benutzer\- oder Anmeldenamen basiert.  
  
-   Gewähren Sie den Datenbankrollen die Berechtigung EXECUTE für die gespeicherten Prozeduren. Die Benutzer können nur auf Daten zugreifen, die über die gespeicherten Prozeduren bereitgestellt werden.  
  
## Externe Ressourcen  
 Weitere Informationen finden Sie in der folgenden Ressource:  
  
|||  
|-|-|  
|[Implementieren zeilen\- und zellenbasierter Sicherheit in klassifizierten Datenbanken unter Verwendung von SQL Server 2005](http://go.microsoft.com/fwlink/?LinkId=98227) auf der SQL Server TechCenter\-Website.|Beschreibt, wie Sie mithilfe von zeilen\- und zellenbasierter Sicherheit die Sicherheitsanforderungen für klassifizierte Datenbanken erfüllen können.|  
  
## Siehe auch  
 [Zeilenbasierte Sicherheit](https://msdn.microsoft.com/library/dn765131.aspx)   
 [Sichern von ADO.NET\-Anwendungen](../../../../../docs/framework/data/adonet/securing-ado-net-applications.md)   
 [Übersicht über die SQL Server\-Sicherheit](../../../../../docs/framework/data/adonet/sql/overview-of-sql-server-security.md)   
 [Anwendungssicherheitsszenarios in SQL Server](../../../../../docs/framework/data/adonet/sql/application-security-scenarios-in-sql-server.md)   
 [Verwalten von Berechtigungen mit gespeicherten Prozeduren in SQL Server](../../../../../docs/framework/data/adonet/sql/managing-permissions-with-stored-procedures-in-sql-server.md)   
 [Schreiben von sicherem dynamischen SQL in SQL Server](../../../../../docs/framework/data/adonet/sql/writing-secure-dynamic-sql-in-sql-server.md)   
 [ADO.NET Managed Provider und DataSet Developer Center](http://go.microsoft.com/fwlink/?LinkId=217917)