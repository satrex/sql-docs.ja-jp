---
title: MSRS 2014 Web サービスおよび MSRS 2014 Windows Service パフォーマンスオブジェクトのパフォーマンスカウンター (ネイティブモード) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology: reporting-services-native
ms.topic: conceptual
helpviewer_keywords:
- performance counters [Reporting Services]
- Report Server Web service, performance counters
- Reporting Services performance object
- RS Web Service performance object
- counters [Reporting Services]
- performance [Reporting Services]
ms.assetid: c642fc4f-8734-4626-a194-42ac9cd8e2ef
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: e3acf02dff2d0d699a6d4dfe271d0aed8419683a
ms.sourcegitcommit: a165052c789a327a3a7202872669ce039bd9e495
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/22/2019
ms.locfileid: "72782823"
---
# <a name="performance-counters-for-the-msrs-2014-web-service-and-msrs-2014-windows-service-performance-objects-native-mode"></a>MSRS 2014 Web Service と MSRS 2014 Windows Service パフォーマンス オブジェクトのパフォーマンス カウンター (ネイティブ モード)
  このトピックでは、`MSRS 2014 Web Service` および `MSRS 2014 Windows Service` のパフォーマンスオブジェクトのパフォーマンスカウンターについて説明します。  
  
> [!NOTE]  
>  これらのパフォーマンス オブジェクトは、ローカル レポート サーバー上のイベントを監視します。 スケールアウト配置でレポート サーバーを実行している場合、カウントはスケールアウト配置ではなく、現在のサーバーに適用されます。  
  
 **[!INCLUDE[applies](../../includes/applies-md.md)]**  [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] Native mode  
  
 パフォーマンス オブジェクトは、Windows パフォーマンス モニター (**Perfmon.exe**) で利用できます。 詳細については、Windows のマニュアルの「[ランタイム プロファイリング](https://msdn.microsoft.com/library/w4bz2147.aspx)」(https://msdn.microsoft.com/library/w4bz2147.aspx) を参照してください。  
  
 SharePoint モードのパフォーマンスカウンタに関連する情報については、「 [msrs 2014 Web Service Sharepoint モードのパフォーマンスカウンター」および「msrs 2014 Windows &#40;Service Sharepoint&#41;mode performance Objects sharepoint mode](../report-server/performance-counters-msrs-2011-web-service-performance-objects.md)」を参照してください。  
  
 **このトピックの内容:**  
  
-   [MSRS 2014 Web サービスのパフォーマンスカウンター](#bkmk_webservice)  
  
-   [MSRS 2014 Windows サービスのパフォーマンスカウンター](#bkmk_windowsservice)  
  
-   [PowerShell コマンドレットを使用して一覧を取得する](#bkmk_powershell)  
  
##  <a name="bkmk_webservice"></a>MSRS 2014 Web サービスのパフォーマンスカウンター  
 `MSRS 2014 Web Service` パフォーマンス オブジェクトは、レポート サーバーのパフォーマンスを監視します。 このパフォーマンス オブジェクトには複数のカウンターが含まれ、主に対話的なレポート表示操作によって開始されるレポート サーバー処理の追跡に使用されます。 設定したカウンターは [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] のすべてのインスタンスに適用することも、特定のインスタンスにだけ適用することもできます。 これらのカウンターは、 [!INCLUDE[vstecasp](../../../includes/vstecasp-md.md)] がレポート サーバー Web サービスを停止した時点でリセットされます。  
  
 次の表に、`MSRS 2014 Web Service` パフォーマンスオブジェクトに含まれているカウンターの一覧を示します。  
  
|カウンター|Description|  
|-------------|-----------------|  
|`Active Sessions`|アクティブなセッションの数。 このカウンターは、レポートの実行によって生成されたすべてのブラウザー セッション (アクティブであるかどうかにかかわらず) の累積数を表示します。<br /><br /> セッション レコードが削除されると、カウンターの値は減少します。 既定では、セッションは利用されない状態が 10 分間続くと削除されます。|  
|`Cache Hits/Sec`|キャッシュされたレポートに対する 1 秒あたりの要求数。 これはレポートの再表示の要求であり、キャッシュから直接処理されるレポートの要求ではありません (このトピックの `Total Cache Hits` を参照してください)。|  
|`Cache Hits/Sec (Semantic Models)`|キャッシュされたモデルに対する 1 秒あたりの要求数。 これはレポートの再表示の要求であり、キャッシュから直接処理されるレポートの要求ではありません。|  
|`Cache Misses/Sec`|キャッシュからレポートを返すことに失敗した 1 秒あたりの要求数。 このカウンターは、キャッシュに使用するリソース (ディスクまたはメモリ) が十分であるかどうかを判断する場合に使用します。|  
|`Cache Misses/Sec (Semantic Models)`|キャッシュからモデルを返すことに失敗した 1 秒あたりの要求数。 このカウンターは、キャッシュに使用するリソース (ディスクまたはメモリ) が十分であるかどうかを判断する場合に使用します。|  
|`First Session Requests/Sec`|レポート サーバーのキャッシュから 1 秒あたりに開始される新しいユーザー セッションの数。|  
|`Memory Cache Hits/Sec`|メモリ内キャッシュからレポートが取得される 1 秒あたりの回数。 *メモリ内キャッシュ* は、CPU メモリにレポートを格納するキャッシュの一部です。 メモリ内キャッシュが使用されると、レポート サーバーでは、キャッシュ済みコンテンツに対して [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] へのクエリが実行されません。|  
|`Memory Cache Misses/Sec`|メモリ内キャッシュからレポートを取得できなかった 1 秒あたりの回数。|  
|`Next Session Requests/Sec`|既存のセッション内 (セッション スナップショットから表示されるレポートなど) で開いているレポートに対する 1 秒あたりの要求の数。|  
|`Report Requests`|現在アクティブで、レポート サーバーで管理されているレポートの数。|  
|`Reports Executed/Sec`|レポート実行に成功した 1 秒あたりの回数。 このカウンターでは、レポートのボリュームに関する統計を取得できます。 `Request/Sec` と共にこのカウンターを使用して、キャッシュから返される可能性があるレポート要求とレポート実行を比較します。|  
|`Requests/Sec`|レポート サーバーに対して行われる 1 秒あたりの要求数。 このカウンターでは、レポート サーバーによって管理されるすべての種類の要求を追跡します。|  
|`Total Cache Hits`|サービスの開始後に行われたキャッシュからのレポート要求の総数。 このカウンターは、 [!INCLUDE[vstecasp](../../../includes/vstecasp-md.md)] によってレポート サーバー Web サービスが停止すると必ずリセットされます。|  
|`Total Cache Hits (Semantic Models)`|サービスの開始後に行われたキャッシュからのモデル要求の総数。 このカウンターは、ASP.NET によってレポート サーバー Web サービスが停止すると必ずリセットされます。|  
|`Total Cache Misses`|サービスの開始後に、レポートがキャッシュから返されなかった合計回数。 このカウンターは、 [!INCLUDE[vstecasp](../../../includes/vstecasp-md.md)] によってレポート サーバー Web サービスが停止すると必ずリセットされます。 このカウンターを使用して、ディスク領域およびメモリが十分であるかどうかを判断します。|  
|`Total Cache Misses (Semantic Models)`|サービスの開始後に、モデルがキャッシュから返されなかった合計回数。 このカウンターは、ASP.NET によってレポート サーバー Web サービスが停止すると必ずリセットされます。 このカウンターを使用して、ディスク領域およびメモリが十分であるかどうかを判断します。|  
|`Total Memory Cache Hits`|サービスの開始後に、メモリ内キャッシュから返されたキャッシュされたレポートの総数。 このカウンターは、 [!INCLUDE[vstecasp](../../../includes/vstecasp-md.md)] によってレポート サーバー Web サービスが停止すると必ずリセットされます。 *メモリ内キャッシュ* は、CPU メモリにレポートを格納するキャッシュの一部です。 メモリ内キャッシュが使用されると、レポート サーバーでは、キャッシュ済みコンテンツに対して [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] へのクエリが実行されません。|  
|`Total Memory Cache Misses`|サービスの開始後に発生したメモリ内キャッシュのキャッシュ ミスの総数。 このカウンターは、 [!INCLUDE[vstecasp](../../../includes/vstecasp-md.md)] によってレポート サーバー Web サービスが停止すると必ずリセットされます。|  
|`Total Processing Failures`|レポート サーバー Web サービスの要求処理で発生したエラーの数。|  
|`Total Rejected Threads`|非同期処理が拒否された後、同一スレッド内の同期処理として処理されたスレッドの総数。 各データ ソースは、それぞれ 1 つのスレッドで処理されます。 スレッドの容量がいっぱいになった場合、スレッドでは非同期処理が拒否され、連続して処理されます。|  
|`Total Reports Executed`|サービスの開始後に、正常に実行されたレポートの総数。 このカウンターは、 [!INCLUDE[vstecasp](../../../includes/vstecasp-md.md)] によってレポート サーバー Web サービスが停止すると必ずリセットされます。|  
|`Total Requests`|サービスの開始後、レポート サーバーに対して行われたすべての要求の総数。 このカウンターは、 [!INCLUDE[vstecasp](../../../includes/vstecasp-md.md)] によってレポート サーバー Web サービスが停止すると必ずリセットされます。|  
  
##  <a name="bkmk_windowsservice"></a>MSRS 2014 Windows サービスのパフォーマンスカウンター  
 `MSRS 2014 Windows Service` パフォーマンス オブジェクトは、レポート サーバー Windows サービスを監視します。 このパフォーマンス オブジェクトには複数のカウンターが含まれ、スケジュールされた操作を介して開始されるレポート処理の追跡に使用されます。 スケジュールされた操作には、サブスクリプションと配信、レポート実行スナップショット、およびレポート履歴を含めることができます。 設定したカウンターは [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] のすべてのインスタンスに適用することも、特定のインスタンスにだけ適用することもできます。  
  
 次の表は、`MSRS 2014 Windows Service` パフォーマンス オブジェクトに含まれているカウンターの一覧です。  
  
|カウンター|Description|  
|-------------|-----------------|  
|`Active Sessions`|レポート サーバー データベースに格納されるアクティブ セッションの数です。 このカウンターは、レポート サブスクリプションから生成される使用可能なすべてのブラウザー セッションの累積数と、セッションがまだアクティブかどうかを示します。|  
|`Cache Flushes/Sec`|1 秒あたりのキャッシュ フラッシュ回数。|  
|`Cache Hits/Sec`|キャッシュされたレポートに対する 1 秒あたりの要求数。 これはレポートの再表示の要求であり、キャッシュから直接処理されるレポートの要求ではありません (このトピックの `Total Cache Hits` を参照してください)。|  
|`Cache Hits/Sec (Semantic Models)`|キャッシュされたモデルに対する 1 秒あたりの要求数。|  
|`Cache Misses/Sec`|キャッシュからレポートを返すことに失敗した 1 秒あたりの要求数。 このカウンターは、キャッシュに使用するリソース (ディスクまたはメモリ) が十分であるかどうかを判断する場合に使用します。|  
|`Cache Misses/Sec (Semantic Models)`|キャッシュからモデルを返すことに失敗した 1 秒あたりの要求数。 このカウンターは、キャッシュに使用するリソース (ディスクまたはメモリ) が十分であるかどうかを判断する場合に使用します。|  
|`Delivers/Sec`|1 秒間に任意の配信拡張機能からレポートが配信される回数です。|  
|`Events/Sec`|1 秒間に処理されるイベントの数です。 監視対象のイベントには、`SnapshotUpdated` や `TimedSubscription` があります。|  
|`First Session Requests/Sec`|1 秒あたりに作成された新しいレポート実行セッションの数。|  
|`Memory Cache Hits/Sec`|メモリ内キャッシュからレポートが取得される 1 秒あたりの回数。 *メモリ内キャッシュ* は、CPU メモリにレポートを格納するキャッシュの一部です。 メモリ内キャッシュが使用されると、レポート サーバーでは、キャッシュ済みコンテンツに対して [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] へのクエリが実行されません。|  
|`Memory Cache Misses/Sec`|メモリ内キャッシュからレポートが取得できない 1 秒あたりの回数です。|  
|`Next Session Requests/Sec`|既存のセッション内 (セッション スナップショットから表示されるレポートなど) で開いているレポートに対する 1 秒あたりの要求の数。|  
|`Report Requests`|現在アクティブで、レポート サーバーで管理されているレポートの数。 このカウンターを使用して、キャッシュ方法を評価します。 生成されるレポートと比べて多くの要求が行われていることがあります。|  
|`Reports Executed/Sec`|1 秒あたりに正常に生成されたレポートの数です。|  
|`Requests/Sec`|1 秒間にレポート サーバー サービスが正常に処理した要求の総数。|  
|`Snapshot Updates/Sec`|1 秒あたりのレポート実行スナップショットの更新回数の合計。|  
|`Total App Domain Recycles`|レポート サーバー Windows サービス開始後のアプリケーション ドメイン サイクルの合計数です。|  
|**Total Cache Flushes**|サービスの開始後に、レポート サーバーのキャッシュが更新された合計回数です。 このカウンターは、アプリケーション ドメインが再利用される際にリセットされます。 「`Cache Flushes/Sec`」を参照してください。|  
|`Total Cache Hits`|レポート サーバー Windows サービス開始後にキャッシュから直接処理されたレポートの要求の合計数です。 このカウンターは、アプリケーション ドメインが再利用される際にリセットされます。 「`Cache Hits/Sec`」を参照してください。|  
|`Total Cache Hits (Semantic Models)`|レポート サーバー Windows サービス開始後にキャッシュから直接処理されたモデルの要求の合計数です。 このカウンターは、アプリケーション ドメインが再利用される際にリセットされます。 「`Cache Hits/Sec`」を参照してください。|  
|`Total Cache Misses`|レポート サーバー Windows サービスの開始後に、レポートがキャッシュから返されなかった合計回数です。 このカウンターは、アプリケーション ドメインが再利用される際にリセットされます。 「`Cache Misses/Sec`」を参照してください。|  
|`Total Cache Misses (Semantic Models)`|レポート サーバー Windows サービスの開始後に、モデルがキャッシュから返されなかった合計回数。 このカウンターは、アプリケーション ドメインが再利用される際にリセットされます。|  
|`Total Deliveries`|すべての配信拡張機能に対して、スケジュールおよび配信のプロセッサによって配信されたレポートの総数です。 このカウンターは、アプリケーション ドメインが再利用される際にリセットされます。|  
|`Total Events`|レポート サーバー Windows サービス開始後のイベントの合計数です。 このカウンターは、アプリケーション ドメインが再利用される際にリセットされます。|  
|`Total Memory Cache Hits`|レポート サーバー Windows サービス開始後にメモリ内キャッシュから返された、キャッシュされたレポートの要求の合計数です。 このカウンターは、アプリケーション ドメインが再利用される際にリセットされます。|  
|`Total Memory Cache Misses`|サービスの開始後に発生したメモリ内キャッシュのキャッシュ ミスの総数。 このカウンターは、アプリケーション ドメインが再利用される際にリセットされます。|  
|`Total Processing Failures`|レポート サーバー Windows サービスの要求処理で発生したエラーの数。|  
|`Total Rejected Threads`|非同期処理が拒否された後、同一スレッド内の同期処理として処理されたスレッドの総数。 負荷が中程度または高い状況では、このカウンターは徐々に増加します。|  
|`Total Reports Executed`|実行されたレポートの総数です。|  
|`Total Requests`|サービスの開始後に、正常に実行されたレポートの総数。 このカウンターは、アプリケーション ドメインが再利用される際にリセットされます。|  
|`Total Snapshot Updates`|レポート実行スナップショットの更新回数の合計。|  
  
##  <a name="bkmk_powershell"></a> PowerShell コマンドレットを使用して一覧を取得する  
 ![PowerShell 関連コンテンツ](../media/rs-powershellicon.jpg "PowerShell 関連コンテンツ")次の Windows PowerShell スクリプトでは、CounterSetName が "msr" で始まるカウンターセットが返されます。  
  
```powershell
Get-Counter -ListSet msr*  
```  
  
 次の Windows PowerShell スクリプトは CounterSetName のパフォーマンス カウンターの一覧を返します  
  
```powershell
(Get-Counter -ListSet "MSRS 2014 Windows Service").Paths  
```  
  
## <a name="see-also"></a>「  
 [レポート サーバーのパフォーマンスの監視](monitoring-report-server-performance.md)   
 [Msrs 2014 Web Service sharepoint モードおよび msrs 2014 Windows service sharepoint mode パフォーマンスオブジェクト&#40;のパフォーマンスカウンター (sharepoint モード&#41; ](../report-server/performance-counters-msrs-2011-web-service-performance-objects.md)   
 [Performance Counters for the ReportServer:Service  and ReportServerSharePoint:Service Performance Objects](performance-counters-reportserver-service-performance-objects.md)  
