---
title: Microsoft Store 앱 열기
description: 이 항목에서는 windows 스토어 URI 체계에 대해 설명 합니다. 앱은이 URI 체계를 사용 하 여 스토어의 특정 페이지에 Microsoft Store 앱을 시작할 수 있습니다.
ms.assetid: 9A9C6576-1637-47D1-AC3B-D1A20D49E0FF
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1a31c762c002e711a87e99e2f97de6c26e2c8b48
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172987"
---
# <a name="launch-the-microsoft-store-app"></a>Microsoft Store 앱 열기



이 항목에서는 **ms-windows 저장소** URI 체계에 대해 설명 합니다. 앱은 [**LaunchUriAsync**](/uwp/api/windows.system.launcher.launchuriasync) 메서드를 사용 하 여이 URI 체계를 사용 하 여 스토어의 특정 페이지에 Microsoft Store 앱을 시작할 수 있습니다.

이 예제에서는 게임 페이지에서 스토어를 여는 방법을 보여 줍니다.

```cs
bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-windows-store://navigatetopage/?Id=Games"));
```

## <a name="ms-windows-store-uri-scheme-reference"></a>ms-chap: URI 체계 참조

<table>
<tr><th>Description</th><th></th><th>URI 체계</th></tr>
<tr><td>저장소의 홈 페이지를 시작 합니다.</td><td /><td>ms-windows 스토어:/home</td></tr>
<tr><td>저장소에서 최상위 수준 세로를 시작 합니다.<p>참고: 모든 사용자가 모든 감축할에 액세스할 수 있는 것은 아닙니다.</p>
</td><td /><td>
<p>//navigatetopage/:? Id = 앱 </p>
<p>//navigatetopage/:? Id = 게임</p>
<p>//navigatetopage/:? Id = 음악</p>
<p>//navigatetopage/:? Id = 비디오</p>
<p>//navigatetopage/:? Id = LOB</p>
</td>
</tr>
<tr>
<td rowspan="4">제품에 대 한 PDP (제품 정보 페이지)를 시작 합니다. <p>스토어 ID는 Windows 10의 고객에 게 권장 되며 모든 OS 버전에서 작동 하지만 이전에 수행 하는 방법 (예: PFN)은 계속 지원 됩니다.</p>
<p>이러한 값은 각 앱에 대 한 앱 관리 섹션의 <a href="https://docs.microsoft.com/windows/uwp/publish/view-app-identity-details">앱 id</a> 페이지에서 <a href="https://partner.microsoft.com/dashboard">파트너 센터</a> 에서 찾을 수 있습니다.</p>
</td>
<td>
저장소 ID <p>좋습니다.</p>
</td>
<td>
<p>ms-windows 스토어:/? ProductId = 9WZDNCRFHVJL</p>
</td>
</tr>
<tr>
<td>PFN (패키지 패밀리 이름)</td>
<td>ms-windows 스토어:/? PFN = Microsoft OneNote_8wekyb3d8bbwe
</td>
</tr>
<tr>
<td>제품 ID (Windows Phone 4.x/.x)</td>
<td>ms-windows 스토어:/? PhoneAppId = ca05b3ab-f157-450c-8c49-a1f127f5e71d</td>
</tr>
<tr>
<td>제품 ID (Windows 8.x)</td>
<td>ms-windows 스토어:/? AppId = f022389f-f3a6-417e-ad23-704fbdf57117
</td>
</tr>
<tr>
<td rowspan="4">제품에 대 한 검토 작성 환경을 시작 합니다.</td>
<td>저장소 ID <p>좋습니다.</p></td>
<td>//review/:? ProductId = 9WZDNCRFHVJL </td>
</tr>
<tr>
<td>PFN (패키지 패밀리 이름)</td>
<td>//review/:? PFN = Microsoft OneNote_8wekyb3d8bbwe
</td>
</tr>
<tr>
<td>제품 ID (Windows Phone 4.x/.x)</td>
<td>//reviewapp/:? AppId = ca05b3ab-f157-450c-8c49-a1f127f5e71d </td>
</tr>
<tr>
<td>제품 ID (Windows 8.x)</td>
<td>//review/:? AppId = f022389f-f3a6-417e-ad23-704fbdf57117 </td>
</tr>
<tr>
<td>파일 확장명과 연결 된 제품에 대 한 검색을 시작 합니다. </td>
<td />
<td>//assoc/:? FileExt = pdf
</td>
</tr>
<tr>
<td>프로토콜과 연결 된 제품에 대 한 검색을 시작 합니다.</td>
<td />
<td>//assoc/:? Protocol = ms 단어 </td>
</tr>
<tr>
<td>하나 이상의 태그와 연결 된 제품에 대 한 검색을 시작 합니다. 태그는 쉼표로 구분 해야 합니다.
</td>
<td />
<td>
<p>//assoc/:? 태그 = Photos_Rich_Media_Edit </p>
<p>//assoc/:? 태그 = Photos_Rich_Media_Edit, Camera_Capture_App</p>
</td>
</tr>
<tr>
<td>
지정 된 쿼리에 대 한 검색을 시작 합니다. 쿼리의 공백이 허용 됩니다.
</td>
<td />
<td>ms-windows 스토어:/search/? query = OneNote </td>
</tr>
<tr>
<td>범주에서 제품 검색을 시작 합니다.</td>
<td />
<td>
<p>//browse/: 형식 = 앱 &amp; cat = 생산성</p>
<p>//browse/:? type = Apps &amp; cat = Health + %26 + 적합성 </p>
</td>
</tr>
<tr>
<td>지정 된 게시자에서 제품 검색을 시작 합니다. 이름에 공백을 사용할 수 있습니다.
</td>
<td />
<td>//publisher/? name = Microsoft Corporation
</td>
</tr>
<tr><td>다운로드 및 업데이트 페이지를 시작 합니다.</td>
<td />
<td>//downloadsandupdates: </td>
</tr>
<tr>
<td>저장소 설정 페이지를 시작 합니다.</td>
<td />
<td>ms-windows 스토어:/설정 </td>
</tr>
</table>

 

 