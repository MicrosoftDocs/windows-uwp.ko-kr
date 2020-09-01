---
title: 화면 캡처 시작
description: 이 항목에서는 ms screenclip 및 ms screenclip URI 체계에 대해 설명 합니다. 앱은 이러한 URI 체계를 사용 하 여 캡처 & 스케치 앱을 시작 하거나 새 캡처를 열 수 있습니다.
ms.date: 08/09/2017
ms.topic: article
keywords: windows 10, uwp, uri, 캡처, 스케치
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 2d7471f414922eb1e4923079082ee6abfd8418bd
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167817"
---
# <a name="launch-screen-snipping"></a>화면 캡처 시작

**Ms screenclip:** 및 **ms screenclip:** URI 체계를 사용 하 여 캡처를 시작 하거나 스크린샷을 편집할 수 있습니다.

## <a name="open-a-new-snip-from-your-app"></a>앱에서 새 캡처를 엽니다.

**Ms screenclip:** URI를 사용 하면 앱에서 자동으로 새 캡처를 열고 시작할 수 있습니다. 결과 캡처가 사용자의 클립보드에 복사 되지만 자동으로 여는 앱으로 다시 전달 되지 않습니다.

**ms screenclip:** 다음 매개 변수를 사용 합니다.

| 매개 변수 | 형식 | 필수 | Description |
| --- | --- | --- | --- |
| source | 문자열 | no | URI를 시작한 원본을 나타내는 자유형 문자열입니다. |
| delayInSeconds | int | no | 1에서 30 사이의 정수 값입니다. URI 호출과 캡처 시작 시간 사이의 지연 (전체 초)을 지정 합니다. |
| callbackformat | 문자열 | no | 이 매개 변수는 사용할 수 없습니다. |

## <a name="launching-the-snip--sketch-app"></a>캡처 & 스케치 앱 시작

**Ms screensketch:** URI를 사용 하면 프로그래밍 방식으로 캡처 & 스케치 앱을 시작 하 고, 해당 앱에서 주석을 위해 특정 이미지를 열 수 있습니다.

**ms screensketch:** 다음 매개 변수를 사용 합니다.

| 매개 변수 | 형식 | 필수 | Description |
| --- | --- | --- | --- |
| sharedAccessToken | 문자열 | no | 캡처 & 스케치 앱에서 열 파일을 식별 하는 토큰입니다. SharedStorageAccessManager에서 검색 [되었습니다. AddFile](/uwp/api/windows.applicationmodel.datatransfer.sharedstorageaccessmanager.addfile). 이 매개 변수를 생략 하면 파일이 열려 있지 않고 앱이 시작 됩니다. |
| secondarySharedAccessToken | 문자열 | no | 캡처에 대 한 메타 데이터가 포함 된 JSON 파일을 식별 하는 문자열입니다. 메타 데이터에는 x, y 좌표 및/또는 [userActivity](/uwp/api/windows.applicationmodel.useractivities.useractivity)배열이 있는 **clipPoints** 필드가 포함 될 수 있습니다. |
| source | 문자열 | no | URI를 시작한 원본을 나타내는 자유형 문자열입니다. |
| isTemporary | bool | no | True로 설정 되 면 화면 스케치는 파일을 연 후에 파일을 삭제 하려고 시도 합니다. |

다음 예제에서는 [LaunchUriAsync](/uwp/api/Windows.System.Launcher#Windows_System_Launcher_LaunchUriAsync_Windows_Foundation_Uri_) 메서드를 호출 하 여 사용자의 앱에서 캡처 & 스케치로 이미지를 보냅니다.

```csharp

bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-screensketch:edit?source=MyApp&isTemporary=false&sharedAccessToken=2C37ADDA-B054-40B5-8B38-11CED1E1A2D"));

```

다음 예제에서는 **ms screensketch** 의 **secondarySharedAccessToken** 매개 변수에 지정 된 파일에 포함 될 수 있는 내용을 보여 줍니다.

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
  "userActivity": "{\"$schema\":\"http://activity.windows.com/user-activity.json\",\"UserActivity\":\"type\",\"1.0\":\"version\",\"cross-platform-identifiers\":[{\"platform\":\"windows_universal\",\"application\":\"Microsoft.MicrosoftEdge_8wekyb3d8bbwe!MicrosoftEdge\"},{\"platform\":\"host\",\"application\":\"edge.activity.windows.com\"}],\"activationUrl\":\"microsoft-edge:https://support.microsoft.com/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"contentUrl\":\"https://support.microsoft.com/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"visualElements\":{\"attribution\":{\"iconUrl\":\"https://www.microsoft.com/favicon.ico?v2\",\"alternateText\":\"microsoft.com\"},\"description\":\"https://support.microsoft.com/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"backgroundColor\":\"#FF0078D7\",\"displayText\":\"Use snipping tool to capture screenshots - Windows Help\",\"content\":{\"$schema\":\"http://adaptivecards.io/schemas/adaptive-card.json\",\"type\":\"AdaptiveCard\",\"version\":\"1.0\",\"body\":[{\"type\":\"Container\",\"items\":[{\"type\":\"TextBlock\",\"text\":\"Use snipping tool to capture screenshots - Windows Help\",\"weight\":\"bolder\",\"size\":\"large\",\"wrap\":true,\"maxLines\":3},{\"type\":\"TextBlock\",\"text\":\"https://support.microsoft.com/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"size\":\"normal\",\"wrap\":true,\"maxLines\":3}]}]}},\"isRoamable\":true,\"appActivityId\":\"https://support.microsoft.com/help/13776/windows-use-snipping-tool-to-capture-screenshots\"}"
}

```