---
title: SetEmailConfiguration メソッド (WMI MSReportServer_ConfigurationSetting) | Microsoft Docs
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.suite: ''
ms.technology:
- reporting-services-native
ms.tgt_pltfrm: ''
ms.topic: conceptual
api_name:
- SetEmailConfiguration (WMI MSReportServer_ConfigurationSetting Class)
api_location:
- reportingservices.mof
topic_type:
- apiref
helpviewer_keywords:
- SetEmailConfiguration method
ms.assetid: b40a2224-2c90-4d32-892f-1fe73a0591ca
caps.latest.revision: 19
author: markingmyname
ms.author: maghan
manager: craigg
ms.openlocfilehash: aeb662c9457c2f07bda1541c5d21a59cd5cca8ee
ms.sourcegitcommit: c18fadce27f330e1d4f36549414e5c84ba2f46c2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/02/2018
ms.locfileid: "37258018"
---
# <a name="setemailconfiguration-method-wmi-msreportserverconfigurationsetting"></a>SetEmailConfiguration メソッド (WMI MSReportServer_ConfigurationSetting)
  レポート サーバーが電子メール送信に使用する電子メール配信拡張機能を構成します。  
  
## <a name="syntax"></a>構文  
  
```vb  
Public Sub SetEmailConfiguration(ByVal SendUsingSMTPServer As Boolean, _  
    ByVal SMTPServer As String, ByVal SenderEmailAddress As String, _  
    ByRef HRESULT As Int32)  
```  
  
```csharp  
public void SetEmailConfiguration (Boolean SendUsingSMTPServer,   
   string SMTPServer, string SenderEmailAddress,   
   out Int32 HRESULT);  
```  
  
## <a name="parameters"></a>パラメーター  
 *SendUsingSMTPServer*  
 サーバーが電子メール送信に SMTP サーバーを使用するかどうかを示すブール値。 この値は、true にのみ設定できます。 既定値は false です。  
  
 *SMTPServer*  
 SMTP サーバーの名前または IP アドレスを含む文字列。  
  
 *SenderEmailAddress*  
 レポート サーバーから送信された電子メールの 'From:' フィールドで使用された電子メール アドレス。  
  
 *HRESULT*  
 [out] 呼び出しの成功または失敗を示す値。  
  
## <a name="return-value"></a>戻り値  
 メソッド呼び出しの成功または失敗を示す *HRESULT* を返します。 値 0 は、メソッド呼び出しが成功したことを示します。 0 以外の値は、エラーが発生したことを示します。  
  
## <a name="remarks"></a>コメント  
 ときに、 *SendUsingSMTPServer*にパラメーターが設定されている`true`、 **SendUsing**レポート サーバーの構成ファイル内のエントリが 1 に設定します。 ときに*SendUsingSMTPServer*に設定されている`false`、 **SendUsing**エントリが構成されていません。  
  
 このメソッドでは、レポート サーバー構成ファイルの **SendUsing** エントリを 1 以外の値に設定することはできません。 SMTP メール以外を使用するようにレポート サーバーを構成するには、構成ファイルを手動で編集する必要があります。  
  
## <a name="requirements"></a>要件  
 **名前空間:** [!INCLUDE[ssRSWMInmspcA](../../includes/ssrswminmspca-md.md)]  
  
## <a name="see-also"></a>参照  
 [MSReportServer_ConfigurationSetting メンバー](msreportserver-configurationsetting-members.md)  
  
  