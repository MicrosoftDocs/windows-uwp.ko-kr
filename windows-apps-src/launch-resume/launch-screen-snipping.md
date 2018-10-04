---
author: QuinnRadich
title: 화면 캡처를 시작 합니다.
description: 이 항목에서는 ms screenclip 및 ms-screensketch URI 스키마를 설명합니다. 앱 캡처 및 스케치 앱을 시작 하거나 새 캡처를 열고 이러한 URI 체계를 사용할 수 있습니다.
ms.author: quradic
ms.date: 8/1/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, uri, 캡처, 스케치
ms.localizationpriority: medium
ms.openlocfilehash: e18662125ef72051a289b3f1d0f3dc09b452d256
ms.sourcegitcommit: 5c9a47b135c5f587214675e39c1ac058c0380f4c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/04/2018
ms.locfileid: "4353290"
---
# <a name="launch-screen-snipping"></a>화면 캡처를 시작 합니다.

**ms screenclip:** 및 **ms screensketch:** URI 스키마를 사용 하면 캡처 또는 스크린샷을 편집을 시작할 수 있습니다.

## <a name="open-a-new-snip-from-your-app"></a>앱에서 새 캡처를 엽니다.

**ms screenclip:** URI에 앱이 자동으로 열고, 하 고 새 캡처를 시작할 수 있도록 합니다. 결과 캡처는 사용자의 클립보드에 복사 됩니다 하지만 자동으로 앱에 다시 전달 되지 않습니다.

**ms screenclip:** 다음 매개 변수를 사용 합니다.

| 매개 변수 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- |
| 원본 | 문자열 | 아니요 | URI를 시작 하는 소스를 나타내는 자유형 문자열입니다. |
| delayInSeconds | int | 아니요 | 1 ~ 30의 정수 값입니다. 지연 되는 URI 호출 및 캡처 시작 되 면 사이 전체 초를 지정 합니다. |

## <a name="launching-the-snip--sketch-app"></a>캡처 및 스케치 앱 시작

**ms screensketch:** URI를 사용 하면 프로그래밍 방식으로 캡처 및 스케치 앱을 실행 하 고 주석에 해당 앱의 특정 이미지를 열 수 있습니다.

**ms screensketch:** 다음 매개 변수를 사용 합니다.

| 매개 변수 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- |
| sharedAccessToken | 문자열 | 아니요 | 캡처 및 스케치 앱에서 열 파일을 식별 하는 토큰입니다. [SharedStorageAccessManager.AddFile](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.sharedstorageaccessmanager.addfile)에서 검색 합니다. 이 매개 변수를 생략 하면 앱을 열고 파일 없이 시작 됩니다. |
| 원본 | 문자열 | 아니요 | URI를 시작 하는 소스를 나타내는 자유형 문자열입니다. |
| isTemporary | 부울 | 아니요 | 화면 스케치 True로 설정 연 후 파일을 삭제 하려고 합니다. |

다음 예제에서는 사용자의 앱의 이미지 캡처 및 스케치 보내려면 [LaunchUriAsync](https://docs.microsoft.com/uwp/api/Windows.System.Launcher#Windows_System_Launcher_LaunchUriAsync_Windows_Foundation_Uri_) 메서드를 호출 합니다.

```csharp

bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-screensketch:edit?source=MyApp&isTemporary=false&sharedAccessToken=2C37ADDA-B054-40B5-8B38-11CED1E1A2D"));

```