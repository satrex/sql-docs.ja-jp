---
title: 'タスク 1: 照合ポリシーの定義 |Microsoft Docs'
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.suite: ''
ms.technology:
- data-quality-services
- integration-services
- master-data-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 6f89a720-fce5-4f60-bef3-a159bbc9f25c
caps.latest.revision: 6
author: douglaslms
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 58507594fa2872bc5433b63e0f932d2914291a31
ms.sourcegitcommit: c18fadce27f330e1d4f36549414e5c84ba2f46c2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/02/2018
ms.locfileid: "37282358"
---
# <a name="task-1-defining-a-matching-policy"></a>タスク 1: 照合ポリシーを定義する
  ここでは、1 つのルールを持つ照合ポリシーを作成します。 ルールでは 1 つの前提条件: **Supplier ID**、つまり、ルール内の他のドメインを使用する前に Supplier Id が一致する必要があります。 ルールでは、その他の 2 つのドメイン: **Supplier Name**で**類似性**値に設定**70%** と**Contact Email**で**類似性**値に設定**30%** します。  
  
1.  メイン ページで**DQS クライアント**、 をクリックして**右矢印**横に**Suppliers**ナレッジ ベース、および選択**照合ポリシー**します。  
  
     ![照合ポリシー メニューのメイン ページ](../../2014/tutorials/media/et-definingamatchingpolicy-01.jpg "照合ポリシー メニューのメイン ページ")  
  
2.  **マップ**] ページで、[ **Excel ファイル**の**データソース**します。  
  
3.  クリックして**参照**にそのフィルターが設定されていることを確認**Excel ブック**を選択し、 **Cleansed Supplier List.xls**クレンジングのアクティビティを実行した後にエクスポートしたファイルします。  
  
    > [!NOTE]  
    >  このアクティビティは照合ポリシーの定義を主な目的としています。このため、このアクティビティの最後に結果をエクスポートすることはできません。 次のレッスンでは、照合アクティビティ用のデータ品質プロジェクトを作成し、この照合ポリシーを使用して仕入先の一覧から重複を削除します。  
  
4.  マップ**SupplierID**列**Supplier ID**ドメイン、 **Supplier Name**列**Supplier Name**ドメイン、 **ContactEmailAddress**列**Contact Email**ドメイン。 照合ポリシーの定義で使用するドメインのみにソース列をマップする必要があります。 このレッスンでは、照合ポリシー アクティビティで使用するための Supplier ID、Supplier Name、および Contact Email ドメインを作成します。  
  
     ![照合ポリシーの定義のプロセスのページ マップ](../../2014/tutorials/media/et-definingamatchingpolicy-02.jpg "マップ ポリシー定義のプロセスを一致する ページ")  
  
5.  をクリックして**次**に移動する、**照合ポリシー**ページの場所を定義して、照合ポリシー ルールを 1 つにします。  
  
6.  をクリックして**照合ルールを作成**ポリシーのルールを作成するには、ツールバーのボタンをクリックします。  
  
     ![一致するルールのツール バー ボタンを作成する](../../2014/tutorials/media/et-definingamatchingpolicy-03.jpg "照合ルールのツール バー ボタンの作成")  
  
7.  **ルールの詳細**、右側のウィンドウの入力**Remove Duplicate Suppliers**の**ルール名**します。  
  
8.  クリックして**新しいドメイン要素を追加**右側のウィンドウのツールバーでします。  
  
     ![新しいドメイン要素 ボタンを追加するルールの詳細 -](../../2014/tutorials/media/et-definingamatchingpolicy-04.jpg "ルールの詳細 - 新しいドメイン要素 ボタンを追加")  
  
9. 選択**Supplier ID**の**ドメイン**を選択し、**前提条件となる**チェック ボックスをオンします。 注意**類似性**に自動的に設定されている**Exact**します。 設定して**Supplier ID**として、**前提条件となる**、2 つのレコードでこのフィールドの値が 100% 一致を返す必要がありますを一致とその他の句でそれ以外の場合、レコードは考慮されませんを指定する、規則は無視されます。  
  
     ![重複する仕入先のルールの定義を削除する](../../2014/tutorials/media/et-definingamatchingpolicy-05.jpg "仕入先の重複する規則の定義を削除します。")  
  
10. クリックして**新しいドメイン要素を追加**再度ツールバーから。  
  
11. 選択**Supplier Name**ドメインを選択し**と同様**の**類似性**、および種類**70**の**重み**.  ここでは、レコードが一致と見なされるためには仕入先名が類似しているだけでよいこと、つまり完全に一致しなくてもよいことを指定しています。 重みは、このフィールドのスコアが照合スコア全体に与える影響を示します。  
  
12. 追加する 2 つ前の手順を繰り返して**Contact Email**を持つドメイン**30**の**重み**。  
  
13. 注意して、**最小照合スコア**に設定されている**80%**、これは、値で参照してください、**全般**のタブ、**構成**のページ**DQS 管理**します。 このスコアは、しきい値より大きな値に増やすことだけができます。  
  
14. 注意**重複するクラスター**オプションを選択します。 このオプションを使用すると、レコードを複数のクラスターに表示できます。 [重複しないクラスター] に設定を変更すると、共通のレコードを持つクラスターが 1 つのクラスターに結合されます。  
  
15. **開始**一方には、次のページで [スタート] ボタンでは、ポリシー全体 (ポリシーのすべてのルール) をテストできます。 このページのボタンを使用すると、ポリシーとは別に、各ルールをテストします。  
  
16. をクリックして**次**に切り替える、**照合結果**ページ。  
  
## <a name="next-step"></a>次の手順  
 [タスク 2: 照合ポリシーをテストおよびパブリッシュする](../../2014/tutorials/task-2-testing-and-publishing-the-matching-policy.md)  
  
  