---
title: Azure Cosmos DB языка запросов
description: Узнайте о том, как функция CEILING SQL System в Azure Cosmos DB возвращает наименьшее целочисленное значение, большее или равное указанному числовому выражению.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 2da7820a6c9f1f90585b4deb605bb99c7580b0e5
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2019
ms.locfileid: "75444800"
---
# <a name="ceiling-azure-cosmos-db"></a>CEILING (Azure Cosmos DB)
 Возвращает наименьшее целочисленное значение, которое больше или равно указанному числовому выражению.  
  
## <a name="syntax"></a>Синтаксис
  
```sql
CEILING (<numeric_expr>)  
```  
  
## <a name="arguments"></a>Аргументы
  
*numeric_expr*  
   Числовое выражение.  
  
## <a name="return-types"></a>Типы возвращаемых данных
  
  Возвращает числовое выражение.  
  
## <a name="examples"></a>Примеры
  
  В следующем примере показаны положительные числовые, отрицательные и нулевые значения с помощью функции `CEILING`.  
  
```sql
SELECT CEILING(123.45) AS c1, CEILING(-123.45) AS c2, CEILING(0.0) AS c3  
```  
  
 Результирующий набор:  
  
```json
[{c1: 124, c2: -123, c3: 0}]  
```  

## <a name="next-steps"></a>Дальнейшие действия

- [Математические функции Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Системные функции Azure Cosmos DB](sql-query-system-functions.md)
- [Знакомство со службой Azure Cosmos DB. API DocumentDB](introduction.md)
