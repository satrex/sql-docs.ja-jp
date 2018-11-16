---
title: Azure Kubernetes サービスを SQL Server 2019 ビッグ データ クラスターのデプロイの構成 |Microsoft Docs
description: SQL Server 2019 ビッグ データ クラスター (プレビュー) のデプロイ用 Azure Kubernetes Service (AKS) を構成する方法について説明します。
author: rothja
ms.author: jroth
manager: craigg
ms.date: 11/06/2018
ms.topic: conceptual
ms.prod: sql
ms.openlocfilehash: 07ee0ac0db742eca9a55decfcd78cb76b75e0160
ms.sourcegitcommit: cb73d60db8df15bf929ca17c1576cf1c4dca1780
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2018
ms.locfileid: "51221658"
---
# <a name="configure-azure-kubernetes-service-for-sql-server-2019-preview-deployments"></a>SQL Server 2019 (プレビュー) の展開用 Azure Kubernetes サービスを構成します。

この記事では、SQL Server 2019 ビッグ データ クラスター (プレビュー) のデプロイ用 Azure Kubernetes Service (AKS) を構成する方法について説明します。 

AKS によって、作成、構成、およびコンテナー化されたアプリケーションを実行するための Kubernetes クラスターであらかじめ構成されている仮想マシンのクラスターの管理を簡単にできます。 これにより、既存のスキルを使用して、または Microsoft Azure でコンテナー ベースのアプリケーション展開および管理する、コミュニティの専門知識の増加し続けるの本文に描画することができます。

この記事では、Azure CLI を使用して AKS で Kubernetes をデプロイする手順について説明します。 Azure サブスクリプションを持っていない場合は、開始する前に、無料のアカウントを作成します。

> [!TIP] 
> AKS と SQL Server の両方のビッグ データ クラスターをデプロイするサンプル python スクリプトについては、次を参照してください。[ビッグ データ クラスター Azure Kubernetes Service (AKS) で SQL Server 展開](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/deployment/aks)します。

## <a name="prerequisites"></a>前提条件

- 検証の基本的なシナリオの中に最適なエクスペリエンスのため、AKS の環境で少なくとも 4 つの Vcpu と 32 GB のメモリが各 (マスター) だけでなく、少なくとも 3 つのエージェントの Vm はお勧めします。 Azure インフラストラクチャ Vm のサイズの複数のオプションを参照してください[ここ](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sizes)の展開を計画しているリージョンの選択項目。
  
- このセクションでは、ことが必要です実行、Azure CLI バージョン 2.0.4 以降。 インストールまたはアップグレードを表示する必要がある場合[Azure CLI 2.0 のインストール](https://docs.microsoft.com/cli/azure/install-azure-cli)します。 実行`az --version`必要な場合は、バージョンを確認します。

- インストール[kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) 1.10 サーバーおよびクライアントの両方のバージョンの最小値にします。 Kubectl クライアントを特定のバージョンをインストールする場合は、「 [kubectl curl を使用してバイナリをインストール](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl)します。 For AKS を使用する必要があります。 `--kubernetes-version` 、既定値以外のバージョンを指定するパラメーター。

> [!NOTE]
クライアント/サーバーのバージョンのスキューされていることは、1 のマイナー バージョンの +/-サポートされています。 Kubernetes のドキュメントの状態を"が、クライアント、マスターから複数の傾斜のマイナー バージョンにする必要がありますに 1 つのマイナー バージョンによって、マスターがあります。 たとえば、v1.3 マスター v1.1、バージョン 1.2、および v1.3 のノードを使用する必要があり、v1.2、v1.3、v1.4 クライアントを操作する必要があります。" 詳細については、次を参照してください。 [Kubernetes がサポートされるのは、リリースと傾斜コンポーネント](https://github.com/kubernetes/community/blob/master/contributors/design-proposals/release/versioning.md#supported-releases-and-component-skew)します。

また、`az aks kubernetes install-cli`バージョンで kubectl クライアントをインストールを下げる必要 1.10 します。 手順については、適切なバージョンの kubectl クライアントをインストールする前に従います。

## <a name="create-a-resource-group"></a>リソース グループを作成します。

Azure リソース グループは、azure リソースのデプロイし、管理論理グループです。 次の手順では、Azure にサインインし、AKS クラスターのリソース グループを作成します。

> [!TIP]
> Windows を使用している場合は、PowerShell を使用して、残りの手順を実行します。

1. コマンド プロンプトで次のコマンドを実行し、Azure サブスクリプションへのログインの指示に従います。

    ```bash
    az login
    ```

1. 複数のサブスクリプションがある場合は、次のコマンドを実行してすべてのサブスクリプションを表示できます。

   ```bash
   az account list
   ```

1. 別のサブスクリプションに変更したい場合は、このコマンドを実行できます。

   ```bash
   az account set --subscription <subscription id>
   ```

1. 使用してリソース グループを作成、 **az グループ作成**コマンド。 次の例は、という名前のリソース グループを作成します。`sqlbigdatagroup`で、`westus2`場所。

   ```bash
   az group create --name sqlbigdatagroup --location westus2
   ```

## <a name="create-a-kubernetes-cluster"></a>Kubernetes クラスターを作成します。

1. 使用して AKS で Kubernetes クラスターを作成、 [az aks 作成](https://docs.microsoft.com/cli/azure/aks)コマンド。 次の例では、という名前の Kubernetes クラスターを作成する*kubcluster* 1 つの Linux マスター ノードと 2 つの Linux エージェント ノード。 前のセクションで使用したのと同じリソース グループで、AKS クラスターを作成することを確認します。

    ```bash
   az aks create --name kubcluster \
    --resource-group sqlbigdatagroup \
    --generate-ssh-keys \
    --node-vm-size Standard_E4s_v3 \
    --node-count 3 \
    --kubernetes-version 1.10.8
    ```

    増やすことも変更することで、既定のエージェント数を減らす、`--node-count <n>`場所`<n>`たいエージェント ノードの数です。

    数分後、コマンドが完了し、クラスターに関する情報を JSON 形式を返します。

1. 後で使用できるは、前のコマンドからの JSON 出力を保存します。

## <a name="connect-to-the-cluster"></a>クラスターに接続します。

1. Kubernetes クラスターに接続するように kubectl を構成するには、実行、 [az aks 資格情報の取得](https://docs.microsoft.com/cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials)コマンド。 この手順では、資格情報をダウンロードし、それらを使用する CLI kubectl を構成します。

   ```bash
   az aks get-credentials --resource-group=sqlbigdatagroup --name kubcluster
   ```

1. クラスターへの接続を確認するため、 [kubectl get](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands)コマンドをクラスター ノードの一覧を返します。  次の例は、出力を示しています。 1 つのマスターと 3 つのエージェント ノードがある場合。

   ```bash
   kubectl get nodes
   ```

## <a name="next-steps"></a>次の手順

この記事の手順では、AKS で Kubernetes クラスターを構成します。 次の手順では、SQL Server 2019 ビッグ データ クラスターをデプロイします。

[クイック スタート: Azure Kubernetes Service (AKS) での SQL Server のビッグ データ クラスターをデプロイします。](quickstart-big-data-cluster-deploy.md)