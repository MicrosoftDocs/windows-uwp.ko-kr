---
title: 화면 캡처 시작
description: 이 항목에서는 ms screenclip 및 ms-screensketch URI 스키마를 설명합니다. 앱 캡처 & 스케치 응용 프로그램을 실행 하거나 새 캡처를 열고 이러한 URI 체계를 사용할 수 있습니다.
ms.date: 08/09/2017
ms.topic: article
keywords: windows 10, uwp, uri, 캡처, 스케치
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 06e988387f574b74d511b14a2ebca24b0a149158
ms.sourcegitcommit: 079801609165bc7eb69670d771a05bffe236d483
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2019
ms.locfileid: "9116175"
---
# <a name="launch-screen-snipping"></a>화면 캡처 시작

**ms-screenclip:** 및 **ms-screensketch:** URI 스키마를 사용 하면 캡처 또는 스크린샷을 편집을 시작할 수 있습니다.

## <a name="open-a-new-snip-from-your-app"></a>앱에서 새 캡처를 엽니다.

**ms-screenclip:** URI에 앱이 자동으로 열고, 하 고 새 캡처를 시작할 수 있도록 합니다. 결과 캡처는 사용자의 클립보드에 복사 됩니다 하지만 자동으로 앱에 다시 전달 되지 않습니다.

**ms-screenclip:** 다음 매개 변수를 사용 합니다.

| 매개 변수 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- |
| 소스 | 문자열 | 아니요 | URI를 시작 하는 소스를 나타내는 자유형 문자열입니다. |
| delayInSeconds | int | 아니요 | 1 ~ 30의 정수 값입니다. 지연 되는 URI 호출과 캡처 시작 되는 시기 사이 전체 초를 지정 합니다. |
| callbackformat | 문자열 | 아니요 | 이 매개 변수를 사용할 수 없는 경우 |

## <a name="launching-the-snip--sketch-app"></a>캡처 & 스케치 앱 시작

**ms-screensketch:** URI를 사용 하면 프로그래밍 방식으로 캡처 & 스케치 앱을 실행 하 고 주석에 대 한 해당 앱의 특정 이미지를 열 수 있습니다.

**ms-screensketch:** 다음 매개 변수를 사용 합니다.

| 매개 변수 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- |
| sharedAccessToken | 문자열 | 아니요 | 캡처 & 스케치 응용 프로그램에서 열 파일을 식별 하는 토큰입니다. [SharedStorageAccessManager.AddFile](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.sharedstorageaccessmanager.addfile)에서 검색 합니다. 이 매개 변수를 생략 하면 앱에서 파일 없이 시작 됩니다. |
| secondarySharedAccessToken | 문자열 | 아니요 | 캡처에 대 한 메타 데이터를 사용 하 여 JSON 파일을 식별 하는 문자열입니다. 메타 데이터는 x, y 좌표 배열을 및/또는 [userActivity](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity) **clipPoints** 필드를 포함 될 수 있습니다. |
| 소스 | 문자열 | 아니요 | URI를 시작 하는 소스를 나타내는 자유형 문자열입니다. |
| isTemporary | 부울 | 아니요 | 화면 스케치 True로 설정 연 후 파일을 삭제 하려고 합니다. |

다음 예제에서는 사용자의 앱의 이미지 캡처 & 스케치 보내려면 [LaunchUriAsync](https://docs.microsoft.com/uwp/api/Windows.System.Launcher#Windows_System_Launcher_LaunchUriAsync_Windows_Foundation_Uri_) 메서드를 호출 합니다.

```csharp

bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-screensketch:edit?source=MyApp&isTemporary=false&sharedAccessToken=2C37ADDA-B054-40B5-8B38-11CED1E1A2D"));

```

다음 예제에서는 들어 **ms-screensketch** 의 **secondarySharedAccessToken** 매개 변수로 지정 된 파일을 보여 줍니다.

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
