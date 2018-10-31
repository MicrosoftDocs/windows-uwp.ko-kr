---
author: TylerMSFT
title: Microsoft Store 앱 실행
description: 이 항목에서는 ms-windows-store URI 스키마에 대해 설명합니다. 앱은 스토어의 특정 페이지를 Microsoft Store 앱을 시작 하려면이 URI 체계를 사용할 수 있습니다.
ms.assetid: 9A9C6576-1637-47D1-AC3B-D1A20D49E0FF
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 42dfecd7d1bc2c298a70fec2bdebfc4c4f9dd66c
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5834914"
---
# <a name="launch-the-microsoft-store-app"></a>Microsoft Store 앱 열기



이 항목에서는 **ms-windows-store:** URI 스키마를 설명합니다. 앱은 [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) 메서드를 사용 하 여 스토어의 특정 페이지를 Microsoft Store 앱을 시작 하려면이 URI 체계를 사용할 수 있습니다.

이 예제에서는 스토어에 게임 페이지를 여는 방법을 보여 줍니다.

```cs
bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-windows-store://navigatetopage/?Id=Games"));
```

## <a name="ms-windows-store-uri-scheme-reference"></a>ms-windows-store: URI 스키마 참조

<table>
<tr><th>설명</th><th></th><th>URI 스키마</th></tr>
<tr><td>스토어의 홈 페이지를 실행합니다.</td><td /><td>ms-windows-store://home</td></tr>
<tr><td>스토어의 최상위 범주를 실행합니다.<p>참고: 모든 사용자는 일부 범주에 액세스할 수 없습니다.</p>
</td><td /><td>
<p>ms-windows-store://navigatetopage/?Id=Apps </p>
<p>ms-windows-store://navigatetopage/?Id=Games</p>
<p>ms-windows-store://navigatetopage/?Id=Music</p>
<p>ms-windows-store://navigatetopage/?Id=Video</p>
<p>ms-windows-store://navigatetopage/?Id=LOB</p>
</td>
</tr>
<tr>
<td rowspan="4">제품에 대한 PDP(제품 세부 정보 페이지)를 실행합니다. <p>저장소 ID는 Windows 10 고객에게 권장되고 모든 OS 버전에서 작동하지만 이전 방식(예: PFN)도 여전히 지원됩니다.</p>
<p>이러한 값은 각 앱에 대한 앱 관리 섹션의 <a href="https://msdn.microsoft.com/library/windows/apps/mt148561.aspx">앱 ID</a> 페이지에 있는 Windows 개발자 센터 대시보드에서 확인할 수 있습니다.</p>
</td>
<td>
스토어 ID <p>(권장)</p>
</td>
<td>
<p>ms-windows-store://pdp/?ProductId=9WZDNCRFHVJL</p>
</td>
</tr>
<tr>
<td>PFN(패키지 패밀리 이름)</td>
<td>ms-windows-store://pdp/?PFN= Microsoft.Office.OneNote_8wekyb3d8bbwe
</td>
</tr>
<tr>
<td>제품 ID(Windows Phone 7.x/8.x)</td>
<td>ms-windows-store://pdp/?PhoneAppId=ca05b3ab-f157-450c-8c49-a1f127f5e71d </td>
</tr>
<tr>
<td>제품 ID(Windows 8.x)</td>
<td>ms-windows-store://pdp/?AppId=f022389f-f3a6-417e-ad23-704fbdf57117
</td>
</tr>
<tr>
<td rowspan="4">제품에 대한 리뷰 작성 환경을 실행합니다.</td>
<td>스토어 ID <p>(권장)</p></td>
<td>ms-windows-store://review/?ProductId=9WZDNCRFHVJL </td>
</tr>
<tr>
<td>PFN(패키지 패밀리 이름)</td>
<td>ms-windows-store://review/?PFN= Microsoft.Office.OneNote_8wekyb3d8bbwe
</td>
</tr>
<tr>
<td>제품 ID(Windows Phone 7.x/8.x)</td>
<td>ms-windows-store://reviewapp/?AppId=ca05b3ab-f157-450c-8c49-a1f127f5e71d </td>
</tr>
<tr>
<td>제품 ID(Windows 8.x)</td>
<td>ms-windows-store://review/?AppId=f022389f-f3a6-417e-ad23-704fbdf57117 </td>
</tr>
<tr>
<td>파일 확장명과 연결된 제품에 대한 검색을 실행합니다. </td>
<td />
<td>ms-windows-store://assoc/?FileExt=pdf
</td>
</tr>
<tr>
<td>프로토콜과 연결된 제품에 대한 검색을 실행합니다.</td>
<td />
<td>ms-windows-store://assoc/?Protocol=ms-word </td>
</tr>
<tr>
<td>하나 이상의 태그와 연결된 제품에 대한 검색을 실행합니다. 태그는 쉼표로 구분해야 합니다.
</td>
<td />
<td>
<p>ms-windows-store://assoc/?Tags=Photos_Rich_Media_Edit </p>
<p>ms-windows-store://assoc/?Tags=Photos_Rich_Media_Edit, Camera_Capture_App</p>
</td>
</tr>
<tr>
<td>
지정된 쿼리에 대한 검색을 실행합니다. 쿼리에 공백을 사용할 수 있습니다.
</td>
<td />
<td>ms-windows-store://search/?query=OneNote </td>
</tr>
<tr>
<td>범주에서 제품에 대한 검색을 실행합니다.</td>
<td />
<td>
<p>ms-windows-store://browse/?type=Apps&amp;cat=Productivity</p>
<p>ms-windows-store://browse/?type=Apps&amp;cat=Health+%26+fitness </p>
</td>
</tr>
<tr>
<td>지정된 게시자에서 제품에 대한 검색을 실행합니다. 이름에 공백을 사용할 수 있습니다.
</td>
<td />
<td>ms-windows-store://publisher/?name=Microsoft Corporation
</td>
</tr>
<tr><td>다운로드 및 업데이트 페이지를 실행합니다.</td>
<td />
<td>ms-windows-store://downloadsandupdates </td>
</tr>
<tr>
<td>스토어 설정 페이지를 실행합니다.</td>
<td />
<td>ms-windows-store://settings </td>
</tr>
</table>

 

 
