---
title: fn_all_changes_&lt;capture_instance&gt; (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/02/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- fn_all_changes
- sys.fn_all_changes
- fn_all_changes_TSQL
- sys.fn_all_changes_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- fn_all_changes_<capture_instance>
- sys.fn_all_changes_<capture_instance>
ms.assetid: 564fae96-b88c-4f22-9338-26ec168ba6f5
author: rothja
ms.author: jroth
ms.openlocfilehash: 6b9b6e62d0f69c5182ad69e21cb46800d4ddcc86
ms.sourcegitcommit: 2a06c87aa195bc6743ebdc14b91eb71ab6b91298
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2019
ms.locfileid: "72909397"
---
# <a name="sysfn_all_changes_ltcapture_instancegt-transact-sql"></a>fn_all_changes_&lt;capture_instance&gt; (Transact-sql)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  **すべての変更**クエリ関数のラッパー。 これらの関数を作成するために必要なスクリプトは、sys.sp_cdc_generate_wrapper_function ストアド プロシージャで生成されます。  
  
 ![トピック リンク アイコン](../../database-engine/configure-windows/media/topic-link.gif "トピック リンク アイコン") [Transact-SQL 構文表記規則](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>構文  
  
```  
  
fn_all_changes_<capture_instance> ('start_time' ,'end_time', '<row_filter_option>' )  
  
<capture_instance> ::= The name of the capture instance.  
<row_filter_option> ::=  
{ all  
  | all update old  
}  
```  
  
## <a name="arguments"></a>[引数]  
 *start_time*  
 結果セットに含める変更テーブルエントリの範囲の下端を表す**datetime**値です。  
  
 結果セットには、 *start_time*よりも大きいコミット時間が関連付けられている < > capture_instance の行のみが含まれます。  
  
 この引数に NULL 値を指定した場合、クエリ範囲の下限は、キャプチャ インスタンスの有効な範囲の下限に対応します。  
  
 *end_time*  
 結果セットに含める変更テーブルエントリの範囲の上限を表す**datetime**値です。  
  
 このパラメーターは、ラッパー関数の create スクリプトを生成するために sp_cdc_generate_wrapper_function が呼び出されたときに、@closed_high_end_point に対して選択した値に応じて、次の2つの意味のいずれかを使用できます。  
  
-   @closed_high_end_point = 1  
  
     結果セットに含まれるのは、end_time 以下に関連付けられているコミット時間がある capture_instance > の変更テーブル内 < の行だけです。  
  
-   @closed_high_end_point = 0  
  
     結果セットには、capture_instance_CT 変更テーブル内の行のうち、関連付けられたコミット時間が厳密に end_time より小さい行だけが含まれます。  
  
 この引数に NULL 値が指定されている場合、クエリ範囲の上端は、キャプチャインスタンスの有効な範囲の上限に対応します。  
  
 < row_filter_option >:: = {all | update old}  
 メタデータ列の内容と、結果セットで返される行を制御するオプション。  
  
 次のいずれかのオプションを指定できます。  
  
 すべての  
 指定された LSN 範囲内のすべての変更を返します。 更新操作の結果として発生する変更の場合、このオプションは、更新が適用された後に、新しい値を含む行のみを返します。  
  
 すべての更新プログラムが古い  
 指定された LSN 範囲内のすべての変更を返します。 更新操作の結果として発生する変更の場合、このオプションは更新前と更新後の列値を含む2行を返します。  
  
## <a name="table-returned"></a>返されるテーブル  
  
|列名|列の型|Description|  
|-----------------|-----------------|-----------------|  
|__CDC_STARTLSN|**binary(10)**|変更に関連付けられているトランザクションのコミット LSN。 同じトランザクションでコミットされたすべての変更は、同じコミット LSN を共有します。|  
|__CDC_SEQVAL|**binary(10)**|特定のトランザクションに含まれる行の変更を並べ替えるためのシーケンス値です。|  
|@column_list> からの列の \<|**varies**|*Column_list*引数で指定されている列は、ラッパー関数を作成するスクリプトを生成するために呼び出されるときに sp_cdc_generate_wrapper_function に対して指定されます。|  
|__CDC_OPERATION|**nvarchar(2)**|ターゲット環境に行を適用するために必要な操作を示す操作コード。 これは、呼び出しで指定される引数*row_filter_option*の値によって異なります。<br /><br /> *row_filter_option* = ' all '<br /><br /> ' D '-削除操作<br /><br /> ' I '-挿入操作<br /><br /> ' UN '-更新操作の新しい値<br /><br /> *row_filter_option* = ' all update old '<br /><br /> ' D '-削除操作<br /><br /> ' I '-挿入操作<br /><br /> ' UN '-更新操作の新しい値<br /><br /> ' UO '-更新操作の古い値|  
|@update_flag_list> からの列の \<|**bit**|ビットフラグには、列名に _uflag を追加することによって名前が付けられます。 フラグは常に NULL に設定されます。これは、' UO ' の \_(_d) の場合、' I ' です。 \_が ' UN ' の場合、更新によって対応する列が変更された場合は、1に設定されます。 それ以外の場合は0です。|  
  
## <a name="remarks"></a>備考  
 Fn_all_changes_ < capture_instance > 関数は、fn_cdc_get_all_changes_ < capture_instance > クエリ関数のラッパーとして機能します。 ラッパーを作成するスクリプトを生成するには、sys.sp_cdc_generate_wrapper ストアド プロシージャを使用します。  
  
 ラッパー関数が自動的に作成されることはありません。 ラッパー関数を作成するには、次の 2 つを行う必要があります。  
  
1.  ラッパーの作成スクリプトを生成するストアド プロシージャを実行します。  
  
2.  スクリプトを実行して、実際にラッパー関数を作成します。  

 ラッパー関数を使用すると、LSN 値ではなく**datetime**値によって制限された間隔内で発生した変更を体系的にクエリできます。 ラッパー関数は、指定された**datetime**値と、クエリ関数の引数として内部的に必要な LSN 値の間に必要なすべての変換を実行します。 ラッパー関数を順番に使用して変更データのストリームを処理する場合、次の規則に従うと、データが失われたり、繰り返されたりすることはありません。1回の呼び出しに関連付けられている間隔の @end_time 値がと @start_time して提供されます。後続の呼び出しに関連付けられている間隔の値。  
  
 スクリプトの作成時に @closed_high_end_point パラメーターを使用すると、指定されたクエリウィンドウで閉じた上限または開いた上限をサポートするラッパーを生成できます。 つまり、コミット時間が抽出範囲の上限と等しくなるエントリを間隔に含めるかどうかを決定できます。 既定では、上限が含まれます。  
  
 **All changes**ラッパー関数によって返される結果セットでは、変更テーブルの __ $ start_lsn 列と \_\_$seqval 列が \_\_列として返されます。 ラッパーが生成されたときに、 *\@column_list*パラメーターに含まれていた追跡対象列のみを使用して、これらの列に従います。 *\@column_list*が NULL の場合、追跡対象のすべてのソース列が返されます。 変換元の列の後に操作列 (\_) が続きます。この操作は、操作を識別する1つまたは2文字の列です。  
  
 次に、@update_flag_list パラメーターで指定された各列の結果セットに対し、ビット フラグが追加されます。 すべての**変更**のラッパーでは、__ CDCOPERATION が d '、' I '、または ' uo ' の場合、ビットフラグは常に NULL になります。 \_が ' UN ' の場合は、更新操作によって列が変更されたかどうかに応じて、フラグが1または0に設定されます。  
  
 変更データキャプチャの構成テンプレート ' インスタンス化 CDC Wrapper Tvf for Schema ' は、sp_cdc_generate_wrapper_function ストアドプロシージャを使用して、スキーマの定義済みクエリ関数に対するすべてのラッパー関数の作成スクリプトを取得する方法を示しています。 その後、テンプレートによってこれらのスクリプトが作成されます。 テンプレートの詳細については、「[テンプレートエクスプローラー](../../ssms/template/template-explorer.md)」を参照してください。  
  
## <a name="see-also"></a>「  
 [sp_cdc_generate_wrapper_function &#40;transact-sql&#41; ](../../relational-databases/system-stored-procedures/sys-sp-cdc-generate-wrapper-function-transact-sql.md)   
 [fn_cdc_get_all_changes_&#60;capture_instance&#62;  &#40;transact-sql&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-all-changes-capture-instance-transact-sql.md)  
  
  
