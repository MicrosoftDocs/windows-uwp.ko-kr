---
title: 화면 캡처 시작
description: 이 항목에서는 ms screenclip 및 ms screensketch URI 체계를 설명 합니다. 앱 캡처 & 스케치 앱을 시작 하거나 새 캡처를 열려면 이러한 URI 체계를 사용할 수 있습니다.
ms.date: 08/09/2017
ms.topic: article
keywords: windows 10, uwp, uri, 캡처, 스케치
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 06e988387f574b74d511b14a2ebca24b0a149158
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595388"
---
# <a name="launch-screen-snipping"></a>화면 캡처 시작

합니다 **ms screenclip:** 고 **ms screensketch:** URI 체계를 사용 하면 스크린샷 편집 또는 캡처를 시작할 수 있습니다.

## <a name="open-a-new-snip-from-your-app"></a>앱에서 새 캡처를 열려면

**ms screenclip:** URI에는 앱을을 자동으로 열고 새 캡처를 시작할 수 있습니다. 결과 캡처를 클립보드에 복사할 수 있지만 열기 앱으로 다시 자동으로 전달 되지 않습니다.

**ms screenclip:** 다음 매개 변수를 사용 합니다.

| 매개 변수 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- |
| 소스 | 문자열 | 아니요 | 자유 형식 문자열 URI를 시작 하는 소스를 나타내는입니다. |
| delayInSeconds | int | 아니요 | 정수 값을 1 ~ 30입니다. 전체 차이 (초), URI 호출 캡처를 시작할 때의 지연 시간을 지정 합니다. |
| callbackformat | 문자열 | 아니요 | 이 매개 변수를 사용할 수 없는 경우 |

## <a name="launching-the-snip--sketch-app"></a>캡처 및 스케치 앱 시작

**ms screensketch:** URI를 사용 하면 프로그래밍 방식으로 캡처 및 스케치 앱을 시작 및 주석에 대 한 해당 앱에서 특정 이미지를 열 수 있습니다.

**ms screensketch:** 다음 매개 변수를 사용 합니다.

| 매개 변수 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- |
| sharedAccessToken | 문자열 | 아니요 | 캡처 & 스케치 앱에서 열려는 파일을 식별 하는 토큰입니다. 검색 [SharedStorageAccessManager.AddFile](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.sharedstorageaccessmanager.addfile)합니다. 이 매개 변수를 생략 하면 앱을 열고 파일 없이 시작 됩니다. |
| secondarySharedAccessToken | 문자열 | 아니요 | 캡처에 대 한 메타 데이터를 사용 하 여 JSON 파일을 식별 하는 문자열입니다. 메타 데이터가 포함 될 수 있습니다는 **clipPoints** x, y 좌표 배열을 사용 하 여 필드 및/또는 [userActivity](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity)합니다. |
| 소스 | 문자열 | 아니요 | 자유 형식 문자열 URI를 시작 하는 소스를 나타내는입니다. |
| isTemporary | 부울 | 아니요 | 화면 스케치 True로 설정 됩니다 연 후 파일을 삭제 하려고 합니다. |

다음 예제에서는 합니다 [LaunchUriAsync](https://docs.microsoft.com/uwp/api/Windows.System.Launcher#Windows_System_Launcher_LaunchUriAsync_Windows_Foundation_Uri_) 메서드 캡처 & 스케치를 사용자의 앱에서 이미지를 보냅니다.

```csharp

bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-screensketch:edit?source=MyApp&isTemporary=false&sharedAccessToken=2C37ADDA-B054-40B5-8B38-11CED1E1A2D"));

```

다음 예제에서는 지정 된 된 파일을 **secondarySharedAccessToken** 의 매개 변수 **ms screensketch** 포함 될 수 있습니다.

```json
{
  "clipPoints": [
    {
      "x": 0,
      "y": 0
    },
    {
      "x": 2080,
      "y": 0
    },
    {
      "x": 2080,
      "y": 780
    },
    {
      "x": 0,
      "y": 780
    }
  ],
  "userActivity": "{\"$schema\":\"http://activity.windows.com/user-activity.json\",\"UserActivity\":\"type\",\"1.0\":\"version\",\"cross-platform-identifiers\":[{\"platform\":\"windows_universal\",\"application\":\"Microsoft.MicrosoftEdge_8wekyb3d8bbwe!MicrosoftEdge\"},{\"platform\":\"host\",\"application\":\"edge.activity.windows.com\"}],\"activationUrl\":\"microsoft-edge:https://support.microsoft.com/en-us/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"contentUrl\":\"https://support.microsoft.com/en-us/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"visualElements\":{\"attribution\":{\"iconUrl\":\"https://www.microsoft.com/favicon.ico?v2\",\"alternateText\":\"microsoft.com\"},\"description\":\"https://support.microsoft.com/en-us/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"backgroundColor\":\"#FF0078D7\",\"displayText\":\"Use snipping tool to capture screenshots - Windows Help\",\"content\":{\"$schema\":\"http://adaptivecards.io/schemas/adaptive-card.json\",\"type\":\"AdaptiveCard\",\"version\":\"1.0\",\"body\":[{\"type\":\"Container\",\"items\":[{\"type\":\"TextBlock\",\"text\":\"Use snipping tool to capture screenshots - Windows Help\",\"weight\":\"bolder\",\"size\":\"large\",\"wrap\":true,\"maxLines\":3},{\"type\":\"TextBlock\",\"text\":\"https://support.microsoft.com/en-us/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"size\":\"normal\",\"wrap\":true,\"maxLines\":3}]}]}},\"isRoamable\":true,\"appActivityId\":\"https://support.microsoft.com/en-us/help/13776/windows-use-snipping-tool-to-capture-screenshots\"}"
}

```
