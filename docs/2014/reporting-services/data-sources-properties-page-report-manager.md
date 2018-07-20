---
title: データ ソース プロパティ ページ (レポート マネージャー) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.suite: ''
ms.technology:
- reporting-services-native
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: f37edda0-19e6-489e-b544-8751fa6b6cfb
caps.latest.revision: 35
author: markingmyname
ms.author: maghan
manager: craigg
ms.openlocfilehash: e826f27b2ce6bbb75d4aabc9d8537d0f867a0cce
ms.sourcegitcommit: c18fadce27f330e1d4f36549414e5c84ba2f46c2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/02/2018
ms.locfileid: "37179559"
---
# <a name="data-sources-properties-page-report-manager"></a>[データ ソース] プロパティ ページ (レポート マネージャー)
  [データ ソース] プロパティ ページでは、現在のレポートの外部データ ソースへの接続方法を定義できます。 レポートに本来パブリッシュされていたデータ ソース接続情報をオーバーライドすることができます。 レポートで複数のデータ ソースが使用される場合、プロパティ ページに各データ ソースの独自のセクションがあります。 データ ソースは、レポートで定義された順に一覧表示されます。  
  
 レポートで使用するデータ ソースを指定している場合に、そのデータ ソースを使用するレポートから切り離して作成および管理されている共有データ ソースを使用することができます。 共有データ ソース アイテムを使用しない場合は、レポートで使用するデータ ソース接続を定義することができます。  
  
## <a name="navigation"></a>ナビゲーション  
 ユーザー インターフェイス (UI) のこの場所に移動するには、次の手順に従います。  
  
### <a name="to-open-the-data-sources-properties-page"></a>[データ ソース] プロパティ ページを開くには  
  
1.  レポート マネージャーを開き、データ ソースの選択対象のレポートを探します。  
  
2.  レポートの上にマウス ポインターを移動し、下矢印をクリックします。  
  
3.  ドロップダウン メニューの **[管理]** をクリックします。 この操作により、レポートの **[全般]** プロパティ ページが開きます。  
  
4.  **[データ ソース]** タブをクリックします。  
  
