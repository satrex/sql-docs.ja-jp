---
title: ビッグデータクラスターとは
titleSuffix: SQL Server Big Data Clusters
description: Kubernetes で実行される [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] (プレビュー) について説明し、リレーショナルデータと HDFS データの両方に対してスケールアウトオプションを提供します。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 08/28/2019
ms.topic: overview
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: cffb1bcf353932183703f810c876b6adacaa7bbf
ms.sourcegitcommit: 873504573569546eb7223d3afefd89bb3d422d6f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/15/2019
ms.locfileid: "72359529"
---
# <a name="what-are-includebig-data-clusters-2019includesssbigdataclusters-ss-novermd"></a>@No__t-0 とは何ですか?

[!INCLUDE[tsql-appliesto-ssver15-xxxx-xxxx-xxx](../includes/tsql-appliesto-ssver15-xxxx-xxxx-xxx.md)]

@No__t 0 以降、[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] を使用すると、Kubernetes で実行されている SQL Server、Spark、HDFS コンテナーのスケーラブルなクラスターをデプロイできます。 これらのコンポーネントを並行して実行し、Transact-SQL または Spark からのビッグ データの読み取り、書き込み、処理を行うことができるので、高価値のリレーショナル データと大量のビッグ データを簡単に組み合わせて分析できます。

最新リリースの新機能と既知の問題の詳細については、[リリース ノート](release-notes-big-data-cluster.md)を参照してください。

[!INCLUDE [Limited public preview note](../includes/big-data-cluster-preview-note.md)]

## <a name="scenarios"></a>シナリオ

[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] は、ビッグデータとの対話方法を柔軟に提供します。 外部データ ソースに対してクエリを実行する、SQL Server が管理する HDFS にビッグ データを格納する、またはクラスターを介して複数の外部データ ソースのデータに対してクエリを実行することができます。 このデータは、AI、機械学習、その他の分析タスクに使用できます。 以下に、これらのシナリオについて詳しく説明します。

### <a name="data-virtualization"></a>データの仮想化

[SQL Server PolyBase](../relational-databases/polybase/polybase-guide.md)を利用することにより、@no__t はデータを移動またはコピーせずに外部データソースに対してクエリを実行できます。 [!INCLUDE[SQL Server 2019](../includes/sssqlv15-md.md)] では、データ ソースに新しいコネクタが導入されています。

![データの仮想化](media/big-data-cluster-overview/data-virtualization.png)

### <a name="data-lake"></a>データ レイク

SQL Server ビッグ データ クラスターには、スケーラブルな HDFS *記憶域プール*が含まれています。 これは、複数の外部ソースから取り込まれた可能性があるビッグ データを格納するために使用できます。 ビッグ データ クラスターの HDFS にビッグ データが格納されたら、そのデータを分析してクエリを実行し、リレーショナル データと組み合わせることができます。

![データ レイク](media/big-data-cluster-overview/data-lake.png)

### <a name="scale-out-data-mart"></a>スケールアウト データ マート

[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] は、データの分析のパフォーマンスを向上させるために、スケールアウトコンピューティングとストレージを提供します。 さまざまなソースのデータを取り込み、さらに分析するためにキャッシュとして*データ プール* ノード全体に分散することができます。

![データ マート](media/big-data-cluster-overview/data-mart.png)

### <a name="integrated-ai-and-machine-learning"></a>AI と機械学習の統合

[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] HDFS ストレージプールとデータプールに格納されているデータに対して AI と機械学習のタスクを有効にします。 R、Python、Scala、または Java を使用し、Spark だけでなく SQL Server の組み込みの AI ツールを使用できます。

![AI と ML](media/big-data-cluster-overview/ai-ml-spark.png)

### <a name="management-and-monitoring"></a>管理と監視

管理と監視は、コマンド ライン ツール、API、ポータル、および動的管理ビューを組み合わせて実行できます。

Azure Data Studio を使用すると、ビッグ データ クラスターに対してさまざまなタスクを実行できます。 これは、新しい **SQL Server 2019 拡張機能 (プレビュー)** によって実現します。 この拡張機能には、次の機能があります。

