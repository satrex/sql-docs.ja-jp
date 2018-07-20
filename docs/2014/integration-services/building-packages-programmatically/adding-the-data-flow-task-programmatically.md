---
title: プログラムによるデータ フロー タスクの追加 | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.suite: ''
ms.technology:
- docset-sql-devref
- integration-services
ms.tgt_pltfrm: ''
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- adding Data Flow task
- SSIS data flow
- data flow task [Integration Services], adding
- MainPipe object
ms.assetid: 0ca03712-a82e-4aa7-949b-f869a8936ddf
caps.latest.revision: 46
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: e5accdf9b1ceed154397d00a2f1675a3fee07235
ms.sourcegitcommit: c18fadce27f330e1d4f36549414e5c84ba2f46c2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/02/2018
ms.locfileid: "37331942"
---
# <a name="adding-the-data-flow-task-programmatically"></a>プログラムによるデータ フロー タスクの追加
  [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] にはデータ フロー タスクというタスクがあります。これは、オブジェクト モデルでは <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper> 名前空間として表されます。 データ フロー タスクは、パッケージの実行時にデータを変換、移動する目的専用の、特殊で高性能なタスクです。 他のタスクと同様、データ フロー タスクは <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> オブジェクトによってラップされており、ランタイム エンジンはこれを単にパッケージ内のタスクの 1 つとして扱います。 ただし、データ フロー内には、別にデータ フロー コンポーネントというオブジェクトもあります。 これは変換元から変換先にデータを移動するコンポーネントで、その際に変換を経由する場合もあります。 コンポーネントには、移動の方向とデータの変換方法が定義されています。 データ フロー タスクを設定するには、タスクにコンポーネントを追加し、それを接続します。これにより、データ フローの確立と目的の変換を実行します。  
  
 データ フロー タスク内のコンポーネントには、**[データ フローの変換元]**、**[データ フロー変換]**、**[データ フローの変換先]** の 3 種類があり、[!INCLUDE[ssIS](../../includes/ssis-md.md)] デザイナー ツールボックスにこの順序で表示されています。 これらの種類は単に変換元、変換、変換先と呼ばれる場合もあります。 名前からもわかるように、データは変換元から変換へと流れ、次に変換から変換先へと流れます。 これはデータ フローの概念をごく簡単に説明したにすぎません。データ フロー タスクは柔軟で強力なタスクであり、複数の変換元を処理することも、複数の変換先に出力を送信する変換を多数接続することもできます。  
  
 データ フロー タスクをパッケージに追加する方法は、他のタスクの場合と同じです。 タスクを追加したら、コンポーネントをデータ フロー タスクに追加したり、タスク内でコンポーネントを設定および接続することにより、データ フロー タスクを設定します。  
  
## <a name="sample"></a>サンプル  
 次のコード サンプルは、データ フロー タスクをパッケージに追加する方法を示しています。 この例では、Microsoft.SqlServer.PipelineHost、Microsoft.SqlServer.DTSPipelineWrap、および Microsoft.SqlServer.ManagedDTS アセンブリへの参照が必要です。  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
using Microsoft.SqlServer.Dts.Pipeline;  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      Package p = new Package();  
      Executable e = p.Executables.Add("STOCK:PipelineTask");  
      TaskHost thMainPipe = e as TaskHost;  
      MainPipe dataFlowTask = thMainPipe.InnerObject as MainPipe;   
    }  
  }  
}  
```  
  
```vb  
Imports System.IO  
Imports Microsoft.SqlServer.Dts.Runtime  
Imports Microsoft.SqlServer.Dts.Pipeline  
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper  
  
Module Module1  
  
  Sub Main()  
  
    Dim p As Package = New Package()  
    Dim e As Executable = p.Executables.Add("STOCK:PipelineTask")  
    Dim thMainPipe As TaskHost = CType(e, TaskHost)  
    Dim dataFlowTask As MainPipe = CType(thMainPipe.InnerObject, MainPipe)  
  
  End Sub  
  
End Module  
```  
  
## <a name="external-resources"></a>外部リソース  
 blogs.msdn.com のブログ「[EzAPI – SQL Server 2012 用の更新](http://go.microsoft.com/fwlink/?LinkId=243223)」  
  
![Integration Services のアイコン (小)](../media/dts-16.gif "Integration Services アイコン (小)")**Integration Services の日付を維持します。** <br /> マイクロソフトが提供する最新のダウンロード、アーティクル、サンプル、ビデオ、およびコミュニティで選択されたソリューションについては、MSDN の [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] のページを参照してください。<br /><br /> [MSDN の Integration Services のページを参照してください。](http://go.microsoft.com/fwlink/?LinkId=136655)<br /><br /> これらの更新が自動で通知されるようにするには、ページの RSS フィードを定期受信します。  
  
## <a name="see-also"></a>参照  
 [プログラムによるデータ フロー コンポーネントの検出](../building-packages-programmatically/discovering-data-flow-components-programmatically.md)  
  
  