## <a name="options"></a>および  
 **共有データ ソース**  
 レポートで使用する共有データ ソースを指定します。 新しいデータ ソースの作成の詳細については、次を参照してください。[作成、削除、または共有データ ソースを変更&#40;レポート マネージャー&#41;](../../2014/reporting-services/create-delete-or-modify-a-shared-data-source-report-manager.md)します。  
  
 **[参照]**  
 **[参照]** をクリックして、データ ソースの選択ページを表示します。このページは、共有データ ソースを選択するために使用します。 詳細については、次を参照してください。[データ ソースの選択 ページ&#40;レポート マネージャー&#41;](../../2014/reporting-services/data-source-selection-page-report-manager.md)します。  
  
 **カスタム データ ソース**  
 レポートのデータ ソースへの接続方法を指定します。  
  
 以下のオプションを使用して、カスタム データ ソース接続を指定します。  
  
 **データ ソースの種類**  
 データ ソースから取得したデータの処理に使用する、データ処理拡張機能を指定します。 組み込みデータ拡張機能の一覧で、次を参照してください。 [Reporting Services でサポートされるデータ ソース&#40;SSRS&#41;](create-deploy-and-manage-mobile-and-paginated-reports.md)します。 サード パーティ ベンダーから、別のデータ処理拡張機能を入手できる可能性もあります。  
  
 **[接続文字列]**  
 レポート サーバーがデータ ソースへの接続に使用する接続文字列を指定します。 接続の種類によって使用する構文が決まります。 たとえば、XML データ処理拡張機能の接続文字列は、XML ドキュメントの URL になります。 ほとんどの場合、データベース サーバーおよびデータ ファイルは一般的な接続文字列で指定されます。 次に、MyData という名前の [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] データベースへの接続に使用する接続文字列の例を示します。  
  
 `data source=<a SQL Server instance>;initial catalog=MyData`  
  
 接続文字列は式として構成できます。これにより、データ ソースを実行時に指定することができます。 データ ソースの式は、レポート デザイナーを使用してレポートで定義されます。 データ ソースの式は、レポート マネージャー上で定義、表示、変更できません。 ただし、**[既定値よりオーバーライド]** をクリックして静的な接続文字列を入力することで、データ ソースの式をオーバーライドすることができます。 式に戻す場合は、 **[既定値に戻す]** をクリックします。 レポート サーバーは、復元する必要がある場合に備えて元の接続文字列を保存します。 データ ソースの式を使用するには、レポートに本来パブリッシュされていたデータ ソース接続情報を使用する必要があります。 共有データ ソースでは、接続文字列の式の使用はサポートされていません。  
  
 **使用して接続します。**  
 資格情報の取得方法を決定するオプションを指定します。  
  
> [!IMPORTANT]  
>  資格情報が接続文字列で指定される場合、このセクションで指定したオプションおよび値は無視されます。 接続文字列で資格情報を指定する場合、このページを表示するすべてのユーザーに対して、その値がクリア テキストで表示されることに注意してください。  
  
 **レポートを実行しているユーザーによって指定された資格情報**  
 各ユーザーは、ユーザー名およびパスワードを入力してデータ ソースにアクセスする必要があります。 ユーザーに資格情報の入力を要求するメッセージを定義できます。 既定のテキスト文字列は、"データ ソースへのアクセスに使用するユーザー名とパスワードを入力してください。" です。  
  
 ユーザーが指定する資格情報が Windows 認証の資格情報である場合は、 **[データ ソースへの接続時に Windows 資格情報として使用する]** を選択します。 データベース認証を使用している場合にこのチェック ボックスをオンされません (たとえば、[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]認証)。  
  
 **レポート サーバーに安全に格納されている資格情報**  
 暗号化されたユーザー名およびパスワードを、レポート サーバーのデータベースに格納します。 このオプションを選択すると、レポート (たとえば、ユーザーの操作によってではなく、スケジュールまたはイベントによって起動されるレポート) を自動的に実行できます。 既定のセキュリティを使用している場合、ユーザー名は Windows ドメイン アカウントである必要があります。 この形式でアカウントを指定:\<ドメイン >\\< ユーザー名\>します。 指定するアカウントには、レポートが使用するデータ ソースをホストするコンピューター上でローカルにログオンする権限が必要です。  
  
 資格情報が Windows 認証の資格情報である場合は、 **[データ ソースへの接続時に Windows 資格情報として使用する]** を選択します。 データベース認証を使用している場合にこのチェック ボックスをオンされません (たとえば、[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]認証)。  
  
 **[データ ソースへの接続が確立した後に、認証されているユーザーの権限を借用する]** チェック ボックスをオンにすると、データ ソースが権限借用をサポートする場合に限り、資格情報の委任が可能になります。 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]データベースでは、このオプションは、SETUSER 関数を設定します。  
  
> [!TIP]  
>  [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)]Windows アカウントの資格情報のみをサポートします。 したがって、オプションは、「データ ソースに接続するときは、Windows 資格情報として使用して」と「偽装、認証されたユーザー データ ソースへの接続の確立後」の両方に選択、[!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)]データ ソース。  
  
 **Windows 統合セキュリティ**  
 現在のユーザーの Windows 資格情報を使用して、データ ソースにアクセスします。 データ ソースへのアクセスに使用される資格情報が、ネットワーク ドメインへのログオンに使用される資格情報と同じである場合に、このオプションを選択します。 Kerberos 認証が有効なドメインに参加している場合、またはレポート サーバーと同一のコンピューターにデータ ソースがある場合に、このオプションは最適です。 Kerberos 認証が無効になっている場合、Windows 資格情報を別のコンピューターに渡すこともできます。 別のコンピューターの接続が必要な場合に、想定されるデータではなく、エラーが返されます。  
  
 レポート サーバー管理者は、レポート データ ソースのアクセスに対して Windows 統合セキュリティの使用を無効にできます。 この値がグレーで表示される場合、その機能は使用できません。  
  
 レポートをスケジュールまたはレポートにサブスクライブする場合は、このオプションを使用しないでください。 スケジュールされたレポート処理や自動実行のレポート処理では、ユーザー入力または現在のユーザーのセキュリティ コンテキストがなくても取得できる資格情報が必要になります。 このようなレポート処理で使用できるのは、保存済みの資格情報のみです。 この理由で、レポートが資格情報の種類に Windows 統合セキュリティを使用するよう構成されている場合、レポート サーバーではレポートまたはサブスクリプションの処理をスケジュール設定できません。 既にサブスクライブされたレポートまたはスケジュールされた操作を備えたレポートに対してこのオプションを選択する場合、サブスクリプションおよびスケジュールされた操作は停止されます。  
  
 **資格情報は必要ありません。**  
 データ ソースへのアクセスに、資格情報が必要でないことを指定します。 データ ソースがユーザーのログインを必要とする場合、このオプションを選択しても効果がありません。 このオプションは、データ ソース接続にユーザーの資格情報を必要としない場合にのみ選択してください。  
  
 このオプションを使用するには、レポート サーバーの配置用の自動実行アカウントをあらかじめ構成しておく必要があります。 自動実行アカウントは、他に使用できる資格情報のソースがない状況で外部データ ソースに接続するために使用します。 このオプションを指定していてアカウントが構成されていない場合、レポート データ ソースへの接続は失敗し、レポート処理は実行されません。  このアカウントの詳細については、次を参照してください。[自動実行アカウントを構成する&#40;SSRS 構成マネージャー&#41;](install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md)します。  
  
 **[適用]**  
 変更を保存する場合にクリックします。  
  
## <a name="see-also"></a>参照  
 [レポート データ ソースを管理します。](report-data/manage-report-data-sources.md)   
 [レポート データ ソースに関する資格情報と接続情報を指定する](report-data/specify-credential-and-connection-information-for-report-data-sources.md)   
 [レポート マネージャー F1 ヘルプ](../../2014/reporting-services/report-manager-f1-help.md)  
  
  