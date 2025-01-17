---
title: syssubscriptions (システム ビュー) (Transact-SQL) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- syssubscriptions_TSQL
- syssubscriptions
dev_langs:
- TSQL
helpviewer_keywords:
- syssubscriptions view
ms.assetid: c9613858-9512-43a9-aa53-7ee8064f064c
author: stevestein
ms.author: sstein
ms.openlocfilehash: 517d9359085f7cb4bc4c94eb941981a09ca06eef
ms.sourcegitcommit: 43c3d8939f6f7b0ddc493d8e7a643eb7db634535
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/14/2019
ms.locfileid: "72304788"
---
# <a name="syssubscriptions-system-view-transact-sql"></a>syssubscriptions (システム ビュー) (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  **Syssubscriptions**ビューは、サブスクリプション情報を公開します。 このビューは、ディストリビューション データベースに保存されます。  
  
|列名|データ型|説明|  
|-----------------|---------------|-----------------|  
|**artid**|**int**|サブスクライブされるアーティクルの一意な ID。|  
|**srvid**|**smallint**|サブスクライバーのサーバー ID。|  
|**dest_db**|**sysname**|サブスクリプション データベースの名前。|  
|**status**|**tinyint**|サブスクリプションの状態。<br /><br /> **0** = 非アクティブ。<br /><br /> **1** = サブスクライブ済み。<br /><br /> **2** = アクティブ。|  
|**sync_type**|**tinyint**|初期同期の種類。<br /><br /> **1** = 自動。<br /><br /> **2** = なし。|  
|**login_name**|**sysname**|サブスクリプションを追加するためパブリッシャーに接続するときのログイン名。|  
|**subscription_type**|**int**|サブスクリプションの種類です。<br /><br /> **0** = プッシュ-ディストリビューションエージェントはディストリビューターで実行されます。<br /><br /> **1** = プル-ディストリビューションエージェントはサブスクライバーで実行されます。|  
|**distribution_jobid**|**binary(16)**|サブスクリプションの同期で使用されるディストリビューション エージェント ジョブの識別子。|  
|**timestmap**|**timestamp**|サブスクリプションを作成した日付と時刻。|  
|**update_mode**|**tinyint**|更新モード。<br /><br /> **0** = 読み取り専用。<br /><br /> **1** = 即時更新。|  
|**loopback_detection**|**bit**|双方向トランザクション レプリケーション トポロジの一部であるサブスクリプションに適用されます。 ディストリビューション エージェントが、サブスクライバーで発生したトランザクションをサブスクライバーに戻すかどうかを示します。<br /><br /> **0** = 返送します。<br /><br /> **1** = を返しません。|  
|**queued_reinit**|**bit**|アーティクルが初期化または再初期化の対象としてマークされているかどうかを指定します。 値**1**は、サブスクライブされたアーティクルが初期化または再初期化のマークが付けられることを示します。|  
|**nosync_type**|**tinyint**|サブスクリプションの初期化の種類。<br /><br /> **0** = 自動 (スナップショット)<br /><br /> **1** = レプリケーションのサポートのみ<br /><br /> **2** = バックアップを使用した初期化<br /><br /> **3** = ログシーケンス番号 (LSN) からの初期化<br /><br /> 詳細については、 [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md)の **@no__t 1sync_type**パラメーターを参照してください。<br /><br /> **3** = [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**srvname**|**sysname**|サブスクライバーの名前です。|  
  
## <a name="see-also"></a>参照  
 [レプリケーション テーブル &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [レプリケーション ビュー &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [syssubscriptions &#40;Transact-SQL&#41;](../../relational-databases/system-tables/syssubscriptions-transact-sql.md)  
  
  
