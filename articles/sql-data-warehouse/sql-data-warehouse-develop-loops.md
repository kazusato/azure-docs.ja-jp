---
title: Azure SQL Data Warehouse で T-SQL のループを使う | Microsoft Docs
description: ソリューションを開発するための、Azure SQL Data Warehouse での T-SQL のループの使用とカーソルの置換に関するヒントです。
services: sql-data-warehouse
author: ckarst
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: cakarst
ms.reviewer: igorstan
ms.openlocfilehash: 8d51c8f18d7c00d21fcc057efcda73e2a6b46cc7
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/19/2018
ms.locfileid: "31598967"
---
# <a name="using-t-sql-loops-in-sql-data-warehouse"></a>SDL Data Warehouse での T-SQL のループの使用
ソリューションを開発するための、Azure SQL Data Warehouse での T-SQL のループの使用とカーソルの置換に関するヒントです。

## <a name="purpose-of-while-loops"></a>WHILE ループの目的

SQL Data Warehouse は、ステートメント ブロックを繰り返し実行するための [WHILE](/sql/t-sql/language-elements/while-transact-sql) ループをサポートします。 この WHILE ループは、指定された条件が true の場合に限り、またはコードが BREAK キーワードを使用してループを終了するまで実行されます。 ループは、SQL コードで定義されているカーソルを置き換えるために便利です。 また、SQL コードで記述されているほとんどすべてのカーソルは、高速順方向、読み取り専用など豊富です。 そのため、[WHILE] ループはカーソルの置換に対する優れた代替手段です。

## <a name="replacing-cursors-in-sql-data-warehouse"></a>SQL Data Warehouse でのカーソルの置換
ただし、始める前に、まず、自分で "このカーソルは、セット ベースの操作を使用して再作成できるか" と問いかけてください。 多くの場合、答えは "はい" であり、この方法が最適です。 セット ベースの操作は、1 行ずつの反復的なアプローチをとるよりも速く実行されます。

高速順方向の読み取り専用カーソルは、ループ構造で簡単に置き換えることができます。 単純な例を次に示します。 このコード例は、データベース内のすべてのテーブルの統計を更新します。 ループ内のテーブルを反復処理することで、各コマンドは順番に実行されます。

最初に、個々のステートメントを識別するための一意の行番号が含まれている、一時テーブルを作成します。

```
CREATE TABLE #tbl
WITH
( DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT  ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS Sequence
,       [name]
,       'UPDATE STATISTICS '+QUOTENAME([name]) AS sql_code
FROM    sys.tables
;
```

次に、ループの実行に必要な変数を初期化します。

```
DECLARE @nbr_statements INT = (SELECT COUNT(*) FROM #tbl)
,       @i INT = 1
;
```

1 つずつ実行するステートメントをループします。

```
WHILE   @i <= @nbr_statements
BEGIN
    DECLARE @sql_code NVARCHAR(4000) = (SELECT sql_code FROM #tbl WHERE Sequence = @i);
    EXEC    sp_executesql @sql_code;
    SET     @i +=1;
END
```

最後に、最初の手順で作成した一時テーブルを削除します。

```
DROP TABLE #tbl;
```

## <a name="next-steps"></a>次の手順
開発に関するその他のヒントについては、[開発の概要](sql-data-warehouse-overview-develop.md)のページをご覧ください。