- 一般的な管理タスク用の組み込みスニペット。
- HDFS の参照、ファイルのアップロード、ファイルのプレビュー、およびディレクトリの作成を行う機能。
- Jupyter 互換ノートブックを作成、開く、および実行する機能。
- 外部データ ソースの作成を簡易化するデータ仮想化ウィザード。

## <a id="architecture"></a> アーキテクチャ

SQL Server ビッグ データ クラスターは、[Kubernetes](https://kubernetes.io/docs/concepts/) によって調整された Linux コンテナーのクラスターです。

### <a name="kubernetes-concepts"></a>Kubernetes の概念

Kubernetes はオープン ソースのコンテナー オーケストレーターであり、必要に応じてコンテナーの展開を拡張できます。 次の表では、重要な Kubernetes 用語をいくつか定義します。

|||
|:--|:--|
| **Cluster** | Kubernetes クラスターは、ノードと呼ばれる一連のマシンです。 1 つのノードがクラスターを制御し、マスター ノードに指定されます。残りのノードはワーカー ノードです。 Kubernetes マスターは、ワーカー間で作業を分散し、クラスターの正常性を監視する役割を担います。 |
| **[Node]** | ノードによって、コンテナー化されたアプリケーションが実行されます。 物理マシンまたは仮想マシンのいずれかです。 Kubernetes クラスターには、物理マシンと仮想マシンの両方のノードを含めることができます。 |
| **ポッド** | ポッドは、Kubernetes のアトミック展開単位です。 ポッドは、アプリケーションの実行に必要な 1 つ以上のコンテナーと関連するリソースの論理グループです。 各ポッドはノード上で実行されます。ノードでは、1 つ以上のポッドを実行できます。 Kubernetes マスターによって、クラスター内のノードにポッドが自動的に割り当てられます。 |
| &nbsp; ||

@No__t-0 の場合、Kubernetes は @no__t の状態を示します。Kubernetes は、クラスターノードの構築と構成を行い、ポッドをノードに割り当て、クラスターの正常性を監視します。

### <a name="big-data-clusters-architecture"></a>ビッグ データ クラスターのアーキテクチャ

次の図は、SQL Server 用のビッグ データ クラスターのコンポーネントを示しています。

![アーキテクチャの概要](media/big-data-cluster-overview/architecture-diagram-overview.png)

### <a id="controlplane"></a> コントローラー

コントローラーには、クラスターの管理とセキュリティ機能があります。 これには、制御サービス、構成ストア、および Kibana、Grafana、エラスティック検索などのその他のクラスターレベルサービスが含まれています。

### <a id="computeplane"></a> コンピューティング プール

コンピューティング プールは、クラスターにコンピューティング リソースを提供します。 これには SQL Server on Linux ポッドを実行するノードが含まれます。 コンピューティング プール内のポッドは、特定の処理タスクのために *SQL コンピューティング インスタンス*に分割されます。 

### <a id="dataplane"></a> データ プール

データ プールは、データの永続化とキャッシュに使用されます。 データ プールは、SQL Server on Linux を実行している 1 つ以上のポッドで構成されます。 これは、SQL クエリまたは Spark ジョブからデータを取り込むために使用されます。 SQL Server ビッグ データ クラスターのデータ マートは、データ プールに保持されます。 

### <a name="storage-pool"></a>記憶域プール

記憶域プールは、SQL Server on Linux、Spark、および HDFS で構成される記憶域プール ポッドで構成されます。 SQL Server ビッグ データ クラスター内のすべての記憶域ノードは、HDFS クラスターのメンバーです。

> [!TIP]
> ビッグデータクラスターのアーキテクチャとインストールの詳細については、「[ワークショップ: Microsoft [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] のアーキテクチャ](https://github.com/Microsoft/sqlworkshops/tree/master/sqlserver2019bigdataclusters)」を参照してください。

## <a name="next-steps"></a>次のステップ

ビッグデータクラスター SQL Server のデプロイの詳細については、「 [SQL Server ビッグデータクラスターの概要](deploy-get-started.md)」を参照してください。
