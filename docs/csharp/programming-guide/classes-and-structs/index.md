---
title: Klassen und Strukturen (C#-Programmierhandbuch)
description: Beschreibt die Verwendung von Klassen und Strukturen (Strukturen) in C#.
keywords: Klassen (C#), Strukturen (C#), Verweistypen (C#), Werttypen (C#)
ms.date: 2016-01-17
ms.prod: .net
ms.technology:
- devlang-csharp
ms.topic: article
dev_langs:
- CSharp
helpviewer_keywords:
- structs [C#], about structs
- classes [C#], overview
- C# language, structs
- C# language, objects
- objects [C#]
- C# language, classes
ms.assetid: cc39dbda-8754-423e-b5b1-16a1db0734c0
caps.latest.revision: 48
author: BillWagner
ms.author: wiwagn
translation.priority.ht:
- cs-cz
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pl-pl
- pt-br
- ru-ru
- tr-tr
- zh-cn
- zh-tw
ms.translationtype: HT
ms.sourcegitcommit: 306c608dc7f97594ef6f72ae0f5aaba596c936e1
ms.openlocfilehash: 2076de96c58fb89813495cb59adacb8a0082d93c
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---
# <a name="classes-and-structs-c-programming-guide"></a>Klassen und Strukturen (C#-Programmierhandbuch)
Klassen und Strukturen sind zwei der grundlegenden Konstrukte des allgemeinen Typsystems in .NET Framework. Bei beiden handelt es sich um eine Datenstruktur, die einen als logische Einheit zusammengehörenden Satz von Daten und Verhalten kapselt. Die Daten und die Verhalten sind die *Member* der Klasse oder Struktur. Diese enthalten deren Methoden, Eigenschaften, Ereignisse usw., die weiter unten in diesem Thema aufgeführt werden.  
  
 Die Deklaration einer Klasse oder Struktur ist mit einem Entwurf vergleichbar, mit dem zur Laufzeit Instanzen oder Objekte erstellt werden. Wenn Sie eine Klasse oder eine Struktur mit dem Namen `Person` definieren, hat der Typ den Namen `Person`. Wenn Sie eine Variable `p` vom Typ `Person` deklarieren und initialisieren, wird `p` als Objekt oder Instanz von `Person` bezeichnet. Vom selben Typ `Person` können mehrere Instanzen erstellt werden, und jede Instanz kann über unterschiedliche Werte in ihren Eigenschaften und Feldern verfügen.  
  
 Eine Klasse ist ein Verweistyp. Wenn ein Objekt der Klasse erstellt wird, enthält die Variable, der das Objekt zugewiesen wurde, lediglich einen Verweis auf den entsprechenden Speicherort. Wenn der Objektverweis einer neuen Variablen zugewiesen wird, verweist die neue Variable auf das ursprüngliche Objekt. Über eine Variable vorgenommene Änderungen gelten auch für die andere Variable, da beide auf dieselben Daten verweisen.  
  
 Eine Struktur ist ein Werttyp. Wenn eine Struktur erstellt wird, enthält die Variable, der die Struktur zugewiesen wird, die eigentlichen Daten der Struktur. Wenn die Struktur einer neuen Variable zugewiesen wird, werden diese kopiert. Die neue Variable und die ursprüngliche Variable enthalten daher zwei separate Kopien der gleichen Daten. Änderungen an einer Kopie wirken sich nicht auf die andere Kopie aus.  
  
 Im Allgemeinen werden Klassen zum Modellieren von komplexerem Verhalten oder von Daten verwendet, die dafür vorgesehen sind, nach der Erstellung eines Klassenobjekts geändert zu werden. Strukturen sind am besten für kleine Datenstrukturen geeignet, die überwiegend Daten enthalten, deren Änderung nach der Erstellung der Struktur nicht vorgesehen ist.  
  
 Weitere Informationen finden Sie unter [Klassen](../../../csharp/programming-guide/classes-and-structs/classes.md), [Objekte](../../../csharp/programming-guide/classes-and-structs/objects.md) und [Strukturen](../../../csharp/programming-guide/classes-and-structs/structs.md).  
  
## <a name="example"></a>Beispiel  
 Im folgenden Beispiel `CustomClass` in `ProgrammingGuide` weist der Namespace drei Member auf: einen Instanzkonstruktor, eine Eigenschaft mit der Bezeichnung `Number` und eine Methode mit der Bezeichnung `Multiply`. Die `Main`-Methode in der `Program`-Klasse erstellt eine Instanz (Objekt) von `CustomClass`. Auf die Methoden und Eigenschaften des Objekts wird mit der Punktnotation zugegriffen.
  
 [!code-cs[csProgGuideObjects#1](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/class1.cs#1)]  
  
## <a name="encapsulation"></a>Kapselung  
 *Kapselung* wird gelegentlich als erster Pfeiler oder als Prinzip der objektorientierten Programmierung bezeichnet. Dem Prinzip der Kapselung zufolge kann eine Klasse oder Struktur festlegen, inwieweit Code außerhalb der Klasse oder Struktur auf deren Member zugreifen kann. Nicht für die Verwendung von außerhalb der Klasse oder Assembly vorgesehene Methoden und Variablen können versteckt werden, um die Wahrscheinlichkeit von Programmierfehlern und böswilligen Angriffen zu verringern.  
  
 Weitere Informationen zu Klassen und Klassenmembern finden Sie unter [Klassen](../../../csharp/programming-guide/classes-and-structs/classes.md) und [Objekte](../../../csharp/programming-guide/classes-and-structs/objects.md).  
  
### <a name="members"></a>Mitglieder  
 Alle Methoden, Felder, Konstanten, Eigenschaften und Ereignisse müssen innerhalb eines Typs deklariert werden. Diese Elemente werden als *Member* des Typs bezeichnet. In C# gibt es im Gegensatz zu einigen anderen Sprachen keine globalen Variablen oder Methoden. Selbst der Einstiegspunkt eines Programms, die `Main`-Methode, muss innerhalb einer Klasse oder einer Struktur deklariert werden. In der folgenden Liste werden sämtliche Arten von Membern aufgeführt, die in einer Klasse bzw. einer Struktur deklariert werden können.  
  
-   [Felder](../../../csharp/programming-guide/classes-and-structs/fields.md)  
  
-   [Konstanten](../../../csharp/programming-guide/classes-and-structs/constants.md)  
  
-   [Eigenschaften](../../../csharp/programming-guide/classes-and-structs/properties.md)  
  
-   [Methoden](../../../csharp/programming-guide/classes-and-structs/methods.md)  
  
-   [Konstruktoren](../../../csharp/programming-guide/classes-and-structs/constructors.md)  
  
-   [Ereignisse](../../../csharp/programming-guide/events/index.md)  
  
-   [Finalizer](../../../csharp/programming-guide/classes-and-structs/destructors.md)  
  
-   [Indexer](../../../csharp/programming-guide/indexers/index.md)  
  
-   [Operatoren](../../../csharp/programming-guide/statements-expressions-operators/operators.md)  
  
-   [Geschachtelte Typen](../../../csharp/programming-guide/classes-and-structs/nested-types.md)  
  
### <a name="accessibility"></a>Barrierefreiheit  
 Einige Methoden und Eigenschaften sind für den Aufruf oder Zugriff von als *Clientcode* bezeichnetem Code außerhalb der Klasse oder Struktur vorgesehen. Andere Methoden und Eigenschaften dienen nur der Verwendung in der Klasse oder Struktur selbst. Es ist wichtig, den Zugriff auf den Code einzuschränken, damit nur der Clientcode darauf zugreifen kann, der dafür vorgesehen ist. Inwieweit Clientcode auf die Typen und deren Member zugreifen kann, können Sie mit den Zugriffsmodifizierern [public](../../../csharp/language-reference/keywords/public.md), [protected](../../../csharp/language-reference/keywords/protected.md), [internal](../../../csharp/language-reference/keywords/internal.md) `protected internal` und [private](../../../csharp/language-reference/keywords/private.md) festlegen. Die Standardeinstellung für den Zugriff lautet `private`. Weitere Informationen finden Sie unter [Zugriffsmodifizierer](../../../csharp/programming-guide/classes-and-structs/access-modifiers.md).  
  
### <a name="inheritance"></a>Vererbung  
 Klassen (jedoch nicht Strukturen) unterstützen das Konzept der Vererbung. Eine Klasse, die von einer anderen Klasse (der *Basisklasse*) abgeleitet ist, enthält automatisch alle öffentlichen, geschützten und internen Member der Basisklasse mit Ausnahme der Konstruktoren und Finalizer. Weitere Informationen finden Sie unter [Vererbung](../../../csharp/programming-guide/classes-and-structs/inheritance.md) und [Polymorphie](../../../csharp/programming-guide/classes-and-structs/polymorphism.md).  
  
 Klassen können als [abstrakt](../../../csharp/language-reference/keywords/abstract.md) deklariert werden. Das bedeutet, dass mindestens eine ihrer Methoden nicht implementiert ist. Obwohl abstrakte Klassen nicht direkt instanziiert werden können, können Sie als Basisklassen für andere Klassen dienen, von denen die fehlende Implementierung bereitgestellt wird. Klassen können auch als [versiegelt](../../../csharp/language-reference/keywords/sealed.md) deklariert werden, um zu verhindern, dass andere Klassen von ihnen erben. Weitere Informationen zu abstrakten Klassen finden Sie unter [Abstrakte und versiegelte Klassen und Klassenmember](../../../csharp/programming-guide/classes-and-structs/abstract-and-sealed-classes-and-class-members.md).  
  
### <a name="interfaces"></a>Schnittstellen  
 Klassen und Strukturen können von mehreren Schnittstellen erben. Von einer Schnittstelle erben bedeutet, dass der Typ alle in der Schnittstelle definierten Methoden implementiert. Weitere Informationen finden Sie unter [Schnittstellen](../../../csharp/programming-guide/interfaces/index.md).  
  
### <a name="generic-types"></a>Generische Typen  
 Klassen und Strukturen können mit einem oder mehreren Typparametern definiert werden. Der Typ wird beim Erstellen einer Instanz des Typs vom Clientcode bereitgestellt. Beispielsweise ist die <xref:System.Collections.Generic.List%601>-Klasse im <xref:System.Collections.Generic>-Namespace mit einem Typparameter definiert. Vom Clientcode wird eine Instanz von `List<string>` oder `List<int>` erstellt, um den Typ anzugeben, den die Liste enthalten soll. Weitere Informationen finden Sie unter [Generika](../../../csharp/programming-guide/generics/index.md).  
  
### <a name="static-types"></a>Statische Typen  
 Klassen (nicht jedoch Strukturen) können als [statisch](../../../csharp/language-reference/keywords/static.md) deklariert werden. Eine statische Klasse kann nur statische Member enthalten und nicht mit dem Schlüsselwort "new" instanziiert werden. Beim Laden des Programms wird eine Kopie der Klasse in den Speicher geladen. Auf deren Member wird über den Klassennamen zugegriffen. Sowohl Klassen als auch Strukturen können statische Member enthalten. Weitere Informationen finden Sie unter [Statische Klassen und statische Klassenmember](../../../csharp/programming-guide/classes-and-structs/static-classes-and-static-class-members.md).  
  
### <a name="nested-types"></a>Geschachtelte Typen  
 Eine Klasse oder Struktur kann innerhalb einer anderen Klasse oder Struktur geschachtelt werden. Weitere Informationen finden Sie unter [Geschachtelte Typen](../../../csharp/programming-guide/classes-and-structs/nested-types.md).  
  
### <a name="partial-types"></a>Partielle Typen  
 Sie können einen Teil einer Klasse, Struktur oder Methode in einer Codedatei und einen anderen Teil in einer separaten Codedatei definieren. Weitere Informationen finden Sie unter [Partielle Klassen und Methoden](../../../csharp/programming-guide/classes-and-structs/partial-classes-and-methods.md).  
  
### <a name="object-initializers"></a>Objektinitialisierer  
 Klassen- oder Strukturobjekte sowie Auflistungen von Objekten können instanziiert und initialisiert werden, ohne deren Konstruktor explizit aufzurufen. Weitere Informationen finden Sie unter [Objekt- und Auflistungsinitialisierer](../../../csharp/programming-guide/classes-and-structs/object-and-collection-initializers.md).  
  
### <a name="anonymous-types"></a>Anonyme Typen  
 Anonyme Typen werden in Fällen verwendet, in denen benannte Klassen nicht ohne Weiteres erstellt werden können bzw. nicht benötigt werden, beispielsweise beim Füllen einer Liste mit Datenstrukturen, die nicht erhalten bleiben oder an andere Methoden übergeben werden müssen. Weitere Informationen finden Sie unter [Anonyme Typen](../../../csharp/programming-guide/classes-and-structs/anonymous-types.md).  
  
### <a name="extension-methods"></a>Erweiterungsmethoden  
 Sie können Klassen "erweitern", ohne eine abgeleitete Klasse zu erstellen, indem Sie einen separaten Typ erstellen, dessen Methoden aufgerufen werden können, als würden sie zum ursprünglichen Typ gehören. Weitere Informationen finden Sie unter [Erweiterungsmethoden](../../../csharp/programming-guide/classes-and-structs/extension-methods.md).  
  
### <a name="implicitly-typed-local-variables"></a>Implizit typisierte lokale Variablen  
 Innerhalb einer Klassen- oder Strukturmethode können Sie implizite Typisierung verwenden, um den Compiler anzuweisen, bei der Kompilierung den richtigen Typ zu bestimmen. Weitere Informationen finden Sie unter [Implizit typisierte lokale Variablen](../../../csharp/programming-guide/classes-and-structs/implicitly-typed-local-variables.md).  
  
## <a name="c-language-specification"></a>C#-Programmiersprachenspezifikation  
 [!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]  
  
## <a name="see-also"></a>Siehe auch  
 [C#-Programmierhandbuch](../../../csharp/programming-guide/index.md)

