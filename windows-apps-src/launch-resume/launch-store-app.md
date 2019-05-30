---
title: Microsoft Store 앱 열기
description: 이 항목에서는 ms-windows-store URI 체계에 대해 설명합니다. 앱이 URI 체계를 사용 하 여 저장소의 특정 페이지에 Microsoft Store 앱을 시작 수 있습니다.
ms.assetid: 9A9C6576-1637-47D1-AC3B-D1A20D49E0FF
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: fd0e7137f31a8f1620f7937b52efe1ca84a6b99a
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370780"
---
# <a name="launch-the-microsoft-store-app"></a>Microsoft Store 앱 열기



이 항목에 설명 합니다 **ms-windows-스토어:** URI 체계입니다. 앱이 URI 체계를 사용 하 여 사용 하 여 저장소의 특정 페이지에 Microsoft Store 앱을 시작 하려면 수는 [ **LaunchUriAsync** ](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync) 메서드.

다음 예는 게임에 연결되는 Store 페이지를 여는 방법을 보여줍니다.

```cs
bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-windows-store://navigatetopage/?Id=Games"));
```

## <a name="ms-windows-store-uri-scheme-reference"></a>ms-windows-store: URI 체계 참조입니다.

<table>
<tr><th>설명</th><th></th><th>URI 스키마</th></tr>
<tr><td>스토어의 홈 페이지를 실행합니다.</td><td /><td>ms-windows-store://home</td></tr>
<tr><td>스토어의 최상위 범주를 실행합니다.<p>참고: 일부 사용자가 모든 감축할에 대 한 액세스.</p>
</td><td /><td>
<p>ms-windows-store://navigatetopage/?Id=Apps </p>
<p>ms-windows-store://navigatetopage/?Id=Games</p>
<p>ms-windows-store://navigatetopage/?Id=Music</p>
<p>ms-windows-store://navigatetopage/?Id=Video</p>
<p>ms-windows-store://navigatetopage/?Id=LOB</p>
</td>
</tr>
<tr>
<td rowspan="4">제품에 대한 PDP(제품 세부 정보 페이지)를 실행합니다. <p>Store ID Windows 10에서 고객에 게 권장 되 고 모든 OS 버전 있지만 그렇게 하는 이전 가지 작동 (예: PFN) 계속 지원 됩니다.</p>
<p>이러한 값을 찾을 수 있습니다 <a href="https://partner.microsoft.com/dashboard">파트너 센터</a> 에 <a href="https://docs.microsoft.com/windows/uwp/publish/view-app-identity-details">앱 id</a> 페이지의 각 앱에 대 한 앱 관리 섹션입니다.</p>
</td>
<td>
Store ID <p>좋습니다.</p>
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
<td>ms-windows-store://pdp/?PhoneAppId=ca05b3ab-f157-450c-8c49-a1f127f5e71d</td>
</tr>
<tr>
<td>제품 ID(Windows 8.x)</td>
<td>ms-windows-store://pdp/?AppId=f022389f-f3a6-417e-ad23-704fbdf57117
</td>
</tr>
<tr>
<td rowspan="4">제품에 대한 리뷰 작성 환경을 실행합니다.</td>
<td>Store ID <p>좋습니다.</p></td>
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

 

 
