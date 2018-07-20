---
title: データベース スキーマの理解 |Microsoft Docs
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.suite: ''
ms.technology:
- analysis-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Schema Generation Wizard, database schema
- database schema [Analysis Services]
- relational schema [Analysis Services], database schema
- subject area schema options [Analysis Services]
- staging area schema options [Analysis Services]
- denormalized schemas
ms.assetid: 51e411f9-ee3f-4b92-9833-c2bce8c6b752
caps.latest.revision: 28
author: minewiskan
ms.author: owend
manager: craigg
ms.openlocfilehash: ef75cf2773781f94bd02a26c5c94958b9f4dfe3f
ms.sourcegitcommit: c18fadce27f330e1d4f36549414e5c84ba2f46c2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/02/2018
ms.locfileid: "37282248"
---
# <a name="understanding-the-database-schemas"></a>データベース スキーマの理解
  スキーマ生成ウィザードでは、 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]のディメンションおよびメジャー グループに基づいて、サブジェクト領域のデータベース用の非正規化されたリレーショナル スキーマを生成します。 このウィザードでは、ディメンション データを格納するための各ディメンション用のリレーショナル テーブル (ディメンション テーブル)、およびファクト データを格納するための各メジャー グループ用のリレーショナル テーブル (ファクト テーブル) が生成されます。 このウィザードを使用してこれらのリレーショナル テーブルを生成する場合、リンク ディメンション、リンク メジャー グループ、およびサーバー時間ディメンションは無視されます。  
  
## <a name="validation"></a>検証  
 基になるリレーショナル スキーマの生成を開始する前に、スキーマ生成ウィザードによって、 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] キューブおよびディメンションが検証されます。 ウィザードによってエラーが検出された場合には、ウィザードは停止し、 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]の [タスク一覧] ウィンドウにエラー レポートが表示されます。 スキーマの生成を妨げるエラーには次のようなものがあります。  
  
-   ディメンションに複数のキー属性がある。  
  
-   親属性がキー属性と異なるデータ型を持っている。  
  
-   メジャー グループにメジャーがない。  
  
-   逆ディメンションまたはメジャーが不適切に構成されている。  
  
-   代理キーが不適切に構成されている (`ScdOriginalID` 属性型を使用した複数の属性や、整数データ型を使用した列にバインドされていない `ScdOriginalID` を使用した属性など)。  
  
## <a name="dimension-tables"></a>ディメンション テーブル  
 各ディメンションに対して、スキーマ生成ウィザードによって、サブジェクト領域データベースに含まれるディメンション テーブルが生成されます。 ディメンション テーブルの構造は、基になるディメンションをデザイン中に行った選択によって異なります。  
  
 [列]  
 ウィザードは、ディメンション テーブルは、基になるバインドなど、ディメンション内の各属性に関連付けられているバインディングの 1 つの列を生成、 `KeyColumns`、 `NameColumn`、 `ValueColumn`、 `CustomRollupColumn`、 `CustomRollupPropertiesColumn`、および`UnaryOperatorColumn`の各属性のプロパティ。  
  
 リレーションシップ  
 このウィザードでは、各親属性の列とディメンション テーブルの主キーの間のリレーションシップが生成されます。  
  
 また、必要に応じて、キューブ内で参照ディメンションとして定義されている、それぞれの追加ディメンション テーブルの主キーへのリレーションシップも生成されます。  
  
 制約  
 このウィザードでは、既定で、ディメンションのキー属性に基づいて各ディメンション テーブル用の主キー制約が生成されます。 主キー制約が生成された場合には、既定で、個別の名前列が生成されます。 データベースに主キーを作成しない場合でも、データ ソース ビューに論理主キーが作成されます。  
  
> [!NOTE]  
>  ディメンション テーブルの基になるディメンションに複数のキー属性が指定されている場合、エラーが発生します。  
  
 翻訳  
 このウィザードでは、翻訳列が必要なすべての属性に対して、翻訳済みの値を格納する個別のテーブルが生成されます。 また、必要な言語ごとに個別の列が作成されます。  
  
## <a name="fact-tables"></a>ファクト テーブル  
 キューブ内の各メジャー グループに対して、スキーマ生成ウィザードによって、サブジェクト領域データベースに含まれるファクト テーブルが生成されます。 ファクト テーブルの構造は、基になるメジャー グループのデザイン中に行われた選択と、メジャー グループと含まれるディメンションの間のリレーションシップによって異なります。  
  
 [列]  
 ウィザードの各メジャーに使用するメジャー以外の 1 つの列の生成、`Count`集計関数。 これらのメジャーは、ファクト テーブルに対応する列を必要としません。  
  
 さらに、必要に応じて、メジャー グループ上の標準のディメンション リレーションシップごとの各粒度属性列に 1 つの列が生成され、このテーブルの基になるメジャー グループへのファクト ディメンション リレーションシップがあるディメンションの各属性に関連付けられたバインドに 1 つまたは複数の列が生成されます。  
  
 リレーションシップ  
 このウィザードでは、ファクト テーブルからディメンション テーブルの粒度属性への標準のディメンション リレーションシップごとに 1 つのリレーションシップが生成されます。 粒度がディメンション テーブルのキー属性に基づいている場合には、リレーションシップはデータベース内およびデータ ソース ビュー内に作成されます。 粒度がその他の属性に基づいている場合には、リレーションシップはデータ ソース ビュー内のみに作成されます。  
  
 ウィザードでインデックスの生成を選択すると、これらのリレーションシップ列のそれぞれに対して非クラスター化インデックスが生成されます。  
  
 制約  
 主キーはファクト テーブル上では生成されません。  
  
 参照整合性の適用を選択すると、参照整合性制約がディメンション テーブルとファクト テーブルの間の適切な場所に生成されます。  
  
 翻訳  
 このウィザードでは、翻訳列を必要とするメジャー グループのすべてのプロパティの翻訳済みの値を格納する個別のテーブルが生成されます。 また、必要な言語ごとに個別の列が作成されます。  
  
## <a name="data-type-conversion-and-default-lengths"></a>データ型変換と既定の長さ  
 スキーマ生成ウィザードでは、[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] `wchar` データ型を使用する列以外のすべての場合に、データ型が無視されます。 `wchar`データ サイズに直接変換されます、`nvarchar`データ型。 ただし、`wchar` サイズを使用している列の指定された長さが 4,000 バイトより長い場合には、スキーマ生成ウィザードではエラーが発生します。  
  
 属性のバインドなどのデータ アイテムに長さが指定されていない場合、次の表に示す既定の長さが列に使用されます。  
  
|データ アイテム|既定の長さ (バイト)|  
|---------------|------------------------------|  
|KeyColumn|50|  
|NameColumn|50|  
|CustomRollupColumn|3000|  
|CustomRollupPropertiesColumn|500|  
|UnaryOperatorColumn|1|  
  
## <a name="see-also"></a>参照  
 [増分生成の理解](understanding-incremental-generation.md)   
 [データ ソース ビューおよびデータ ソースへの変更の管理](manage-changes-to-data-source-views-and-data-sources.md)  
  
  