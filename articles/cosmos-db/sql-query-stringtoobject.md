---
title: StringToObject in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion StringToObject in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: c3e61d1efe20910d84ef4ff583d74982b3ea9f3d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "78296380"
---
# <a name="stringtoobject-azure-cosmos-db"></a>StringToObject (Azure Cosmos DB)
 Gibt den Ausdruck übersetzt in ein Objekt zurück. Wenn der Ausdruck nicht übersetzt werden kann, wird „undefined“ zurückgegeben.  
  
## <a name="syntax"></a>Syntax
  
```sql
StringToObject(<str_expr>)  
```  
  
## <a name="arguments"></a>Argumente
  
*str_expr*  
   Ist ein Zeichenfolgenausdruck, der als JSON-Objektausdruck analysiert werden soll. Beachten Sie, dass geschachtelte Zeichenfolgenwerte in doppelten Anführungszeichen angegeben werden müssen, damit sie gültig sind. Ausführliche Informationen zum JSON-Format finden Sie unter [json.org](https://json.org/).  
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen Ausdruck für ein Objekt oder „undefined“ zurück.  
  
## <a name="examples"></a>Beispiele
  
  Im folgenden Beispiel wird das typübergreifende Verhalten von `StringToObject` veranschaulicht. 
  
 Die folgenden Beispiele zeigen eine gültige Eingabe.

```sql
SELECT 
    StringToObject("{}") AS obj1, 
    StringToObject('{"A":[1,2,3]}') AS obj2,
    StringToObject('{"B":[{"b1":[5,6,7]},{"b2":8},{"b3":9}]}') AS obj3, 
    StringToObject("{\"C\":[{\"c1\":[5,6,7]},{\"c2\":8},{\"c3\":9}]}") AS obj4
``` 

Hier ist das Resultset.

```json
[{"obj1": {}, 
  "obj2": {"A": [1,2,3]}, 
  "obj3": {"B":[{"b1":[5,6,7]},{"b2":8},{"b3":9}]},
  "obj4": {"C":[{"c1":[5,6,7]},{"c2":8},{"c3":9}]}}]
```

 Die folgenden Beispiele zeigen eine ungültige Eingabe.
Sie sind zwar innerhalb einer Abfrage gültig, werden jedoch nicht als gültige Objekte interpretiert. Zeichenfolgen innerhalb der Objektzeichenfolge müssen mit Escapezeichen versehen werden: "{\\"a\\":\\"str\\"}". Alternativ kann die Objektzeichenfolge in einfache Anführungszeichen eingeschlossen werden: '{"a": "str"}'.

Einfache Anführungszeichen um Eigenschaftennamen sind in JSON nicht gültig.

```sql
SELECT 
    StringToObject("{'a':[1,2,3]}")
```

Hier ist das Resultset.

```json
[{}]
```  

Eigenschaftennamen ohne umgebende Anführungszeichen sind in JSON nicht gültig.

```sql
SELECT 
    StringToObject("{a:[1,2,3]}")
```

Hier ist das Resultset.

```json
[{}]
``` 

Die folgenden Beispiele zeigen eine ungültige Eingabe.

 Der übergebene Ausdruck wird als JSON-Objekt analysiert. Die folgenden Eingaben werden nicht als Objekttyp ausgewertet und geben daher „undefined“ zurück:

```sql
SELECT 
    StringToObject("}"),
    StringToObject("{"),
    StringToObject("1"),
    StringToObject(NaN), 
    StringToObject(false), 
    StringToObject(undefined)
``` 
 
 Hier ist das Resultset.

```json
[{}]
```

## <a name="remarks"></a>Bemerkungen

Der Index wird von dieser Systemfunktion nicht verwendet.

## <a name="next-steps"></a>Nächste Schritte

- [Zeichenfolgenfunktionen in Azure Cosmos DB](sql-query-string-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
