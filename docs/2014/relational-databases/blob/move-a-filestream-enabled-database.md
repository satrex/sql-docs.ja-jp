---
title: FILESTREAM が有効なデータベースの移動 | Microsoft Docs
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.suite: ''
ms.technology: filestream
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- FILESTREAM [SQL Server], moving a FILESTREAM-enabled database
ms.assetid: dd4d270d-9283-431a-aa6b-e571fced1893
caps.latest.revision: 10
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: cf263ca51d07319acb0b3284191a787cac44b734
ms.sourcegitcommit: c18fadce27f330e1d4f36549414e5c84ba2f46c2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/02/2018
ms.locfileid: "37316562"
---
# <a name="move-a-filestream-enabled-database"></a>FILESTREAM が有効なデータベースの移動
  このトピックでは、FILESTREAM が有効なデータベースを移動する方法について説明します。  
  
> [!NOTE]  
>  このトピックの例では、「 [FILESTREAM が有効なデータベースを作成する方法](create-a-filestream-enabled-database.md)」で作成した Archive データベースが必要です。  
  
### <a name="to-move-a-filestream-enabled-database"></a>FILESTREAM が有効なデータベースを移動するには  
  
1.  [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]で、 **[新しいクエリ]** をクリックしてクエリ エディターを開きます。  
  
2.  次の [!INCLUDE[tsql](../../includes/tsql-md.md)] スクリプトをクエリ エディターにコピーして、 **[実行]** をクリックします。 このスクリプトは、FILESTREAM データベースで使用される物理データベース ファイルの場所を表示します。  
  
    ```tsql  
    USE Archive  
    GO  
    SELECT type_desc, name, physical_name from sys.database_files  
    ```  
  
3.  次の [!INCLUDE[tsql](../../includes/tsql-md.md)] スクリプトをクエリ エディターにコピーして、 **[実行]** をクリックします。 このコードでは、 `Archive` データベースがオフラインに設定されます。  
  
    ```tsql  
    USE master  
    EXEC sp_detach_db Archive  
    GO  
    ```  
  
4.  `C:\moved_location`というフォルダーを作成し、手順 2. で一覧表示されたファイルとフォルダーをそのフォルダーに移動します。  
  
5.  次の [!INCLUDE[tsql](../../includes/tsql-md.md)] スクリプトをクエリ エディターにコピーして、 **[実行]** をクリックします。 このスクリプトでは、 `Archive` データベースがオンラインに設定されます。  
  
    ```tsql  
    CREATE DATABASE Archive ON  
    PRIMARY ( NAME = Arch1,  
        FILENAME = 'c:\moved_location\archdat1.mdf'),  
    FILEGROUP FileStreamGroup1 CONTAINS FILESTREAM( NAME = Arch3,  
        FILENAME = 'c:\moved_location\filestream1')  
    LOG ON  ( NAME = Archlog1,  
        FILENAME = 'c:\moved_location\archlog1.ldf')  
    FOR ATTACH  
    GO  
    ```  
  
## <a name="see-also"></a>参照  
 [sp_detach_db &#40;Transact-SQL&#41;](/sql/relational-databases/system-stored-procedures/sp-detach-db-transact-sql)  
  
  